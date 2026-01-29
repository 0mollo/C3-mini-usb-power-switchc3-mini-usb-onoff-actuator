![image alt](https://github.com/0mollo/C3-mini-usb-power-switchc3-mini-usb-onoff-actuator/blob/main/USB%20On_off%20switch%20top%20View.PNG) | ![image alt](https://github.com/0mollo/C3-mini-usb-power-switchc3-mini-usb-onoff-actuator/blob/main/USB%20On_off%20switch%20bottom%20View.PNG)
# C3-Mini USB Power ON/OFF Switch

A compact, logic-level MOSFET-based power switch that allows an ESP32-C3 Mini
microcontroller to **reliably switch a 5 V USB power output ON and OFF**.

The board provides:
- A **USB-C power output** (5 V only, no data)
- A **two-pin 5 V load connector**
- Both outputs are switched simultaneously using a **logic-level N-MOSFET**
- Control via **GPIO D3**

This design is suitable for smart actuators, power cycling peripherals,
IoT power management, and embedded automation systems.


##  Features

- ESP32-C3 Mini compatible
- Logic-level N-MOSFET switching (3.3 V GPIO compatible)
- USB-C power output (VBUS only)
- Two-pin alternative 5 V output (solder-in load)
- CC1 / CC2 resistors for USB-C power advertisement
- Gate resistor and pull-down for safe boot behavior
- Compact, low-cost PCB design


##  Electrical Overview

| Parameter | Value |
|---------|------|
| Input voltage | 5 V DC |
| Control voltage | 3.3 V (GPIO) |
| Switching device | N-MOSFET (IRLML6344) |
| USB type | USB-C (Power only) |
| Control pin | D3 |


##  How It Works

- GPIO **D3** drives the MOSFET gate through a series resistor
- When D3 = HIGH (3.3 V), the MOSFET turns ON
- 5 V is delivered simultaneously to:
  - USB-C VBUS
  - Two-pin 5 V output header
- When D3 = LOW, both outputs are disconnected

Low-side switching is used, meaning the load ground is switched via the MOSFET.

##  USB-C Configuration

- VBUS pins tied together
- GND pins tied together
- CC1 → 5.1 kΩ → GND
- CC2 → 5.1 kΩ → GND

This advertises a **default 5 V USB power source** (non-PD).

##  Pinout Summary

| Signal | Description |
|------|------------|
| 5V | Input supply |
| GND | System ground |
| D3 | MOSFET control (ON/OFF) |
| USB-C | Switched 5 V output |
| 2-Pin Header | Switched 5 V output |


##  Example Firmware

```cpp
#define POWER_PIN 3   // D3

void setup() {
  pinMode(POWER_PIN, OUTPUT);
  digitalWrite(POWER_PIN, LOW); // OFF by default
}

void loop() {
  digitalWrite(POWER_PIN, HIGH); // Turn ON USB power
  delay(5000);

  digitalWrite(POWER_PIN, LOW);  // Turn OFF USB power
  delay(5000);
}
