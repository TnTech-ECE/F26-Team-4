# Conceptual Design

This document outlines the objectives of a conceptual design. After reading your conceptual design, the reader should understand:

- The fully formulated problem.
- The fully decomposed conceptual solution.
- Specifications for each of the atomic pieces of the solution.
- Any additional constraints and their origins.
- How the team will accomplish their goals given the available resources.

With these guidelines, each team is expected to create a suitable document to achieve the intended objectives and effectively inform their stakeholders.


## General Requirements for the Document
- Submissions must be composed in Markdown format. Submitting PDFs or Word documents is not permitted.
- All information that is not considered common knowledge among the audience must be properly cited.
- The document should be written in the third person.
- An introduction section should be included.
- The latest fully formulated problem must be clearly articulated using explicit "shall" statements.
- A comparative analysis of potential solutions must be performed
- The document must present a comprehensive, well-specified high-level solution.
- The solution must contain a hardware block diagram.
- The solution must contain an operational flowchart.
- For every atomic subsystem, a detailed functional description, inputs, outputs, and specifications must be provided.
- The document should include an acknowledgment of ethical, professional, and standards considerations, explaining the specific constraints imposed.
- The solution must include a refined estimate of the resources needed, including: costs, allocation of responsibilities for each subsystem, and a Gantt chart.


## Introduction

The introduction is intended to reintroduce the fully formulated problem. 


## Restating the Fully Formulated Problem

## Fully Formulated Problem

The Primary objective of this project is to build a low cost, accessible, and effective Network Timer Server that provides accurate timestamps by using time references like satellite constellations and online servers. The system shall also be secure from attempts to subvert it from attacks like jamming, DOS, Man in the Middle attacks, and general attacks.

The system shall be able to be constructed easily and inexpensively for use by the general public. In doing so, individuals and organizations will have access to extraordinarily accurate and secure time to ensure proper operation of their systems.



## Comparative Analysis of Potential Solutions

In this section, various potential solutions are hypothesized, design considerations are discussed, and factors influencing the selection of a solution are outlined. The chosen solution is then identified with justifications for its selection.


## High-Level Solution

The Network Time Server provides a secure platform that will allow its users to retrieve accurate timestamps which will allow them to properly operate their systems. By integrating accurate time sources, precise and reliable hold-over systems, protection from malicious entities, and flexible, capable open source software,the design will achieve accurate timestamps even in hostile enviorments.

### Hardware Block Diagram

Block diagrams are an excellent way to provide an overarching understanding of a system and the relationships among its individual components. Generally, block diagrams draw from visual modeling languages like the Universal Modeling Language (UML). Each block represents a subsystem, and each connection indicates a relationship between the connected blocks. Typically, the relationship in a system diagram denotes an input-output interaction.

In the block diagram, each subsystem should be depicted by a single block. For each block, there should be a brief explanation of its functional expectations and associated constraints. Similarly, each connection should have a concise description of the relationship it represents, including the nature of the connection (such as power, analog signal, serial communication, or wireless communication) and any relevant constraints.

The end result should present a comprehensive view of a well-defined system, delegating all atomic responsibilities necessary to accomplish the project scope to their respective subsystems.


### Operational Flow Chart

<img src="figures/Operational_Flowchart.svg" alt = "Operational Flowchart" width="600">

  The NTP server is designed to operate autonomously after the initial admin configuration. During normal operation the server receives data from upstream time sources and uses them to discipline its own system clock. Upon request from a client the server will relay its system time to allow the client to sync its own clock with the NTP server. Users will primarily be interacting with the server during the initial install and subsequent maintenance and troubleshooting. The operational flowchart showcases the normal operating behavior of the server.


## Atomic Subsystem Specifications

Based on the high-level design, provide a comprehensive description of the functions each subsection will perform.

Inclued a description of the interfaces between this subsystem and other subsystems:
- Give the type of signal (e.g. power, analog signal, serial communication, wireless communication, etc).
- Clearly define the direction of the signal (input or output).
- Document the communication protocols used.
- Specifying what data will be sent and what will be received.

Detail the operation of the subsystem:
- Illustrate the expected user interface, if applicable.
- Include functional flowcharts that capture the major sequential steps needed to achieve the desired functionalities.

For all subsystems, formulate detailed "shall" statements. Ensure these statements are comprehensive enough so that an engineer who is unfamiliar with your project can design the subsystem based on your specifications. Assume the role of the customer in this context to provide clear and precise requirements.

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

## SoC

The Soc is responsible for orchestration of the entire system. Its primary duties are recieving timestamps from the GNSS module and checking the stamp's integrity.
Run an open source software called Chrony, which is an extremely powerful software that allows for rigorous security, and flexible distrobution of the timestamps
to clients downstream. It will also tune an oscillator to timestamps recieved, which is needed if the system loses synchronization to the time sources.
The SoC will also be used to program external modules like the GNSS module to achieve the desired settings the user wants. Lastly, the SoC will transmitt
the secure timestamps to clients, and will display its current settings and status to the user.

  Functions:
    - Recieve and process timestamps
    - Tune the hold-over system
    - Enforce rigorous security measures
    - Transmitt timestamps to clients

  Inputs:
    - Commands to change system settings
    - Timestamps from GNSS via UART/I2C
    - Status from peripherials
    - Current Oscillator Frequency
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
    -The system shall recieve accurate timestamps from satellite constellations
    -The system will ensure the timestamps are secure and that the system is resistant from attempts to subvert it
    -The system will be able properly control peripherials and transmitt accurate timestamps to clients

## Client Communication  
Client communication is the system that uses the hyper-accurate system time determined by the Clock synchronization subsystem and relays that to the clients in need of NTP data. The client will request NTS credentials to be able to verify its identity in the future using TCP port 4460. This is called the Transport Layer Security (TLS) handshake. The client and server register each other as trusted communicators and create credentials called cookies that are used in future communications [1]. When the client needs NTP data it sends a request on UDP port 123 to the server using its cookies, the server receives processes and sends back the accurate time data. Using the time given by the server the client will adjust its internal clock to match the server, taking into account any delay accrued from the transmission. [2]  

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
Device security encapsulates the measures needed to protect the server from cybersecurity threats beyond incorrect timing data. It will allow our device to be safe from bad actors on its network. To ensure a secure server, we will implement a firewall to lock down the ports and allow only the ones needed for basic server function. This includes port 123 for NTP, port 4460 for NTS and port 22 for SSH [1][2]. To allow for secure administrator control SSH key authentication will be implemented to prevent potentially faulty passwords that can be compromised[4]. We will restrict Chrony to a non-root user to ensure that if Chrony is compromised, the attacker cannot gain access to the entire server[3]. Monitoring will also be used to ensure important configuration files are secure and unchanged and noting any NTP synchronization anomalies[3].  

Functions:  
- Protect the NTP server from unauthorized access and attempted compromise.
- Safely monitor the NTP server to allow for secure admin access.
- Log suspicious activity for administrator review.

Inputs: 
- All incoming internet communication.

Outputs: 
- Validated inputs to be passed onto the Clock Synchronization subsystem

High-Level Requirements:
- The system shall remain secure and protected from intentional interference using firewalls and software quarantining. 
- The system shall block all incoming signals except those needed for device functionaility. 
- The system will provide secure access for administrators using SSH.


## Ethical, Professional, and Standards Considerations

In the project proposal, each team must evaluate the broader impacts of the project on culture, society, the environment, public health, public safety, and the economy. Additionally, teams must consider relevant standards organizations that will inform the design process. A comprehensive discussion should be included on how these considerations have influenced the design. This includes detailing constraints, specifications, and practices implemented as a result, and how these address the identified considerations.


## Resources

You have already estimated the resources needed to complete the solution. Now, let's refine those estimates.

## Budget
### SoC
| Item         | Description                              | Quanity | Cost (Estimate) |
|--------------|------------------------------------------|---------|-----------------|
| Computer     | A small computer to run the Linux server | 1       | $100            |
| 32Gb SD Card | Small resiliant storage                  | 1       | $30             |
|              |                                          | Total:  | $130            |    

The computer will be the core unit of the system and  should be chosen for its combination of cost and performance, as well as it's ability to run the computations necessary. The 32Gb micro SD card will be chosen as a trustworthy storage medium and its ability to hold the required software and operating system. 

### GNSS + Radio
| GNSS Satelite Antenna/Reciever | The antenna and reciever for the GPS system                    | 1      | $300  |
|--------------------------------|----------------------------------------------------------------|--------|-------|
| PPS Wire                       | A wire for the PPS singal to connect to a I/O port on the SOC  | 1      | $5    |
| AM Radio Reciever              | The antenna and reciever for the 60kHz WWVB  radio time source | 1      | $15   |
|                                |                                                                | Total: | $320  |  

The GNSS portion of this project is a major factor in what makes this system resiliant. The GNSS receiver needs to be capable of receiving signals from multiple constelations as well as being accruate enough to allow for our desired system accuracy of ()us. To ensure both of these criteria are met, a more robust GNSS module is needed. The seperate PPS wire will allow for the PPS signal to reach a I/O port on the SoC, increasing accuracy. In addition to GNSS signal our server will recieve radio signal from the WWVB NTP frequency. This adds an additional input source that is not connected to any other, improving the resiliancy of our system.  


### Enclosure
| Enclosure   | Enclosure to store all components in a way that is neat and orderly    | 1      | $100  |
|-------------|------------------------------------------------------------------------|--------|-------|
| Mounting    | Mounting for the SOC and other ports                                   | 1      | $30   |
| Cooling     | Possible internal fans and cooling methods                             | 1      | $30   |
| misc wiring | The remaining physical connectors we may need (Ethernet, Jumper, etc.) | 5      | $30   |
|             |                                                                        | Total: | $190  |  

The enclosure subsystem contains the parts holding the entire project together. We will determine the size of the enclosure based on the specific parts that will be chosen in the future. It will provide physical protection to the systems inside. Mounting will be used to hold the components inside of the enclosure. They are what ensures the peices remain where they are intended to be. Depending on the overall wattage and tempterature produced, some kind of cooling may be needed to maintain desired internal temperature. A small fan will likely be used to acheive this. Finally, our systems need to be connected with wiring to allow the needed communication between systems. The misc wiring  was added to cover for the connections we will need. 

###Power + Holdover
| External Oscillator | A device to keep highly accurate system time                     | 1      | $150 |
| ------------------- | ---------------------------------------------------------------- | ------ | ---- |
| Battery Bank        | Allows the devices to keep running in the case of a power outage | 1      | $100 |
| SOC AC Adapter      | Cables to power the SOC                                          | 1      | $20  |
|                     |                                                                  | Total: | $270 |  

Holdover is the systems ability to remain accurate for a duration of time without activly receiving input from the various sources. A strong holdover time allows our system to be a trusted source regardless of what external circumstances are affecting the inputs. A powerful external oscillator will allow this to happen. To allow for the desired accuracy of ()us we need a well tested and trusted oscillator. In addition to the loss of input signal, it is possible for the system to lose power for a duration of time. To remedy this we will encorporate a power bank/UPS to allow for the server to continue running without power for a set time, ensuring our time server remains accurate regardless of outages. The SoC AC adapter will allow for our selected SoC to receive direct power.

Develop a budget proposal with justifications for expenses associated with each subsystem. Note that the total of this budget proposal can also serve as a specification for each subsystem. After creating the budgets for individual subsystems, merge them to create a comprehensive budget for the entire solution.

### Division of Labor

First, conduct a thorough analysis of the skills currently available within the team, and then compare these skills to the specific requirements of each subsystem. Based on this analysis, appoint a team member to take the specifications for each subsystem and generate a corresponding solution (i.e. detailed design). If there are more team members than subsystems, consider further subdividing the solutions into smaller tasks or components, thereby allowing each team member the opportunity to design a subsystem.

### Timeline

Revise the detailed timeline (Gantt chart) you created in the project proposal. Ensure that the timeline is optimized for detailed design. Address critical unknowns early and determine if a prototype needs to be constructed before the final build to validate a subsystem. Additionally, if subsystem $A$ imposes constraints on subsystem $B$, generally, subsystem $A$ should be designed first.


## References

All sources utilized in the conceptual design that are not considered common knowledge must be properly cited. Multiple references should be included.


## Statement of Contributions

Each team member is required to make a meaningful contribution to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.

