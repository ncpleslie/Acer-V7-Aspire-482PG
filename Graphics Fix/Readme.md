I found that when I booted it would display a garbled image from the Apple boot logo to login. 
To remedy this you could simply put the laptop to sleep, or close the lid for 3 seconds.

I created a small AppleScript to fix this bug. 

Files needed: [Sleep Display - From Kimhunter](https://github.com/kimhunter/SleepDisplay)
I've placed mine in my documents folder, in a folder called SleepDisplay. 

```
tell application "Finder" to sleep
delay 2
do shell script "/Users/YOURUSERNAME/Documents/SleepDisplay/kimhunter-SleepDisplay-32b3780/dist/1.1/x64/SleepDisplay --wake"
```



Add this code to an AppleScript file, save as an app, and add it to the start up items, under ```System Pref -> User and Groups -> Login Items -> + Button```

This code will sleep the display, on boot, and awaken the laptop 2 seconds later. This should remove any graphics garble. 
