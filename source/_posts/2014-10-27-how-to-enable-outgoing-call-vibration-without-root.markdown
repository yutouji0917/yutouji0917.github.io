---
layout: post
title: "How to Enable Outgoing Call Vibration without ROOT"
date: 2014-10-27 21:46:16 +0800
comments: true
categories: 
---

Call Vibrator requires the **radio log** of phone to detect when outgoing call is answered. But since **Android JellyBean(4.1)**, the permission for radio log is not granted to non-system app by default.

If your device is **rooted**, Call Vibrator will prompt a dialog to let user grant the permission to itself.

If your device is **not rooted**, follow the instructions below to grant the permission to Call Vibrator manually.

Guide
=====

1. Install the USB driver of your phone

2. On your phone, go to *Settings -> Developer options -> toggle on Android debugging* . (If you don’t  see Developer options in Settings, go to *Settings -> About phone* and tap Build number until you see the message that you are a developer and return to Settings, you will then see Developer options)

    {% img http://huangyu.qiniudn.com/octopress_Settings.png 320 480 'Android Settings' %}
    {% img http://huangyu.qiniudn.com/octopress_DeveloperOptions.png 320 480 'Developer Options' %}
    

3. One your PC, download *Read Log Permission Enabler* script from [here](https://bitbucket.org/shaobin0604/readlogpermissionenabler/downloads/readlogpermissionenabler.zip), it’s about 500KB zip file

4. Unzip the content to `C:\`

   ```
   C:\readlogpermissionenabler
    ├── grant_read_log_permission.bat
    └── libs
         ├── adb.exe
         ├── AdbWinApi.dll
         └── AdbWinUsbApi.dll
   ```

5. Click *Start -> Run*, type `cmd`, when the black window opens up, type:

    ```
    cd C:\readlogpermissionenabler\libs
    ```
    
    {% img http://huangyu.qiniudn.com/octopress_EnterLibsDir.png 'Enter lis dir' %}

6. Connect your phone to the PC with USB cable.

7. make sure that in command prompt your are now at `C:\readlogpermissionenabler\libs` and type:

    ```
    adb devices
    ```

    you will see a combination of numbers and letters and the word device, that means you are all set.

    {% img http://huangyu.qiniudn.com/octopress_AdbDevices.png 'adb devices' %}

8. for CallVibrator(Lite) type

    ```
    adb shell "pm grant io.github.yutouji0917.callvibrator.ad android.permission.READ_LOGS"
    ```

    for CallVibrator(Donate) type

    ```
    adb shell "pm grant io.github.yutouji0917.callvibrator.donate android.permission.READ_LOGS" 
    ```

    {% img http://huangyu.qiniudn.com/octopress_GrantPermission.png 'grant permission' %}

    or if you are tired of input commands, just run the script `grant_read_log_permission.bat` located in `C:\readlogpermissionenabler`

9. Unplug your phone from PC

10. Open the Call Vibrator app. Check all the options you need.

11. Try making a call. If the phone does not vibrate, try reboot the phone and then making a call once again.

That's all, have fun with outgoing call vibration.

FAQ
===

1. **Q:** My phone is rooted, but outgoing call vibrate still not work.

   **A:** Outgoing call vibrate requires the **radio log** of phone to detect when the call is answered. But the **radio log** is closed on some phones manufactured by SONY/Samsung/Huawei. You can use the following command to check this:

   ```
   adb logcat -b radio -v time
   ```
   
   if you see something like this, your phone can output radio log

   ```
   11-01 11:45:46.878 E/use-Rlog/RLOG-RIL(s)(   96): RX: (M)0x80  (S)0x01  (T)0x02 (len:0x0C mseq:0xFF aseq:0x37) [ 09 02 03 00 80 ]
   11-01 11:45:46.878 D/RILJ    (  606): [5282]< SET_MUTE 
   11-01 11:45:46.878 D/GsmConnection(  606): [GSMConn] --dssds----null
   11-01 11:45:46.882 D/GsmConnection(  606): [GSMConn] update: parent=DIALING, hasNewParent=false, wasConnectingInOrOut=true, wasHolding=false, isConnectingInOrOut=true, changed=false
   11-01 11:45:47.237 D/RILJ    (  606): [5283]> GET_CURRENT_CALLS
   11-01 11:45:47.241 E/use-Rlog/RLOG-RIL(s)(   96): RX: (M)0x02  (S)0x06  (T)0x02 (len:0x62 mseq:0x00 aseq:0x38) [ 01 00 01 01 01 03 00 05 20 31 30 30 31 30 00 00 
   11-01 11:45:47.241 V/RILJ    (  606): Incoming UUS : NOT present!
   11-01 11:45:47.241 D/RILJ    (  606): InCall VoicePrivacy is disabled
   11-01 11:45:47.245 D/RILJ    (  606): [5283]< GET_CURRENT_CALLS  [id=1,DIALING,toa=129,norm,mo,0,voc,noevp,,cli=1,,0] 
   11-01 11:45:48.003 D/RILJ    (  606): [5284]> SET_MUTE false
   11-01 11:45:48.014 E/use-Rlog/RLOG-RIL(s)(   96): TX: (M)0x09  (S)0x02  (T)0x03 (len:0x08 mseq:0x39 aseq:0x00) [ 00 ]
   11-01 11:45:48.018 E/use-Rlog/RLOG-RIL(s)(   96): RX: (M)0x80  (S)0x01  (T)0x02 (len:0x0C mseq:0xFF aseq:0x39) [ 09 02 03 00 80 ]
   11-01 11:45:48.018 D/RILJ    (  606): [5284]< SET_MUTE 
   11-01 11:45:48.038 D/GsmConnection(  606): [GSMConn] --dssds----null
   11-01 11:45:48.038 D/GsmConnection(  606): [GSMConn] update: parent=DIALING, hasNewParent=false, wasConnectingInOrOut=true, wasHolding=false, isConnectingInOrOut=true, changed=false

   ```      

2. **Q:** I've granted read log permission to Call Vibrator, but outgoing call vibrate still not work.

   **A:** Same as FAQ1, please check whether the **radio log** of your phone.

3. **Q:** My phone can output radio log and radio log permission is granted, but outgoing call vibrate still not work.

   **A:** Maybe the radio log format is modified by device manufacturer and is not compatible with AOSP source code. In such case, outgoing call feature would never work. 
   
   You may try other ways to enable outgoing call vibrate, such as:
   
   * Flash a custom **ROM** which has built-in support for outgoing call vibrate, e.g. [CyanogenMod ROM](http://www.cyanogenmod.org/)
   * Install a Xposed module [Android Phone Vibrator](http://repo.xposed.info/module/com.gzplanet.xposed.xperiaphonevibrator)

