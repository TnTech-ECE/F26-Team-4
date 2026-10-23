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

The fully formulated problem is the overall objective and scope complete with the set of shall statements. This was part of the project proposal. However, it may be that the scope has changed. So, state the fully formulated problem in the introduction of the conceptual design and planning document. For each of the constraints, explain the origin of the constraint (customer specification, standards, ethical concern, broader implication concern, etc).


## Comparative Analysis of Potential Solutions
**Merge this bad boy with the one from Project Proposal**

The biggest consideration within this project was deciding our solution for NTP communication; our two proposed solutions were to either use a SoC running Linux with an open-source solution such as Chrony or some microcontroller with bare-metal implementation. There are advantages and disadvantages for each approach:

|       | SoC Implementation                                                                                                                                                                                                                   | Microcontroller Implementation                                                                   |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| Pros: | - Ease of implementation<br>- Built-in NTS and other security protocols<br>- Simple Multiple Time Source integration<br>- Easy time source metric tracking, such as drift of individual sources<br>- Simpler Networking capabilities | - Lower Cost<br>- More Compact                                                                   |
| Cons: | -May increase the total cost of the project                                                                                                                                                                                          | - Harder overall implementation<br>- Requires more time to develop<br>-  Potentially less secure |
Given the options proposed, we decided that the marginally lower cost wasn't worth the difficulties involving implementation and potential security issues of a bare-metal solution. With the advantages that an SoC provides, it's the clear choice for this project.

Once we decided our system type, the next item of consideration was our time sources. Our client specifications required a GPS source, so that made it non-negotiable. The additional sources (Radio, Oscillator, and Internet Time Servers) we consider adding are to improve the robustness of the system.

## High-Level Solution

This section presents a comprehensive, high-level solution aimed at efficiently fulfilling all specified requirements and constraints. The solution is designed to maximize stakeholder goal attainment, adhere to established constraints, minimize risks, and optimize resource utilization. Please elaborate on how your design accomplishes these objectives.


### Hardware Block Diagram
![[Hardware Diagram 2.svg]]
It is worth noting that the radio clock module in this diagram is an optional component. 
### Operational Flow Chart

Similar to a block diagram, the flow chart aims to specify the system, but from the user's point of view rather than illustrating the arrangement of each subsystem. It outlines the steps a user needs to perform to use the device and the screens/interfaces they will encounter. A diagram should be drawn to represent this process. Each step should be represented in the diagram to visually depict the sequence of actions and corresponding screens/interfaces the user will encounter while using the device.


## Atomic Subsystem Specifications

### The Holdover Oscillator   
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

## Ethical, Professional, and Standards Considerations

In the project proposal, each team must evaluate the broader impacts of the project on culture, society, the environment, public health, public safety, and the economy. Additionally, teams must consider relevant standards organizations that will inform the design process. A comprehensive discussion should be included on how these considerations have influenced the design. This includes detailing constraints, specifications, and practices implemented as a result, and how these address the identified considerations.


## Resources

You have already estimated the resources needed to complete the solution. Now, let's refine those estimates.

### Budget

Develop a budget proposal with justifications for expenses associated with each subsystem. Note that the total of this budget proposal can also serve as a specification for each subsystem. After creating the budgets for individual subsystems, merge them to create a comprehensive budget for the entire solution.

### Division of Labor

Holdover Oscillator - Jonathan Salvato
Enclosure - Jonathan Salvato

### Timeline

Revise the detailed timeline (Gantt chart) you created in the project proposal. Ensure that the timeline is optimized for detailed design. Address critical unknowns early and determine if a prototype needs to be constructed before the final build to validate a subsystem. Additionally, if subsystem $A$ imposes constraints on subsystem $B$, generally, subsystem $A$ should be designed first.


## References

All sources utilized in the conceptual design that are not considered common knowledge must be properly cited. Multiple references should be included.


## Statement of Contributions

Block Diagram - Jonathan Salvato
Comparative Analysis - Jonathan Salvato
Atomic Subsystems: Holdover Oscillator - Jonathan Salvato