#  Klicky/auto z stuff
- https://gist.github.com/conlank/7904ba9529a27b03d707d3a6417877df#klipper-z-calibration
- https://github.com/LoganFraser/VoronMods/tree/main/KlickySetup

# Probe Testing
- https://github.com/sporkus/probe_accuracy_tests/tree/master
- https://github.com/KiloQubit/probe_accuracy

# Input shaper
- https://www.youtube.com/watch?v=M-yc_XM8sP4

# Useful stuff
- Z Locks for gantry work
  - https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/tallman5/z-locks/

# Verify extruder tension

- Loosen the tension
- Heat up the hotend so extruder motor is energized
- Open the latch
- Insert filament
- Close the latch and pull
- If it comes out, tighten one full turn
- Pull again and repeat if it slips
- Once it does not slip, add another full turn, maybe one and a half turns

# Frequency tester
- https://gist.github.com/EricZimmerman/d6bf8a4b0a200611f3d9aa308a31e3c1

# Understanding Input shaper graphs
- https://www.youtube.com/watch?v=M-yc_XM8sP4

# Macros
- https://github.com/ViThreeDimension/voron2.4/blob/feature/print-start-filament-type-dependant/klipper/client_macros.cfg
- https://github.com/majarspeed/Profiles-Gcode-Macros/tree/main/Beeper%20tunes

# Update klipper firmware ON the pi (mcu)

1. cd ~/klipper
2. type `make clean`
3. type `make menuconfig`
4. set **microarchitechture** to `Linux process`
5. save and exit
6. Run command below

```
sudo service klipper stop
make flash
sudo service klipper start
```

# Update klipper firmware from the pi

1. Update all software
2. Run these commands and set options per image below

```
cd ~/klipper
make clean
make menuconfig
 ```

4. Q and save when prompted
5. Flash with comand below (ADJUST YOUR ID ACCORDINGLY. You can get it with **ls /dev/serial/by-id/** command in the shell)
6. ???
7. Profit

```
sudo service klipper stop
make flash FLASH_DEVICE=/dev/serial/by-id/usb-Klipper_stm32f446xx_310012001550324E31333220-if00
sudo service klipper start
```

On restart, klipper should show most current version in Mailsail

![image](https://user-images.githubusercontent.com/4265254/198883349-bb3c9e14-1339-4a10-8706-6c6e036a2dcb.png)
