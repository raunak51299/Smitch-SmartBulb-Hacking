# Smitch Smart Bulb Hacking Guide

The company behind the Smitch smart bulb has gone out of business, which caused the bulb to stop functioning through its official app. This guide explains how to hack the Smitch smart bulb, which is based on the TYWE3L module, and flash WLED firmware to regain control of the RGB+CCT LEDs. One can also use other binaries like tasmota using this guide.

---

## Components Required

1. Smitch Smart Bulb (TYWE3L module-based). [mine](https://templates.blakadder.com/smitch_SB161001-B22.html)
2. USB to TTL adapter (e.g., FTDI adapter) or as I have used the ESP32-WROOM-32 for serial flashing.
3. Soldering tools and jumper wires
4. Computer with a flashing tool (e.g., `esptool.py`) or web installer

---

## Pins

| GPIO Pin | Function                                    |
| -------- | ------------------------------------------- |
| GPIO04   | PWM1 - Red                                  |
| GPIO12   | PWM2 - Green                                |
| GPIO14   | PWM3 - Blue                                 |
| GPIO05   | PWM4 - Warm White                           |
| GPIO13   | PWM5 - Cold White                           |
| GPIO00   | Flash Mode (connect to GND during flashing) |

![image](https://github.com/user-attachments/assets/8c298bfa-4df6-4619-8b88-a9c217428250)

![temp_image_20250412_000703_1ae801d6-9aca-4558-8d0f-3d33fd5b1823](https://github.com/user-attachments/assets/2de4e5a5-0510-4357-820e-92417bc3b2e7)

![image](https://github.com/user-attachments/assets/72dbd71f-cc15-4369-ab7b-230ce75c2e49)


---

## Steps to Hack the Bulb

### 1. Open the Bulb

Carefully open the Smitch smart bulb to expose the PCB. Identify the TYWE3L module and locate its pins as shown in the schematic above.

### 2. Solder Wires

Solder wires to the following pins on the TYWE3L module:

- **TX**
- **RX**
- **VCC (3.3V)**
- **GND**
- **GPIO00 -> gnd** (for enabling flash mode)

Refer to this images for soldering pins.

![undefined - Imgur](https://github.com/user-attachments/assets/c6386fd0-6396-46fd-8bc2-05c1d284b78a)

### 3. Prepare for Flashing

1. Connect the wires to a USB to TTL adapter as follows:

   - **TX** -> **RX** on the adapter
   - **RX** -> **TX** on the adapter
   - **VCC** -> **3.3V** on the adapter
   - **GND** -> **GND** on the adapter
   - **GPIO00** -> **GND** (for flash mode)

OR in my case due to unavailibility of adapter I used my nodemcu esp32-wroom-32 microcontroller instead
remember to connect the en pin to gnd to disable the onboard chip and to connect  **TX** -> **TX** and  **RX** -> **RX**.
![WhatsApp Image 2025-01-01 at 09 19 31_2fd78def](https://github.com/user-attachments/assets/98a41cf0-9d3e-44c4-a86a-64d1e2da38e4)



### 4. Flash WLED Firmware

1. Use the [WLED Web Installer](https://install.wled.me/) for an easier flashing process.

2. Follow the on-screen instructions to select your device and flash [the firmware.](https://github.com/Aircoookie/WLED)

3. If you prefer manual flashing, download the latest WLED firmware binary and use a tool like `esptool.py.`

4. Install the Python tool `esptool.py` by running:

   ```bash
   pip install esptool
   ```

5. Erase the existing firmware:

   ```bash
   esptool.py --port <PORT> erase_flash
   ```

6. Flash the WLED firmware:

   ```bash
   esptool.py --port <PORT> --baud 115200 write_flash -fm dout 0x0 <WLED_BINARY>
   ```

   Replace `<PORT>` with your USB port (e.g., `/dev/ttyUSB0` or `COM3`) and `<WLED_BINARY>` with the path to the WLED binary file.

7. Disconnect **GPIO00** from GND and restart the bulb.

### 5. Configure WLED

1. Connect to the WLED access point (SSID: `WLED-AP`). (password: wled1234)
2. Open the WLED web interface by navigating to `http://4.3.2.1`.
3. Set up Wi-Fi credentials to connect the bulb to your network.
4. Configure the GPIOs for LED control:
   - Open the "LED Preferences" section.
   - Set the following:
     - **GPIO04** -> PWM1 (Red)
     - **GPIO12** -> PWM2 (Green)
     - **GPIO14** -> PWM3 (Blue)
     - **GPIO05** -> PWM4 (Warm White)
     - **GPIO13** -> PWM5 (Cold White)

![image](https://github.com/user-attachments/assets/b9d26308-e3e6-41e6-9f4f-c0d98e66ed53)

---

## Notes

1. Always ensure the power supply is 3.3V for the TYWE3L module.
2. Do not connect to AC mains while the bulb is opened; use the 3.3V pin to power it instead.

---

## Resources

- [Tasmota Getting Started Guide](https://tasmota.github.io/docs/Getting-Started/)
- [OpenHAB Community Discussion](https://community.openhab.org/t/noob-new-help-add-unlisted-smitch-wifi-lights-esp8266-google-alexa/96119/16)
- [WLED](https://kno.wled.ge/)

---

Happy Hacking!

![bmc_qr](https://github.com/user-attachments/assets/ff61b58f-3a43-465c-a755-916d5eb59542)

