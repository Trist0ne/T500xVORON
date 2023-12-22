# T500-Configuration-Public
A Public repository with my optimized Klipper configurations for the T500 3D printer

# TO USE THESE FILES
These configurations assume that you have:
> Installed and configured KAMP https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging

> Edited your Slicer's Start/End Code like this (ALL IN ONE LINE):
* PRINT_START EXTRUDER=[nozzle_temperature_initial_layer] BED=[bed_temperature_initial_layer_single] CHAMBER=[chamber_temperature]
* PRINT_END
  ![Screenshot 2023-12-11 at 8 43 46 PM](https://github.com/Trist0ne/T500-Configuration-Public/assets/41755299/81664cf2-d13c-4ff8-9aad-0ed6432638cd)


> Additionally, if you're using Orca slicer, for best results in the 'others' tab you'll want to enable
* Label Objects
* Exclude Objects
* Arc Fitting (Quality Tab)

Once you've done the above, in your printers web config BACK UP YOUR EXISTING CONFIGURATIONS, then delete them ALL except klipperscreen.conf, saved_variables, and KAMP's configuration files, then copy over each of the .cfg files in this repository. YOU SHOULD RETUNE INPUT SHAPER, PRESSURE ADVANCE, AND Z-OFFSET BEFORE PRINTING! Happy printing!

Do not remove saved_variables; I have no idea why but if you do, it will bootloop your screen. If you run into an error with the screen constantly rebooting, re-apply the firmware update from comgrow (even if you've done it before). The makerbase firmware can be touchy for some reason, havent quite worked out why. After re-updating the issue vanished, including between reboots. Thankfully it wont overwrite the new configurations. 

Firmware update is located here: https://drive.google.com/drive/folders/1Yt9YJvf4PYsZnZN0SJ2QfM854uHyY-OW?fbclid=IwAR0X5d5XWNX3VRbQPU41ONEbuIXbYEswDVW4dIzs3GIlIn9iSdKypPv_4bw
