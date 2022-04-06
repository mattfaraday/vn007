# China Unicom VN007/VN007+ stuff

## Disclaimer/Warning: 

The stuff on this page can brick your modem if you don't follow it exactly. 

You might even brick it if you do follow it exactly. 

I am not responsible for what you do to your modem. 


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

![image1](/img/sessionID.jpg)


Click the console tab and paste in the following:
```
$.post( 'http://192.168.0.1/cgi-bin/http.cgi' , '{"adbSwitch":1, "cmd": 237, "method":"POST", "language":"CN ", "sessionId": "XXXXXXXXXXXXXXXXXXXX"}' , function ( result ) { console .log(result) })
```  
Replace XXXXXX with your SessionID. !!!!
Press ENTER to send the command. 
A successful command will return: 

```'{success: true, cmd:237, message: ''} ```

![image2](/img/adb-on.jpg)

[Download an ADB client:](https://www.xda-developers.com/install-adb-windows-macos-linux/)


```
adb connect 192.168.0.1:5555
adb shell
grep SYS_SENIOR_LOGIN_PWD /tmp/mdlcfg.sysconfig
```
This will echo a line such as: 

```EXPORT SYS_SENIOR_LOGIN_PWD="abc1234"```

Where abc1234 will be the root password for the device. You can go ahead and log in to the web gui (http://192.168.0.1) as root / abc1234 (replace with the output from the command!)


## The SYS_SUPER / superadmin user 

Be careful with this user! this is some kind of engineering/test account that gives access to things that can break your modem!
This user unlocks many options not visible to any other user, such as VLANs, VPNs, test menus, more WiFi options etc. 

You have to set this password. As above in the ADB console type : 
```
mdlcfg -f SYS_WEB_SUPER_PWD_RULE="1"
mdlcfg -a SYS_WEB_SUPER_PWD_RULE="1"
mdlcfg -f SYS_SUPER_LOGIN_PWD="NEW_PASSWORD_HERE"
mdlcfg -a SYS_SUPER_LOGIN_PWD="NEW_PASSWORD_HERE"
mdlcfg -c
```

You can now login as superadmin with the password you just set. 

## Logo

Want to remove the logo ? 

```mount -o rw,remount / && rm /tzwww/images/logo_unicom_5g.png && touch /tzwww/images/logo_unicom_5g.png```

Note this permanently deletes it. 

You can also replace the image, it has to be 166x65 pixels. Once you have saved it (as a PNG) use ADB push to upload.

First, set the filesystem to read/write:

```mount -o rw,remount /``` 

then in ADB: 

```adb push c:\myfile.png /tzwww/images/logo_unicom_5g.png```

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

### VN007+  (Note, guess work at the moment)

| PCB Antenna Number | Function | Antenna port on main board | 
| ------------------ | -------- | -------------------------- |
| 1 | ? | ANT 7 |
| 2 | 2.4GHz WiFi | 2.4G-2 | 
| 3 | 5G  | ANT 1 | 
| 4 | 5G & 5GHz WiFi | 5G-2 & ANT 3 & ANT 6 |
| 5 | ? | ANT 5 | 
| 6 | 2.4GHz WiFi & ??? | 2.4G-1 & ANT 2 | 
| 7 | ??? | ANT 4 | 
| 8 | 5GHz WiFi | 5G-1 | 

Antenna (external) Hack: Use ANT1, ANT6, ANT2, ANT3. Leave all the others connected to PCB antennas. 

Or, you can also just connect up all of them....

![likethis](/img/mod1.jpg)


Required parts: 

1. 2.5mm drill bit 
2. 6.5mm drill bit
3. Masking tape
4. A template (I just drew circles in mspaint and printed it out)
5. Drill
6. 9 x U.FL to SMA cable (1.13mm IPEX connector) [like this](https://www.amazon.co.uk/gp/product/B0931T2SKZ)

Technique: 

I put masking tape over everything, to stop scratches. 

Print out a template, or at least mark where you want to drill. Don't go too far up because of the length of the cable, and the fact that unless you want to stick these connectors on the front of the device, you'll need to stretch the cables from front to back as well. Removing the internal PCB antennas will make this super easy, but then you cannot easily swap back to them. You could also decide to go fully external and buy some rabbit ears antennas if you wanted? 

Once you've marked your holes to drill, using the 2.5mm drill bit drill a pilot hole in the centre of your circles. 

Swap to the 6.5mm drill bit and enlarge the hole. 

Then you just need to lift off the original cables. Put a plastic probe or a small screwdriver under the tail of the connector and gently lift it up. 
You can remove the entire PCB antenna assembly by removing the 4 retaining crosshead screws. 

Attach the cables, put the SMA connectors through the holes, tighten up. 
