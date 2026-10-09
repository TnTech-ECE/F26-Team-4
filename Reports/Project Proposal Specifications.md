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

#### Specifications

The GPS regulated Secure Network Time Server will:
1. Keep and Broadcast the current time:
	-The server shall keep time within an accuracy of ## (ms, us, ns?) to UTC with an internal holdover of ## s/day
	-The server shall obtain the time from multiple sources including:
		-GPS such as GNSS
		-An Internal Oscillator
		-Network sources such as NIST
		-*If time allows, radio time such as WWVB*
	-The server shall communicate the current time data through the NTPv4 standard
2. Maintain Security:
	-The server shall maintain a secure connection to clients connected through NTS
	-The server shall be resilient to external attacks, such as:
		-GPS Spoofing
		-DOS/DDOS attacks
		-The loss of any one time source
3. Be open source and accessible to a hobbyist:
	-The server shall have components and build instructions that are clear and easy to follow
	-The server's cost needs to be accessible (***maybe mention an actual cost***)
	-The server's Licenses for software must be compatible and open source
#### Constraints

The GPS Regulated Secure Network Time Server will adhere to:
1. Server Standards and regulations
	-The server shall be compliant with the RFC 5905 Network Time Protocol Version 4
	
	-The server shall follow the guidelines set by the NISTTN2187 for resilient architecture
	
2. Wireless Receiver and processing Standards and reguations
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

Define how the project’s success will be measured. This involves explaining the experiments and methodologies to verify that the system meets its specifications and constraints.


## Resources

Each project proposal must include a comprehensive description of the necessary resources.

### Budget

Provide a budget proposal with justifications for expenses such as software, equipment, components, testing machinery, and prototyping costs. This should be an estimate, not a detailed bill of materials.

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

Each team member must contribute meaningfully to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.
