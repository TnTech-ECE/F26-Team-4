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

## **Overview of Solution**



