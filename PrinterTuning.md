# Printer tuning considerations

This is the best order many of us have found to tune things with your printer:

1. Belt shaper
2. Input shaper
3. Vibrations calibration
4. Get max accels from #2
5. Get external perimeter speed from #3
6. Use external perimeter speed and accels for PA test
7. EM
8. Test print a cube
9. Adjustments

Let's break each of these down as to why this order is preferred over what is currently on the [Ellis Print tuning guide](https://ellis3dp.com/Print-Tuning-Guide/). 

**NOTE:** THIS IS IN NO WAY SAYING ELLIS IS WRONG!! If you prefer his tests for PA or EM, go for it, but consider the order of operations above.

Its just that the order is not optimal, because, for example, acceleration and speed affects PA, so why not START there?


## Pre-requisites

Either [Klippain](https://github.com/Frix-x/klippain) (for the awesome PA and EM test macros) or at least [Shake n Tune](https://github.com/Frix-x/klippain-shaketune)

Some [background](https://www.youtube.com/watch?v=7TtBJAJMfNg) from Reth on Shake n Tune

This guide does not cover establishing your rotation distance, as that is part of the initial Voron startup guide, found [here](https://docs.vorondesign.com/build/startup/#extruder-calibration-e-steps)

## Belt Shaper

Documentation for this is available [here](https://github.com/Frix-x/klippain-shaketune/blob/main/docs/macros/belts_tuning.md)

(more info here)

Reth's Belt shaper [video](https://www.youtube.com/watch?v=zfnWsBOt3_8)

## Input shaper

Documentation for this is available [here](https://github.com/Frix-x/klippain-shaketune/blob/main/docs/macros/axis_tuning.md)

Reth's input shaper [video](https://www.youtube.com/watch?v=fjs4TQc1kZI)

## Vibrations calibration

Documentation for this is available [here](https://github.com/Frix-x/klippain-shaketune/blob/main/docs/macros/vibrations_tuning.md)

In short, stay in the valleys!

## Get max accels from Input shaper

(more info here)

## Get external perimeter speed from Vibrations calibration

(more info here)

## Use external perimeter speed and accels for PA test

Documentation for the Klippain PA test is [here](https://github.com/Frix-x/klippain/blob/main/docs/features/pa_calibration.md) and Ellis's test is [here](https://ellis3dp.com/Pressure_Linear_Advance_Tool/)

(more info here)

A fantastic way to verify your PA and tune it live is to use Thor's method, found [here](https://discord.com/channels/460117602945990666/461133450636951552/1017098926748348518)

## EM

Documentation for the Klippain EM test is [here](https://github.com/Frix-x/klippain/blob/main/docs/features/flow_calibration.md) and Ellis's method is https://ellis3dp.com/Print-Tuning-Guide/articles/extrusion_multiplier.html

## Test print a cube

(more info here)

You do not need the whole cube initially. You can do less infill and just the top half of the cube if you want by cutting it in the slicer.

## Adjustments

(more info here)

In the rare event your work turns out perfect, congrats! You are well on your way. If not, keep tweaking and have fun while doing it!
