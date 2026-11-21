# **Detailed Design of GNSS Module and Antenna**

## **Function of Subsystem**
The Function of the subsystem is to retrieve signals from satellite constellations which contain timestamps of the agreed universal time, demodulate the signal, and then send the timestamp to the processor. The module will also provide some protection against spoofing and analyze signal integrity.

## **Specifications and Constraints**

[1]“Federal Register :: Request Access,” unblock.federalregister.gov. https://www.ecfr.gov/current/title-47/chapter-I/subchapter-A/part-15/subpart-B (accessed Nov. 15, 2026)


- **Multiple GNSS Constellations: In order for the system to be more reliable and accurate, the module shall be able to receive from multiple satellite constellations**
- **Timing Accuracy: Provide accurate timestamps and have PPS for better precision**
- **Signal Monitoring: The module should be able to report the current integrity of the signal**
- **Spoofing Detection: The module should be able to warn the user of potential attacks**
- **Interface Compatibility: The module should be able to seamlessly integrate with the SoC and Antenna**
- **The Antenna and GNSS Module shall not cost more than 300$**
- **Standard: The module shall follow Title 47 CFR Part 15 Subpart B for unintentional radiators[1]**


## **Governing Principles**

### **Pseudorange**


“ITU-T Technical Report GSTR-GNSS Considerations on the Use of GNSS as a Primary Time Reference in Telecommunications,” 2020. Accessed: Nov. 09, 2026. [Online]. Available: https://www.itu.int/dms_pub/itu-t/opb/tut/T-TUT-HOME-2020-PDF-E.pdf ↩

The main sources of uncertainty introduced when the signal is transmitted from the satellite are the ionosphere, troposphere, multipathing, and hardware delays. However, it’s hard to calculate the effects from many of these sources, so we will use the simplified model of pseudo-range.
Pseudo-range is the apparent (not absolute geometric distance due to phenomena that affects propogation) distance between a satellite and the receiver, which is calculated as the difference between transmission and reception times[2]. 

It is defined as:

$$
\
\sigma =
\sqrt{(x_i-x_a)^2+(y_i-y_a)^2+(z_i-z_a)^2}
+c(t_{\text{offset}}+dt_{ar})
=c(t_{ar}-t_i)
\
$$

$x_i$ , $y_i$ , $z_i$ is the current satellite location

$x_a$ , $y_a$ , $z_a$ is the current receiver location

$c$ is the speed of light

$t_{offset}$ is the real time offset of the receiver’s local time

$dt_{ar}$ is the real propagation delay between the antenna phase center and receiver correlator

To account for the time delay rearrange the above equation:

Set:

$$
\
R = \sqrt{(x_i-x_a)^2+(y_i-y_a)^2+(z_i-z_a)^2}
\
$$

$$
\
\delta t = t_{\text{offset}} + dt_{ar}
\
$$

$$
\
\sigma = R + c\delta t
\
$$

Rearranging to isolate the timing offset:

$$
\
\boxed{\delta t = \frac{\sigma-R}{c}}
\
$$

These equations show how the geometric relationship between the satellites and the receiver corresponds to a total timing offset. In reality, it takes multiple satellites to actually solve the above equation. 

Since our system is stationary, we can approximate this geometric uncertainty to around 3-6ns if we know the position of our receiver within an accuracy of a few meters[2].

### **Troposphere**

The troposphere is the densest part of the atmosphere, where life and weather occur. It is hard to quantify the uncertainty caused from this band of the atmosphere as it depends and the satellite's elevation and current conditions[3].

### **Ionosphere**

The ionosphere is the ionized portion of earth's atmosphere exists. Ultraviolet and X-ray radiation are very prevalent. This region of the atmosphere introduces uncertainty in the range of a few ns at night to hundreds of ns during the day[3].

### **Overall Uncertainty**
With a well-designed receiver, the overall uncertainty will reach about +/- 10ns of uncertainty, with is negligible for most systems[3].


## **Overview of Solution**
To meet the proposed criteria above, the NEO-M8T GNSS was picked.

![GNSS MODULE](https://m.media-amazon.com/images/I/71y5AwOZCgL._AC_SX679_.jpg)


[4]“U-Blox 8 / U-Blox M8 Receiver Description Including Protocol Specification.” Accessed: Nov. 17, 2026. [Online]. Available: https://content.u-blox.com/sites/default/files/products/documents/u-blox8-M8_ReceiverDescrProtSpec_UBX-13003221.pdf

## **GNSS Module Specifications**:
The module shown above meets all the criteria proposed in the specifications and constraints section.
The module can[4]:

- **Connect to up to 3 constellations at one time, such as GPS, Beidou, Galileo, and Glonass**
- **Includes UART, I2C, and PPS for data tranfer to external processors**
- **Has built in security measures to improve riliability by detecting spoofing attempts**
- **Has a stationary mode to improve accuracy, which is usefull as the server will be in a server room**
- **Has a built in battery holder to store information in case power is lost**
- **The module costs 133$**

## **NEO-M8T Power and Connection Requirements**[4]:
- **Operating Voltage: 2.7 V to 3.6 V**
- **Minimum Gain at RF Input: 5 dB**
- **Maximum Gain at RF Input: 40 dB**
- **Maximum Noise Figure: 1.5 dB**
- **SMA Cable Needed**

The antenna chosen is the Waveshare GNSS L1+L5 Dual-Frequency Active Antenna.

![Antenna](https://m.media-amazon.com/images/I/61750qZ5iWL._AC_SX679_.jpg) 

Needs source other than amazon ):{


## **Antenna Specifications**:
- **Operating Voltage: 3 V to 18 V**
- **Gain: 28 +/- 2 dB**
- **SMA Cable**
- **Capable of receiving multiple Satellite Constellations**


## **Compatibility and Analysis**:

Both antenna and GNSS Module have compatible connectors and specifications and therefore should be able to seamlessly integrate. The only issue is that from the datasheet of the NEO-M8T, is that it would barely be able to power the antenna, however, since we are using an external power supply, we can power the antenna with it.

The GNSS Module is capable of receiving multiple signals from constellations at the same time, has built in detection of spoofing attempts, dedicated stationary modes for increased accuracy, and has configurations specifically to increase timing accuracy[4].

The antenna is also capable of receiving multiple signals at the same time time, and its gain and connectors are compatible with the GNSS Module. 

Therefore, the Waveshare Antenna and NEO-M8T should be capable of achieving our specifications.

## **Interfaces**

## **UART**

[5]S. Campbell, “Basics of UART Communication,” Circuit Basics, Feb. 13, 2016. https://www.circuitbasics.com/basics-uart-communication/ (accessed Nov. 19, 2026).

UART, Universal Asynchronous Receiver/Transmitter, will be the main method of communication between the GNSS Module and the SoC. It works like this:The transmitting UART converts parallel data from a controlling device like a CPU into serial form, transmits it in serial to the receiving UART, which then converts the serial data back into parallel data for the receiving device. Only two wires are needed to transmit data between two UARTs. Data flows from the Tx pin of the transmitting UART to the Rx pin of the receiving UART[5].

UART allows our project to achieve two major goals of our project. It will allow us to configure the GNSS Module to our desired specifications, and it will allow us transmit the timestamps recieved from the satellite constellations to the SoC.

## **UART: UBX Protcol**

https://content.u-blox.com/sites/default/files/products/documents/u-blox8-M8_ReceiverDescrProtSpec_UBX-13003221.pdf?utm_content=UBX-13003221 [6]

The NEO-M8T uses a specialized UART protocol made by the board's manufacturer. Using this protocol, the massage transmitted will consist of[6]:

<img width="535" height="447" alt="image" src="https://github.com/user-attachments/assets/828e6a31-c993-46c5-b453-e382347c854e" />

With proper configuration of the GNSS Module, we can configure the payload to contain UBX-NAV-PVT:

<img width="529" height="566" alt="image" src="https://github.com/user-attachments/assets/391e09ac-060b-49c7-8f95-118be1dc3e23" />
<img width="500" height="394" alt="image" src="https://github.com/user-attachments/assets/e7635c38-bb3b-4eb5-9fc2-5398bd67f173" />

This configuration not only gives us the universal time, but also has quantified error that we can use to correct any error.


## **PPS:**

[7]GNSS Metrics, “PPS Output and Disciplined Oscillators | GNSS Metrics,” GNSS Metrics. https://gnssmetrics.com/learn/timing/pps-output (accessed Nov. 19, 2026).


A Pulse Per Second (PPS) signal is a periodic electrical pulse output by a GNSS receiver or timing device. The rising edge of each pulse is precisely synchronised to GNSS time — typically GPS time or UTC — with nanosecond accuracy. The pulse width is usually 100 ms or 500 ms, but the timing reference point is always the rising edge[7].

PPS is of extraordinary importance to precision timing. It allows us to associate the current UART message to the past received signal as there will be a small amount of time difference between them due to hardware delays. This association will be handled by the software in the SoC.

## **Operation:**

<img width="568" height="285" alt="image" src="https://github.com/user-attachments/assets/eeb239c7-f415-4adf-a98c-ce4e721c0aaf" />

At the start of operation, the SoC will configure the GNSS Module. For example, we will tell the module what constellations we want it to lock on to, the messages in the payload that are delivered to the SoC, and how often they are delivered. If the module is not configured, or not properly configured, the module will return an error to the user. After proper configuration, the module will start to scan for the constellations. If it cannot detect any, it will send an error to the SoC, and the SoC will trigger the Holdover Subsystem until a lock is achieved. Then, when a lock is achieved, it will receive the signal, demodulate it, then send the payload with the timestamp, as well as a time pulse, to the SoC, which will then be distributed to clients. At the same time, the module will try to detect spoofing attempts and will alert the user if there is a possible spoofing event. It should be noted that it cannot guarantee that it will detect spoofing but adds an extra layer of security. Also, the module must be placed with a clear view of the sky, or accuracy will be severely reduced.

## **Analysis:**

| Constraint | Design Fulfillment | Evidence / Justification |
| --- | --- | --- |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |









   





