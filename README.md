# **Arduino-Aime-Reader**
**All the main features are implemented now, so if there are no more bugs there should be no more updates.**
中文: [Sucareto/Arduino-Aime-Reader](https://github.com/Sucareto/Arduino-Aime-Reader)

Aime compatible card reader made using Arduino + PN532 + WS2812B.
Supported card types: [FeliCa](https://en.wikipedia.org/wiki/FeliCa) (Amusement IC, Suica, Octopus, etc.) and [MIFARE](https://en.wikipedia.org/wiki/MIFARE) (Aime, Banapassport).
The implementation logic is the official card reader serial port data comparison + brain compensation, the correct implementation is not guaranteed.
TThe communication data format is referenced from [Segatools](https://github.com/rakisaionji/segatools) and the official card reader packet capture data, which can be viewed at [example.txt](doc/example.txt) and [nfc.txt](doc/nfc.txt).
An example on how one could use this: [ESP32-CardReader](https://github.com/Sucareto/ESP32-CardReader)


### **How To Use:**
1. Follow the prompts in [PN532](https://github.com/elechouse/PN532) to install the library.
2. Connect the cable(s) (I2C or SPI) between Arduino and PN532 according to your usage, and adjust the dip switch on the PN532.
3. Connect the WS2812B light bar (optional)
4. Upload [ReaderTest](tools/ReaderTest/ReaderTest.ino) to test if the hardware is working correctly or not.
5. If the card reading is normal, you can open the device manager and set the COM port number according to the support list
6. Set the `high_baudrate` option of the code according to the baud rate of the game, `115200` is `true` and `38400` is `false`
7. Upload the program and open the game to test
8. Install [MifareClassicTool](https://github.com/ikarus23/MifareClassicTool), modify [Aime card example](doc/aime_example.mct) and write to a blank MIFARE UID/CUID card.

Some Arduinos may need to send DTR/RTS to the serial port at the correct baud rate before the main game is connected. You may need to open the Arduino serial monitor once before starting the main program.
If the game is SDBT, you can run [DTR-RTS.exe](tools/DTR-RTS.exe) once before starting to send DTR/RTS to COM1 and COM12
If you need to send to other ports and specific baud rates, you can modify [DTR-RTS.c](tools/DTR-RTS.c) then compile.


### **Support List:**
| Game ID | COM Port Number | Supported Cards | Default Baud Rate |
| - | - | - | - |
| SDDT/SDEZ | COM1 | FeliCa,MIFARE | 115200 |
| SDEY | COM2 | MIFARE | 38400 |
| SDHD | COM4 | FeliCa,MIFARE | cvt=38400,sp=115200 |
| SBZV/SDDF | COM10 | FeliCa,MIFARE | 38400 |
| SDBT | COM12 | FeliCa,MIFARE | 38400 |

- If the card reader is not working properly, you can switch the baud rate to try and fix it.
- If you are using amdaemon, you can refer to aime > unit > port in config_common.json to confirm the port number.
- If `"high_baudrate": true`, then the baud rate is `115200`, otherwise it is `38400`.


### **Tested Development Boards:**
- SparkFun Pro Micro (ATmega32u4), need to send DTR/RTS
- SparkFun SAMD21 Dev Breakout（ATSAMD21G18）
- NodeMCU 1.0（ESP-12E + CP2102 & CH340), SDA=D2, SCL=D1
- NodeMCU-32S（ESP32-S + CH340）

### **Known Issues:**
- The write Felica operation of the NDA_08 command was not implemented because it was not confirmed whether it would affect the subsequent use of the card.
- The meaning of `res.status` is not determined, so `res.status = 1; ` may be wrong.
- `mifare_select_tag` is not implemented, multi-card selection is not supported, only the first recognized card will be read.


### **Reference Library:**
- Driver WS2812B: [FastLED](https://github.com/FastLED/FastLE )
- Driver PN532: [PN532](https://github.com/elechouse/PN532)
- Reference for reading FeliCa data: [How to read FeliCa student ID card with Arduino using PN532](https://qiita.com/gpioblink/items/91597a5275862f7ffb3c)
- Program for reading FeliCa data: [NFC TagInfo](https://play.google.com/store/apps/details?id=at.mroland.android.apps.nfctaginfo), [NFC TagInfo by NXP](https://play.google.com/store/apps/details?id=com.nxp.taginfolite)
