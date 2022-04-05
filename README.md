# China Unicom VN007/VN007+ stuff

## Android app 
[Unicom manager](/android)

## Firmware 

NOTE: The VN007+ firmware starts with X21G. 
      The VN007  firmware starts with X21 

Do not put VN007+ fw on a 007 and vice versa. 

[vn007](/fw/007)

[vn007plus](/fw/plus)

[vn007plus with SMS feature (experimental)](/fw/plus/X21G_1.12.5_IDU_1810_UN2020C_20220222_VN007_1.15UP_update.bin)

## Super user password 

The VN007 has a 'superuser' password (different from admin/admin) which enables extra features (IP Passthrough etc) 
First you need to enable ADB on the modem. Log into the web admin GUI (usually http://192.168.0.1) and press F12 in chrome to open up the developer console. 
Click on Network
Click on any of the http.cgi requests and click 'Payload' 
Make a note of the 'sessionId' 

Click the console tab and paste in the following:
```
$.post( 'http://192.168.0.1/cgi-bin/http.cgi' , '{"adbSwitch":1, "cmd": 237, "method":"POST", "language":"CN ", "sessionId": "XXXXXXXXXXXXXXXXXXXX"}' , function ( result ) { console .log(result) })
```  
Replace XXXXXX with your SessionID. !!!!
Press ENTER to send the command. 
A successful command will return: 

```'{success: true, cmd:237, message: ''} ```

[Download an ADB client:](https://www.xda-developers.com/install-adb-windows-macos-linux/)


```
adb connect 192.168.0.1:5555
adb shell
grep SYS_SENIOR_LOGIN_PWD /tmp/mdlcfg.sysconfig
```
This will echo a line such as: 

```EXPORT SYS_SENIOR_LOGIN_PWD="abc1234"```

Where abc1234 will be the root password for the device. You can go ahead and log in to the web gui (http://192.168.0.1) as root / abc1234 (replace with the output from the command!)


## The SYS_SUPER user 

Another user, as far as I can tell the only difference is this one lets you send AT commands direct to the modem. 
You have to set this password. As above in the ADB console type : 
```
mdlcfg -f SYS_WEB_SUPER_PWD_RULE="1"
mdlcfg -a SYS_WEB_SUPER_PWD_RULE="1"
mdlcfg -f SYS_SUPER_LOGIN_PWD="NEW_PASSWORD_HERE"
mdlcfg -a SYS_SUPER_LOGIN_PWD="NEW_PASSWORD_HERE"
mdlcfg -c
```


##Antenna Setup

The PCB Antennas are labelled from 1 to 8. Some PCB antennas actually have two antennas on the PCB. 

The PCB antennas functions are as follows:

1. 4G LTE Diversity
2. 5G Div
3. WiFi  
4. 5G Main
5. 4G Main 
6. 5G Div
7. WiFi
8. 5G Main 

If you want to modify the modem to take external antennae, you need IPEX4/MHF4 to SMA female cables. 
[Like this](https://www.amazon.co.uk/gp/product/B07T977771)

