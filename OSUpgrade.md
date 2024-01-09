Numpy got you down? Do you hate Python 3? Crowsnest blues? 

Stop dealing with an old version of your OS and upgrade to the latest and greatest, with general ease!

# Overview

- Turn off printer
- Pull SD card (if you have another SD card, use that and your working OS will stay as is for now)
- Use raspberry pi imager and install Bookworm LITE, 64 bit (NOT DESKTOP)
  - Be sure to configure additional options like wifi info, ENABLE SSH, and set your username and password
- Use imager to put the OS on the SD card
- When it finishes, ssh into the pi
- Install kiauh via the website
- Start kiauh
- Install klipper. Use all defaults presented
- Install moonraker. Use all defaults presented
- Install mainsail. Use all defaults presented
- Connect to mainsail via pi ip address
- Upload printer.cfg
- Test

## Detailed version

Coming soon

# Bonus material

Crowsnest doesnt work well on bookworm, so use this!

Go to the Crow's nest website

You're going to see a table and it will say bookworm and then there's going to be a link that says `hint`

Click on that link and it will take you to a completely different website and that is the streaming software that you should use

It will give you a command to install which you can simply just copy paste

However, when it comes time to actually install the service, you cannot just copy paste the second step because you have a USB-based camera and rather you're going to have to find the service that actually exists in the directory that they reference
Once you use system control to enable that service and start it, you will simply be able to go into mainsail. Click add camera and it will instantly show up and you're done
Image
https://github.com/mainsail-crew/crowsnest/
GitHub
GitHub - mainsail-crew/crowsnest: Webcam Service for multiple Cams
Webcam Service for multiple Cams. Contribute to mainsail-crew/crowsnest development by creating an account on GitHub.
GitHub - mainsail-crew/crowsnest: Webcam Service for multiple Cams
This is the software

https://github.com/ayufan/camera-streamer
GitHub
GitHub - ayufan/camera-streamer: High-performance low-latency camer...
High-performance low-latency camera streamer for Raspberry PI's - GitHub - ayufan/camera-streamer: High-performance low-latency camera streamer for Raspberry PI's
GitHub - ayufan/camera-streamer: High-performance low-latency camer...
Image
Find the precompiled Debian link
