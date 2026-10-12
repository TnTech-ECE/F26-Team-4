# Project Proposal

## Introduction

The introduction must be the opening section of the proposal. It acts as the "elevator pitch" of the project, briefly introducing the objective, its importance, and the proposed solution. Because readers may only read this section, it should effectively capture their attention and encourage them to read further.

Toward the end of the introduction, include a subsection that outlines what the proposal will cover. This helps set reader expectations for the ensuing sections.


## Formulating the Problem

Formulating the problem or objective involves clearly defining it through background information, specifications, and constraints. Think of it as "fencing in" the objective to make it unambiguously clear what is and is not being addressed and why.

Questions to consider:
- Who does the problem affect (i.e. who is your customer)?
- Why do we need this solution?
- What challenges necessitate a dedicated, multi-person engineering team?
- Why aren’t off-the-shelf solutions sufficient?

### Background
Secure and accurate timing is paramount in most modern systems in a multitude of industries. 
Examples include power generation, where plants need accurate time to log errors and 
synchronize generators, the finance industry needs accurate timestamps to track transactions to 
prevent theft and fraud, and modern navigation systems, like GPS, needs perfect(better phrasing) timing so that it 
can track a position accurately, otherwise the target will drift off course[1]. As the dependence on 
more accurate time increases, the need for more reliable time distribution has become 
increasingly important.

Network Timer Servers (NTS) solve this by obtaining accurate time from trusted sources such as:
satellite constellations and local atomic clocks. Then, it is distributed downstream 
to clients via the Network Time Protocol[2]. In a typical system, the server receives a reference 
time, disciplines a local oscillator to maintain reference time, and distributes that time to clients.

While this system is effective and efficient at distributing time, it has several security 
vulnerabilities. The system is reliant on external time sources, which can be interrupted and 
subverted by an attacker. Attacks such as spoofing, where an attacker can disguise themselves as 
a time signal, and jamming, where signals are interrupted, can cause the server to lose its 
synchronization with the correct time. Since many systems rely on having access to this absolute 
time for logging, authentication, and decision-making, a compromised time source can have 
consequences that can cause immense damage[In-person testimony?].

Therefore, creating a secure and resilient time source is of extraordinary importance to the 
functioning of society. It must be secure from attempts from malicious entities to subvert the system, and from damage 
from outside sources to ensure functioning even in extreme and hostile situations.


### Specifications and Constraints
#### Specifications
The GPS regulated Secure Network Time Server will:
1. Keep and Broadcast the current time:
	-The server shall keep time within an accuracy of at most 100 us to UTC with an internal holdover of  at most  s/day
	-The server shall obtain the time from multiple sources including:
		-GNSS sources such as GPS
		-An Internal Oscillator
		-Network sources such as NIST Internet time servers
		-*If time allows, radio time such as WWVB*
	-The server shall communicate the current time data through the NTPv4 standard
2. Maintain Security:
	-The server shall maintain a secure connection to clients connected through NTS
	-The server shall be resilient to external attacks, such as:
		-GPS Spoofing or Jamming
		-DOS/DDOS attacks
		-The loss of any one time source
		-Power Loss
3. Be open source and accessible to a hobbyist:
	-The server's design and details shall be published to the public
	-The server shall have components and build instructions that are clear and easy to follow
	-The server's cost needs to be minimized 
	-The server's Licenses for software must be compatible and open source
#### Constraints
The GPS Regulated Secure Network Time Server will adhere to:
1. Server Standards and regulations
	-The server shall be compliant with the RFC 5905 Network Time Protocol Version 4
	
	-The server shall follow the guidelines set by the NISTTN2187 for resilient architecture
	
2. Wireless Receiver and device Standards and regulations
	-The server shall be compliant with the ANSI C63.10 Compliance Testing of Unlicensed Wireless Devices
	
	-The server shall be compliant with the Title 47 CFR Part 15 regulation for Radio Frequency Devices

## Survey of Existing Solutions
Many GPS governed NTP servers already exist on the market today. Many of these solutions, however, are for an industrial scale which makes them bulky, incredibly high in cost, and generally inaccessible.

For example, Masterclock's GMR1000 is an enterprise-grade time server that can synchronize to GPS, NTP, and PTP sources (among others). It contains a high-stability oscillator which gives it a strong holdover of a 5us drift per day[^1] It's a great time server, but costs ~$1595.00[^2], which makes it inadequate for hobbyist use.

For another example of an industrial server, Microchip’s SyncServer S600. Similarly, this is a high security and high accuracy NTP/PTP time server. It has an optional atomic clock, enabling a holdover drift of <1 us per day. The standard drift without the atomic clock is around 400 us per day[^3]. This NTP server hovers around $5000-11000[^4][^5] making this an extreme example of high cost.

Of course, there are more accessible options on the market, but most of these come with tradeoffs of precision, security, or relative cost.

For a lower cost, Time Machines Corp. has a fleet of NTP time servers[^6]: The TM1000A ($349.99)[^6], TM2000B ($549.99)[^6], TM2500C ($799.99)[^6], and TM3000A ($999.99)[^6]. All of these are capable of GPS synchronization and support NTPv4. However, only the TM3000A is capable of security protocols such as NTS. Additionally, the TM1000A lacks a holdover time source and none have more than one backup time source[^7]. While these servers will work, they're either expensive or lacking in security features.

As can be seen, there is no shortage in available time server solutions, however many of these solutions are expensive and/or lacking in available security or timekeeping measures. With this project we hope to offer a solution with a low cost, secure, and accurate time server.


## Measures of Success

The success of this project will be based on the time source's ability to provide reliable time in hostile and normal conditions. The following Key Performance Indicators (KPIs) will be used to determine whether the system meets the technical, timing, and security requirements.

***1. Timing and Synchronization Accuracy***

Objective: Verify that the system can maintain accurate time reference using external sources while delivering it to clients.

Methodology:

- Reference accuracy: Compare the system's output to a trusted time source to confirm correctness.
- Synchronization accuracy: Compare the system's oscillator to the time sources over a period.
- Timing Stability: Observe the error of the system to identify drift or other variations.
- Longterm stability: Operate the system over a period of time to determine whether the system can maintain synchronicity.
- Time Verification: Determine whether clients downstream are receiving the correct time, and is stable.

***2. GPS Reliability***

Objective: Verify whether the system can reliably acquire signals and accurately acquire timestamps.

Methodology:

- Initial Acquisition: Measure the time it takes for the module to acquire a signal from a cold start.
- Signal Strength: Measure the performance of the system under varying signal strengths and antenna conditions.
- Satellite Availability: Test the system over a variation in satellite number, and evaluate the minimum conditions for accurate acquisition.
- Tracking: Determine whether the system can maintain a lock with synchronization.
- Restart Testing: Test whether the system can cycle power and acquire a signal and gain synchronization.
- Antenna Testing: Determine whether the antenna and cabling can provide a quality signal over a period of time.

***3. Holdover Performance***

Objective: Verify the signal can provide accurate timestamps without a time source

Methodology:

- Time Source Interruption: Intentionally interrupt the signal and measure the timing error as the system transitions to using the internal oscillator.
- Duration: Determine how long the system can maintain accurate timestamps without a time reference.
- Oscillator Stability: Determine oscillator drift and its effect on time accuracy.
- Recovery Testing: Provide a valid time reference and measure whether the synchronization is correct quickly so not to produce time discontinuation.
- Repeated Time Source Interruption: Repeatedly disconnect and reconnect a signal and verify consistency.

***4. Network Time Distribution***

Objective: Verify that a externally disciplined time source can accurately distribute timestamps to clients.

Methodology:

- NTP Synchronization: Connect multiple clients and verify that each client receives the correct timestamps.
- Network Testing: Test synchronization under varying network loads and communication delays.
- Client Scalability: Increase the number of clients and determine whether increased traffic affects performance.
- Timestamp Accuracy: Compare timestamps received by timestamps against a valid reference to determine timing error.
- Continuous Operation: Observe timestamps transmission over a period of time to determine synchronization remains accurate.

***5. Security and Robustness***

Objective: Verify that the time source is robust against external attacks such as spoofing and DOS attacks, as well as attacks like jamming.

Methodology:

- Configuration Security: Verify only authorized users can modify timing, network and system parameters.
- Network Security: Evaluate the system's resistance to unauthorized NTP requests, configuration access, and other network based attacks.
- Time Integrity: Verify unexpected changes in the reference time or time source is logged.
- Authentication: Ensure that administrative action and protected services require proper authentication.
- Event Logging: Verify that significant events including time source loss, synchronization changes, configuration changes, and system failures are logged for future analysis.
- Failure Response: When the integrity of the external time sources cannot be verified, ensure the system enters a safe operating mode.

***6. Power Efficiency and Reliability***

Objective: Verify that the time source can operate while maintaining safe electrical and thermal performance.

Methodology:

- Power Consumption: Measure Power consumption of the system during startup, normal conditions, and holdover conditions.
- Continuous Operation: Test the system for a predefined period of time to identify unexpected resets, power failures , and other reliability issues.
- Thermal Testing: Monitor temperatures of the oscillator, SoC, GPS Modules, miscellaneous modules, and power circuitry during extended operation.
- Power Interruption Testing: Perform controlled power interruptions, and verify the system properly transitions to backup power sources , and maintains synchronization.
- Power Supply Verification: Confirm that the power supply provides stable voltage and current under expected operating conditions.


## Resources

**Material and Hardware Resources** - Our project will be primarily built around a single computer that will act as the controller behind the operations of the NTP server. It will be an SoC capable of running Linux, as that is the OS needed to run our desired software. We will have an external oscillator to ensure high accuracy in the event of a loss of signal from external time sources. A power bank and the necessary wiring will be used to allow the system to continue running in the case of a power outage. All of the above-mentioned components will exist inside a custom enclosure to ensure proper function and security. A GNSS antenna and receiver, as well as an AM radio antenna and receiver, will collect and process incoming satellite signals to allow for non-internet-based time sources. 

**Software Resources** - Our team will be primarily utilizing an open-source, well-respected NTP software called Chrony. It is capable of receiving and filtering the incoming time signals and using them to determine the exact time in a given moment, with up to nanosecond accuracy. It does all of this while filtering out suspicious or incorrect sources. We intend to implement a firewall as well to ensure the computer is protected against cybersecurity threats. 

**Facilities and Support** - We will use the Capstone Lab for assembly, prototyping, and testing our design, as well as the iMakerSpace for its 3D printers and measurement tools.


### Budget

The below table breaks down the total recources and estimated cost of each item needed. The selected components and prices were chosen based on cost, reliability, and performance. The final estimate ($895) is well below the initial overall budget of $1000. 

|  Subsystem       | Part                           | Description                                                              | Justification                                                                                                       | Quantity | Cost Per Item | Total Cost (Estimate) |
|------------------|--------------------------------|--------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|----------|---------------|-----------------------|
| SOC              | Computer                       | A small computer to run the Linux   server                               | To use Chrony a linux machine is   a necessity so MCUs are not an option                                            | 1        | $100          | $100                  |
| SOC              | 32Gb SD Card                   | Small resiliant storage                                                  | 32Gb is the max many small SOCs   are capable of                                                                    | 1        | $30           | $30                   |
| SOC              | SOC AC Adapter                 | Cables to power the SOC                                                  | Needed to power the SOC                                                                                             | 1        | $20           | $20                   |
| GNSS             | GNSS Satelite Antenna/Reciever | The antenna and reciever for the   GPS system                            | One of the core characteristics   for this project                                                                  | 1        | $300          | $300                  |
| GNSS             | PPS Wire                       | A wire for the PPS singal to   connect to a I/O port on the SOC          | Necessary for the computer to   receive both NMEA and PPS signals simultaneously                                    | 1        | $5            | $5                    |
| Power + Holdover | External Oscillator            | A device to keep highly accurate   system time                           | Needed to ensure the level of   accuracy required for the NTP server to be a valid option in the NTP sphere         | 1        | $150          | $150                  |
| Power + Holdover | Battery Bank                   | Allows the devices to keep   running in the case of a power outage       | The task is to make a resiliant   device, and power in the case of an outage needs to be considered                 | 1        | $100          | $100                  |
| Enclosure        | Enclosure                      | Enclosure to store all components   in a way that is neat and orderly    | The components will need to be   contained in a secure and well ventilated enclosure to ensure proper   functioning | 1        | $100          | $100                  |
| Enclosure        | Mounting                       | Mounting for the SOC and other   ports                                   | Required for the components to   fit in the enclosure                                                               | 1        | $30           | $30                   |
| Enclosure        | Cooling                        | Possible internal fans and   cooling methods                             | To ensure the system doesn't   overheat and is able to function properly                                            | 1        | $30           | $30                   |
| Enclosure        | misc wiring                    | The remaining physical connectors   we may need (Ethernet, Jumper, etc.) | Needed to send and receive the   signals to and from the server                                                     | 5        | $6            | $30                   |
| GNSS + Radio     | AM Radio Reciever              | The antenna and reciever for the   60kHz WWVB  radio time source         | Needed as an additional time   source to ensure accuracy between sources                                            | 1        | $15           | $15                   |
| | | | | |**Total :** |**$895**|

### Personel


Identify the skills present in the team and compare them to those required to complete the project. Address any skill gaps with a plan to acquire the necessary knowledge.

Besides the team, also state who you choose to be you supervisor and why.

State who your instrucotr is and what role you expect them to play in the project.

### Timeline

Provide a detailed timeline, including all major deadlines and tasks. This should be illustrated with a professional Gantt chart.


## Specific Implications

Implementing the Resilient and Secure Network Time Server (NTS) that was proposed will provide many solutions to our customer's issues: secure, resilient, and affordable network time synchronization.

By using the Network Time Security (NTS) protocol, the system will give a cryptographic authentication of timing information shared back and forth from the server and client. Through this, the risk of malicious sources providing false information can be reduced, creating a more secure alternative to traditional NTP \[1],\[2]. Timestamps that have been authenticated and proven reliable not only improve the trustworthiness of systems logs to many other network operations as well.

Even when primary GPS timing sources become unavailable, this system will be designed to maintain a usable time reference. GPS and other PNT services (positioning, navigation, and timing) are commonly affected by power outages, interference, and other disruptions such as GPS jamming \[3]. By incorporating backups we can reduce the system's dependence on GPS allowing clients to continue receiving data during disruptions \[4].

The Network Time Security protocol will protect the system from being manipulated by people with malicious intents. With the ability to establish cryptographic keys and authenticate traffic, NTS allows clients to verify that time information has not been tampered with \[2]. This provides confidence to the customer as the time distributed by the system is protected.

One of the primary objectives of the project is to achieve high end security and resilience through the use of common, inexpensive hardware. Ideally, hobbyist, students, and small organizations with limited budgets will be able to use the proposed system as it avoids the use of expensive commercial equipment. This provides protected time outputs to those who might not have the necessary infrastructure to use current NTS technology. Our team's GitHub Repository will also be available distribution as source of information on how to build their own NTS.

The simplicity of the system will provide a platform for experimentation, education, and future development. With a modular design, individual components like timing sources, networking hardware, and backup oscillators can be interchanged for testing and optimization. When designed like this, a single customer can build a system that is not limited to just one application, but many as this system can serve as the foundation for future resilient time projects. 

Combining aspects like secure network communication, GPS disciplined timing, and resistance to individual failures, a system can have a massive reduction to the operational and cybersecurity risks that plague conventional services. Along with this, maintaining a low total hardware cost reduces the financial challenges of implementing such a system. The final design would provide both incredible security and affordability to clients.


## Broader Implications, Ethics, and Responsibility as Engineers

Consider the project’s broader impacts in global, economic, environmental, and societal contexts. Identify potential negative impacts and propose mitigation strategies. Detail the ethical considerations and responsibilities each team member bears as an engineer.


## References
[^1]:  “GMR1000 Compact GPS Master Clock & NTP Server | Masterclock,” _Masterclock.com_, 2026. https://www.masterclock.com/masterclock-gmr-1000.html (accessed Oct. 09, 2026).
[^2]: “Masterclock GMR1000 Master Clock,” _Broadcasters General Store_, 2026. https://bgs.cc/masterclock-gmr1000/ (accessed Oct. 09, 2026).
[^3]: “SyncServer® S600 NTP/PTP Time Server,” _Microchip.com_, 2026. https://www.microchip.com/en-us/products/clock-and-timing/systems/enterprise-network-time-servers/syncserver-s600 (accessed Oct. 09, 2026).
[^4]: “Microchip SyncServer S600 - network time server - 090-15200-601 - Network Management Devices - CDW.com,” _CDW.com_, 2026. https://www.cdw.com/product/microchip-syncserver-s600-network-time-server/3984841 (accessed Oct. 09, 2026).
[^5]: “Microchip SyncServer S600 - network time server - with Rubidium Atomic Oscillator - 090-15200-606 - Network Management Devices - CDW.com,” _CDW.com_, 2026. https://www.cdw.com/product/microchip-syncserver-s600-network-time-server-with-rubidium-atomic-osci/4354775 (accessed Oct. 09, 2026).
[^6]: “Shop GPS Time Servers + Accessories | TimeMachines,” _TimeMachines Inc._, Dec. 11, 2025. https://timemachinescorp.com/gps-time-servers-accessories/ (accessed Oct. 09, 2026).
[^7]:  “Shop GNSS Network Time Server | TimeMachines,” _TimeMachines Inc._, Mar. 05, 2026. https://timemachinescorp.com/product/tm3000/ (accessed Oct. 09, 2026).


## Statement of Contributions

Specifications and Constraints - Jonathan Salvato
Existing Solutions - Jonathan Salvato  
