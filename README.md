# seeedstudio-mr24hpc1-mmwave-kit
SeeedStudio mmWave Human Detection Sensor Kit - Simplified HomeAssistant Integration and Binary_Sensor

This project is a simple Home Assistant / ESPHome integration using the example code from https://github.com/limengdu

![ESP Capive Portal](/static/images/ESP%20Captive%20Portal.png)

Find the original project here:
https://github.com/limengdu/MR24HPC1_HomeAssistant/tree/mmwave-kit

I've done very little other than remove extranious code not useful for my purposes and added, quite messy in fact, a simple binary_sensor for human presense

Here are the primary Human Presence sensors in their basic states:

Nobody... you'll see the binary_sensor Presense is now OFF

![ESPHome HA Integration](/static/images/Nobody%20-%20Binary_Sensor%3DOFF.png)

Somebody... moving... you'll see the binary_sensor Presense is now ON

![ESPHome HA Integration](/static/images/Someone%2BActive%20-%20Binary_Sensor%3DON.png)

Somebody... but not moving... you'll see the binary_sensor Presense REMAINS ON

![ESPHome HA Integration](/static/images/Someone%2BMotionless%20-%20Binary_Sensor%3DON.png)


# Installation:
 * Download the C++ header file and copy it (keeping the subfolder paths) into your Home Assistant config/esphome main folder:

   ```
   header/seeedstudio-mmwave-kit.h
   
   ```
 
 * In Home Assistant add-on, click ESPHome>open web gui and create a new device choosing the "continue" option and give it a name such as:

   ```
   seeedstudio-mmwave-kit
   
   ```

* Click next and chose the type of ESP module you used in your build, this isn't a critical thing to have match but as long as it's some kind of ESP32 you can just select that for now and click next.
* You'll see a new card appear in the background for your ESP device, this is just an empty shell with only basic initialization code so far... click skip because you don't want to install this basic code to the ESP quite yet.
* Now click edit on your new sensor in ESPHome and you'll see the basic code:
   ```
   esphome:
    name: seeedstudio-mmwave-kit

   esp32:
    board: esp32dev
    framework:
      type: arduino

   # Enable logging
   logger:

   # Enable Home Assistant API
   api:
     encryption:
       key: "your-key-here"

   ota:
     password: "your-password-here"

   wifi:
    ssid: !secret wifi_ssid
    password: !secret wifi_password

     # Enable fallback hotspot (captive portal) in case wifi connection fails
    ap:
      ssid: "seeedstudio-mmwave-kit"
     password: "your wifi password here"

   captive_portal:
   ```

* The easiest way to proceed is to copy all the code above out to notepad++ or your favorite editor and then paste back in the entire code from:
   ```
   seeedstudio-mmwave-kit.yaml
   ```
* Now just edit some key lines in your new config:

   ```
    substitutions:
      # change device name to match your desired name
      device_name: "seeedstudio-mmwave-kit"
      # change sensor name below to the one you want to see in Home Assistant
      device_name_pretty: SeeedStudio mmWave Human Detection Sensor Kit
      # change room name below to the one you want to see in Home Assistant
      room: "Home"
      # change the below to be your WiFi SSID
      ssid: !secret wifi_ssid
      # change the below to be your WiFi password
      wifi_password: !secret wifi_password
      
   ```
* The room: "name" is key as it will be the name of each sensor object in HA so if you chose "Office" here, you sensors will be Office Motion, Office Presence, etc...

* You'll also want to copy the generated api encryption key and ota password into this section of the full code:

   ```
   # Enable Home Assistant API
   api:
      encryption:
         key: "assigned_when_you_add_to_esphome"

   ota:
      password: "assigned_when_you_add_to_esphome"
   ```

* Lastly, in the wifi: section, there is a line that says "use_address: XXX.XXX.XXX.XXX" this is an optional element to workaround typical issues with mDNS and wifi/subnets. Basically, if you find your device is always showing as offline in ESPHome or has any issues at all when making changes or updating OTA, you'll want this setting. Note: this is not a static IP, it just tells ESPHome to use an IP address to do all OTA work with a given ESP device. You can of course set a static IP but this isn't that. You'll also notice that the wifi ssid and passwords are variables, you'll also see at the top of the full config are references to "secrets" file. You'll need to create one or update the one in this repo and copy it to the config/esphome main folder... if you have setup other esp devices, changes are you have this already.

   ```
   # Connect to WiFi & create captive portal and web server
   wifi:
      ssid: "${ssid}"
      password: "${wifi_password}"
      use_address: XXX.XXX.XXX.XXX
   ```
