This is a quick write up on how do deal with BTT parts as it relates to CAN. This in no way is meant to take away from the awesome work that Maz0r did, which is [here](https://github.com/maz0r/klipper_canbus/).

Rather, this document covers using the U2C 2.1 and EBB36 to get CAN going for a typical setup.

Why write this? its a pretty common setup and it can be confusing/easy to miss some of the steps, so here we go.

# Parts overview

1. Raspbery Pi (CB1 or other hardware may change some of the steps, like the network setup)
2. BTT U2C (I have a 2.1)
3. BTT EBB36 (I have a 1.2)
4. Some wire in the 22awg range
5. USB cables
6. A Voron

It is recommended you do as much of this work OUTSIDE your printer. In other words, do not rip out everything from your working printer to see if you can get CAN working. Rather, set all this up next to the printer. Yes this may involve making some short wire runs to connect things, but its worth it, especially if you ned to print things for mounting and so on.

If you do not have a bench power supply, consider getting one as this can make your life a lot easier. Otherwise just wire up power off your printer's PSU. I personally have a USB keystone jack coming off the back of my printer that goes to my Pi, but if not, just run a temporary connection from under the deck to your Pi for the boards.

# Pi

Summarized from [here](https://github.com/maz0r/klipper_canbus/blob/main/controller/u2c.md)

1. Use nano to create a new file with the command `sudo nano /etc/network/interfaces.d/can0`
2. Add the following code
  ```
  allow-hotplug can0
  iface can0 can static
   bitrate 1000000
   up ifconfig $IFACE txqueuelen 256
   pre-up ip link set can0 type can bitrate 1000000
   pre-up ip link set can0 txqueuelen 256
   ```

3. Save the file with `CTRL-x` and reboot the pi with `sudo reboot`

4. Verify the network via `ip -s link show can0` which should reflect that the CAN network is UP. If the network is not found, connect your U2C via USB and repeat the command to verify the `can0` network is up.

# U2C

Note that the U2C does not need much done to it, short of making sure it has the newest firmware from BTT that fixed an earlier
issue with not being able to use CANBoot to flash things behind the U2C on older firmware. Once you do the steps below, you do not
have to touch the U2C again

1. make sure you have the latest firmware for the U2C from BTT, which is available [here](https://github.com/Arksine/CanBoot/issues/44#issuecomment-1381555466): 
2. The steps to flash this firmware are (taken from above link):
    - Download the file from bigtreetech comment.
    - Stop klipper `sudo systemctl stop klipper`
    - Bring down the can interface `sudo ifdown can0`
    - Unzip the firmware file from bigtreetech
    - Unplug the u2c controller from usb
    - Press and hold the boot button on the u2c and plug the usb cable back in, then release the boot button.
    - Verify that the u2c is in dfu mode `dfu-util -l` You should see lines that contain Found DFU: [0483:df11]
    - Flash the new firmware to the u2c `dfu-util -D G0B1_U2C_V2.bin -d 0483:df11 -a 0 -s 0x08000000:leave`
    - Unplug and replug in the usb cable for the u2c to reset it
    - The CAN interface should now be back up
    - Start klipper `sudo systemctl start klipper`
    - Follow the steps to compile and flash canboot to the EBB (more on this later)
    - Follow the steps to compile and flash klipper over CAN (more on this later)
    - Success
3. When this is done, ADD THE JUMPER for the 120 ohm resistor to the U2C and set the U2C aside

# EBB

Go to [here](https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.1.md) and do what it says, with the following changes:

1. Use a CAN bus speed of `1000000` everywhere the guide shows 500000
2. When you get to step 7, STOP after disconnecting the board from USB
3. When this is done, ADD THE JUMPER for the 120 ohm resistor to the EBB and set the EBB aside

# Connecting all the things

For my setup, I chose to wire things as shown below.

I have 24v and ground going INTO the U2C to supply voltage to all the other connectors. I then use all 4 wires on the Molex connector to take power and CAN signals to the EBB. The U2C is powered by the USB connection tho, so do not forget that.

NOTE: Once you connect your U2C to USB, you should see two little lights turn on. If you do not see this, you may have a bad USB cable.

![image](img/EBBCANSetup.png)

Now there is nothing wrong with wiring up your 24v right to the EBB board (more on that in a minute), but for me, I found this to be the simplest way to set things up.

I am using 22awg Igus cable for my CAN cable. You will need to get motion rated wire, or something with a high strand count (19 or more) so you do not have issues with things breaking later.

Make your cable and route it however you want. Some people use a gland back the A motor mount, I chose to run it out up by where the bowden tube exits. As long as the four wires gets from the toolhead to under the deck, its probably not wrong.

Next up is wiring up the plugs for the CAN cable. Refer to the following images from GadgetAngel:

TRIPLE CHECK your plarity and wiring here. The BTT stuff is not the same on both sides (see image above as a reference).

Once you have the cable wired up (I used the molex connection), plug it into the toolhead and the U2C (or however you decided to power things up.

# Verifying you see CanBoot and flashing Klipper to the Ebb

(Some steps taken from [here](https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.1.md), which you should review as well)

At this point, after checking twice that everything is connected correctly, power up the printer and SSH into the pi.

1. Open [this](https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.1.md) webpage
2. Scroll down to step 9
3. Do what it says, making sure to use a CAN bus speed of `1000000` vs 500000
4. Follow the rest of the page until the end, where it directs you to the config file

# Wiring up a config file

1. On the pi, run the following commands:

```
cd ~/printer_data/config
wget https://raw.githubusercontent.com/maz0r/klipper_canbus/main/toolhead/example_configs/toolhead_btt_ebbcan_G0B1_v1.2.cfg
```

2. Edit the file using nano
```
nano toolhead_btt_ebbcan_G0B1_v1.2.cfg
```

3. Remember the UUID you recorded earlier? Find it and scroll down to the section that looks like this:
```
[mcu can0]
canbus_uuid: 2c77b9d71a11
```
5. Replace `2c77b9d71a11` with **YOUR UUID**

# Now what?

At this point everything should be up and running, but how can you test it? The easiest way is to change the `[fan]` to a `[fan_generic]` so you can see a new fan slider in Mainsail. To do that, find the following section in the ebb cfg you downloaded above:

```
## PART COOLING
[fan]
pin: can0:FAN1
kick_start_time: 0.25
cycle_time: 0.15
off_below: 0.10
```

and change it to

```
## PART COOLING TEST
[fan_generic fantest]
pin: can0:FAN1
kick_start_time: 0.25
cycle_time: 0.15
off_below: 0.10
```

Then save and reload the cfg. 

The config file you downloaded has aliases set for the pins, like this:

```
[board_pins EBB36_G0B1_v1.2] 
mcu: can0
aliases:
aliases_step:
    EXT_EN=PD2,EXT_STEP=PD0,EXT_DIR=PD1,EXT_UART=PA15
aliases_limitsw: # these are preferred for endstops (including klicky)
    LIMIT_1=PB7,LIMIT_2=PB5,LIMIT_3=PB6
aliases_bltouch: # these are the dupont connectors for bltouch
    PROBE_1=PB9,PROBE_2=PB8
aliases_fans:
    FAN0=PA1,FAN1=PA0
aliases_thermistors:
    TH0=PA3,PT100_CS=PA4,PT100_SCLK=PA5,PT100_MISO=PA6,PT100_MOSI=PA7
aliases_heaters:
    HE0=PB13
aliases_rgb:
    RGBLED=PD3
aliases_adxl:
    ADXL_CS=PB12,ADXL_SCLK=PB10,ADXL_MISO=PB2,ADXL_MOSI=PB11
aliases_i2c:
    AUX0=PB3,AUX1=PB4
```

Here is a pin diagram for the EBB36 1.x:

![image](img/ebb36_v1.x_pinout.png)

So that means we need to plug the fan into this port:

![image](img/PA0.png)

In order for the config to match the physical connection on the board.

Once all these changes are made and saved, simply add an [include] for the new EBB related cfg file, then save and restart. Once Klipper restarts you should see a new fan in Mainsail for fantest. Assuming you have connected a fan to the correct pin, you should be able to move the slider and see the fan react!

Once happy with your testing, be sure to reverse the changes you made so that `fantest` is a [fan] again.

You are now ready to rewire your toolhead to the EBB36! Now wire it up, TEST TEST TEST, and then rip out those drag chains! =)
