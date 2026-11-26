# Client Communication


## Function of the Subsystem

The client communication subsystem is responsible for the security and delivery of the NTP data to the different devices on the network. It interfaces directly with the Clock Synchronization system to relay the system time dedicated by that system to its clients. Chrony is the software driving all NTP communication and synchronization. It allows the communications to be secured by NTS protocols. 

Upon startup the subsystem will receive the system time, as dictated by the Clock Synchronization subsystem. Before any NTP data will be transferred all clients must be authorized. To do this NTS uses the TLS handshake. This handshake takes place by the client initially reaching out to the server to ask for permission to communicate. Once accepted the client and server will negotiate credentials to be used during future communications. Once established the server will give the client cookies that act as proof of identity, the handshake concludes, and does not need to be repeated as long as the devices stay online. 

Now that the client is an authorized NTP recipient, it can request data from the server. The client constructs a request containing 4 main sections. A unique identifier, one of the NTS cookies, the number of cookies it would like in return, and an authenticator. This completed packet is then sent to the server. 

The server will validate the request using the cookie and construct an NTP response. It will capture its system time in the moment it receives the request. It will construct its response including the 3 timestamps it possesses, its estimated offset, and its return cookies before sending it back to the client. 

The client will validate the response, and then calculate its exact offset from the server clock using the 4 timestamps it possesses. It will then correct its system clock by slewing to the desired time. 


## Specifications and Constraints

This section should provide a list of constraints applicable to the subsystem, along with the rationale behind these limitations. For instance, constraints can stem from physics-based limitations or requirements, subsystem prerequisites, standards, ethical considerations, or socio-economic factors.

The team should set specifications for each subsystem. These specifications may require modifications, which must be authorized by the team. It could be necessary to impose additional constraints as further information becomes available.

Every subsystem must incorporate at least one constraint stemming from standards, ethics, or socio-economic factors.  

**List all constraints that NTS follows**  
RFC 9846 — The Transport Layer Security (TLS) Protocol Version 1.3  
RFC 8915 — Network Time Security for the Network Time Protocol   
RFC 9325 — Recommendations for Secure Use of TLS and DTLS   
RFC 9525 — Service Identity in TLS   
**List all constraints that NTP follows**  
RFC 8915 — Network Time Security for NTP  
RFC 5905 — Network Time Protocol Version 4  



## Overview of Proposed Solution

Describe the solution and how it will fulfill the specifications and constraints of this subsystem.


## Interface with Other Subsystems

Provide detailed information about the inputs, outputs, and data transferred to other subsystems. Ensure specificity and thoroughness, clarifying the method of communication and the nature of the data transmitted.


## 3D Model of Custom Mechanical Components

Should there be mechanical elements, display diverse views of the necessary 3D models within the document. Ensure the image's readability and appropriate scaling. Offer explanations as required.


## Buildable Schematic 

Integrate a buildable electrical schematic directly into the document. If the diagram is unreadable or improperly scaled, the supervisor will deny approval. Divide the diagram into sections if the text and components seem too small.

The schematic should be relevant to the design and provide ample details necessary for constructing the model. It must be comprehensive so that someone, with no prior knowledge of the design, can easily understand it. Each related component's value and measurement should be clearly mentioned.


## Printed Circuit Board Layout

Include a manufacturable printed circuit board layout.


## Flowchart

For sections including a software component, produce a chart that demonstrates the decision-making process of the microcontroller. It should provide an overview of the device's function without exhaustive detail.


## BOM

Provide a comprehensive list of all necessary components along with their prices and the total cost of the subsystem. This information should be presented in a tabular format, complete with the manufacturer, part number, distributor, distributor part number, quantity, price, and purchasing website URL. If the component is included in your schematic diagram, ensure inclusion of the component name on the BOM (i.e R1, C45, U4).

## Analysis

**NTS and TLS Security**  
A large portion of this project is its Network Time Security (NTS) capabilities. NTS is the protocol that allows the clients of our server to be sure the NTP signal they are receiving is both accurate, and from the device it should be. The additional security of NTS is broken down into two major parts: the TLS handshake, also called NTS-KE, and NTS for NTP. 
	The first portion of this process is the Transport Layer Security (TLS) handshake. It’s the protocol that establishes an initial connection between a server and client and sets the ground rules for their interaction in future communications. The version of TLS we implemented is specified as TLS 1.3. The process is as follows: 


## NTS-KE:
**Step 1**: The client connects to the server via, commonly via TCP, and initiates first contact. TCP is a transfer protocol that prioritizes accuracy of transmitted data over speed, and is the counterpart to UDP. The TCP three way handshake occurs with the client sending a SYN, the server responding with a SYN-ACK to acknowledge the signal, and the server finally responding with an ACK to acknowledge its acknowledgement. At this point the server and client have established a connection but the connection is not yet secured by TLS. [RFC 9846]  

**Step 2**: The client will begin TLS by sending the server a ClientHello message. This message contains multiple fields that communicate information to the server. The breakdown is as follows:  

       ProtocolVersion legacy_version = 0x0303;    /* TLS v1.2 */
       Random random;
       opaque legacy_session_id<0..32>;
       CipherSuite cipher_suites<2..2^16-2>;
       opaque legacy_compression_methods<1..2^8-1>;
       Extension extensions<7..2^16-1>;

*ProtocolVersion* is hardcoded to 0x0303 as it was a field used in older versions of TLS. In TLS 1.3 the protocol version is actually specified in the Extensions field, however for backwards compatibility the hardcoded 0x0303 is left untouched. 

*Random* is a randomly generated value of 32 bytes. It is used to generate the keying information that will be used to verify the client and server in future communications. 

*Legacy_session* is a 32 byte value that is used when a system is being upgraded to TLS 1.3 from an earlier version.  It allows for backwards compatibility.

*Cipher_suites* is a  list of the symmetric cipher options supported by the client, specifically the record protection algorithm (including secret key length) and a hash to be used with HKDF, in descending order of client preference.

*Legacy_compression_methods* is another field that was used in older versions of TLS where compression was used. In TLS 1.3 this field is a single byte, set to 0.

*Extensions* is the field where most of the client's information is stored. This field includes many subfields that are recommended but not required. The mandatory fields are bolded below. 


       Extension                   TLS 1.3
    supported_groups         [RFC7919] [RFC9846]  
    signature_algorithms     [RFC9846]  
    supported_versions       [RFC9846]  
    psk_key_exchange_modes   [RFC9846]  
    key_share                [RFC9846]  


The client also generates a secret key, and a public key. The public key is included in the ClientHello. 

After all of this information is sent the server knows what the client supports and knows its public key.  [RFC 9846]  

**Step 3**: ServerHello is the response the server sends after receiving the ClientHello. It is structurally the same as the ClientHello, including the same primary fields. It ,however, only responds with the extensions needed to secure a connection. The ClientHello sends the server its preferred settings and parameters as a list of options and the server selects among those options. Like the client the server also develops both a secret and a public key. The public key is among the extensions returned to the client. At the end of this step the client and server have shared their secret material and agreed on cryptographic parameters.  [RFC 9846]

**Step 4**: To generate the keys needed to authenticate, TLS uses the HMAC-based Extract-and-Expand Key Derivation Function (HKDF). The function is defined below: 

    HKDF-Expand-Label(Secret, Label, Context, Length) =
             HKDF-Expand(Secret, HkdfLabel, Length)
    
    
    struct {
            uint16 length = Length;
            opaque label<7..255> = "tls13 " + Label;
            opaque context<0..255> = Context;
        } HkdfLabel;

    Derive-Secret(Secret, Label, Messages) =
         HKDF-Expand-Label(Secret, Label,
                           Transcript-Hash(Messages), Hash.length)


With the keys acquired, the server and client encrypt the remaining steps in the handshake with these parameters. This encryption protects the confidentiality of the rest of the authentication.  [RFC 9846]

**Step 5**: This step is where the server sends its Certificate message. This is done to provide the client with the server’s identity credentials. The client then performs several checks before accepting the server’s identity. The client verifies the server’s, hostname, key usage, and cryptographic validity among others.  [RFC 9846]

**Step 6**: The server sends a CertificateVerify message to prove that it controls the private key associated with the present certificate. It does this by sending a signature algorithm, which the server and client agree on via the ClientHello and ServerHello. If the verification from steps 5 and 6 complete, the client has evidence the server possesses the private key and signed the current handshake. [RFC 9846]

**Step 7**: The server sends a Finished message to the client telling it it possesses the needed secrets and is ready for secure communication.  [RFC 9846]

**Step 8**: The client responds with its own Finished message to communicate the same thing. With this the TLS handshake is concluded and the devices are able to communicate securely.  [RFC 9846]

**Step 9**: With the handshake completed, the server and client exchange secret application data. The client sends a NTS-KE request. The server processes it and responds with the negotiated parameters and the initial cookies for NTS communications. The purpose of this step is to allow the different devices to communicate with the various applications running on the respective machines. Examples of such applications are: Web Browsing (HTTPS), APIs and Mobile Apps, Email, Messaging, and Database connections. Of course, for the NTP server, none of these applications will be used.  [RFC 9846]

This communication took place over TCP port 4460, however once the NTS-KE is complete, that connection ends and the NTP communications happen over UDP port 123. This is because the TCP and the TLS tunnel add much overhead that slows down the rate the devices can communicate. UDP requires much less overhead as it avoids retransmission and ordered-delivery behavior that can introduce variability in timing. [RFC 8915][RFC 5905]





## NTS for NTP:


**Step 1**: The client creates a standard NTPv4 packet with the NTS extension fields determined in the NTS-KE section appended onto it. The structure of the packet is defined below 

|------------------------------|
| NTPv4 Header|
|Unique Identifier | 
| NTS Cookie |
| Cookie Placeholders |
| NTS Authenticator | 
|------------------------------|

The NTPv4 Header contains the bulk of the NTP info and will be further discussed later. The Unique Identifier is a random value of at least 32 bytes. This random value is how each request is differentiated from others. The NTS Cookie was received in the NTS-KE section and is used to tell the server how to interpret the authentication tags, it does not act as the authenticator itself. The cookie is consumed after use. The NTS cookie placeholder is where the client can tell the server how many cookies it wants in return to then use in future communications. The NTS authenticator is the information that confirms to the server that the client is an authorized recipient. [RFC 8915]





**Step 2**: This is when the client authenticates the NTP request. It uses Authenticated Encryption with Associated Data (AEAD) to protect the packet. [RFC 8915]


**Step 3**: The client transmits the packet over UDP port 123. The reason UDP is used over TCP was specified above. [RFC 5905]

**Step 4**: The server receives the NTP request. This is the most important stage in the Client Communication Subsystem and happens in 4 stages: 
		*4.1* - The server captures the time T2 that the packet arrived at its network interface. It captures this time before doing any analysis on the received packet.  
		*4.2* - The server parses the NTP packet and verifies that it possesses all the required fields that satisfy both NTP and NTS protocols. If the packet is malformed it will reject it.  
		*4.3* - Recover NTS credentials. The server will extract the cookie from the request, retrieve all necessary keys from it. This allows the server to know how to respond with the client without it needing to have a record of all past interactions.  
		*4.4* - Verify the NTS authenticator. With the Client-to-Server (C2S) key and AEAD algorithm extracted from the cookie the server will verify the NTS authenticator sent with the request. If it succeeds, the packet will be accepted as an NTP request.[RFC 8915]  

**Step 5**: The server will construct an NTP response. The server will use the current synchronized time provided by the Clock Synchronization Subsystem. Four total timestamps are needed to perform the synchronization. 

$T_1$ = The time the client originally sent the request  
$T_2$ = The time the server initially received said request  
$T_3$ = The time the server sends its response  
$T_4$ = the time the client receives the response from the server  


It creates an NTP response containing: 


T3 is a challenge due to the fact that a lot of cryptographic processing happens after it is captured. To best counter this, T3 needs to be captured as one of the last things added to the packet to have the least possible amount of steps after it, before transmission. [RFC 5905]

**Step 6**: The server generates replacement cookies according to the amount specified in the clients original request. [RFC 8915]

**Step 7**: The server authenticates and transmits its response. It follows the same AEAD algorithm agreed upon previously, and formats the packet in the same structure as step 2. However, instead of the C2S key, it uses the Server-to-Client (S2C) key instead. Once the packet has been authenticated it transmits it over UDP port 123. [RFC 8915]

**Step 8**: The client receives the response and immediately calculates T4. It then follows the same verification process as step 4, this time retrieving and storing the returned cookies for future use. Once the packet has been accepted, it uses the 4 timestamps to calculate the clock offset and trip delay. 

It will then calculate the offset ($\theta$) and the round trip delay ($\delta$) with the following formulas


Offset:  

$$
\boxed{\theta = \frac{(T_2 - T_1) + (T_3 - T_4)}{2}}
$$

Round Trip Delay:  

$$
\boxed{\delta = (T_4 - T_1) - (T_3 - T_2)}
$$

With the calculations complete and the timestamps acquired, the client is able to update its local clock according to any processes and algorithms it chooses. [RFC 8915]



This process continues repeatedly every time the client wants its clock updated.


## References

All sources that have contributed to the detailed design and are not considered common knowledge should be duly cited, incorporating multiple references.
