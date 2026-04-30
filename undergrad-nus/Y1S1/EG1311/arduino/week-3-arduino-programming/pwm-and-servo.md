# PWM & Servo

## PWM

Pulse Width Modulation (PWM) is a technique used in Arduino to simulate analog output using digital signals. Since the Arduino's digital pins can only output **HIGH (5V)** or **LOW (0V)**, PWM allows you to create an average voltage between 0 and 5V by rapidly switching between high and low states. The duration for which **the signal stays high relative to the time it stays low** is called the **duty cycle**, which determines the average output voltage.

### Duty Cycle

* If the duty cycle is **10%**, the signal is high for 10% of the time and low for the remaining 90%. This results in a low average voltage.
* If the duty cycle is **50%**, the signal is high and low for equal amounts of time, giving a medium average voltage (approximately 2.5V in the case of 5V).
* If the duty cycle is **90%**, the signal is high for most of the time, leading to a higher average voltage (close to 5V).

### PWM Pins on Arduino

The Arduino has specific pins that support PWM, such as **pins 3, 5, 6, 9, 10, and 11**. These pins are marked with a tilde (`~`) next to their number. The `analogWrite(pin, value)` function is used to generate PWM signals on these pins.

### How analogWrite(pin, value) works

The `analogWrite(pin, value)` function writes a PWM signal to a pin. The `value` parameter is an 8-bit number, ranging from **0 to 255**.

1. `analogWrite(pin, 0)` corresponds to a **0% duty cycle** (always LOW, 0V).
2. `analogWrite(pin, 255)` corresponds to a **100% duty cycle** (always HIGH, 5V).
3. Intermediate values adjust the duty cycle between 0% and 100%, allowing you to generate varying voltages. For example, `analogWrite(pin ,127)` will generate around 2.5V.

### Examples

```arduino
int motor_pin = 3;

void setup() {
}

void loop() {
    // Increasing Speed
    for (int i = 0; i < 256; i++) {
        analogWrite(motor_pin, i);
        delay(10);
    }
    // Decreasing Speed
    for (int i = 255; i >= 0; i--) {
        analogWrite(motor_pin, i);
        delay(10);
    }
}
```

When this code runs, you would observe the motor gradually speeds up to its maximum speed and then gradually slows down to a stop.

## Servo Motor

A **servo motor** is a type of electromechanical device that allows for precise control of angular position. It is widely used in robotics, automation, and various other applications where accurate control of movement is required. Arduino makes it easy to control servo motors using its built-in libraries and functions. In Arduino, almost every digital pin can be used as the pin for the **servo**. The common practice is to use **pins 9 and 10**.

### Usage

```arduino
#include <Servo.h>

// Definition
Servo servo;

// In setup
servo.attach(pin, 1000, 2000);

// In loop
servo.write(deg);
```

In the `setup()` section, the last two parameters or `servo.attach()` specifies the `min` and `max` [Pulse Width](#user-content-fn-1)[^1] in microseconds, which corresponds to the minimum (0) and maximum (180) angle on the servo motor. See more on [Arduino Reference](https://www.arduino.cc/reference/en/libraries/servo/attach/). So, in this situation, we are regarding $$1000\mu s$$ as the 0 degree and $$2000\mu s$$ as the 180 degree. However, the actual case may not be as perfact as we expect.&#x20;

<details>

<summary>Pulse Width and angle in Servo Motors</summary>

Servo motors use PWM signals to determine their position. The motor receives a series of electrical pulses, and the duration of each pulse (pulse width) tells the servo how far to turn. But now the question is **How can we find the correct pulse width for 0 degree and 180 degree**. The method is to use **trial and error**. But notice that **do not** put value **smaller than 500 for 0 degree** and value **bigger than 2500 for the 180 degree**.

</details>

{% hint style="info" %}
Note that using servo will take away PWM timer from **pins 9,10**. Do not use PWM on **Pin 9,10** if using servo.
{% endhint %}

### Example

The below code will make the servo motor spin between 0 and 180 back and forth continuously.

```c
#include <Servo.h>
Servo servo;
int servo_pin = 2; // can be any digit pin!

void setup() {
    servo.attach(servo.pin, 660, 2400);
}

void loop() {
    for (int i = 0; i <= 180; i++) {
        servo.write(i);
        delay(5);
    }
    for (int i = 180; i >= 0; i--) {
        servo.write(i);
        delay(5);
    }
}
```

[^1]: **Pulse width** refers to the duration of time that a signal (usually a square wave) remains at a high (active) state during a cycle. It is a crucial concept in digital signal processing, especially in applications such as PWM (Pulse Width Modulation) and controlling devices like servo motors.
