# **Detailed Design of Processor Subsystem-Hardware**

## **Function of Subsystem**
The function of this subsystem is to configure other subsystems such as the GNSS Module and Holdover Subsystem. It will also be responsible for hosting the software that runs the cybersecurity and disciplines the Holdover subsystem. Lastly, it will send the timestamps to clients.

## **Specifications and Constraints**

- **Shall be in the Raspberry Ecosystem and have the Standard 40 Pin Layout for ease of Integration with the NEO-M8T**
- **Shall be able to run the PiOS Lite**
- **Shall have the ability to process and send UART,Serial,and PPS**
- **Socioeconomic:Shall be relatively inexpensive so that the system is available to as many individuals and organizations as possible as per our stakeholder**

## **Overview of Proposed Solution**

To meet the proposed criteria: running PiOS Lite, standard pin layout, have the requisite peripherals, be relatively inexpensive, the solution chosen is the Raspberry 3 Model B, paired with an 8 GB SD card[1].

<img width="300" height="300" alt="raspberry-pi-github" src="https://github.com/user-attachments/assets/e46ac416-a83d-44e8-90f1-98510df198a9" />
<img width="300" height="300" alt="sd-card-github" src="https://github.com/user-attachments/assets/2c6cd559-70a9-4681-9f6f-25f909f49a3f" />

We chose the Raspberry Pi 3 Model B due to several factors. Firstly, it has the capabilities to receive UART through two dedicated pins, GPIO 14, and GPIO 15. Secondly, it natively supports PiOS Lite, which is meant to run in the Raspberry Pi ecosystem. PiOS Lite lets the system run our main Network Time Server software, Chrony. Lastly, it has pins that allow reception of Pulse Per Second (PPS), which is important for accurate timing[2].

The 8 GB SD card is the minimum and most inexpensive option that can run the PiOS Lite.

### **Raspberry Pi Model B: Power Requirements[3]**

- **Needs a 5.1 V Supply**
- **Needs a 2.5 A Supply**
- **Uses a Micro USB Connection**

These requirements will be discussed in the Power Supply Subsystem.

### **Raspberry Pi Model B: Thermal Requirements[4]**

- **Maximum Temperature for Operation is 85 Degrees Celsius**
- **Throttling of CPU and GPU start between 80 and 85 Degrees Celsius**
- **Default Soft Temperature is 60 Degrees Celsius**

To meet these requirements, the Enclosure Subsystem will have cooling mechanisms, and will be discussed more in-depth there.

## **Interfaces**

### **GNSS Interface**
On its own, the processor cannot receive or generate the Universal Timestamps that our system requires, and therefore, our system uses the NEO-M8T Hat, which retrieves and demodulates signals from satellite constellations, and then sends these timestamps to the to the Raspberry Pi via two signals, UART, which contains the actual time, and a pulse via Pulse Per Second (PPS) which allows our system to know when the timestamp was received, so the system knows what actual timestamp to use as there is a slight delay between the GNSS Module and the Processor.

The benefit of choosing the Raspberry Pi ecosystem is that the NEO-M8T seats directly on top the 40 pins on the board, which makes integration seamless[5].

Pinout of the Raspberry Pi 3:

<img width="608" height="334" alt="image" src="https://github.com/user-attachments/assets/c4d8e9ac-40ac-4b6a-b073-0057e9ddb7e1" />

<img width="583" height="689" alt="image" src="https://github.com/user-attachments/assets/e1745692-a149-47e2-8f89-208ad4800569" />

The NEO-M8T connects to the Processor as such:

<img width="787" height="565" alt="image" src="https://github.com/user-attachments/assets/172d8f90-9de5-41e1-ad00-ff9f0cefcbfa" />

As can be seen, the corresponding Rx and Tx, and the pins for PPS emission and reception are directly aligned.

The pins being used are:
- **GPIO 8 for UART Tx to the NEO-8MT**
- **GPIO 10 for UART Rx from the NEO-8MT**
- **GPIO 12 for PPS from the NEO-8MT**

### UART Protocol
The message received in the GPIO 10 pin will be 92 bytes long and will contain UTC Day, Hour, Minute, and Second. Along with error flags to represent possible issues. More detail on UART is available in the GNSS Subsystem. How the system uses the received timestamp will be in the Clock Synch Subsystem.  

### PPS
The pulse from the PPS from the NEO-M8T is an interrupt that the pin will receive, and then the Clock Synch Subsystem will correlate the timestamp from the UART pins. More detail can be found in the Clock Synch Subsystem.







## **References**       
[1]A. Negi, “Raspberry Pi 3 B+ Pinout With GPIO Functions, Schematic and Specs in Detail.” https://www.etechnophiles.com/raspberry-pi-3-b-pinout-with-gpio-functions-schematic-and-specs-in-detail/ (accessed Nov. 26, 2026).

[2]Raspberry Pi, “Raspberry Pi Documentation - Getting Started,” www.raspberrypi.com. https://www.raspberrypi.com/documentation/computers/getting-started.html (accessed Nov. 27, 2026).

[3]Raspberry Pi, “Raspberry Pi Documentation - Raspberry Pi Hardware,” www.raspberrypi.com. https://www.raspberrypi.com/documentation/computers/raspberry-pi.html (accessed Nov. 26, 2026).

[4]Raspberry Pi, “Raspberry Pi Documentation - Raspberry Pi Hardware,” www.raspberrypi.com. https://www.raspberrypi.com/documentation/computers/raspberry-pi.html (accessed Nov. 27, 2026).

[5]Waveshare, “NEO-M8T GNSS TIMING HAT for Raspberry Pi,” Waveshare. https://www.waveshare.com/neo-m8t-gnss-timing-hat.htm (accessed Nov. 27, 2026).
