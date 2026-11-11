# Conceptual Design
## Introduction
Precise, accurate, and secure timing is critical for many applications, from banking networks to national security, however, in certain cases, the individual, such as a hobbyist, may also require such timing systems for their own needs. Our project shall create and demonstrate a framework in which an individual may have their own precise, secure, and available timing system.   

Unsecured time servers are vulnerable to a multitude of issues, including spoofing, man-in-the-middle attacks, connection loss, as well as unavailability due to power loss and other factors.  Additionally, being reliant upon only one source for time exacerbates these concerns. A failure or attack on any of these elements could create a cascade of issues.  

This project aims to provide a solution to this issue, via a more secure and robust, GPS-based, NTP server that may be replicated by the individual. We aim to use GPS as the primary time source, with secondary sources acting to corroborate and confirm the GPS. These include other GNSS systems, such as Galileo or BeiDou, radio broadcasts, oscillators, and internet sources. We shall provide a multilayered defense, including NTP and authentication, as well as comparing and contrasting our various sources, and fail-safes for signal or power loss.   

Off-the-shelf solutions, while available, do not provide the same functionality relative to our expected cost, often featuring less robust servers or being prohibitively expensive. Additionally, due to our design being intended to be disseminated to the public, there will theoretically exist space within the project for the individual to make changes, and add or remove elements as they need. 

  
## Fully Formulated Problem
The Primary objective of this project is to build a low cost, accessible, and effective Network Timer Server that provides accurate timestamps by using time references like satellite constellations and online servers. The system shall also be secure from attempts of subversion such as DDOS and Man in the Middle attacks.

The system shall be able to be constructed easily and inexpensively for use by the general public. In doing so, individuals and organizations will have access to extraordinarily accurate and secure time to ensure proper operation of their systems.

## High-Level Solution
The Network Time Server provides a secure platform that will allow its users to retrieve accurate timestamps which will allow them to properly operate their systems. By integrating accurate time sources, precise and reliable hold-over systems, protection from malicious entities, and flexible, capable open source software,the design will achieve accurate timestamps even in hostile enviorments.

To meet the criterion proposed above, the system will be composed of:


- A Power Supply capable of providing adequate power during normal operation and able to switch to a backup if there is an interruption 
- A RHCP Antenna to properly receive GNSS signals which contain the timestamps desired
- A dedicated GNSS Receiver that will perform the signal processing, and able to transmit the acquired time via PPS
- A Processor that will receive the processed GNSS signal with the timestamp, discipline the holdover system, run security software, and distribute timestamps to clients
- A software that will run Network Time Protocol and Network Time Security
- A software that will ensure reliable timestamps are distributed to clients
- An enclosure that will hold and protect the system


## Comparative Analysis of Potential Solutions

### Power Supply
The Power Supply subsystem is responsible for providing consistent electrical power to the Network Time Server and its connected components. Reliable power is important to ensure the server can maintain accurate time and continue distributing timestamps to clients. Because interruptions in electrical power could cause the server to lose operation, several potential power configurations were considered.

**External AC/DC Power Supply:** This option would involve using an AC/DC adapter to convert standard wall power into the DC voltage required by the system. The proposed design uses a 5.1 V, 15 W AC/DC adapter to power the Raspberry Pi 3. This method is relatively simple and inexpensive to implement and provides a consistent source of power during normal operation. However, the primary disadvantage is that the server would lose power during an electrical outage unless an additional backup source were included.

**Battery-Only Power Supply:** This option would involve using a rechargeable battery as the primary power source for the server. The main advantage is that the system would not depend on a continuous connection to an AC outlet. However, the amount of time the server could operate would depend on the available battery capacity and the total power consumption of the system. The battery would also require periodic charging and monitoring to ensure that sufficient power remains available.

**AC/DC Power Supply with Battery Backup:** This option combines an external AC/DC adapter with a rechargeable battery backup system. During normal operation, the AC/DC adapter supplies power to the server while the battery remains available for backup. If the primary power source is interrupted, the backup system is intended to maintain power to the server and its critical components. The proposed design uses a 5.1 V, 15 W AC/DC adapter along with a 16,000 mAh, 18 W mini UPS. Although this configuration introduces additional cost and implementation considerations, it provides the benefit of having both a primary power source and backup power available.

After considering the advantages and disadvantages of each configuration, the AC/DC power supply with battery backup was selected for the project. This configuration provides a reliable primary power source while also allowing the server to continue operating during power interruptions, provided the backup system supports the complete system load. Before final implementation, the voltage and current requirements of the Raspberry Pi, GNSS receiver, OCXO, and other connected components must be verified to ensure compatibility with the selected equipment.

| **Power Configuration** | **Advantages** | **Disadvantages** |
| ------------ | ------------ | ------------ |
| External AC/DC Power Supply | Simple implementation, relatively inexpensive, provides consistent power during normal operation. | No backup power during an electrical outage. |
| Battery-Only Power Supply | Independent of wall power, allows operation during electrical outages. | Limited operating time, requires charging and battery monitoring. |
| AC/DC Power Supply with Battery Backup | Provides a primary power source and backup power, improves system availability during outages. | Higher cost, additional components, requires verification of power compatibility and switchover behavior. |

### Antenna


[1]Novatech, “What is Circular Polarization and Its Impact on GNSS/GPS Antennas?,” Novatech.com, 2026. https://novotech.com/pages/circular-polarization (accessed Nov. 10, 2026).

[2]“GNSS Antennas RF Design Considerations for U-Blox GNSS Receivers Application Note.” Accessed: Nov. 10, 2026. [Online]. Available: https://content.u-blox.com/sites/default/files/products/documents/GNSS-Antennas_AppNote_%28UBX-15030289%29.pdf

The antenna is the backbone of the entire system. It is what receives timestamps from the satellite constellations and therefore is of vital importance. From research, the antenna must be able to receive Right-Hand Circular Polarization (RHCP) signals as it's what most GNSS satellites use. RHCP signals have these advantages: reduced signal obstruction, as RHCP maintains the integrity of signals through buildings, trees ect, Enhanced Multipath Rejection, as RHCP favors direct line of sight, and improved reception quality as RHCP maintains signals quality across various orientations[1]. To utilize these qualities, the system will acquire a RHCP antenna of one of these types: Patch Antenna, Helix Antenna, and Choke Ring Antenna.



### GNSS Module
The GNSS Module is the main source of timestamps of the system. It should be able provide accurate timestamps even in nonideal conditions such as cloudy skies, or bad atmospheric conditions. It will also be able to detect spoofing and jamming attempts. There are two main approaches to processing the data received from the antenna which are using a dedicated GNSS Module or using a Software Defined Radio (SDR).  

The SDR approach is viable but has severe disadvantages that prevent it from being practicable. Firstly, it requires the user to have experience in the broad field of Digital Signal Processing. They must know how to properly set of filters, handle IQ manually, and understand modulation schemes. They also must be generally skilled in embedded system development. They must have knowledge of GNSS operation and how to account for them. They need to be able to account for timing delay incurred by the distance the signal will travel, and effects caused by the atmosphere. Another consideration is that to acquire an SDR capable of receiving and processing GNSS signal, the cost is usually much greater than what a dedicated GNSS Module would cost. With these considerations, the SDR approach introduces unnecessary complications and cost.

The GNSS approach is the preferred approach for our project. They are completely self-contained boards, which require little to no knowledge of the underlying mechanics of GNSS and signal processing. They are capable of filtering and extracting useful information like timestamps and positioning data with just configuring the board, and letting it do all the work. Its also capable of delivering UART, SPI, and I2C to communicate with external modules and it's able to emit PPS to transmit accurate timing events. The last consideration is cost. We can acquire a board that can perform all these functions for around 100$, which is well within budget.

With these considerations, the project will move forward with a dedicated GNSS Module.

| Approach | Advantages | Disadvantages |
|---|---|---|
| **Dedicated GNSS Receiver** | Integrated RF and signal-processing components | Less flexible than an SDR |
| **Dedicated GNSS Receiver** | Requires minimal GNSS signal-processing expertise | Limited access to raw GNSS signal data |
| **Dedicated GNSS Receiver** | Provides timestamps, positioning data, and receiver status | Limited control over internal processing algorithms |
| **Dedicated GNSS Receiver** | Supports UART, SPI, and I²C communication | Dependent on manufacturer-provided functionality |
| **Dedicated GNSS Receiver** | Provides precise PPS timing output | Spoofing and jamming detection capabilities vary |
| **Dedicated GNSS Receiver** | Lower development and integration complexity | Performance can be affected by interference, obstruction, and multipath |
| **Dedicated GNSS Receiver** | Relatively low cost (approximately $100) | |
| **SDR** | Highly configurable and flexible | Requires knowledge of DSP, RF engineering, and GNSS |
| **SDR** | Provides access to raw I/Q signal samples | Requires software development for signal acquisition and tracking |
| **SDR** | Allows custom GNSS signal-processing implementations | Additional effort required to generate and validate accurate PPS output |
| **SDR** | Supports experimentation with different GNSS signals | May require additional RF hardware and processing resources |
| **SDR** | Greater control over signal acquisition and tracking | Greater development and integration complexity |
| **SDR** | Can be adapted for custom interference detection and analysis | Potentially higher overall system cost |
| **SDR** | Flexible for future signal-processing applications | Requires extensive testing to establish timing accuracy and reliability |



### Processor

This is the brains of the system. It coordinates the operation of the GNSS Module, external oscillator, and transmission of timestamps to clients. It will be able to change the settings of the system to the user's desired settings. It will also coordinate security and quality checks. However, there are three possible architectures we could implement the NTS on: a System on Chip (SoC), Microcontroller Unit (MCU) and Field Programmable Gate Array (FPGA). All three capable of receiving timing data from a GNSS Module and tuning a holdover system, but they differ in raw processing capability, software support, and the ability to deliver timestamps to clients[1].

A MCU architecture offers several advantages such as low power consumption, simplicity, and deterministic timing due to most MCUs using light weight operating systems or Real Time Operating Systems. This is very beneficial as it allows the system to receive time signals like PPS (Pulse Per Second) accurately. However, there is little support when it comes to implementing NTS and NTP. It would require manual set up of network communication, holdover synchronization, and security protocols. Overall, MCU architecture would introduce significant software complexity to the project.

An SoC architecture provides many advantages such as much greater processing power, and features a full operating system, like Linux. This allows the project to make full use of existing solutions to synchronization, network communication, and security. For example, Chrony is a popular software that incorporates security, synchronization, and communication in one complete package that is very easy to set up on a Linux based system. However, since the system is running a complete operating system, additional jitter and uncertainty are introduced due to scheduling and processing latency[2]. This can be accounted by using a real-time kernel or configuring the kernel with proper consideration.

A FPGA architecture potentially offers the best performance of all the architectures. Due to the fact that we are implementing our algorithms in hardware, there is little to no latency introduced. However, this is where any advantage ends. FPGAs are notoriously hard to program and debug. There is no support for existing solutions to discipline and oscillator, and then distribute timestamps. FPGAs are also generally more expensive than both SoCs and MCUs. Due to these considerations, FPGAS will not be considered. 


|       | SoC Implementation | Microcontroller Implementation | FPGA Implementation |
| ----- | ------------------ | ------------------------------ | ------------------- |
| **Pros** | - Ease of implementation<br>- Built-in support for NTP/NTS and other security protocols<br>- Greater processing capability<br>- Support for existing synchronization software, such as Chrony<br>- Simplified integration of multiple time sources<br>- Easier time source monitoring and quality tracking<br>- Advanced networking capabilities<br>- Easier remote configuration and system management<br>- Greater flexibility for future software updates and features | - Lower power consumption<br>- Lower hardware cost<br>- More compact design<br>- Deterministic timing behavior<br>- Less operating system overhead<br>- Simpler hardware architecture<br>- Potentially faster response to PPS signals | - Highly deterministic timing behavior<br>- Very low and predictable hardware latency<br>- Parallel processing capabilities<br>- Precise control over PPS capture and generation<br>- Customizable hardware architecture<br>- Can implement dedicated timing and signal-processing logic |
| **Cons** | - Potentially higher hardware cost<br>- Operating system scheduling introduces timing jitter and uncertainty<br>- Greater power consumption<br>- More complex operating system configuration and maintenance<br>- May require a real-time kernel or hardware timestamping to improve timing performance | - Limited processing and memory resources<br>- Limited support for existing NTP/NTS software<br>- Requires custom implementation of networking and security features<br>- More difficult integration of multiple time sources<br>- Greater software development effort<br>- More difficult remote management and maintenance<br>- Limited flexibility for future functionality | - High development complexity<br>- Requires specialized hardware description languages and FPGA development knowledge<br>- Difficult debugging and verification<br>- Limited support for existing NTP/NTS software<br>- Requires additional software or processing hardware for network communication and security<br>- Potentially higher hardware cost<br>- Longer development time |

Due to these facts, the system will use an SoC with a Linux based operating system. With the immense ecosystem setup around this choice, the project will be able to be created with much more ease and reliability.


### Client Communication and Clock Synchronization
The Client Communication amd Clock Synchronization subsystems are the backbone of the NTP server. They ensure accurate system time in the server and ensure proper distribution of timestamps to clients, and ensure the clients have proper authentication.

The functionality of these subsystems is what makes this project an NTP server at all, and therefore we need to take careful consideration of how they will be implemented. When researching solutions to the software side of the project, a few major potential solutions stuck out. 

**Custom NTP Software:** 
This would involve writing the software to receive timestamps, calculate accurate internal timing, process, and respond to NTP requests securely. This method would allow us full control over the implementation, and forces a deep understanding of the protocols used. The downside is the mountain of research and development that would go into it.  

**Existing NTP Software:**
This would involve using an established NTP implementation such as Chrony or NTPsec. There are many advantages to taking this route. One of which is that we can build off of tools with proven implementation and support for everything that may be needed. In addition, there exists a wealth of documentation and user threads that explain and help troubleshoot these softwares. The primary downside would be that we have less control over the implementation. Given the many advantages to this type of implementation, this is the route we went with.   

However even after the decision to use existing software was made, there still existed many software options to choose from. Below is a breakdown of the main NTP tools available.  


| Software | Pros                                                                                                                                                                     | Cons                                                                                                                                                              |
|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Chrony[2]   | It's the modern day gold standard. It has functionality for up to nanosecond accuracy, NTS capabilities, is VM-friendly, and designed to work well on conjested networks | Complex configuration when compared to its alternatives                                                                                                           |
| NTPD[3]     | Capable of working on very old systems. It is also well documented as a result of its age.                                                                               | It is the original NTP software. Requires legacy operating systems. Includes very little security functionality                                                   |
| NTPsec[4]   | Modernized version of NTPD. Developed to add modern day security requirements to these legacy systems.                                                                   | It is not used very often and as a result has limited documentation and support. It is a direct competitor to Chrony and is often looked over in favor of Chrony. |
| OpenNTPD[5] | Very lightwieght and requires very few system resources. Developed to be very secure.                                                                                    | Less accurate as a result of being designed for security over all else.                                                                                           |

### Holdover Oscillator
Under normal conditions, the system will receive timing from the satellite constellations or other known sources. However should the connection get lost, or if timing information is required between communication periods of the other time sources, an accurate backup, or holdover, time source is needed. While other types of oscillators exist, a crystal oscillator is among the most consistent, accurate, and available holdover source on the market, as such a crystal oscillator will be our holdover oscillator source.
There are three main types of crystal oscillator, each with different levels of accuracy[^1000]:
- A standard Crystal Oscillator (XO), with a typical frequency tolerance of +/-25 ppm to +/-100 ppm
- A Temperature Compensated Crystal Oscillator (TCXO), with a typical frequency tolerance of <+/-2ppm
- An Oven Compensated Crystal Oscillator (OCXO), with a typical frequency tolerance of <+/-500 ppb or <+/-0.5ppm
It is worth noting that the units ppm is defined by the relative error of the oscillator (f) compared to the nominal frequency (fo):
$$
\text{ppm offset}=\frac{f-f_o}{f_o}*10^6
$$
To determine what level of accuracy we need, we first recognize that the oscillator and nominal frequency are both periodic in nature, so we approximate each as a sinusoid:
$$
H_{nom}(t)=A_{nom}\sin{(2\pi f_ot)}
$$
$$
H_{osc}(t)=A_{osc}\sin{(2\pi ft)}
$$
As a sinusoid, each will reach a compete oscillation at an even integer multiple of pi:
$$
2\pi f_ot_{nom} = 2n_o\pi
$$
$$
2\pi ft_{osc} = 2n\pi
$$
Simplifying these expressions down to find the time at some zero, n:
$$
t_{nom}=\frac{n}{2f_o}
$$
$$
t_{osc}=\frac{n}{2f}
$$
We can then find the difference between the time values to approximate the time delay between the signals at some zero n:
$$
t_{nom}-t_{osc}=n\left( \frac{1}{f_o}-\frac{1}{f} \right)
$$
Finally, to find the accuracy, we let n be the number of oscillations per second, which will just be the nominal frequency
$$
n=f_o
$$
$$
t_{nom}-t_{osc}=\left(1-\frac{f_o}{f}   \right)
$$
This is the approximate drift per second, to get the drift per day we just need to scale this value by the number of seconds in a day (86400 s/day)

$$
\text{drift (s/day)} = 86400 \left(1-\frac{f_o}{f}\right)
$$

To determine what class of oscillator to use, we reverse this process, taking our drift per day to find the ppm (or ppb).

Using the equations defined above, the following expression can be obtained:
$$
\text{ppm offset}=\left( 1- \frac{86400}{86400+\text{drift(s/day)}} \right)*10^6
$$
## CHANGE THE NUMBERS IN THIS SECTION RAAAAAAAAA
Finally, we can plug in our desired drift of **1s/day** and find that we need a ppm offset of ~**11.57** ppm which means that a TCXO is, oddly the best choice. 
### Enclosure
The enclosure is the housing that will contain all the subsystems, except for antenna, in a neat way. Given the nature of the project a custom enclosure is required. As this project needs to be accessible, complex housing techniques such as milling or injection molding are off the table, which leaves 3D printing. 3D printing will provide the most accessible results, and will allow the user to customize more easily if desired.
The material used to print the enclosure is ultimately up to the end user to decide, but PETG offers a good middle ground in heat resistance, durability, and ease of printing and as such is recommended for this project.

### Hardware Block Diagram
<img src="figures/Hardware Diagram.svg" alt = "Hardware Diagram" width="600">
Note that the radio clock module in this diagram is an optional component.  

### Operational Flow Chart

<img src="figures/Operational_Flowchart1.svg" alt = "Operational Flowchart" width="1100">

  The NTP server is designed to operate autonomously after the initial admin configuration. During normal operation the server receives data from upstream time sources and uses them to discipline its own system clock. Upon request from a client the server will relay its system time to allow the client to sync its own clock with the NTP server. Both the Clock Synchronization and Client Communication subsystems loop repeatedly, with Signal Acquisition being self contained loop inside the Clock Synchronization subsystem. To show further detail, Signal Acquisition and Clock Synchronization are seperated above. Users will primarily be interacting with the server during the initial install and subsequent maintenance and troubleshooting. The operational flowchart showcases the normal operating behavior of the server.


## Subsystems

### Power System

The Power System is responsible for supplying stable and continuous electrical power to the components of the Resilient and Secure Network Time Server. During normal operation, the system will receive power from an external source and provide the required DC power to the System-on-Chip (SoC), GNSS receiver, external oscillator, and other supporting hardware. In the event that the primary power source is lost, a backup battery bank will provide an alternate source of power to allow the system to continue operating. The final power requirements will be determined by the voltage and current requirements of the hardware selected for each subsystem.

**Functions:**

- Supply the required electrical power to the components of the Network Time Server
- Convert the external power source to the DC power required by the system hardware
- Maintain stable power during normal system operation
- Provide backup power during the loss of the primary power source
- Distribute power to the SoC, GNSS receiver, external oscillator, and other required hardware
- Support continued operation of critical system components during a temporary loss of primary power

**Inputs:**

- External electrical power from the primary power source
- DC electrical power from the backup battery bank when primary power is unavailable

**Outputs:**

- Regulated DC power supplied to:
	- The System-on-Chip (SoC)
	- The GNSS receiver
	- The external oscillator
	- Additional supporting hardware as required by the final design

**High-Level Requirements:**

- The Power System shall provide the required operating power to all critical components of the Network Time Server.
- The Power System shall provide voltage and current within the operating requirements of each connected component.
- The Power System shall provide backup power when the primary power source is unavailable.
- The Power System shall support continued operation of the SoC and critical timing hardware while sufficient backup power is available.
- The Power System shall return the system to primary power operation when the primary power source is restored.
- The Power System shall provide sufficient power to support the maximum expected electrical load during normal operation.
  
## GNSS Module

The GNSS Module is the main source for retrieveing the timestamps that our system will be distributing downstream. First, it recieves a signal containing the 
timestamp from a satellite constallation. Second, it will run a check to attempt to detect spoofing, and it will then log ephemermis imformation if power is lost
or a hot start is attempted. Lastly it will transmitt the timestamp to the main processing unit for further use.

Functions:
- Recieve Timestamps
- Perform Security Checks

Inputs:
- Timestamps from satellites
- Programming from processor using USB
- Power from power supply

Outputs:
- Timestamp sent to processor using I2C/UART/PPS
- Current Status sent to processor using I2C/UART

Interfaces:
- Settings Control from processor 
- Data echange with processor
- Power from power supply

High-Level Requirements:
- Shall Retrieve accurate timestamps
- Shall be secure from attempts to spoof or attack it

Sources of uncertainty:

[1]“ITU-T Technical Report GSTR-GNSS Considerations on the Use of GNSS as a Primary Time Reference in Telecommunications,” 2020. Accessed: Nov. 09, 2026. [Online]. Available: https://www.itu.int/dms_pub/itu-t/opb/tut/T-TUT-HOME-2020-PDF-E.pdf


The main sources of uncertainty introduced when the signal is transmitted from the satellite are the ionosphere, troposphere, multipathing, and hardware delays. However, it’s hard to calculate the effects from many of these sources, so we will use the simplified model of pseudo-range.

Pseudo-range is the apparent (not absolute geometric distance due to phenomena that affects propogation) distance between a satellite and the receiver, which is calculated as the difference between transmission and reception times. 

It is defined as:

$$
\
\sigma =
\sqrt{(x_i-x_a)^2+(y_i-y_a)^2+(z_i-z_a)^2}
+c(t_{\text{offset}}+dt_{ar})
=c(t_{ar}-t_i)
\
$$

$x_i$ , $y_i$ , $z_i$ is the current satellite location

$x_a$ , $y_a$ , $z_a$ is the current receiver location

$c$ is the speed of light

$t_{offset}$ is the real time offset of the receiver’s local time

$dt_{ar}$ is the real propagation delay between the antenna phase center and receiver correlator

To account for the time delay rearrange the above equation:

Set:

$$
\
R = \sqrt{(x_i-x_a)^2+(y_i-y_a)^2+(z_i-z_a)^2}
\
$$

$$
\
\delta t = t_{\text{offset}} + dt_{ar}
\
$$

$$
\
\sigma = R + c\delta t
\
$$

Rearranging to isolate the timing offset:

$$
\
\boxed{\delta t = \frac{\sigma-R}{c}}
\
$$



[1]“One Way GNSS Time Transfer | NIST,” NIST, May 17, 2016. https://www.nist.gov/pml/time-and-frequency-division/time-services/one-way-gnss-time-transfer (accessed Nov. 10, 2026).


In practice, one satellite cannot solve the above equations accurately, which is why modern GNSS systems us multiple satellites to solve the above equations. For our purposes, we can used established sources and we can estimate that the time delay from the satellites to our receiver will be roughly 3 to 6 ns if we know the location of the receiver within a few meters, which works as that is around the accuracy of most satellites[1].


## Processor

The processor is responsible for orchestration of the entire system. Its primary duties are recieving timestamps from the GNSS module and checking the stamp's integrity.
Run an open source software called Chrony, which is an extremely powerful software that allows for rigorous security, and flexible distrobution of the timestamps
to clients downstream. It will also tune an oscillator to timestamps recieved, which is needed if the system loses synchronization to the time sources.
The processor will also be used to program external modules like the GNSS module to achieve the desired settings the user wants. Lastly, the processor will transmit
the secure timestamps to clients, and will display its current settings and status to the user.

Functions:
- Recieve and process timestamps
- Tune the holdover system
- Enforce rigorous security measures
- Transmit timestamps to clients

Inputs:
- Commands to change system settings
- Timestamps from GNSS via UART/I2C
- Status from peripherials
- Power from Power Supply

Outputs:
- Timestamps to clients over wifi or ethernet cable.
- Commands to peripherials over I2C/UART
- Current status and settings

Interfaces:
- Information from external modules (GNSS, Oscillator)
- Transmission of timestamps to clients
- Power from power supply

High-Level Requirements:
- The system shall recieve accurate timestamps from satellite constellations
- The system will ensure the timestamps are secure and that the system is resistant from attempts to subvert it
- The system will be able properly control peripherials and transmitt accurate timestamps to clients

  
## Client Communication  
Client communication is the system that uses the hyper-accurate system time determined by the Clock synchronization subsystem and relays that to the clients in need of NTP data. The client will request NTS credentials to be able to verify its identity in the future using TCP port 4460. This is called the Transport Layer Security (TLS) handshake. The client and server register each other as trusted communicators and create credentials called cookies that are used in future communications [6]. When the client needs NTP data it sends a request on UDP port 123 to the server using its cookies, the server receives processes and sends back the accurate time data. Using the time given by the server the client will adjust its internal clock to match the server, taking into account any delay accrued from the transmission. [7]  

The client calculates the correct time and offset given 4 timestamps:  
$T_1$ = The time the client originally sent the request  
$T_2$ = The time the server initially received said request  
$T_3$ = The time the server sends its response  
$T_4$ = the time the client receives the response from the server  

It will then calculate the offset ($\theta$) and the round trip delay ($\delta$) with the following formulas


Offset:  

$$
\boxed{\theta = \frac{(T_2 - T_1) + (T_3 - T_4)}{2}}
$$

Round Trip Delay:  

$$
\boxed{\delta = (T_4 - T_1) - (T_3 - T_2)}
$$


Functions: 
- Supply accurate NTP data to the clients on its network  
- Maintain high levels of security using NTS to ensure the accuracy of the data  

Inputs:  
- Clock disciplined system time determined by the clock synchronization system.   

Outputs:  
- NTP signals, secured by NTS, carrying the data to clients

High-Level Requirements:
- The system shall relay secure NTS protected signals  
- The system shall relay accurate information to connected clients

## Device Security  
Device security encapsulates the measures needed to protect the server from cybersecurity threats beyond incorrect timing data. It will allow the device to be safe from bad actors on its network. To ensure a secure server, we will implement a firewall to lock down the ports and allow only the ones needed for basic server function. This includes port 123 for NTP, port 4460 for NTS and port 22 for SSH (A secure network protocol used to remotely access systems)[6][7]. To allow for secure administrator control SSH key authentication will be implemented to prevent potentially faulty passwords that can be compromised[8]. We will restrict Chrony to a non-root user to ensure that if Chrony is compromised, the attacker cannot gain access to the entire server. Monitoring will also be used to ensure important configuration files are secure and unchanged and noting any NTP synchronization anomalies.  

Functions:  
- Protect the NTP server from unauthorized access and attempted compromise.
- Safely monitor the NTP server to allow for secure admin access.
- Log suspicious activity for administrator review.

Inputs: 
- Incoming internet communication and signals of any kind
- Information from connected physical devices such as USB interfaces

Outputs: 
- Security-authorized inputs to be passed onto the Clock Synchronization subsystem

High-Level Requirements:
- The system shall remain secure and protected from intentional interference using firewalls and software quarantining. 
- The system shall block all incoming signals except those needed for device functionaility. 
- The system shall provide secure access for administrators using SSH.

## The Holdover Oscillator   
The Oscillator provides a consistent periodic signal as a time reference for the system to use when all other methods fail, or to use between periods of incoming reference signals for a more accurate device output. Depending on the type of oscillator used, if it is not an enclosed unit, we may need to implement a control loop to output a signal suitable for the SoC. 

Functions:
- Providing a known time reference signal
Inputs:
- Power from power supply
Outputs:
- A known reference frequency sent to the SoC
High-Level Requirements:
- The system shall keep a holdover with at most a 1 s/day drift
- The system shall maintain a consistent and predictable pulse frequency

## Clock Synchronization

The system will receive various signals and, using the data received, output one unified time signal.  GPS will be ordinarily taken as standard, and thus used as the primary time source.  However, if GPS signal is lost, or falls outside of acceptable tolerance, then other sources will be brought to the foreground. [9]  The first to be used after GPS fails will be the alternate GNSS constellations, such as Galileo or BeiDou.  Should those also fail, the internet sources, such as NIST, will be taken.  Finally, the WWVB Radio signal will be taken.  [10]   All of these will be compared against the oscillator's holdover system, which, in the event of total signal loss, will act as the last clock source remaining.  
The output will be the time code received of the most reliable signal. 

Should a jump in real-time output occur, the output will be slewed into accuracy.  Slewing is the process by which a clock, rather than skipping ahead in time, alters its speed to gradually match the correct time.

The reliability check will work as such: each received signal will be taken, with an expected tolerance (transmission/propagation delay, jitter, etc), and compared against each other and the oscillator, weighted in terms of priority.  If a signal is an outlier, it will be momentarily flagged until it comes back into agreement. 

Purpose:
- Validate the accuracy of received time signals and determine output signal.

Input:
- Internet Sources
- Oscillator and PPS
- GPS + GNSS
- WWVB Radio

Output:
- Accurate time signal upon client request

High Level
- The system shall receive multiple signal inputs.
- The system shall validate the received inputs and disregard those declared invalid.
- The system shall output a valid time signal. 




## Enclosure

The enclosure subsystem pertains to the elements physically holding the entire project together. 

The specific dimensions of the outer shell will be determined at a later date when exact hardware is chosen. It will provide physical protection to the systems inside from tampering and physical damage. Mounts will be used to hold the components securely inside of the enclosure. These will ensure that the pieces remain where they are intended to be. Depending on the overall wattage and temperature produced, a cooling device may be needed to maintain a certain internal temperature, so that the hardware's function will not be affected. An internal fan will likely be used for this purpose.  

Additionally, ports are to be included so that the power supply and any other exterior components may interface with the internal components.

Finally, our electrical systems need to be connected with wiring to facilitate the needed communication between systems.

Purpose:
- House the electrical and internal components securely.

High Level:
- The system shall regulate the design's temperature.
- The system shall maintain the device's integrity.



## Ethical, Professional, and Standards Considerations

This project will expand on the security and accuracy of currently available timing systems, which is a crucial part of PTN (positioning, timing, navigation). Timing, due to its broad use and necessity in critical systems, has a great need for improvements to security. This project will aim to provide these needed improvements for an affordable cost.

Additionally, the design and dissemination of this project will contribute to greater time server independence, meaning that this project will likely provide timing more precise and resilient than standard, default internet based timing, while also no longer being so overly reliant on the internet. Finally, the project will be released publicly, thus reducing the barriers for any who may be interested in following this design. This improves accessibility and inclusivity of the project.

Risk to Network Security
This project will involve network connections from various sources, including international sources. These, along with communications between the server and the client, leave the potential open for malicious activity. 
Mitigation: We plan to make the signal communications secure via systems like NTS, and quarantine the signals to only what they are needed for, thus keeping any malicious activity reasonably contained.

False sense of security
This project, as it is designed for improving the security of time servers, may give the user an overconfidence in this design, such that they may imagine hat they will have no risk of tampering or failure. This is not the case, even the completed design will not be immune to all issues, and theoretically a sufficiently skilled malicious actor could still disrupt the design. 
Mitigation: In order to solve this issue, we intend to have clear documentation on each part of the project, clearly specifying exactly what capabilities the design possesses, so that there may be no confusion or estimation as to what the project may do.

Environmental Impacts
This project may impact the environment due to waste products, especially if the design is imprecise or cumbersome to follow. 
Mitigation: We plan to mitigate this issue by having precise documentation, thus allowing any who come after us to create this design without wasting or destroying any components.

Cost and Skill to implement
This project, despite its reduced cost compared to the current market, may still be prohibitively expensive for those creating it, or too challenging to implement.
Mitigation: While there is only so much that may be done for the cost of this project, we intend to reduce necessary costs as much as reasonably possible, thus making the design as accessible as can be helped. Additionally, we may mitigate the skill requirement by being precise and explanatory in our design documents so that the skill requirements may be as low as reasonably possible.

Ethics:
While this project is designed for the individual to have a secure time server, this does not come without ethical considerations. First of all, we as engineers must clearly communicate to both our client and any who come after us that this project will not be a perfectly secure system, as none is. Additionally, our project will employ a number of open source software, as well as off-the-shelf components, and thus will be vulnerable to any issues inherent to them. We must therefore keep expectations clear and precise regarding the exact elements of this project. Honesty, integrity, transparency, and safety must be core pillars of our design process. The engineers involved must hold themselves to the IEEE Code of Ethics. 15

Ethics in Design Decisions:
Additionally, teams must consider relevant standards organizations that will inform the design process. A comprehensive discussion should be included on how these considerations have influenced the design. This includes detailing constraints, specifications, and practices implemented as a result, and how these address the identified considerations. 

These prior ethical considerations have influenced our design decisions in the following ways:

In keeping with our ethical commitments to clarity and functionality, the project shall adhere to the following technical standards and specifications:
	NISTTN2187 
	ANSI C63.10 Compliance Testing of Unlicensed Wireless Devices 
	Title 47 CFR Part 15 regulation for Radio Frequency Devices 
	RFC 5905 Network Time Protocol Version 4 

Also, our decision to use off-the-shelf components and software, rather than designing the entire system from the ground up, was for the sake of ease of use and implementation, so as to not unduly burden those with smaller budgets or limited skillsets.

Finally, our design features many network security features so that malicious activity may be limited and restrained, insofar as reason and our available technology permits. These include a firewall and interface monitoring.  Additionally, our own software will not contain malicious elements. 



## Budget and Resources
### SoC
| Item         | Part Name            | Description                              | Quanity | Cost Per Item (Estimate) |
|--------------|----------------------|------------------------------------------|---------|--------------------------|
| Computer     | Rasberry pi 3 1GB    | A small computer to run the Linux server | 1       | $34.99                   |
| 32Gb SD Card | Sandisk 32GB sd card | Small resiliant storage                  | 1       | $20.99                   |
|              |                      |                                          |  Total:       |  $55.98            |
   

The computer will be the core unit of the system and  should be chosen for its combination of cost and performance, as well as it's ability to run the computations necessary. The 32Gb micro SD card will be chosen as a trustworthy storage medium and its ability to hold the required software and operating system. 

### GNSS + Radio
| Item                           | Part Name                                               | Description                                                    | Quanity | Cost Per Item (Estimate) |
| ------------------------------ | ------------------------------------------------------- | -------------------------------------------------------------- | ------- | ------------------------ |
| GNSS Satelite Antenna/Reciever | Waveshare NEO-M8T GNSS Timing HAT                       | The antenna and reciever for the GPS system                    | 1       | $133.99                  |
| PPS Wire                       | GPIO Jumper Wires for PPS                               | A wire for the PPS singal to connect to a I/O port on the SOC  | 1       | $5.99                    |
| AM Radio Reciever              | CANADUINO 60kHz WWVB Atomic Clock Receiver Module V3/V4 | The antenna and reciever for the 60kHz WWVB  radio time source | 1       | $17.95                   |
|                                |                                                         |                                                                | Total:        |  $157.93           |
 

The GNSS portion of this project is a major factor in what makes this system resiliant. The GNSS receiver needs to be capable of receiving signals from multiple constelations as well as being accruate enough to allow for our desired system accuracy of ()us. To ensure both of these criteria are met, a more robust GNSS module is needed. The seperate PPS wire will allow for the PPS signal to reach a I/O port on the SoC, increasing accuracy. In addition to GNSS signal our server will recieve radio signal from the WWVB NTP frequency. This adds an additional input source that is not connected to any other, improving the resiliancy of our system.  


### Enclosure
| Item              | Part Name                                                   | Description                                                            | Quanity | Cost Per Item (Estimate) |
| ----------------- | ----------------------------------------------------------- | ---------------------------------------------------------------------- | ------- | ------------------------ |
| Enclosure         | 3D printed enclosure                                        | Enclosure to store all components in a way that is neat and orderly    | 1       | ~$30                     |
| Mounting (Screws) | iExcell M2.5 Stainless Steel Hex Button Head Cap Screws Kit | Mounting for the SOC and other ports                                   | 1       | $8.28                    |
| Mounting (Nuts)   | Hilitchi 460-Piece Brass Knurled Threaded Insert Kit       | Mounting for the SOC and other ports| 1                                                                      | $9.60   |
| Cooling           | noctua 60mm 5V PWM fan (Optional)                           | Possible internal fans and cooling methods                             | 1       | $16.95                   |
| misc wiring       | Micro usb, coax, GPIO, ethernet, etc.                       | The remaining physical connectors we may need (Ethernet, Jumper, etc.) | 5       | ~$20                     |
|                   |                                                             |                                                                        | Total:        |  $84.83            |


The enclosure subsystem contains the parts holding the entire project together. We will determine the size of the enclosure based on the specific parts that will be chosen in the future. It will provide physical protection to the systems inside. Mounting will be used to hold the components inside of the enclosure. They are what ensures the peices remain where they are intended to be. Depending on the overall wattage and tempterature produced, some kind of cooling may be needed to maintain desired internal temperature. A small fan will likely be used to acheive this. Finally, our systems need to be connected with wiring to allow the needed communication between systems. The misc wiring  was added to cover for the connections we will need. 

### Power + Holdover
| Item                | Part Name                                   | Description                                                      | Quanity | Cost Per Item (Estimate) |
| ------------------- | ------------------------------------------- | ---------------------------------------------------------------- | ------- | ------------------------ |
| External Oscillator | OH2095TSE-010.0M (OCXO)                     | A device to keep highly accurate system time                     | 1       | $142.61                  |
| Battery Bank        | Yajisi 18W Mini UPS Battery Backup 16000mAh | Allows the devices to keep running in the case of a power outage | 1       | $51.37                   |
| SOC AC Adapter      | AC/DC WALL MNT ADAPTER 5.1V 15W             | Cables to power the SOC                                          | 1       | $8.00                    |
|                     |                                             |                                                                  |  Total:       | $201.98           |

Holdover is the systems ability to remain accurate for a duration of time without activly receiving input from the various sources. A strong holdover time allows our system to be a trusted source regardless of what external circumstances are affecting the inputs. A powerful external oscillator will allow this to happen. To allow for the desired accuracy of ()us we need a well tested and trusted oscillator. In addition to the loss of input signal, it is possible for the system to lose power for a duration of time. To remedy this we will encorporate a power bank/UPS to allow for the server to continue running without power for a set time, ensuring our time server remains accurate regardless of outages. The SoC AC adapter will allow for our selected SoC to receive direct power.

 ***Total Cost: $500.07***

All selected parts are subject to change as deemed necessary by the team. 

### Division of Labor

First, conduct a thorough analysis of the skills currently available within the team, and then compare these skills to the specific requirements of each subsystem. Based on this analysis, appoint a team member to take the specifications for each subsystem and generate a corresponding solution (i.e. detailed design). If there are more team members than subsystems, consider further subdividing the solutions into smaller tasks or components, thereby allowing each team member the opportunity to design a subsystem.

### Timeline
<img width="1052" height="310" alt="Screenshot 2026-10-23 211602" src="https://github.com/user-attachments/assets/2760db70-d0b2-4b54-bc39-530ba3207c02" />
<img width="1006" height="390" alt="Screenshot 2026-11-10 174617" src="https://github.com/user-attachments/assets/1be954ba-7f58-4540-97c8-b6a90039b6d5" />

## References
[1]GeeksforGeeks, “Difference Between MCU and SoC,” GeeksforGeeks, Sept. 04, 2020. https://www.geeksforgeeks.org/computer-organization-architecture/difference-between-mcu-and-soc/ (accessed Nov. 07, 2026). 

[2]“Chrony – Introduction,” chrony-project.org. https://chrony-project.org/ (accessed Nov. 07, 2026).

[3]“Ntpd - Network Time Protocol (NTP) Daemon,” NTP: Network Time Protocol. https://www.ntp.org/documentation/4.2.8-series/ntpd/ (accessed Nov. 07, 2026). 

[4]“Differences From NTP Classic,” Ntpsec.org, 2025. https://docs.ntpsec.org/latest/ntpsec.html (accessed Nov. 07, 2026).  

[5]OpenBSD Project, “OpenNTPD,” OpenBSD Project, July 01, 2026. https://www.openntpd.org/ (accessed Nov. 07, 2026).

[6] D. Franke, D. Sibold, K. Teichel, M. Dansarie, and R. Sundblad, “Network Time Security for the Network Time Protocol,” RFC 8915, Sept. 2020, doi: 10.17487/RFC8915.

[7] D. Mills, J. Burbank, and W. Kasch, “Network Time Protocol Version 4: Protocol and Algorithms Specification,” RFC 5905, June 2010, doi: 10.17487/RFC5905.

[8] “OpenSSH: Manual Pages,” OpenSSH.org, 2025. [Online]. Available: https://www.openssh.org/manual.html.  

[9] “How should chronyd be configured with GPSD?” Chrony Project, Chrony FAQ. [Online]. Available: https://chrony-project.org/faq.html#_how_should_chronyd_be_configured_with_gpsd.  

[10] “An unreachable source is selected,” Chrony Project, Chrony FAQ. [Online]. Available: https://chrony-project.org/faq.html#_an_unreachable_source_is_selected.  

[^1000]:Anand, Ankur, “Oscillators: Key Parameters and Specifications.” June 14, 2021. Accessed: Nov. 09, 2026. [Online]. Available: https://www.ti.com/content/dam/videos/external-videos/en-us/7/3816841626001/6219032061001.mp4/subassets/clocks-and-timing-oscillator-key-parameters-specifications-presentation-quiz.pdf
## Statement of Contributions
- Block Diagram - Jonathan Salvato
- Comparative Analysis - Oscillator/Enclosure - Jonathan Salvato
- Timeline - Jocelyn Lee
- Power System - Jocelyn Lee
- Introduction - Everett Patterson
- Holdover Oscillator - Jonathan Salvato
- Clock Synchronization - Everett Patterson
- Ethics - Everett Patterson
- Enclosure - Everett Patterson
- GNSS - Merric Anderson
- SoC - Merric Anderson
- High Level Solution- Merric Anderson
- State the Problem- Merric Anderson
- Client Communication - Jack Fennell
- Device Security - Jack Fennell
- Budget - Jack Fennell
- Resources - Jack Fennell
