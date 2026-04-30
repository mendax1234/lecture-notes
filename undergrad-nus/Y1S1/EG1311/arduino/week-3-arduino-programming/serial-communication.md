# Serial Communication

## Introduction

Serial communication in Arduino is a way to send and receive data between the Arduino board and other devices such as computers, sensors, or other microcontroller. It is usually used for **debugging and monitoring**.

### Characteristics

1. Arduino has built-in UART communication on pins `0` (RX) and `1` (TX).
2. Serial communication is used to send data serially, **byte-to-byt**e.
3. Baudrate[^1] for Arduino is `9600` or `115200`. (`9600` is **default** and most commonly used)

### Usage

```arduino
// In setup()
void Serial.begin(baudrate);

// In loop()
void Serial.print(string);
void Serial.println(string);
```

## Examples

### Timer Demo

This is a 2-second cycle. The LED will turn ON for the first 1 second of the cycle and OFF for the second second.

{% code lineNumbers="true" %}
```arduino
void setup() {
    pinMode(13, OUTPUT);
    Serial.begin(9600);
}

int count = 0;
void loop() {
    Serial.println(count);
    if ((millis() - count) < 1000) {
        digitalWrite(13, HIGH);
    } else {
        digitalWrite(13, LOW);
    }
    if ((millis() - count) >= 2000) {
        count = millis();
    }
    delay(10);
}
```
{% endcode %}

{% hint style="info" %}
The `delay(10)` at the end of the code helps to avoid rapid cycling and ensures stable behavior.
{% endhint %}

[^1]: The **baud rate** for Arduino serial communication refers to the speed at which data is transmitted and received over the serial connection. It is measured in **bits per second (bps)**, indicating how many bits of data can be sent or received in one second.
