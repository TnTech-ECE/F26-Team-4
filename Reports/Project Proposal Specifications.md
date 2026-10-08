# Project Proposal

This document provides a comprehensive explanation of what a project proposal should encompass. The content here is detailed and is intended to highlight the guiding principles rather than merely listing expectations. The sections that follow contain all the necessary information to understand the requirements for creating a project proposal.


## General Requirements for the Document
- All submissions must be composed in markdown format.
- All sources must be cited unless the information is common knowledge for the target audience.
- The document must be written in third person.
- The document must identify all stakeholders including the instuctor, supervisor, and customer.
- The problem must be clearly defined using "shall" statements.
- Existing solutions or technologies that enable novel solutions must be identified.
- Success criteria must be explicitly stated.
- An estimate of required skills, costs, and time to implement the solution must be provided.
- The document must explain how the customer will benefit from the solution.
- Broader implications, including ethical considerations and responsibilities as engineers, must be explored.
- A list of references must be included.
- A statement detailing the contributions of each team member must be provided.


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

Provide context and details necessary to define the problem clearly and delineate its boundaries.

### Specifications and Constraints

Specifications and constraints define the system's requirements. They can be positive (do this) or negative (don't do that). They can be mandatory (shall or must) or optional (may). They can cover performance, accuracy, interfaces, or limitations. Regardless of their origin, they must be unambiguous and impose measurable requirements.

#### Specifications

Specifications are requirements imposed by **stakeholders** to meet their needs. If a specification seems unattainable, it is necessary to discuss and negotiate with the stakeholders.

#### Constraints

Constraints often stem from governing bodies, standards organizations, and broader considerations beyond the requirements set by stakeholders.

Questions to consider:
- Do governing bodies regulate the solution in any way?
- Are there industrial standards that need to be considered and followed?
- What impact will the engineering, manufacturing, or final product have on public health, safety, and welfare?
- Are there global, cultural, social, environmental, or economic factors that must be considered?


## Survey of Existing Solutions

Research existing solutions, whether in literature, on the market, or within the industry. Present these findings in a coherent, organized manner. Remember to cite all information that is not common knowledge.


## Measures of Success

Define how the project’s success will be measured. This involves explaining the experiments and methodologies to verify that the system meets its specifications and constraints.


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

Explain the implications of solving the problem for the customer. After reading this section, the reader should understand the tangible benefits and the worthiness of the proposed work.


## Broader Implications, Ethics, and Responsibility as Engineers

Consider the project’s broader impacts in global, economic, environmental, and societal contexts. Identify potential negative impacts and propose mitigation strategies. Detail the ethical considerations and responsibilities each team member bears as an engineer.


## References

All sources used in the project proposal that are not common knowledge must be cited. Multiple references are required.


## Statement of Contributions

Each team member must contribute meaningfully to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.
