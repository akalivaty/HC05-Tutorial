<div align="center">
    <img src="./img/front.jpg" width="300"/>
    <img src="./img/back.jpg" width="300"/>
</div>
<br/>

A monitor application is required for inputting AT commands if you want to use PlatformIO. Or Arduino IDE is more easier.

## Some things you need to know in advance

1. HC-05 is kind of different from HC-06, you need to check the module you are using
2. By default, the baud rate of HC-05 is 38400 and the password is 1234
3. There is a button beside EN pin (maybe not, you need to google some related discussions)
4. Voltage division (5V -> 3.3V)* on RX pin and TX pin on HC-05 is optional, if not, it may destroy the module, but I have never tried it
5. `AT+NAME?` is not working (at least the 2 HC-05 modules I have don't work)
6. It will not display the new HC-05 name immediately after you change it, you need to reconnect it or re-pair it some times
7. If you can't find the HC-05 by your pc or phone and you don't know what happened to it, try to reset the module with the AT command `AT+ORGL`

*The voltage from Arduino digital pin is 5V

## AT Command

Here shows 2 preparations to enter AT command mode.

### Method 1

Keep Arduino board clear like below, or you can wire **RESET** pin to GND on Arduino board. And the baud rate of serial monitor is 38400.

```
void setup(){}
void loop(){}
```

**Wiring**

|Arduino|HC-05|
|:----:|:----:|
|5V|VCC**|
|GND|GND|
|D1 (TX)|TX|
|D0 (RX)|RX|

**EN** and **STATE** keep floating

### Method 2

Upload the attached Arduino code. Notice that the baud rate of serial monitor should be 115200. You can modify all baud rate to 38400 if you want. Here I want to show the difference between monitor serial and HC-05 serial.

```
Serial.begin(38400);
BTserial.begin(38400);
```

**Wiring**

|Arduino|HC-05|
|:----:|:----:|
|5V|VCC**|
|GND|GND|
|D2|TX|
|D3|RX|

**EN** and **STATE** keep floating

### Steps for AT command mode

1. VCC** on HC-05 keeps floating in advance
2. Keep the button on the HC-05 pressed and connect VCC to 5V
3. Release the button when LED on HC-05 blinks slowly, indicating that it is in AT command mode
4. (**Important**) Check the baud rate and the output setting, both NL and CR are required (if not, you wouldn't see the "OK" response)
5. Input `AT` and expect "ok" response, if not, check the wiring and monitor setting
6. [This document***](https://s3-sa-east-1.amazonaws.com/robocore-lojavirtual/709/HC-05_ATCommandSet.pdf) shows the all AT commands for HC-05, you can have a look
7. Disconnect VCC pin on HC-05 and connect it again to exit AT command mode

Tip: Try following command if you think you're messing up the module, it will reset the module to factory settings

```
AT+ORGL
```

[***Document Backup](./HC-05_ATCommandSet.pdf)
