# Mod-Bluetooth-Competition-Pro
Mod Bluetooth Competition Pro XInput / Amiga 500

Modification of a Competition Pro joystick for the Amiga 500, adding Bluetooth connectivity and XInput compatibility. The project includes the design and 3D printing of custom parts to adapt and integrate the electronic components inside the controller while preserving its original retro aesthetics and functionality.

The result is a wireless version of the classic Amiga joystick that can be used with modern computers and devices, combining electronics, custom 3D design, and digital fabrication.

Special thanks to:
- [Creality Cloud - Mod Bluetooth Competition Pro XInput](https://www.crealitycloud.com/model-detail/mod-bluetooth-competition-pro-xinput) for sharing and hosting the 3D designs for this project, making the custom parts available to the community.
- LemmingDev and the contributors of [ESP32-BLE-Gamepad](https://github.com/lemmingDev/ESP32-BLE-Gamepad), which provides the Bluetooth gamepad and XInput functionality used in this project.


## Controller Operation

This project is an **Amiga 500-inspired Bluetooth gamepad** based on the **Seeed XIAO ESP32-C3**. It uses the `BleGamepad` library and operates as an **XInput Series X controller** over Bluetooth.

### Features

* Amiga 500 / Competition Pro style controls.
* Bluetooth XInput Series X gamepad compatibility.
* Physical rumble motor support.
* XInput rumble commands are converted to physical motor vibration.
* Battery voltage monitoring through an analog input.
* LED-based battery level indicator.
* Automatic inactivity detection.
* Deep sleep to reduce power consumption.
* Button-controlled wake-up from deep sleep.

### Controls

| Control   | GPIO | Function                         |
| --------- | ---: | -------------------------------- |
| UP        |   D4 | D-Pad Up                         |
| DOWN      |   D5 | D-Pad Down                       |
| LEFT      |   D7 | D-Pad Left                       |
| RIGHT     |   D8 | D-Pad Right                      |
| A         |   D2 | Main action button / wake-up     |
| B         |   D3 | Secondary button / sleep control |
| SELECT    |   D9 | Select                           |
| START     |  D10 | Start                            |
| LED       |   D6 | Status and battery indicator     |
| Vibration |   D1 | Rumble motor                     |
| Battery   |   A0 | Battery voltage measurement      |

### Startup Sequence

When the controller starts or wakes from deep sleep:

1. The controller initializes the GPIOs and Bluetooth gamepad.
2. If it was woken from deep sleep, the controller performs a short vibration sequence.
3. The controller waits **1 second** after the wake-up vibration.
4. The battery voltage is measured.
5. The LED displays the battery level using a **1–5 flash sequence**.
6. After the battery indication finishes, the LED returns to normal status operation.

### Battery Indicator

The battery voltage is measured through the analog input using a **220 kΩ / 220 kΩ voltage divider**.

The battery level is converted into five levels:

* **5 flashes:** ≥ 4.00 V
* **4 flashes:** 3.95–3.99 V
* **3 flashes:** 3.80–3.94 V
* **2 flashes:** 3.60–3.79 V
* **1 flash:** < 3.60 V

Each battery indicator position lasts **400 ms**, with the LED on for **200 ms** during the active positions.

The same battery indication sequence is used both after startup/wake-up and before entering deep sleep.

### Normal LED Operation

Once the battery indication has finished:

* **Bluetooth connected:** LED remains continuously ON.
* **Bluetooth disconnected:** LED slowly blinks once per second.

The battery indicator temporarily takes control of the LED while its sequence is running.

### Sleep and Wake-Up

The **B button** is used to control deep sleep.

Holding B for approximately **3 seconds** triggers the sleep sequence:

1. The gamepad releases the B button.
2. The controller performs a **600 ms shutdown vibration**.
3. The user releases the B button.
4. After B is released, the controller waits **1 second**.
5. The battery voltage is measured.
6. The LED displays the current battery level using the same **1–5 flash sequence**.
7. The LED turns OFF.
8. The controller enters **ESP32 deep sleep**.

The B button is configured as a GPIO wake-up source, allowing the controller to wake from deep sleep when B is pressed.

### Automatic Sleep

The controller also monitors user activity.

* When connected via Bluetooth, it enters deep sleep after **120 seconds** of inactivity.
* When disconnected, it enters deep sleep after **60 seconds** of inactivity.

Before entering deep sleep, the same shutdown sequence is performed, including vibration and battery indication.

### Rumble

The controller supports XInput rumble.

When an XInput rumble command is received from the host:

* Strong or weak rumble activates the physical vibration motor.
* When both rumble values return to zero, the motor is switched off.

The physical motor is driven through the vibration output pin.

### Bluetooth

The gamepad identifies itself as:

**Amiga 500 XInput Series X**

with the controller manufacturer/name:

**Competition Pro**

The controller is configured to use:

**XInput Series X mode**

for compatibility with systems supporting Bluetooth XInput controllers.

## Bill of Materials

* SPEEDLINK Competition Pro Joystick
* Seeed Studio XIAO ESP32-C3
* KY-040 Rotary Encoder
* 10 cm USB-C to USB-C Extension Cable
* 8 mm LED Push Button, 3 V, Self-Reset, Normally Open (NO)
* LiPo Battery, 3.7 V
* Vibration Motor 9000RPM / Driver
* 28 AWG Hook-up / Connecting Wires
* Custom 3D-Printed Parts


### Pictures

<table>
  <tr>
    <td><img width="400" src="https://github.com/user-attachments/assets/082f6426-89ef-498d-8ae7-c9b4a183f82e" /></td>
    <td><img width="400" src="https://github.com/user-attachments/assets/76978373-48f3-4096-9aa6-d667eb835cab" /></td>
  </tr>
  <tr>
    <td><img width="400" src="https://github.com/user-attachments/assets/9e8ca3e9-e0a6-4c99-8783-9d77e417baad" /></td>
    <td><img width="400" src="https://github.com/user-attachments/assets/22634f96-4c74-4fe0-887e-13bf3a444879" /></td>
  </tr>
  <tr>
    <td><img width="400" src="https://github.com/user-attachments/assets/2ca05f13-e2d2-4223-a01a-58ab655ed596" /></td>
    <td><img width="400" src="https://github.com/user-attachments/assets/6a4347bf-23e0-4ab1-a871-e7ee9942320a" /></td>
  </tr>
  <tr>
    <td><img width="400" src="https://github.com/user-attachments/assets/a0c2a6bd-45b3-44b8-9a64-89e3eea48470" /></td>
    <td><img width="400" src="https://github.com/user-attachments/assets/5b598a5f-2872-4c31-82d4-2936e1289fe0" /></td>   
  </tr>
</table>


