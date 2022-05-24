# Arduino-Aime-Reader
Aime compatible card reader made using Arduino + PN532 + WS2812B.
Supported card types: [FeliCa](https://en.wikipedia.org/wiki/FeliCa) (Amusement IC, Suica, Octopus, etc.) and [MIFARE](https://en.wikipedia.org/wiki/MIFARE) (Aime, Banapassport).
The implementation logic is the official card reader serial port data comparison + brain compensation, and the correct implementation is not guaranteed.
The communication data format refers to [Segatools](https://github.com/rakisaionji/segatools) and the official card reader to capture the packet data, which can be found in [example.txt](doc/example.txt) and [nfc.txt](doc/nfc.txt) view.


### How to use：
1. According to [PN532](https://github.com/elechouse/PN532) Prompt to install the library
2. According to the usage method, connect the cable (I2C or SPI) between Arduino and PN532, and adjust the dip switch on PN532
3. Connect to WS2812B light bar (optional)
4. Upload [ReaderTest](tools/ReaderTest/ReaderTest.ino) to test whether the hardware is working properly
5. If the card reading is normal, you can open the device manager and set the COM port number according to the support list
6. Set the `high_baudrate` option of the code according to the baud rate of the game, `115200` is `true` and `38400` is `false`
7. Upload the program to open the game test
8. Install [MifareClassicTool](https://github.com/ikarus23/MifareClassicTool), modify [Aime card example](doc/aime_example.mct) then write to the blank MIFARE UID/CUID card

Some Arduinos may need to send DTR/RTS to the serial port at the correct baud rate before the game is connected. You need to open the Arduino serial monitor once before starting the main program.
If it is SDBT, you can run it once before starting [DTR-RTS.exe](tools/DTR-RTS.exe) to send DTR/RTS to COM1 and COM12.
If you need to send to other ports and specific baud rates, you can modify [DTR-RTS.c](tools/DTR-RTS.c) Then compile.


### Support list：
| Game code | COM port number | Supported cards | Default baud rate |
| - | - | - | - |
| SDDT/SDEZ | COM1 | FeliCa,MIFARE | 115200 |
| SDEY | COM2 | MIFARE | 38400 |
| SDHD | COM4 | FeliCa,MIFARE | cvt=38400,sp=115200 |
| SBZV/SDDF | COM10 | FeliCa,MIFARE | 38400 |
| SDBT | COM12 | FeliCa,MIFARE | 38400 |

-If the card reader is not working properly, you can switch the baud rate and try it
-If you use amdaemon, you can refer to config_common. Confirm the port number in aime > unit > port in json
-If `"high_baudrate": true`, the baud rate is `115200`, otherwise it is `38400`


### Tested development board：
-SparkFun Pro Micro (ATmega32u4), need to send DTR/RTS
- SparkFun SAMD21 Dev Breakout（ATSAMD21G18）
- NodeMCU 1.0（ESP-12E + CP2102 & CH340），SDA=D2，SCL=D1


### Known issues：
-The write Felica operation of the NDA_08 command was not implemented because it was not confirmed whether it would affect the subsequent use of the card.
-Undetermined `res.status`, `res.status=1;` may be wrong
-Because `get_fw` and `get_hw` return custom version numbers, amdaemon's firmware upgrade may be triggered at startup. You can rename or delete the aime_firm folder.
-`mifare_select_tag` is not implemented, multi-card selection is not supported, only the first recognized card will be read


### Reference library：
-Driver WS2812B: [FastLED](https://github.com/FastLED/FastLE )
-Drive PN532: [PN532](https://github.com/elechouse/PN532)
-Reference for reading FeliCa: [PN532 makes use of ArduinoFelFel FeliCa Student ID Reading method](https://qiita.com/gpioblink/items/91597a5275862f7ffb3c)
-Program for reading FeliCa data: [NFC TagInfo](https://play.google.com/store/apps/details?id=at.mroland.android.apps.nfctaginfo), [NFC TagInfo by NXP](https://play.google.com/store/apps/details?id=com.nxp.taginfolite)
