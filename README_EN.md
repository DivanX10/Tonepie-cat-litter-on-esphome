# Tonepie Cat Litter Upgrade
Project for ESPHome and Home Assistant


<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/3ccaa24d-f8f3-4367-8f9f-fe86b93e63e3" width=50%>


### Description
The Tonepie cat litter runs on Tuya, which can be upgraded to ESPHome. We decouple the tray from the Tuya cloud service and can program our tray as we see fit

> Attention! All materials of this project (firmware, circuit diagrams, etc.) are provided "AS IS". Anything you do with your equipment is done at your own risk. The author is not responsible for the result and does not guarantee anything. Re-flashing the cat litter box requires intervention, which will automatically void your warranty.


> Important! Pour the litter into the switched off tray. After power is applied, the automatic tray performs auto-calibration and resets the weight of the filler to zero, and only after that the tray will correctly determine the weight of the pet. If you pour litter into the tray when it is turned on, the tray will think that there is a pet inside. In this case, turn off the power to the tray and reapply power. For example, connect it to a smart socket, then it will be convenient to restart the tray based on power. This way the tray will start auto-calibration and reset the filler weight to zero.

**Tray functions:**
1) Automatic cleaning
2) Manual cleaning
3) Ionizer
4) Child protection
5) Infrared sensor
6) Removing filler
7) Calibration of scales for filler and waste tank
8) Auto calibration when turned on
9) Sensors:
     * pet weight
     * duration of visit
     * trash can
     * pet in a tray
     * cleaning


<details>
  <summary><b>Flashing to ESPHome</b></summary>

The board has a WBR3 chip installed. You can find out more about WBR3 [here](https://developer.tuya.com/en/docs/iot/wbr3-module-datasheet?id=K9dujs2k5nriy)

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/13c54298-f1b6-4954-8e76-75da6ae8de8b" width=70%>


Before unsoldering the WBR3 chip, just in case, solder two wires to the RXD and TXD pins and take logs, see if your sensors will work when adding the [Tuya MCU](https://esphome.io/components/tuya.html#tuya-mcu) component. If the sensors work, you can continue the procedure further.

> For reference. Usually, in order to remove logs by connecting to the RXD and TXD contacts, the connection is made in reverse (screenshot below), but to my surprise the connection was direct, i.e. not RXD>TXD and TXD>RXD, but RXD>RXD and TXD>TXD. So check both options
>
> <img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/4e467b16-b9f6-4b38-98c2-c75933cf316b" width=30%>


<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/00c2be8c-6beb-4407-bfe3-4b0f76afba76" width=50%>

***

To enable debug mode and output packages to the logs, you need to add the following to the lines. This way you can see the packets for each command when you click on the buttons in the Tuya application or through the control panel of the tray itself
```
#Enable Tuya MCU component
tuya:

uart:
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600
  stop_bits: 1
  data_bits: 8
  parity: none
  debug:
    direction: BOTH
    dummy_receiver: false
```

***

ESP12-F will be used instead of WBR3. They say that WBR3 can be reflashed, but I have no experience and do not want to erase the firmware in WBR3, since the chip itself may be useful in the future, for example, soldering it back and taking logs. Do it at your own discretion, you can upload the firmware directly to WBR3 or replace it with ESP12-F.

There are two ways to upload firmware to the ESP12-F

1) Buy a programmer for the ESP8266 module

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/95595588-8338-466d-8a28-cb83634944c6" width=50%>


2) Connect ESP12-F to USB-TTL

<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/a315b484-f4d5-4825-87fc-ad5907123a52" width=100%>


> For reference! To upload the firmware, you need to close the GPIO0, GPIO15 and GND contacts before power is applied (before you insert the USB-TTL into the computer's USB connector), and not after, then the ESP12-F will go into firmware mode

Compile the firmware in ESPHome using the configuration of your choice. View configurations [here](https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/tree/main/files/ESPHome/en)

1) Basic configuration has only control and sensors without control logic
2) The advanced configuration has control logic and statuses, and can also have a cleaning schedule. See comments in the configuration.

I uploaded the firmware to the ESP12-F via NodeMCU Flasher. You can download NodeMCU Flasher [here](https://github.com/nodemcu/nodemcu-flasher/tree/master). 

After uploading the firmware, solder ESP12-F instead of WBR3 and close the contacts with 10 kOhm resistors. Solder a resistor to pins EN and 3.3v, GPIO15 and GND. Why didn't I solder a jumper shorting GPIO15 and GND? Having measured the multimeters, I saw a resistance of 326-327 kOhm, and since the ESP12-F chip was already soldered, and there was no free one on hand, it was not possible to check the GPIO15 and GND contacts on the chip and on the tray board. Therefore, I did not take any risks and, in order to avoid a short circuit, I closed GPIO15 and GND with a resistor.

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/c0acb144-cc21-4b69-9fdf-5c48d39733d3" width=50%>


</details>


<details>
  <summary><b>Photos</b></summary>


<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/36d9dad1-bbee-40a6-aa9d-7b146dd29f74" width=70%>
<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/43f824c1-45f6-4dd4-b888-1458b9763750" width=70%>
<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/69f1177d-d231-48a8-b179-60d68354fe74" width=70%>
<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/dabf01f8-466f-4d3c-899f-fc6344673bc6" width=70%>
  
</details>

<details>
  <summary><b>Home Assistant</b></summary>
  

<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/41bc0602-1d91-4eae-8f21-875333b625ee" width=100%>

**For the card to work, you need to install components**
* [History explorer card](https://github.com/alexarch21/history-explorer-card)
* [Button Card](https://github.com/custom-cards/button-card)

**Card**
* You can get the card code [here](https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/tree/main/files/ESPHome/en)
  

Countdown timer code. This is needed for the card to see the remaining operating time of the ionizer
```
timer:
  cat_toilet_ionizer_timer:
    name: "Cat toilet: Ionizer. Timer"
    duration: "00:30:00"
    icon: mdi:creation

- sensor:
    - name: 'Cat toilet: Ionizer. Remaining Time'
      unique_id: cat toilet ionizer remaining time
      state: >
          {% set f = state_attr('timer.cat_toilet_ionizer_timer', 'finishes_at') %}
          {{ '00:00:00' if f == None else (as_datetime(f) - now()).total_seconds() | timestamp_custom('%H:%M:%S', false) }}
      icon: mdi:timer
```

</details>

<details>
  <summary><b>3D model</b></summary>


I designed a protective removable side, since I didn’t have one in the kit, and I didn’t want to buy it for crazy money from the manufacturer. As a result, I designed and printed a protective removable side. The protective removable side prevents the filler from getting inside the tank, i.e. when cats bury the filler, the filler without a side gets under the tank. You can buy a removable side from the manufacturer

You can download the model [here](https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/tree/main/files/3D%20Printer)
***

Protective removable rim for Tonepie automatic toilet tray

<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/c9bc4853-837a-440e-ab68-e0324e61e956" width=40%>

***

Protective removable side printed on a 3D printer

<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/f95de27e-227f-41ed-8495-18f336531e05" width=30%>
<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/f2202e57-0271-498c-9660-602d294095da" width=30%>


</details>
