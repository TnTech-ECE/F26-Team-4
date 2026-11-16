# **Detailed Design of GNSS Module and Antenna**

## **Function of Subsystem**
The Function of the subsystem is to retrieve signals from satellite constellations which contain timestamps of the agreed universal time, demodulate the signal, and then send the timestamp to the processor. The module will also provide some protection against spoofing and analyze signal integrity.

## **Specifications and Constraints**

[1]“Federal Register :: Request Access,” unblock.federalregister.gov. https://www.ecfr.gov/current/title-47/chapter-I/subchapter-A/part-15/subpart-B (accessed Nov. 15, 2026).


- Multiple GNSS Constellations: In order for the system to be more reliable and accurate, the module shall be able to receive from multiple satellite constellations.
- Timing Accuracy: Provide accurate timestamps and have PPS for better precision.
- Signal Monitoring: The module should be able to report the current integrity of the signal
- Spoofing Detection: The module should be able to warn the user of potential attacks
- Interface Compatibility: The module should be able to seamlessly integrate with the SoC and Antenna
- Standard: The module shall follow Title 47 CFR Part 15 Subpart B for unintentional radiators[1]. 


## **Governing Principles**

### **Pseudorange**

The main sources of uncertainty introduced when the signal is transmitted from the satellite are the ionosphere, troposphere, multipathing, and hardware delays. However, it’s hard to calculate the effects from many of these sources, so we will use the simplified model of pseudo-range.
Pseudo-range is the apparent (not absolute geometric distance due to phenomena that affects propogation) distance between a satellite and the receiver, which is calculated as the difference between transmission and reception times[^25]. 

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

Since our system is stationary, we can approximate this geometric uncertainty to around 3-6ns if we know the position of our receiver within an accuracy of a few meters. As for ionosphere, during nighttime, it can add 10s of ns in uncertainty and in the daytime, can add 100s of ns in uncertainty. Another source of uncertainty is the relativistic Sagnac effect, which can add around 200ns of delay[^26].  





## **Overview of Solution**
To meet the proposed criteria above, the NEO-M8T GNSS was picked.



