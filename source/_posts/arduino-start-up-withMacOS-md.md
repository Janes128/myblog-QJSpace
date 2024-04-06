---
title: Arduino: Start Arduino in MacOS
date: 2024-04-06 23:50:15
updated:
tags: arduino
categories: LifeDIY
---

## Install Arduino IDE and test in MacOS

1. Go to Arduino Office Website: [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software), choosing the download option you need. Then, press the corresponding text button and download it.
2. Install the IDE: after installation, double chick the ".dmg" file. Then, pull the Arduino IDE icon to "Applications" to install the software.

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/61CADF38-458F-423A-98E5-1E25311F4B40/2E0970F3-AF0A-4B24-8377-854F5EE2030E_2/VbhUKPZnBPe8yUwoUuDrEpgLKTpcV8iqq3pNtQfdFqEz/Image.png)

3. Open the IDE from "Launchpad （啟動台）" and enjoy it!

### Install Rosetta 2

> Rosetta 2 enables a Mac with Apple silicon to use apps built for a Mac with an Intel processor.

> From: [https://support.apple.com/en-us/102527](https://support.apple.com/en-us/102527)

Because Arduino development software is built for use on *x86-64* processors. So, we need to install Rosetta for it.

How to install. In terminal, we just command:

```bash
softwareupdate --install-rosetta --agree-to-license
```

Done!

#### What if we don't install Rosetta

I tested that before for you guys! If we uploaded Arduino code without Rosetta, it would show the compiled error like below content:

```bash
fork/exec /Users/user/Library/Arduino15/packages/arduino/tools/avr-gcc/7.3.0-atmel3.6.1-arduino7/bin/avr-g++: bad CPU type in executable

Compilation error: fork/exec /Users/user/Library/Arduino15/packages/arduino/tools/avr-gcc/7.3.0-atmel3.6.1-arduino7/bin/avr-g++: bad CPU type in executable
```

---

## Test with Arduino

> Requirement Device:

1. > Arduino device, e.g. Arduino UNO R3 board
2. > Cable: type A to type B USB
3. > USB hub (All in one to Type C USB)
4. > MacBook, e.g. MacBook Air M1

Test Steps:

1. Connect the Arduino device with cable and USB hub, like picture below:

![Image](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/61CADF38-458F-423A-98E5-1E25311F4B40/280DEF2D-FBF6-4AA1-BD2C-FCE72104F10B_2/ZV63HSlkZzmur55hTK0SQRJSyYBaliqF1EmvQNoooUUz/Image.heic)

make sure Arduino is worked (yellow light keeps flashing)

2. Open the Arduino IDE, select the correct Board to connect to your device. After selecting, it will show up your type of Arduino, e.g. "Arduino UNO" for my case.

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/61CADF38-458F-423A-98E5-1E25311F4B40/B3279C3D-6198-4A3C-88B6-781B19309C1F_2/nbg7gsE9aI14QGzdCgS6WYxTll1OGBf2T6srN3UboA0z/Image.png)

3. Test with the example code: we will test the Arduino with code called "Blink", and this is a program make your Arduino LED blinking with delay time you set. We open the code from "File" > "Examples" > "01.Basics" > "Blink", and it will open a window for this code.

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/61CADF38-458F-423A-98E5-1E25311F4B40/8816B92D-F5CC-4325-8F72-9834EA8D8B18_2/NB4doo3duzVP9zW4bC9pGfbJCzA0KauSYWwsh31WcFgz/Image.png)

4. You can modify the delay time (I set 2 sec in this case), and then chick the "right arrow" button to upload the program into Arduino.

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/61CADF38-458F-423A-98E5-1E25311F4B40/9797497F-9A05-4781-B132-E90B8314DA2A_2/jWva5Sm1dkPcV8xyYByrxmoOAmYC2Vi2fRxx6dhWAD4z/Image.png)

5. If the LED on the Arduino turns on and off every two seconds, that is! Congratulations!

## Conclusion

This is my first time playing with Arduino on MacOS. Initially, I thought it would be difficult to use Arduino with MacBooks, but I was wrong. Setting it up is as easy as it is on Windows. Finally, I can create some interesting things with my MacBook at home!