# co2ampel

![](https://git.unhb.de/smash/co2ampel/raw/branch/master/IMG_20200831_221038.jpg)

## idea
higher concentrations of CO2 inside will make you sleepy and have an impact on your wellbeing. during the current pandemic higher CO2 concentrations inside are also a pretty good indication for higher levels of aerosol and as such a higher risk for infection. the 'CO2 Ampel' (CO2 traffic light) will give you a visual representation of the current situation and remind you to open windows to keep you happy and healthy

## parts list

| description | part | link |
| -------------------- | ----------- | ------------------------------------------- |
| ESP32 | ESP32 | https://de.aliexpress.com/item/32864722159.html?spm=a2g0o.productlist.0.0.6e7e745aBnlwmM&algo_pvid=b9be1fdc-113a-4e2e-aafa-4e08151af66c&algo_expid=b9be1fdc-113a-4e2e-aafa-4e08151af66c-1&btsid=0ab50f6215990625361401073eaad4&ws_ab_test=searchweb0_0,searchweb201602_,searchweb201603_ |
| Infrared CO2 Sensor | mhz-19 | https://de.aliexpress.com/i/32952229446.html |
| Active piezzo buzzer | KY-012 | https://de.aliexpress.com/item/32740686896.html?spm=a2g0o.productlist.0.0.554c8f02ZzXw5y&algo_pvid=a49664ca-1685-4d9a-b7ba-45c1fbf5a7ba&algo_expid=a49664ca-1685-4d9a-b7ba-45c1fbf5a7ba-0&btsid=0ab6f83115990618356642096e52ad&ws_ab_test=searchweb0_0,searchweb201602_,searchweb201603_ |
| OLED Display | SSD1306 |  |
| LED-Ring | WS2812 | https://de.aliexpress.com/item/32835427711.html?spm=a2g0o.productlist.0.0.847c3d32JLroFX&algo_pvid=32c7c01b-df4b-430e-b550-00097f15afde&algo_expid=32c7c01b-df4b-430e-b550-00097f15afde-0&btsid=0ab6f83115990618906573133e52ad&ws_ab_test=searchweb0_0,searchweb201602_,searchweb201603_ |

## build

just solder everything according to the connection schema.

### schema

| ESP32 PIN     | peripherals       | note                           |
| ------------- | ----------------- | ------------------------------ |
| VIN           | mhz-19 (VCC)      |                                |
| GND           | mhz-19 (GND)      |                                |
| GND           | KY-012 (GND)      | passive buzzer will do as well |
| D12           | KY-012 (Signal/+) |                                |
| 3V3           | SSD1306 (VCC)     |                                |
| 3V3           | WS2812 (VCC)      |                                |
| GND           | WS2812 (GND)      |                                |
| GND           | SSD1306 (GND)     |                                |
| D4            | WS2812 (DI)       |                                |
| RX2 / D16     | mhz-19 (TX)       |                                | 
| TX2 / D17     | mhz-19 (RX)       |                                |
| I2C SDA / D21 | SSD1306 (SDA)     |                                |
| I2C SCL / D22 | SSD1306 (SCL)     |                                |

### ws2812
if your ws2812 don't work on 3.3V power (most will do) you can try to use 5V instead. there are plenty different ws2812 builds so ymmv... if the leds don't work with 5V try to add a pullup resistor to your data line and a diode towards D4 to raise your high level while protecting your GPIO (https://forum.arduino.cc/index.php?topic=578735.msg3941756#msg3941756)
looks like this:
![](https://git.unhb.de/smash/co2ampel/raw/branch/master/levelshifter.png)

either way it might be a good idea to have a capacitor between VCC and GND. my prototype worked without any of these, but from time to time some LEDs just randomly turn on...





## readings for inside rooms


| CO2 measure | light  | buzzer | meaning                                                                           |
| ----------- | ------ | ------ | --------------------------------------------------------------------------------- |
| 400 ppm     | none   | no     | more or less the baseline of the sensor hardware and means 'fresh air quality'    |
| 400-800ppm  | none   | no     | safe zone and can be considered good air quality                                  |
| 800-1500ppm | yellow | no     | warning zone (consider to open windows) and is between medium and bad air quality |
| > 1500ppm   | red    | yes       | bad air quality, ppl in the room will be affected in a negative way               |

 


## todo
 * cooldown time for buzzer (alarm only once every n minutes) -> lambda/interval/on_time?
 * add openings for mh-z19 to the case
 * adjust light tunnel to use frosted acrylic glass
 * add a silent switch for meeting rooms and such (no buzzer, but flash at a regular interval to attract attention)