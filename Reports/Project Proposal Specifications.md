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

All sources used in the project proposal that are not common knowledge must be cited. Multiple references are required.


## Statement of Contributions

Each team member must contribute meaningfully to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.
