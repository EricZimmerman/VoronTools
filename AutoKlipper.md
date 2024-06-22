Do you hate updating Klipper and having to flash all your stuff? Can't remember how to do it from the last time? Well now you don't have to remember anything (except once)!

This is a walkthrough for setting up and using the awesome script for updating klipper found [here](https://github.com/fbeauKmi/update_klipper_and_mcus/tree/main)

The way this is set up allows us to then automatically back up our firmware config files AND the settings for updating them to Github, using my tutorial [here](https://github.com/EricZimmerman/Voron-Documentation/blob/main/community/howto/EricZimmerman/BackupConfigToGithub.md).

# Getting started

1. SSH into the pi
2. Get to your  config directory by typing `cd ~printer_data/config`
3. Make a new directory to contain the script via `mkdir flash`
4. Change into this new directory using `cd flash`
5. Download files using these commands:
   ```
   wget https://raw.githubusercontent.com/fbeauKmi/update_klipper_and_mcus/main/update_klipper.sh
   wget https://raw.githubusercontent.com/fbeauKmi/update_klipper_and_mcus/main/examples/mcus.ini
   ```
7. Make the script executable via `chmod +x update_klipper.sh`
8. Edit the configuration file using `nano mcus.ini`
9. Update for your hardware (see examples below)
10. Save `mcus.ini` and exit using `Ctrl-o` and `Ctrl-x`
11. Run the script via `./update_klipper.sh`

## Example mcus.ini files

⚠️ Note that you MUST update your FLASH_DEVICE path and or your CAN uuid to match YOUR settings ⚠️ 

### Octopus Pro 1.1

A simple one. Single mcu connected via USB

```
[mcu]
action_command: make flash FLASH_DEVICE=/dev/serial/by-id/usb-Klipper_stm32h723xx_25003F000751313433343333-if00
```

### Voron 2.4 with Kraken and EBB36

Kraken is in USB-Can bridge mode. Note how the mcu block puts the Kraken into katapult mode via -r and then flashes TO THE USB PATH. You will need do do this step initiall to get the /dev/serial path

The EBB is standard stuff.

```
[mcu]
quiet_command: python3 ~/katapult/scripts/flashtool.py -i can0 -r -u e0136f85b8f5
quiet_command: sleep 2
action_command: python3 ~/katapult/scripts/flashtool.py -d /dev/serial/by-id/usb-katapult_stm32h723xx_0B0036001751313338343730-if00

[ebb36]
action_command: python3 ~/katapult/scripts/flashtool.py -i can0 -u 2ce86205a3c0
```


## Workflow

When you run the script, it will first do a `git pull` to make sure your klipper is current, then it will walk you through the flashing via Yes/No prompts. 

⚠️ ⚠️ YOU WILL NEED TO FIND THE CORRECT `make menuconfig` SETTINGS THE FIRST TIME YOU DO THIS. ⚠️ ⚠️ 

Get these from [here](https://github.com/EricZimmerman/VoronTools/blob/main/FlashKlipper.md) or [here](https://canbus.esoterical.online/mainboard_flashing/common_hardware.html) or [here](https://canbus.esoterical.online/toolhead_flashing/common_hardware.html), or consult the manufacturers documentation.

Once you tell the script to update what you have configured in `mcus.ini`, it will bring up the menuconfig for each configured device. The first time you need to specify all the settings AND VERIFY them. Every other time it will reload your previous settings on a per mcu basis, but you should still verify if any new items are added.

Exit menuconfig the usual way and the script will build klipper, then prompt to flash. Press `Y` and it will flash.

Keep following the script and when its done, SO ARE YOU!

In the case where you do NOT have any updates for klipper that the script can find (like when you use Mainsail or Fluidd to update klipper), you can flash things by using the `./update_klipper.sh -f` option. The rest of the workflow is the same.

## Off to Github

Since all the stuff we added above is under our printer's config folder, it will now be found and added to GitHub for you the next time you run `~/printer_data/config/autocommit.sh`

