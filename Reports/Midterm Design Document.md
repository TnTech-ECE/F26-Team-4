# Midterm Design Document
## Introduction
Precise, accurate, and secure timing is critical for many applications, from banking networks to national security, however, in certain cases, the individual, such as a hobbyist, may also require such timing systems for their own needs. Our project shall create and demonstrate a framework in which an individual may have their own precise, secure, and available timing system.   

Unsecured time servers are vulnerable to a multitude of issues, including spoofing, man-in-the-middle attacks, connection loss, as well as unavailability due to power loss and other factors.  Additionally, being reliant upon only one source for time exacerbates these concerns. A failure or attack on any of these elements could create a cascade of issues.  

This project aims to provide a solution to this issue, via a more secure and robust, GPS-based, NTP server that may be replicated by the individual. We aim to use GPS as the primary time source, with secondary sources acting to corroborate and confirm the GPS. These include other GNSS systems, such as Galileo or BeiDou, radio broadcasts, oscillators, and internet sources. We shall provide a multilayered defense, including NTP and authentication, as well as comparing and contrasting our various sources, and fail-safes for signal or power loss.   

Off-the-shelf solutions, while available, do not provide the same functionality relative to our expected cost, often featuring less robust servers or being prohibitively expensive. Additionally, due to our design being intended to be disseminated to the public, there will theoretically exist space within the project for the individual to make changes, and add or remove elements as they need. 

## Fully Formulated Problem
The Primary objective of this project is to build a low cost, accessible, and effective Network Timer Server that provides accurate timestamps by using time references like satellite constellations and online servers. The system shall also be secure from attempts of subversion such as DDOS and Man in the Middle attacks.

The system shall be able to be constructed easily and inexpensively for use by the general public. In doing so, individuals and organizations will have access to extraordinarily accurate and secure time to ensure proper operation of their systems.

### Background
Secure and accurate timing is paramount in most modern systems in a multitude of industries. 
Examples include power generation, where plants need accurate time to log errors and 
synchronize generators, the finance industry needs accurate timestamps to track transactions to 
prevent theft and fraud, and modern navigation systems, like GPS, needs precise timing so that it 
can track a position accurately, otherwise the target will drift off course[^1]. As the dependence on 
more accurate time increases, the need for more reliable time distribution has become 
increasingly important.

Network Timer Servers (NTS) solve this by obtaining accurate time from trusted sources such as:
satellite constellations and local atomic clocks. Then, it is distributed downstream 
to clients via the Network Time Protocol[^2]. In a typical system, the server receives a reference 
time, disciplines a local oscillator to maintain reference time, and distributes that time to clients.

While this system is effective and efficient at distributing time, it has several security 
vulnerabilities. The system is reliant on external time sources, which can be interrupted and 
subverted by an attacker. Attacks such as spoofing, where an attacker can disguise themselves as 
a time signal, and jamming, where signals are interrupted, can cause the server to lose its 
synchronization with the correct time. Since many systems rely on having access to this absolute 
time for logging, authentication, and decision-making, a compromised time source can have 
consequences that can cause immense damage.

Therefore, creating a secure and resilient time source is of extraordinary importance to the 
functioning of society. It must be secure from attempts from malicious entities to subvert the system, and from damage 
from outside sources to ensure functioning even in extreme and hostile situations.


### Specifications and Constraints
#### Specifications
The GPS regulated Secure Network Time Server will:
1. Keep and Broadcast the current time:
	-The server shall keep time within an accuracy of at most 100 us to UTC with an internal holdover of  at most 1 s/day
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

For example, Masterclock's GMR1000 is an enterprise-grade time server that can synchronize to GPS, NTP, and PTP sources (among others). It contains a high-stability oscillator which gives it a strong holdover of a 5us drift per day[^3] It's a great time server, but costs ~$1595.00[^4], which makes it inadequate for hobbyist use.

For another example of an industrial server, Microchip’s SyncServer S600. Similarly, this is a high security and high accuracy NTP/PTP time server. It has an optional atomic clock, enabling a holdover drift of <1 us per day. The standard drift without the atomic clock is around 400 us per day[^5]. This NTP server hovers around $5000-11000[^6][^7] making this an extreme example of high cost.

Of course, there are more accessible options on the market, but most of these come with tradeoffs of precision, security, or relative cost.

For a lower cost, Time Machines Corp. has a fleet of NTP time servers[^8]: The TM1000A ($349.99)[^8], TM2000B ($549.99)[^8], TM2500C ($799.99)[^8], and TM3000A ($999.99)[^8]. All of these are capable of GPS synchronization and support NTPv4. However, only the TM3000A is capable of security protocols such as NTS. Additionally, the TM1000A lacks a holdover time source and none have more than one backup time source[^9]. While these servers will work, they're either expensive or lacking in security features.

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

**Software Resources** - Our team will be primarily utilizing an open-source, well-respected NTP software called Chrony. It is capable of receiving and filtering the incoming time signals and using them to determine the exact time in a given moment, with up to nanosecond accuracy. It does all of this while filtering out suspicious or incorrect sources[^10]. We intend to implement a firewall as well to ensure the computer is protected against cybersecurity threats. 

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

### Personnel
Required Skills
Software & Programming: Linux, MATLAB, Python, KiCAD, Cybersecurity
Hardware & Systems: Power Systems, Digital Systems, Timing Systems, Knowledge of SoCs, Telecommunications, Networking
Design & Simulation Tools: LTspice, 3D Printing, PCB Design
Theory & Analysis: Signal Processing, Control Systems

### Team Skills

Merric Anderson
- Current Skills: Digital System Processing, Control Systems, Python, MATLAB, Digital Systems, Telecommunications
- Skills to Learn: kiCAD, Power Systems, Knowledge of SoCs, Linux 

Jack Fennell
- Current Skills: Python, Telecommunications, Control Systems, Knowledge of SoCs
- Skills to Learn: Cybersecurity, Power Systems, Networking

Jocelyn Lee
- Current Skills: Power Systems, Digital Systems, LTSpice, 3D Printing
- Skills to Learn: Linux, Cybersecurity, Telecommunications

Everett Patterson
- Current Skills: Digital Systems, /hardware Security, SoCs
- Skills to Learn: Linux, Cybersecurity, Signal Processing

Jonathan Salvato
- Current Skills: Control Systems, MATLAB, LTSpice, 3D Modeling/Printing, PCB Design
- Skills to Learn: Linux, Telecommunications, Timing Systems

Supervisor:
Instructor: Dr. Storm

### Proposal Timeline
<img width="809" height="394" alt="image" src="https://github.com/user-attachments/assets/68267c23-0450-48cd-8e97-922e97e770c6" />

## Specific Implications

Implementing the Resilient and Secure Network Time Server (NTS) that was proposed will provide many solutions to our customer's issues: secure, resilient, and affordable network time synchronization.

By using the Network Time Security (NTS) protocol, the system will give a cryptographic authentication of timing information shared back and forth from the server and client. Through this, the risk of malicious sources providing false information can be reduced, creating a more secure alternative to traditional NTP [^11],[^12]. Timestamps that have been authenticated and proven reliable not only improve the trustworthiness of systems logs to many other network operations as well.

Even when primary GPS timing sources become unavailable, this system will be designed to maintain a usable time reference. GPS and other PNT services (positioning, navigation, and timing) are commonly affected by power outages, interference, and other disruptions such as GPS jamming [^13]. By incorporating backups we can reduce the system's dependence on GPS allowing clients to continue receiving data during disruptions [^14].

The Network Time Security protocol will protect the system from being manipulated by people with malicious intents. With the ability to establish cryptographic keys and authenticate traffic, NTS allows clients to verify that time information has not been tampered with [^12]. This provides confidence to the customer as the time distributed by the system is protected.

One of the primary objectives of the project is to achieve high end security and resilience through the use of common, inexpensive hardware. Ideally, hobbyist, students, and small organizations with limited budgets will be able to use the proposed system as it avoids the use of expensive commercial equipment. This provides protected time outputs to those who might not have the necessary infrastructure to use current NTS technology. Our team's GitHub Repository will also be available distribution as source of information on how to build their own NTS.

The simplicity of the system will provide a platform for experimentation, education, and future development. With a modular design, individual components like timing sources, networking hardware, and backup oscillators can be interchanged for testing and optimization. When designed like this, a single customer can build a system that is not limited to just one application, but many as this system can serve as the foundation for future resilient time projects. 

Combining aspects like secure network communication, GPS disciplined timing, and resistance to individual failures, a system can have a massive reduction to the operational and cybersecurity risks that plague conventional services. Along with this, maintaining a low total hardware cost reduces the financial challenges of implementing such a system. The final design would provide both incredible security and affordability to clients.


## Broader Implications, Ethics, and Responsibility as Engineers
This project will expand on the security and accuracy of currently available timing systems, which is a crucial part of PTN (positioning, timing, navigation). Timing, due to its broad use and necessity in critical systems, has a great need for improvements to security.  This project will aim to provide these needed improvements for an affordable cost.  
Additionally, the design and dissemination of this project will contribute to greater time server independence, meaning that this project will likely provide timing more precise and resilient than standard, default internet based timing, while also no longer being so overly reliant on the internet. 
Finally, the project will be released publicly, thus reducing the barriers for any who may be interested in following this design. This improves accessibility and inclusivity of the project.

### Risk to Network Security
This project will involve network connections from various sources, including international sources. These, along with communications between the server and the client, leave the potential open for malicious activity. 
	Mitigation: We plan to make the signal communications secure via systems like NTS, and quarantine the signals to only what they are needed for, thus keeping any malicious activity reasonably contained.
 
### False sense of security
This project, as it is designed for improving the security of time servers, may give the user an overconfidence in this design, such that they may imagine hat they will have no risk of tampering or failure.  This is not the case, even the completed design will not be immune to all issues, and theoretically a sufficiently skilled malicious actor could still disrupt the design.
	Mitigation: In order to solve this issue, we intend to have clear documentation on each part of the project, clearly specifying exactly what capabilities the design possesses, so that there may be no confusion or estimation as to what the project may do.

### Environmental Impacts
This project may impact the environment due to waste products, especially if the design is imprecise or cumbersome to follow.
	Mitigation: We plan to mitigate this issue by having precise documentation, thus allowing any who come after us to create this design without wasting or destroying any components. 

### Cost and Skill to implement
This project, despite its reduced cost compared to the current market, may still be prohibitively expensive for those creating it, or too challenging to implement.  
	Mitigation:  While there is only so much that may be done for the cost of this project, we intend to reduce necessary costs as much as reasonably possible, thus making the design as accessible as can be helped.  Additionally, we may mitigate the skill requirement by being precise and explanatory in our design documents so that the skill requirements may be as low as reasonably possible.

## Ethics:
While this project is designed for the individual to have a secure time server, this does not come without ethical considerations.  First of all, we as engineers must clearly communicate to both our client and any who come after us that this project will not be a perfectly secure system, as none is.  Additionally, our project will employ a number of open source software, as well as off-the-shelf components, and thus will be vulnerable to any issues inherent to them.  We must therefore keep expectations clear and precise regarding the exact elements of this project. Honesty, integrity, transparency, and safety must be core pillars of our design process. 
The engineers involved must hold themselves to the IEEE Code of Ethics. [^15]

## Statement of Contributions

Specifications and Constraints - Jonathan Salvato  
Existing Solutions - Jonathan Salvato  
Background, Measures of Success, and Formulation of the Problem - Merric Anderson  
Resources and Budget - Jack Fennell  
Timeline and Specific Implications - Jocelyn Lee
Introduction and Broader Impacts/Ethics - Everett Patterson 

## References
[1]:B. Telford, “What Is NTP? A Beginner’s Guide to Network Time Protocol,” Galleon Systems, Mar. 11, 2019. https://www.galsys.co.uk/news/what-is-ntp-a-beginners-guide-to-network-time-protocol/ (accessed Oct. 12, 2026).

[2]:“NIST Authenticated NTP Service | NIST,” NIST, Feb. 10, 2010. https://www.nist.gov/pml/time-and-frequency-division/time-services/nist-authenticated-ntp-service (accessed Oct. 12, 2026).

[3]:  “GMR1000 Compact GPS Master Clock & NTP Server | Masterclock,” _Masterclock.com_, 2026. https://www.masterclock.com/masterclock-gmr-1000.html (accessed Oct. 09, 2026).

[4]: “Masterclock GMR1000 Master Clock,” _Broadcasters General Store_, 2026. https://bgs.cc/masterclock-gmr1000/ (accessed Oct. 09, 2026).

[5]: “SyncServer® S600 NTP/PTP Time Server,” _Microchip.com_, 2026. https://www.microchip.com/en-us/products/clock-and-timing/systems/enterprise-network-time-servers/syncserver-s600 (accessed Oct. 09, 2026).

[6]: “Microchip SyncServer S600 - network time server - 090-15200-601 - Network Management Devices - CDW.com,” _CDW.com_, 2026. https://www.cdw.com/product/microchip-syncserver-s600-network-time-server/3984841 (accessed Oct. 09, 2026).
[7]: “Microchip SyncServer S600 - network time server - with Rubidium Atomic Oscillator - 090-15200-606 - Network Management Devices - CDW.com,” _CDW.com_, 2026. https://www.cdw.com/product/microchip-syncserver-s600-network-time-server-with-rubidium-atomic-osci/4354775 (accessed Oct. 09, 2026).

[8]: “Shop GPS Time Servers + Accessories | TimeMachines,” _TimeMachines Inc._, Dec. 11, 2025. https://timemachinescorp.com/gps-time-servers-accessories/ (accessed Oct. 09, 2026).

[9]:  “Shop GNSS Network Time Server | TimeMachines,” _TimeMachines Inc._, Mar. 05, 2026. https://timemachinescorp.com/product/tm3000/ (accessed Oct. 09, 2026).

[10]: “Chrony – Introduction,” _chrony-project.org_. https://chrony-project.org/ (accessed Oct. 12, 2026).

[11]:D. L. Mills, J. Martin, J. Burbank, and W. Kasch, “Network Time Protocol Version 4: Protocol and Algorithms Specification,” RFC 5905, Internet Engineering Task Force, Jun. 2010.

[12]:D. Franke, D. Sibold, K. Teichel, M. Dansarie, and R. Sundblad, “Network Time Security for the Network Time Protocol,” RFC 8915, Internet Engineering Task Force, Sep. 2020

[13]:J. McCarthy, Y.-S. Li-Baboud, J. Brule, and K. Meldorf, “Foundational PNT Profile: Applying the Cybersecurity Framework for the Responsible Use of Positioning, Navigation, and Timing (PNT) Services,” NIST Interagency/Internal Report 8323 Rev. 1, National Institute of Standards and Technology, Jan. 2023.

[14]:J. Sherman, L. Arissian, R. Brown, M. J. Deutch, E. Donley, V. Gerginov, J. Levine, G. Nelson, A. Novick, B. Patla, T. Parker, B. Stuhl, J. Yao, W. Yates, M. A. Lombardi, V. Zhang, and D. Sutton, “A Resilient Architecture for the Realization and Distribution of Coordinated Universal Time to Critical Infrastructure Systems in the United States: Methodologies and Recommendations from the National Institute of Standards and Technology,” NIST Technical Note 2187, National Institute of Standards and Technology, Nov. 2021.

[15]:IEEE, “IEEE Code of Ethics | IEEE,” Ieee.org, 2020. [https://www.ieee.org/about/corporate/governance/p7-8](https://www.ieee.org/about/corporate/governance/p7-8) (accessed Oct. 12, 2026).


## Comparative Analysis of Potential Solutions
The biggest consideration within this project was deciding our solution for NTP communication; our two proposed solutions were to either use a SoC running Linux with an open-source solution such as Chrony or some microcontroller with bare-metal implementation. There are advantages and disadvantages for each approach:

|       | SoC Implementation                                                                                                                                                                                                                   | Microcontroller Implementation                                                                   |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| Pros: | - Ease of implementation<br>- Built-in NTS and other security protocols<br>- Simple Multiple Time Source integration<br>- Easy time source metric tracking, such as drift of individual sources<br>- Simpler Networking capabilities | - Lower Cost<br>- More Compact                                                                   |
| Cons: | -May increase the total cost of the project                                                                                                                                                                                          | - Harder overall implementation<br>- Requires more time to develop<br>-  Potentially less secure |
Given the options proposed, we decided that the marginally lower cost wasn't worth the difficulties involving implementation and potential security issues of a bare-metal solution. With the advantages that an SoC provides, it's the clear choice for this project.

Once we decided our system type, the next item of consideration was our time sources. Our client specifications required a GPS source, so that made it non-negotiable. The additional sources (Radio, Oscillator, and Internet Time Servers) we consider adding are to improve the robustness of the system.

## High-Level Solution
The Network Time Server provides a secure platform that will allow its users to retrieve accurate timestamps which will allow them to properly operate their systems. By integrating accurate time sources, precise and reliable hold-over systems, protection from malicious entities, and flexible, capable open source software,the design will achieve accurate timestamps even in hostile enviorments.

### Power Supply
The power supplies' job is to provide power to maintain a stable operating condition in normal and in extraordinary conditions, have a back supply incase the wall outlet fails. It should be able to switch fast enough as to maintain synchronicity after loss of power. 

### GNSS Module
The GNSS Module is the main source of timestamps of the system. It should be able provide accurate timestamps even in nonideal conditions such as cloudy skies, or bad atmospheric conditions. It will also be able to detect spoofing and jamming attempts

### Processor
This is the brains of the system. It coordinates the operation of the GNSS Module, external oscillator, and transmission of timestamps to clients. It will be able to change the settings of the system to the user's desired settings. It will also coordinate security and quality checks.

### Client Communication
The client communication is the bridge between the server and the clients. It ensures proper distribution of timestamps to clients, and ensures the clients have proper authentication.

### Device Security
This encompasses the overall security of the system. The system will have a firewall to prevent unwanted intrusions and ensure proper authentication to access the secure timestamps and to change the settings of the system.

### Holdover Oscillator
In normal conditions, the system will receive timing from the satellite constellations, however, sometimes this synchronization will be lost. The oscillator will be continuously tuned with the incoming signal to ensure timestamps will continue to be accurate.

### Enclosure
The enclosure is the body that will contain the entire system. It will provide proper cooling, and protection from external attacks. It will also provide ease of access for authenticated users.

### Hardware Block Diagram
<img width="410" height="568" alt="image" src="https://github.com/user-attachments/assets/b8e5d77b-4971-439e-a1a4-06fd871f4c8e" />

Note that the radio clock module in this diagram is an optional component.  

### Operational Flow Chart

<img width="462" height="535" alt="image" src="https://github.com/user-attachments/assets/60fff472-f0a3-4bc6-9925-40432275a7d1" />

  The NTP server is designed to operate autonomously after the initial admin configuration. During normal operation the server receives data from upstream time sources and uses them to discipline its own system clock. Upon request from a client the server will relay its system time to allow the client to sync its own clock with the NTP server. Both the Clock Synchronization and Client Communication subsystems loop repeatedly, both of which are outlined above. Users will primarily be interacting with the server during the initial install and subsequent maintenance and troubleshooting. The operational flowchart showcases the normal operating behavior of the server.


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
- Timestamp sent to processor using I2C/UART
- Current Status sent to processor using I2C/UART

Interfaces:
- Settings Control from processor 
- Data echange with processor
- Power from power supply

High-Level Requirements:
- Shall Retrieve accurate timestamps
- Shall be secure from attempts to spoof or attack it

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
Client communication is the system that uses the hyper-accurate system time determined by the Clock Synchronization subsystem and relays that to the clients in need of NTP data. The client will request NTS credentials to be able to verify its identity in the future. This is called the Transport Layer Security (TLS) handshake. The client and server register each other as trusted communicators and create credentials called cookies that are used in future communications [1]. When the client needs NTP data it sends a request to the server using its cookies, the server receives processes and sends back the accurate time data. Using the time given by the server the client will adjust its internal clock to match the server, taking into account any delay accrued from the transmission. [2]  

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
Device security encapsulates the measures needed to protect the server from cybersecurity threats beyond incorrect timing data. It will allow the device to be safe from bad actors on its network. To ensure a secure server, we will implement a firewall to lock down the ports and allow only the ones needed for basic server function. This includes port 123 for NTP, port 4460 for NTS and port 22 for SSH (A secure network protocol used to remotely access systems)[1][2]. To allow for secure administrator control SSH key authentication will be implemented to prevent potentially faulty passwords that can be compromised[4]. We will restrict Chrony to a non-root user to ensure that if Chrony is compromised, the attacker cannot gain access to the entire server[3]. Monitoring will also be used to ensure important configuration files are secure and unchanged and noting any NTP synchronization anomalies[3].  

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

The system will receive various signals and, using the data received, output one unified time signal.  GPS will be ordinarily taken as standard, and thus used as the primary time source.  However, if GPS signal is lost, or falls outside of acceptable tolerance, then other sources will be brought to the foreground. [5]  The first to be used after GPS fails will be the alternate GNSS constellations, such as Galileo or BeiDou.  Should those also fail, the internet sources, such as NIST, will be taken.  Finally, the WWVB Radio signal will be taken.  [4]   All of these will be compared against the oscillator's holdover system, which, in the event of total signal loss, will act as the last clock source remaining.  
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

#### Risk to Network Security
This project will involve network connections from various sources, including international sources. These, along with communications between the server and the client, leave the potential open for malicious activity. 
Mitigation: We plan to make the signal communications secure via systems like NTS, and quarantine the signals to only what they are needed for, thus keeping any malicious activity reasonably contained.

#### False sense of security
This project, as it is designed for improving the security of time servers, may give the user an overconfidence in this design, such that they may imagine hat they will have no risk of tampering or failure. This is not the case, even the completed design will not be immune to all issues, and theoretically a sufficiently skilled malicious actor could still disrupt the design. 
Mitigation: In order to solve this issue, we intend to have clear documentation on each part of the project, clearly specifying exactly what capabilities the design possesses, so that there may be no confusion or estimation as to what the project may do.

#### Environmental Impacts
This project may impact the environment due to waste products, especially if the design is imprecise or cumbersome to follow. 
Mitigation: We plan to mitigate this issue by having precise documentation, thus allowing any who come after us to create this design without wasting or destroying any components.

#### Cost and Skill to implement
This project, despite its reduced cost compared to the current market, may still be prohibitively expensive for those creating it, or too challenging to implement.
Mitigation: While there is only so much that may be done for the cost of this project, we intend to reduce necessary costs as much as reasonably possible, thus making the design as accessible as can be helped. Additionally, we may mitigate the skill requirement by being precise and explanatory in our design documents so that the skill requirements may be as low as reasonably possible.

#### Ethics:
While this project is designed for the individual to have a secure time server, this does not come without ethical considerations. First of all, we as engineers must clearly communicate to both our client and any who come after us that this project will not be a perfectly secure system, as none is. Additionally, our project will employ a number of open source software, as well as off-the-shelf components, and thus will be vulnerable to any issues inherent to them. We must therefore keep expectations clear and precise regarding the exact elements of this project. Honesty, integrity, transparency, and safety must be core pillars of our design process. The engineers involved must hold themselves to the IEEE Code of Ethics. 15

#### Ethics in Design Decisions:
Additionally, teams must consider relevant standards organizations that will inform the design process. A comprehensive discussion should be included on how these considerations have influenced the design. This includes detailing constraints, specifications, and practices implemented as a result, and how these address the identified considerations. 

These prior ethical considerations have influenced our design decisions in the following ways:

In keeping with our ethical commitments to clarity and functionality, the project shall adhere to the following technical standards and specifications:
	NISTTN2187 
	ANSI C63.10 Compliance Testing of Unlicensed Wireless Devices 
	Title 47 CFR Part 15 regulation for Radio Frequency Devices 
	RFC 5905 Network Time Protocol Version 4 

Also, our decision to use off-the-shelf components and software, rather than designing the entire system from the ground up, was for the sake of ease of use and implementation, so as to not unduly burden those with smaller budgets or limited skillsets.

Finally, our design features many network security features so that malicious activity may be limited and restrained, insofar as reason and our available technology permits. These include a firewall and interface monitoring.  Additionally, our own software will not contain malicious elements. 



## Resources
## Budget
### SoC
| Item         | Description                              | Quanity | Cost (Estimate) |
|--------------|------------------------------------------|---------|-----------------|
| Computer     | A small computer to run the Linux server | 1       | $100            |
| 32Gb SD Card | Small resiliant storage                  | 1       | $30             |
|              |                                          | Total:  | $130            |    

The computer will be the core unit of the system and  should be chosen for its combination of cost and performance, as well as it's ability to run the computations necessary. The 32Gb micro SD card will be chosen as a trustworthy storage medium and its ability to hold the required software and operating system. 

### GNSS + Radio
| Item         | Description                              | Quanity | Cost (Estimate) |
|--------------|------------------------------------------|---------|-----------------|
| GNSS Satelite Antenna/Reciever | The antenna and reciever for the GPS system                    | 1      | $300  |
| PPS Wire                       | A wire for the PPS singal to connect to a I/O port on the SOC  | 1      | $5    |
| AM Radio Reciever (Optional)             | The antenna and reciever for the 60kHz WWVB  radio time source | 1      | $15   |
|                                |                                                                | Total: | $320  |  

The GNSS portion of this project is a major factor in what makes this system resiliant. The GNSS receiver needs to be capable of receiving signals from multiple constelations as well as being accruate enough to allow for our desired system accuracy of ()us. To ensure both of these criteria are met, a more robust GNSS module is needed. The seperate PPS wire will allow for the PPS signal to reach a I/O port on the SoC, increasing accuracy. In addition to GNSS signal our server will recieve radio signal from the WWVB NTP frequency. This adds an additional input source that is not connected to any other, improving the resiliancy of our system.  


### Enclosure
| Item         | Description                              | Quanity | Cost (Estimate) |
|--------------|------------------------------------------|---------|-----------------|
| Enclosure   | Enclosure to store all components in a way that is neat and orderly    | 1      | $100  |
| Mounting    | Mounting for the SOC and other ports                                   | 1      | $30   |
| Cooling     | Possible internal fans and cooling methods                             | 1      | $30   |
| misc wiring | The remaining physical connectors we may need (Ethernet, Jumper, etc.) | 5      | $30   |
|             |                                                                        | Total: | $190  |  

The enclosure subsystem contains the parts holding the entire project together. We will determine the size of the enclosure based on the specific parts that will be chosen in the future. It will provide physical protection to the systems inside. Mounting will be used to hold the components inside of the enclosure. They are what ensures the peices remain where they are intended to be. Depending on the overall wattage and tempterature produced, some kind of cooling may be needed to maintain desired internal temperature. A small fan will likely be used to acheive this. Finally, our systems need to be connected with wiring to allow the needed communication between systems. The misc wiring  was added to cover for the connections we will need. 

### Power + Holdover
| Item         | Description                              | Quanity | Cost (Estimate) |
|--------------|------------------------------------------|---------|-----------------|
| External Oscillator | A device to keep highly accurate system time                     | 1      | $150 |
| Battery Bank        | Allows the devices to keep running in the case of a power outage | 1      | $100 |
| SOC AC Adapter      | Cables to power the SOC                                          | 1      | $20  |
|                     |                                                                  | Total: | $270 |  

Holdover is the systems ability to remain accurate for a duration of time without activly receiving input from the various sources. A strong holdover time allows our system to be a trusted source regardless of what external circumstances are affecting the inputs. A powerful external oscillator will allow this to happen. To allow for the desired accuracy of ()us we need a well tested and trusted oscillator. In addition to the loss of input signal, it is possible for the system to lose power for a duration of time. To remedy this we will encorporate a power bank/UPS to allow for the server to continue running without power for a set time, ensuring our time server remains accurate regardless of outages. The SoC AC adapter will allow for our selected SoC to receive direct power.


### Division of Labor

Merric Anderson:
This team member has performed extensive research into GNSS systems and has knowledge of Digital System Processing. The team member also is skilled in embedded programming using C, and is proficient in python. Based on this skill set, the team member was assigned the GNSS subsystem and Processor subsystem.

Jonathan Salvato:
Has a strong background in controls, signal processing, and hardware design. This team member also has the most experience with 3D modeling and 3D printing. As such this team member was assigned the Holdover Oscillator and Enclosure subsystems.  

Jack Fennell: Has a background in controls and embedded systems and is the team member with the most experience in software and programming. Because of this, he will take the lead on the software-heavy sections, such as Client Communication and Device Security. Both subsystems require the implementation of cybersecurity concepts and software configuration. 

Jocelyn Lee:

Everett Patterson:






### Conceptual Design Timeline
<img width="1052" height="310" alt="Screenshot 2026-10-23 211602" src="https://github.com/user-attachments/assets/2760db70-d0b2-4b54-bc39-530ba3207c02" />

## References
[1] D. Franke, D. Sibold, K. Teichel, M. Dansarie, and R. Sundblad, “Network Time Security for the Network Time Protocol,” RFC 8915, Sept. 2020, doi: 10.17487/RFC8915.

[2] D. Mills, J. Burbank, and W. Kasch, “Network Time Protocol Version 4: Protocol and Algorithms Specification,” RFC 5905, June 2010, doi: 10.17487/RFC5905.

[3] “OpenSSH: Manual Pages,” OpenSSH.org, 2025. [Online]. Available: https://www.openssh.org/manual.html.

[4] “An unreachable source is selected,” Chrony Project, Chrony FAQ. [Online]. Available: https://chrony-project.org/faq.html#_an_unreachable_source_is_selected.

[5] “How should chronyd be configured with GPSD?” Chrony Project, Chrony FAQ. [Online]. Available: https://chrony-project.org/faq.html#_how_should_chronyd_be_configured_with_gpsd.


## Statement of Contributions
- Block Diagram - Jonathan Salvato
- Comparative Analysis - Jonathan Salvato
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
