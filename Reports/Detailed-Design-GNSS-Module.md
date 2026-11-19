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

### **UART**

   





