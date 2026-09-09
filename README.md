# XIAO ESP32C3 mmWave Motion Sensor
## For use with ESPHome using the Hi-Link LD2410C mmWave motion sensor, includes YAML for enabling Engineering Mode and wiring instructions.
<img width="800" height="800" alt="esp32c6 pin layout" src="https://github.com/user-attachments/assets/d4cbb920-8687-469f-b6a4-5eaacbc53eb6" />

<img width="500" height="500" alt="ld2410c" src="https://github.com/user-attachments/assets/2c20171d-88be-4470-a118-fbb26fbcd7f1" />

## Wiring

- LD2410 TX → Xiao GPIO20 (RX / D7)
- LD2410 RX → Xiao GPIO21 (TX / D6)
- LD2410 VCC → 5V or 3.3V
- LD2410 GND → GND
>[!NOTE]
>This can also be used with XIAO ESP32C6, just be sure to define the board and select the correct variant in your YAML script.

#
<a name="YAML"></a>
## YAML
```
# Board: Seeed XIAO ESP32C3 (Seeed Studio)
# Definition: definitions/boards/seeed-xiao-esp32c3/manifest.yaml # define your board

esphome:
  name: # your-sensor-name
  friendly_name: # Your Sensor Name

esp32:
  variant: esp32c3 # choose your variant
  flash_size: 4MB
  framework:
    type: esp-idf

logger:
  level: WARN
  baud_rate: 0

api:
  encryption:
    key:

ota:
  - platform: esphome

wifi:
  ssid: # ADD YOUR WIFI HERE
  password: # ADD YOUR WIFI PASSWORD HERE
  ap:
    ssid: 
    password:

captive_portal:

uart:
  id: ld2410_uart
  tx_pin: GPIO21    # Xiao TX → LD2410 RX
  rx_pin: GPIO20    # Xiao RX → LD2410 TX
  baud_rate: 256000
  parity: NONE
  stop_bits: 1
  rx_buffer_size: 1024

ld2410:
  uart_id: ld2410_uart

switch:
  - platform: ld2410
    engineering_mode:
      name: Engineering Mode
sensor:
  - platform: ld2410
    light:
      name: "Light Sensor"
      id: light_sensor # must use underscores

  - moving_distance:
      name: "Distance Sensor"
      id: distance_sensor

binary_sensor:
  - platform: ld2410
    has_target:
      name: "Presence"
    has_moving_target:
      name: "Moving Target"
    has_still_target:
      name: "Still Target"
```
### Example Build

<img width="711" height="834" alt="IMG_E0691" src="https://github.com/user-attachments/assets/dd50ee41-ebaa-4a0c-ac09-1fc291ddda56" />

##
### Set up the LD2410C mmWave Sensor

There's several ways to do this, including using a serial adapter to communicate with the sensor. I think the phone app is the best method though.

<img width="1170" height="458" alt="IMG_0531" src="https://github.com/user-attachments/assets/d759610f-be67-42f1-b859-994e2d4662a2" />

Download the app on [Google Play](https://play.google.com/store/apps/details?id=com.hlk.hlkradartool&pcampaignid=web_share)

Or the [App Store](https://apps.apple.com/us/app/hlkradartool/id1638651152)

Rename the sensor to the room you're using it in, and set a baseline for an empty room by selecting "Engineering Mode" and then press the "Auto" to set the levels of the room. The sensor has 8 gates for both Moving and Stationary Targets. Below those you will find the Detection Range [(called Distance in the YAML)](#YAML) and the Photo sensitivity [(called Light in the YAML)](#YAML). There's a lot of other things you can do like limit the range of the sensor or how much distance is in between is measured between each gate in "Settings".
