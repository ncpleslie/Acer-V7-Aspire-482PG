# Acer-V7-Aspire-482PG
Hackintosh project involving the Acer V7 Aspire 482pg Laptop

So far, I have managed to get the Acer laptop running on High Sierra (10.13.2)

**Working**
* Shutdown and Sleep
* Touch Screen
* Internal Graphics Card (Nvidia 750 will never work due to limitations with Optimus)
* Sound (ALC 282)
* Brightness
* CPU Power management
* Ethernet
* iMessage/iCloud/Facetime/Etc.


**Not Working**
* Wifi and Bluetooth (Intel Wifi cards do not work with Hackintosh. I use a USB Bluetooth/Wifi Dongle)
* Computer will boot to screen glitch (See Graphics Fix to symptom fix (Not bug fix))
* Power Preferences do not save (See Power Fix)

**Helpful resources**

Sound: https://www.reddit.com/r/hackintosh/comments/4sil5p/audio_mechanic_old_patchfix_removal_applealc/
Indepth DSDT/SSDT information: https://github.com/Kaijun/Acer-V5-573g-DSDT
How to patch DSDT/SSDT: https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/
Backlight: https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/
