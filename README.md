# water-meter-house-automation
Software for a water meter measurement to provide an JPG-Image of a watermeter with an OV2640 camera and a WS2812-LED illumination

## Version
##### 1.0.0 Initial Version for ESP8266 (with ArduCAM) and ESP32-CAM (with OV2640)
##### 2.0.0 Usage of internal ESP32-CAM Flash LED
* Attention @ ESP32-CAM: compared to v1.0.0 the connection to the LED-strip has been changed from GPIO4 to GPIO2 
##### 2.1.0 Parametrized Output (quality, size) (ESP32-CAM only)
* Introduction of parameter for adjustable resolution and jpeg quality
* V2.1.1 ERROR-CORRECTION: setting of default quality was faulty

There are two code versions available:

#### Main Version for NodeMCU
* [ArduCAM_Server-NodeMCU-OTA_GitHub](ArduCAM_Server-NodeMCU-OTA_GitHub)
* The main code - tested and working with NodeMCU hardware and fitting to the wiring below.

#### ESP32-CAM Version
* [ESP32-CAM_Server-GitHub](ESP32-CAM_Server-GitHub)
* Flashing onto the board is done using an FTDI-Flasher and the board is addressed as "ESP32-Wrover Module"

## Functionality

This is an HTTP-Webserver, sending an image of the OV2640 camera on request and additionally switching on illumination.
The code is implemented in the Arduino IDE and is directly related to an selfmade water meter for house automation. 
For the housing etc. see  [https://www.thingiverse.com/thing:3238162](https://www.thingiverse.com/thing:3238162)

An overview about the complete system can be seen here: [https://github.com/jomjol/water-meter-measurement-system](https://github.com/jomjol/water-meter-measurement-system)

### Commands
- http://IP-ADRESS/lighton   -   Switching the LED-lights on
- http://IP-ADRESS/lightoff - Switching the LED-lights off
- http://IP-ADRESS/capture - Returns a single image
- http://IP-ADRESS/capture_with_light - Turn light on, send JPG, Turn light off

Usefull for ESP32-CAM with onboard flash LED only:

- http://IP-ADRESS/flashon   -   Switching the onboard LED-flash on (GPIO4)
- http://IP-ADRESS/flashoff - Switching the onboard LED-flash off (GPIO4)
- http://IP-ADRESS/capture_with_flashlight - Turn internal flash light on, send JPG, Turn flash light off
- to be extended in future versions ...

For ESP32-CAM the output picture size and quality can be adjusted by adding the following parameters:
- http://IP-ADRESS/capture?quality=10&size=UXGA - Picture with high quality and resolution 1600x1200 pixel
- http://IP-ADRESS/capture_with_flashlight?quality=10&size=UXGA - Picture with high quality and resolution 1600x1200 pixel

Meaning of parametes:

| Parameter | Meaning | Range/Value | Example |
|:---------|:-------|:-----|:--------|
| quality  | Quality setting for jpeg | 0 - 63 (0 = highest quality; 63 = lowest quality, default = 10 | quality=10 |
| size | JPEG resolution of out put | QVGA (320x240), VGA (640x480), SVGA (800x600), XGA (1024x768), SXGA (1280x1024), UXGA (1600x1200), default = SVGA | size=SVGA |




### Compling Code
The Arduino Code is in the subdirectory "ArduCAM_Server-NodeMCU-OTA_GitHub" for the NodeMCU version and "" for the ESP32-CAM version. In order to compile the ESP8266 for NodeMCU, the ESP32 developement environment for ESP32-CAM and the Neopixel Library (for both versions) needs to be installed through the Arduino Library Controll.
The WLAN-SSID and PASSWORD needs to be updated in the code, in order to access the local network.

### Code structure - brief overview
The code is implemented in C++, using different classes.
#### JomjolGitServerClass
- Basic class to implement HTTP-server
#### JomjolGitArduCAMComm / JomjolGitESP32CamComm
* Class to communicate with ArduCAM/ESP32-CAM and LEDs
#### JomjolGitArduCAM-Server-Class
- Inherited class of Server and ArduCAM/ESP32-CAM to combine physical camera controll and HTTP-server
#### ArduCAM_Server-NodeMCU-OTA_GitHub.ino / ESP32-CAM_Server-GitHub.ino
* Arduino file to setup and loop the programm. For NodeMCU version additianlly implementin an OTA-interface to update the NodeMCU over-the-air



# Physical Setup / Wiring / Components

## Node Mini D1 /ArduCam OV2640

### Components
- ArduCAM OV2640 with JPG-Output
- Node Mini D1 with ESP8266
- LED-Strip with WS2812b-Controller
- Capacity 1000uF
- Resistor 470 Ohm

### Wiring
<img src="./images/wiring_sketch_update.png" width="800">
<img src="./images/wiring_drawing.png" width="800">

## ESP32-CAM

### Components
- ESP32-CAM mit OV2640
- LED-Strip with WS2812b-Controller
- Capacity 1000uF
- Resistor 470 Ohm

### Wiring including LED-Strip
<img src="./images/ESP32-CAM_Wiring_neu.png" width="800">

### Wiring for usage internal ESP32-CAM flash LED only
<img src="./images/ESP32-CAM_Wiring_Flash_only.png" width="300">
