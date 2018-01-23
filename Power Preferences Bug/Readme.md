Power preferences would reset on reboot. To fix this bug, I used an AppleScript, that loaded on boot, to fix:
* Battery Icon in Menu Bar
* Power Preferences Settings (Display Sleep, Hibernation)

Add this code to AppleScript, save as app, add to login items. 

```
delay 10
tell application "Terminal"
	do shell script "sudo /usr/local/bin/repair_packages --repair --standard-pkgs --volume /" password "YOURPASSWORD" with administrator privileges
	
	do shell script "sudo pmset -b displaysleep 5 disksleep 0 sleep 10" password "YOURPASSWORD" with administrator privileges
	do shell script "sudo pmset -c displaysleep 15 disksleep 0 sleep 25" password "YOURPASSWORD" with administrator privileges
	do shell script "sudo pmset -a hibernatemode 0" password "YOURPASSWORD" with administrator privileges
	do shell script "sudo pmset -a standby 0" password "YOURPASSWORD" with administrator privileges
	do shell script "sudo pmset -a autopoweroff 0" password "YOURPASSWORD" with administrator privileges
	tell application "SystemUIServer"
		open "/Users/USERNAME/Documents/PowerMgmt/Battery.menu" -- this is the location of my Battery.menu. Will be different for you
		
		tell application "Terminal"
			quit
		end tell
	end tell
end tell
```
