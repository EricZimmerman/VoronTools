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

# Put SKR Pico into UART mode
- sudo nano /boot/cmdline.txt
- change

  ```
  console=tty1 root=PARTUUID=f3481a44-02 rootfstype=ext4 fsck.repair=yes rootwait
  dtoverlay=pi3-miniuart-bt
  ```
- sudo raspi-config and change 
    - Interface, serial port, Login NO, hardware YES

- use /dev/ttyS0 in [mcu]
