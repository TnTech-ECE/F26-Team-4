# **Detailed Design of Processor Subsystem-Hardware**

## **Function of Subsystem**
The function of this subsystem is to configure other subsystems such as the GNSS Module and Holdover Subsystem. It will also be responsible for hosting the software that runs the cybersecurity and disciplines the Holdover subsystem. Lastly, it will send the timestamps to clients.

## **Specifications and Constraints**

- **Shall be in the Raspberry Ecosystem and have the Standard 40 Pin Layout for ease of Integration with the NEO-M8T**
- **Shall be able to run the PiOS Lite**
- **Shall have the ability to process and send UART,Serial,and PPS**
- **Shall be relatively inexpensive so that the system is available to as many individuals and organizations as possible as per our stakeholder**

## **Overview of Proposed Solution**
To meet the proposed criteria: running PiOS Lite, standard pin layout, have the requisite peripherals, be relatively inexpensive, the solution chosen is the Raspberry 3 Model B, paired with an 8 Gb SD card.  
