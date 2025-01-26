So you wanna live dangerously eh?

You heard the rumors, you wondered if its really THAT dangerous, well, let's find out!

tl;dr; Its not dangerous at all!

# Getting started

First, what exactly IS Kalico? 

Kalico is a (hard) fork that aims to add features and improvements beyond what is found in mainline Klipper. While this sounds scary, in reality, you can switch right now to Kalico and if you do not enable or use any of the Kalico specific features, you would not even know you were running Kalico (except for some nice quality of life issues).

You can find it [here](https://github.com/KalicoCrew/kalico)

## Some awesome features

So what are some of the things that makes Kalico worth considering?

1. Ever get an ADC out of range and struggle to figure out which thermistor is causing the issue? Kalico would tell you in plain english, right in the error message, which thermistor is causing your headache!
2. Kalico can automatically rotate your klippy log on startup, so you never end up with 257MB logs to then try and share on Discord
3. MUCH improved sensorless configs, without the need for macros. You can specify both a run_current AND a homing_current and it just works!
4. gcode shell command is built right in. No more manual installs or using Kiauh to install
5. Tired of complex macros for dockable probes? Kalico has you covered with [dockable_probe] support!
6. PID tuning got you down? Tired of overshoots? Consider using pid_v or even better, MPC support, for much more stable temperature management of your bed and hotend
7. Additional features for fans, such as curve vs linear activation, and reverse fans (not that its going backwards, but the lower the temp, the higher the fan RPM)
8. Cool additions to macros, and hot reloading of macros is being worked on! No more full restarts to test your macros!
9. Does your probe have high variance on the first probe? Kalico has you covered, and can drop the first result, leading to lower standard deviation!
10. Do you want to reload your macros you are testing without restarting Klipper? RELOAD_GCODE_MACROS can do that for you
11. and much more

Read about all the awesome stuff in more detail [here](https://docs.kalico.gg/Danger_Features.html)

There is also dedicated Kalico [documentation](https://docs.kalico.gg/) to review for all the cool things mentioned above

## Switching to Kalico

Switching is easy, and there are several ways to accomplish it (some of these steps borrowed from Kalico GitHub repo)

### Method 1

Old school

1. SSH into your pi
2. Rename existing klipper directory with this command
   
   ```
    mv klipper klipperOLD
   ```
   
3. Clone Kalico into the klipper directory

   ```
    git clone https://github.com/KalicoCrew/kalico.git ~/klipper
    sudo systemctl restart klipper
   ```
   
4. You are done!

### Method 2

Use kiuah

1. Install kiauh from here
2. From the KIAUH menu select:
   - [S] Settings
   - 1) Set custom Klipper repository
   - 2) Use https://github.com/KalicoCrew/kalico as the new repository URL
   - 3) Use main or bleeding-edge-v2 as the new branch name
   - 4) Select 'Y' to apply the changes
3. Enter 'B' for back twice
4. 'Q' to quit
5. Run kiauh
   ```
   ~/kiauh/kiauh.sh
   ```
6. Choose option 6
7. Choose option 1, set custom Klipper repository
8. Choose the number for Kalico
9. Confirm with 'Y'
10. Go back to main menu with 'B' until you arrive there
111. Use option 1 to install klipper

This is the method I usually use for a new install.

## Changing branches

Experimental features are generally not yet available on the main branch of Kalico and should be used at your own caution.

An overview of these features can be found on Kalico's [documentation](https://docs.kalico.gg/Bleeding_Edge.html) page, configuration references are found [here](https://docs.kalico.gg/Config_Reference_Bleeding_Edge.html).

To try out experimental features, you need to switch to the `bleeding-edge-v2` branch. In order to switch to that branch, do this:

1. SSH into the pi
2. Change to klipper directory with `cd klipper`
3. Change to a new branch with `git checkout bleeding-edge-v2`
4. Check for updates with `git pull --rebase`
5. Restart Klipper

Thats it! to switch back to master, simply use `git checkout master` in step 2 above.

So thats it! Using Kalico is not that dangerous at all, but it can make your life easier if you are willing to learn some new stuff.

Enjoy!

