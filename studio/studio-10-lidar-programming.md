# Studio 10 - LiDAR Programming

## LiDAR Basics

### Working Principle

LIDAR sensors use light to measure the distance between the sensor and an object or surface. More specifically, the LiDAR sensors measure distance by emitting a laser beam and measuring the time it takes for the beam to reflect off an object and return to the sensor.

### Communication

In CG2111A Project, LiDAR communicates with the RPi using **Serial** over **USB**. The sensor's operation, such as its sampling rate, rotation speed, and scan initiation, is controlled by sending commands as **bytes** defined in its datasheet and protocol documentation. To simplify this process, a **Python Library** is provided!

{% hint style="info" %}
[**Protocols**](studio-13-communication-protocol.md#protocols-the-rules-of-engagement) define how devices communicate, and **libraries** can abstract these details to simplify sensor interaction.
{% endhint %}

### Process LiDAR Output

2D LiDARs provide **angle** and **distance** measurements of the environment around the sensor. (This is known as the **polar coordinates**). To extract useful information from this data, you may need to convert them from **polar coordinates** to **cartesian coordinates**.
