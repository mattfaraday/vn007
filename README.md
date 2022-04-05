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


## Antenna Setup

The PCB Antennas are labelled from 1 to 8. Some PCB antennas actually have two antennas on the PCB, so you will notice
that there are more ports on the board than there are PCB antennas, but some antennas have more than 1 wire going to them.

The PCB antennas functions are as follows:

### VN007

| PCB Antenna Number | Function | Antenna port on main board | 
| ------------------ | -------- | -------------------------- |
| 1 |  4G LTE Diversity | ANT 2 |
| 2 |  5G Div (and 5.8GHz WiFi) | ANT 4 & 5G 2 | 
| 3 |  2.4 GHz WiFi  | 2.4G 2 | 
| 4 |  5G Main (5G NSA?) | ANT 6 | 
| 5 |  4G Main | ANT 1 |
| 6 |  5G Div (and 5.8GHz WiFi) | ANT 5 & 5G 1 | 
| 7 |  2.4 GHz WiFi | 2.4G 1 | 
| 8 |  5G Main (5G SA?) | ANT 3 |  

### VN007+

| PCB Antenna Number | Function | Antenna port on main board | 
| ------------------ | -------- | -------------------------- |
| 1 | 4G LTE Diversity | ANT 7 |
| 2 | 2.4 GHz WiFi | 2.4G-2 | 
| 3 | dont know | ANT 1 | 
| 4 | 5 GHz ( and 5.8GHz WiFi ) | 5G-2 & ANT 3 & ANT 6 |
| 5 | dont know | ANT 5 | 
| 6 | 2.4GHz WiFi & something else | 2.4G-1 & ANT 2 | 
| 7 | dont know | ANT 4 | 
| 8 | 5G | 5G-1 | 

If you want to modify the modem to take external antennae, you need IPEX4/MHF4 to SMA female cables. 
[Like this](https://www.amazon.co.uk/gp/product/B07T977771)

