# **Detailed Design of Processor Subsystem-Hardware**

## **Function of Subsystem**
The function of this subsystem is to configure other subsystems such as the GNSS Module and Holdover Subsystem. It will also be responsible for hosting the software that runs the cybersecurity and disciplines the Holdover subsystem. Lastly, it will send the timestamps to clients.

## **Specifications and Constraints**

- **Shall be in the Raspberry Ecosystem and have the Standard 40 Pin Layout for ease of Integration with the NEO-M8T**
- **Shall be able to run the PiOS Lite**
- **Shall have the ability to process and send UART,Serial,and PPS**
- **Shall be relatively inexpensive so that the system is available to as many individuals and organizations as possible as per our stakeholder**

## **Overview of Proposed Solution**

[1]A. Negi, “Raspberry Pi 3 B+ Pinout With GPIO Functions, Schematic and Specs in Detail.” https://www.etechnophiles.com/raspberry-pi-3-b-pinout-with-gpio-functions-schematic-and-specs-in-detail/ (accessed Nov. 26, 2026).


To meet the proposed criteria: running PiOS Lite, standard pin layout, have the requisite peripherals, be relatively inexpensive, the solution chosen is the Raspberry 3 Model B, paired with an 8 GB SD card.

<img width="860" height="860" alt="image" src="https://github.com/user-attachments/assets/6dd1428e-dbc1-457f-b4b2-58a717834138" />
<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/08e59b99-a829-4c07-9043-8e7aead1b1c5" />





We chose the Raspberry Pi 3 Model B due to several factors. Firstly, it has the capabilities to receive UART through two dedicated pins, GPIO 14, and GPIO 15. Secondly, it natively supports PiOS Lite, which is meant to run in the Raspberry Pi ecosystem. PiOS Lite lets the system run our main Network Time Server software, Chrony. Lastly, it has pins that allow reception of Pulse Per Second (PPS), which is important for accurate timing[1].

### **Raspberry Pi Model B: Power Requirements**

[2]Raspberry Pi, “Raspberry Pi Documentation - Raspberry Pi Hardware,” www.raspberrypi.com. https://www.raspberrypi.com/documentation/computers/raspberry-pi.html (accessed Nov. 26, 2026).



