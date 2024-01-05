# T500xVORON
A Comgrow T500 Rework to install an RC8 Voron Stealthburner, complete with ChaoticLabs CNC Tap for nozzle based probing and Auto-Z offset, and CANBUS single cable intergration. This should be considered a moderatly advanced build; you will need experience or be willing to learn configuring Klipper, crimping/soldering wire, using heat-set inserts, and be comfortable dissasembling/reassembling the T500. If your printer blows up into a million pieces (or otherwise doesn't work like you want it to), you are responsible. It should be noted that this guide is in beta; I did my best to document my process and make the configurations foolproof, but I may have missed something somewhere. You may need to use your brain a bit to troubleshoot!

The guide below will cover the full T500 conversion; you may be able to deviate if you just want parts of it (for example; enabling sensorless homing on your stock T500) or you want to use different components (such as a different toolhead board, different CANBUS adapter, or not using CANBUS at all), but you're on your own if you do so.

## Why would I want this?
* VORON TAP is far more accurate than the stock inductive Z probe, and is not prone to thermal drift. 
* TAP allows for automatic Z offset, every print, regardless of print surface. "Always perfect first layers"!
* The LED's on the hotend can show the current printer status, change as the printer heats up to visually show temperature, and illuminate the nozzle while printing. 
* The VORON Stealthburner allows you to use more ~off-the-shelf~ components that are easy to obtain, since it's difficult to get replacements for Comgrow proprietary parts.
* The modular design allows you to change out hotends easily, or use different hotend styles than the stock one.
* Simplified wiring with CANBUS cuts the amount of wires running to the toolhead down to just one unified cable.
* It just damn **looks** cooler

## TABLE OF CONTENTS
### [THINGS TO BUY](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#things-to-buy)
### [THINGS TO PRINT](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#things-to-print)
### [THE BUILD](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#the-build)
#### [> PREREQUISITES](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#prerequisites)
#### [> MOUNTING THE TOOLHEAD](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#mounting-the-toolhead)
#### [> RAISING THE BED](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#raising-the-bed)
#### [> WIRING WIRING WIRING](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#wiring-wiring-wiring)
#### [> SOFTWARE CONFIGURATION](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#software-configuration)
#### [> SLICER CONFIGURATION](https://github.com/Trist0ne/T500xVORON/blob/main/README.md#slicer-configuration)


## Things to Buy
* [ChaoticLabs CNC TAP **V2**](https://chaoticlab.xyz/products/cnc-voron-tap). This is the nozzle probing system. It MUST be the V2
* [BTT U2C V2.1 CANBUS USB adapter](https://www.amazon.com/BIGTREETECH-Adapter-Supports-Connection-Interface/dp/B0B1X47319). This is the USB to CANBUS adapter
* [BTT SB 2209 RP2040](https://biqu.equipment/products/bigtreetech-ebb-sb2209-can-v1-0?variant=40377392431202) This is the toolhead control board. You should get the 2209 **RP2040** variant.
* [Everything from Fasteners & Electronics section of the BOM of the latest VORON Stealthburner](https://vorondesign.com/voron_stealthburner). Do not cheap out on the BMG extruder kit!
* [A Stealthburner compatible Hotend of your choosing](https://us.store.bambulab.com/products/complete-hotend-assembly-x1-series) I use the Bambu Labs hardened steel hotend assembly, but any stealthburner compatible hotend will work.
* A few extra parts you will need:
  * 4x M3 5x4mm heat set inserts (same ones that VORONs use)
  * 8x M4x40mmm Socket Heat Cap Screws (Or whatever style M4x40mm screws you would like, SHCS's are just the easiest to tighten during the build)
  * A small length of PTFE tubing for the Stealthburner hotend mount

## Things to Print
**THESE MUST ALL BE PRINTED FROM ABS OR ASA. THEY ARE DESIGNED FOR ABS. OTHER MATERIALS ARE NOT SUITABLE EITHER DUE TO THEIR TEMPERATURE RESISTANCE OR STRUCTURE CREEP OVER TIME; YOU HAVE BEEN WARNED**
* [1x Complete Voron Stealthburner Assembly](https://github.com/VoronDesign/Voron-Stealthburner/tree/main)
  * Optionally use the inline filament runout sensor located [here](https://www.printables.com/model/292186-stealthburner-clockwork-2-filament-sensor/files). This replaces the main_body of the CW2 extruder.
  * Ensure you use the Cable_Cover_for_PCB and the toolhead PCB spacer!
* [1x Toolhead Adapter Plate](https://github.com/Trist0ne/T500xVORON/blob/main/STL/Toolhead%20Adapter%20Plate.stl)
* [2x Bed Spacers](https://github.com/Trist0ne/T500xVORON/blob/main/STL/Bed%20Spacer%20T500%20v3.stl). Ensure these print perfectly flat with no warping, or it will throw off your bed level!
* [1x CW2 Cable Bridge](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2240_2209%20CAN/STL/CW2%20Cable%20Bridge.STL)
* [1x CAN Cable Strain Relief](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2240_2209%20CAN/STL/Printed_Part_for_CAN_Cable_V1.2.stl)
* [1x U2C Adapter Plate](https://www.printables.com/model/422274-bigtreetech-u2c-mount/files)
  * You will also need some double sided mounting tape to secure this in the electronics bay

Print Settings:
* Layer height: 0.2mm
* Extrusion width: 0.4mm, forced
* Infill percentage: 40%
* Infill type: grid, gyroid, honeycomb, triangle, or cubic
* Wall count: 4
* Solid top/bottom layers: 5
* Supports: NONE

## The Build
You can do the following sections in any order, they are just listed in the order I did them. **Some of the steps will be linked to other guides** for the non-T500 conversion specific parts as they cover them more in depth, and there's no need to reinvent the wheel.

### Prerequisites
* Flash both KATAPULT and Klipper to the U2C CANBUS adapter AND the SB2209 Toolhead board
  * To make your life easier, do this before assembling anything.
  * An excellent resource for CANBUS flashing I reccomend following is located [here](https://github.com/Esoterical/voron_canbus). There are subfolders for the [U2C](https://github.com/Esoterical/voron_canbus/tree/main/can_adapter/BigTreeTech%20U2C%20v2.1) and the [SB2209 rp2040](https://github.com/Esoterical/voron_canbus/tree/main/toolhead_flashing/common_hardware/BigTreeTech%20SB2209%20(RP2040)).
  * This can be a little tricky if you've never done it before, but its not overly difficult. Technically you can skip installing KATAPULT and just flash Klipper directly, but it will make updating klipper significantly more difficult later. 
* Assemble the VORON Stealthburner
  * Follow the build guide located [here](https://github.com/VoronDesign/Voron-Stealthburner/tree/main/Manual). This is a large step, and your toolhead will only function as well as you assemble it, so take your time.
  * Attach and wire the SB2209 to the Stealthburner, following the build guide [here](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2209%20CAN%20(RP2040)/Build%20Guide/EBB%20SB2209%20CAN%20V1.0（RP2040）BUILD%20GUIDE_1129.pdf); double check your wiring carefully for the CNC-TAP especially. The colors may not be standard on your TAP wiring, and putting them in the wrong order can fry your TAP sensor.

### Mounting the Toolhead
1. Start by inserting {4x} M3 5x4mm heat set inserts into the adapter plate. Take care to ensure they are perfectly level with the surface of the plastic.
![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/1799de0e-7ea4-4598-afaf-f19714c63125)
(Ignore the very bottom two holes in the picture; they didn't make it into the final design)


3. Unclip the stock toolhead umbilical and drag chain mount, and move them out of the way. Unmount the stock T500 toolhead and drag chain plate. There are 4 screws around the outside of the plastic casing (keep these!), several screws holding the drag chain plate to the rail. You should be left with a metal rectangular sliding rail.
![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/e83a2e70-2edc-46f8-97bc-da69bf9ecbf4)


4. Using the 4 casing screws from the stock toolhead, attach the Adapter Plate to the rectangular sliding rail. Check to ensure the rail still slides back and forth. 
![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/f67d71d7-87a8-44ed-b62f-64451ad39fc4)


5. Attach the ChaoticLabs CNC TAP to the Adapter Plate
   * The attachment process is the same as the one located in the [CNC TAP build guide](https://github.com/Chaoticlab/CNC-Tap-for-Voron/blob/master/Manual/CNC_Voron_Tap_V2_Build_Guide_20231013.pdf)

6. Attach the Stealthburner assembly to the CNC TAP. Check to make sure the whole assembly can slide up and down (the 'TAP' function)
   * The attachment process for the Stealthburner is the same as the one located in the [CNC TAP build guide](https://github.com/Chaoticlab/CNC-Tap-for-Voron/blob/master/Manual/CNC_Voron_Tap_V2_Build_Guide_20231013.pdf)
   * Ensure you attach the cable bridge shown at the end of the build guide. We will not use the steel wire, but the bridge will provide strain relief and stability to the CANBUS cable port.
![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/4ad1ae06-10eb-4e5d-9737-56c6c2a6584a)

 
7. Check clearences, and appreciate how nice the Stealthburner looks mounted on the T500

### Raising the Bed
The stock T500 gantry and bed are mounted in such a way that the gantry cannot drop low enough to touch the nozzle to the bed. Obviously, this is problematic. Because of the way the gantry is designed, there is no easy way to gracefully drop the toolhead low enough; thus, we have to go the other direction and raise the bed!
* "Wont I lose Z height??"
  * Nope! In it's stock form, the T500 Z rails can raise the nozzle up to 550mm. With these spacers in place, you will still have 520mm of Z clearance (though it will still be software limited to 500mm), so there is no lost Z build height!
* "Wont these spacers melt so close to the bed? Are they strong enough?"
  * Nope! They're far enough from the bed that they are well within the longterm operating temperature limit of ABS. ABS also does not stress creep over time, and should remain durable for the life of the printer
 
1. Remove the print surface, and remove the 8x M4 socket head screws using the access holes
   * ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/ddf7ffba-6176-486a-8fc0-d5f8d951e9f6)

   * If you have a difficult time getting a tool to fit in those holes, you can unbolt every M4 countersunk screw and remove the headbed entirely.
   * If you remove the bed from the bed frame, remove the screws holding the bed drag chain to the bed frame
   * ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/622494c6-0444-4c62-af16-58f4166bf830)


2. Carefully raise the bed frame, and align the bed spacers to the long rectangular rails
   * Be very careful not to damage the heatbed wiring!
![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/49a9cc11-a071-44c6-a119-31e2612c8e6c)

   * Depending on how much your ABS print shrunk, you may need to slightly drill out the holes in the printed bed spacers; you'll never get them properly aligned unless the screws can slide through the spacers with only light resistance
![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/a1e1170f-3aea-4827-b9fe-b631b5aab490)


3. Using 8x M4x40mm screws, bolt the bed frame to the Y axis rails through the spacers. Ensure everything is very tight and does not have any play; you dont want your bed level changing or your bed flying off its rails!
![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/e2aad75b-baf5-4722-8da7-eb1707f60aba)

4. If you removed it in step 1, reattach the drag chain to the bed frame

5. Check to ensure the bed is able to slide **all** the way back and forth across its rails. If you did not use M4x40mm screws to attach the spacers, they may stick out from the rails and hit the Y Stepper drivers.

### Wiring Wiring Wiring
Thankfully for the most part we are SIMPLIFYING the wiring rather than making it more complicated. It should go without saying that your printer should be **fully powered off and unplugged** for the next few steps especially.

#### U2C Wiring and Mounting
1. Remove the electronics bay cover
  * There are 6 screws, 4 in the front and two in the back
  * You will have to slide the heatbed back and forth to reach them all

2. Carefully unplug and remove the following items from the printer. You will need to cut zip ties and undo screws where appropriate:
* The stock toolhead umbilical (you will need to cut open the Z axis wiring loom to remove this fully. Do NOT cut or remove the X axis stepper wires)
  * Remove the umbilical from the X axis drag chain; we will still utilize the drag chain. Unscrew the mounts on the drag chain and move it as close to the Z axis rails as you can, then reattach it 
* The X and Y axis endstops (you will need to cut open the Z axis wiring loom to remove this fully. Do NOT cut or remove the X axis stepper wires)
* ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/3abb4e46-f0a4-4822-b5aa-caab066ac0e3)

* Flick the switch highlighted below to enable sensorless homing
* ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/6f1cea17-c823-405f-b56b-fb6b61758521)


3. Using at least 20 AWG wire, connect the U2C to the T500 power supply
* There are two open attachment points for power and ground on the stock power supply you can utilize. This is an example; please pay attention to which one is negative and which one is positive
* ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/909fa600-5ebe-4062-ad0d-fbee3cfea6d4)

* Connect the wires from the +/- power supply to the green VIN/GND points on the U2C
* ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/df421d74-ece0-4e10-b0ab-56382d2021f4)
* Do not get them backwards unless you want to fry your boards!!!

4. Insert the jumper included with the U2C on the pins labled 120R, near the USB-C port

5. Mount the U2C in the electronics bay using the printed mount and double sided tape to secure it
* I chose to mount it here ![21556689-9E68-4C35-BBAF-199B8BBE90C0_1_201_a](https://github.com/Trist0ne/T500xVORON/assets/41755299/5607ba5f-a9a9-49f2-927e-d3ad6f1ad170)


6. Run a USB-C to USB-A cable from the U2C an open USB port under the T500 screen, following the existing cable routing path from the stock mainboard. Zip tie up the excess; keep it neat!

#### Toolhead CANBUS Cable

1. Crimp the end of the long cable included with the SB2209 with the Molex Microfit 4 pin connector that came with the U2C. Take great care that you align CAN_L, CAN_H, GND, and VIN appropriatly (see pinout above; the black 4 pin connector next to the large white one), or you will pop your toolhead board.
* You do not need to shorten the cable; there is room in the electronics back to tuck the slack away nicely.

2. Using the screws on top of the Z axis, move the toolhead mount as far up the Z axis and as far to the right of the X axis as it will go.
* This will determine how much cable length you need

3. Thread the CANBUS cable from the toolhead port through the X axis drag chain, following the same path the stock umbilical cable did into the electronics bay. Use the same anchor points to attach the cable to the edge of the X axis and the bottom of the Z axis
* LEAVE SOME ROOM FOR SLACK. While you want these cables managed nicely, you dont want them routed so tightly that they're damaging the things they're attached to, or the internal wiring. Dont attach it so tightly that the Z axis rips the cable out at max height!
* Route the cable cleanly through the electronics bay to the U2C. Plug it in, then zip tie up the rest of the slack to one of the stock anchor points.
* Zip tie the CANBUS cable to the X axis stepper wire to keep everything clean and organized
* Finally, Zip tie the CANBUS cable to the edge of the drag chain. The plasic mount on the EBB cable will provide plenty of strain relief ![0A58DF86-1371-48C0-9A15-90A06EAC607F_1_201_a](https://github.com/Trist0ne/T500xVORON/assets/41755299/5f7182dd-c8aa-468e-adab-82e27f99e1b3)


#### Wiring Checks
Dont think, just double check them
1. Power supply POSITIVE is wired to U2C VIN. Power supply GROUND is wired to U2C GND
2. CAN_L, CAN_H, GND, and VIN on the CANBUS cable are aligned to the CAN_L, CAN_H, GND, and VIN pins on the U2C

When you're 100% sure everything is lined up, plug your printer in and flip the power on. Check to make sure the stock mainboard, the U2C and the SB2209 all light up (and that nothing goes pop!). When you're satisfied with your work, reinstall the electronics bay cover and screw it back down. 

### Software Configuration

#### Initial Startup
1. SSH into your printer. The username is **mks**, and the password is **makerbase**. Install KAMP on your printer, located [here](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging)
2. Access your printers web-ui. In the machine tab, delete ALL the config files.
3. Download the Klipper Configurations folder from this Github ([here](https://github.com/Trist0ne/T500xVORON/tree/main/Klipper%20Configurations)), and upload them ALL to the printer's config section.
* ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/258b8037-95a2-4bea-b8ea-d8ef3a9e6b04)
3. Configure the annotated sections of **printer.cfg**, **toolboard.cfg**, and **u2c.cfg**. You will need to fill in the canbus_uuid sections to to tell your printer the CAN addresses of your U2C and SB2209, as well as configuring your sensorless homing (info below)
4. Save and click 'firmware restart'; resolve any errors that Klipper may throw, if any (the error message will tell you what the configuration issue is).

#### Z Endstop Check
**DO THIS BEFORE ATTEMPTING TO HOME THE PRINTER**
1. Once Klipper has successfully loaded and can interface with the U2C and SB2209, navigate to the machine page of the web UI and press 'sync endstops'
2. Endstop Z should be 'OPEN'
3. By hand, move the toolhead up to the top of it's travel to trigger the CNC TAP sensor. Hold it in place and click the 'sync enstops' button. Endstop Z should now be marked as 'CLOSED'
4. Additionally, there should be a blue light on the rear of the TAP sensor that turns red when triggered. If this doesn't happen, you've either wired the sensor wrong or it is faulty.

#### Sensorless Homing Configuration
**DO THIS BEFORE ATTEMPTING TO HOME THE PRINTER**
1. Follow the guide located [here](https://docs.vorondesign.com/community/howto/clee/sensorless_xy_homing.html) to calibrate the sensorless homing.
* The default configurations are made to work with sensorless homing, and are largely preconfigured. You can start from "Finding the right StallGuard threshold". You do not need to add sensorless homing macros, I built them in. Ensure that your sensitivity numbers are high enough that you do not damage the printer, but low enough that the machine is consistently able to home itself.

#### Z Offset Configuration
Update your Z offset
* Remember, with Tap, your nozzle IS the probe. You'll need to manually calibrate the probe's Z offset by using PROBE_CALIBRATE and your chosen leveling method (I use a sheet of paper)

### Final Checks
1. Ensure that your printer can reliably home X, Y, and Z.
2. Test that your Extruder, Hotend, Hotend Fan, Part Cooling Fan, and [optionally] the runout sensor all work as expected.
3. PID Calibrate your Hotend following [this guide](https://www.obico.io/blog/klipper-pid-tuning/)
4. Follow the [Ellis3dp print tuning guide](https://ellis3dp.com/Print-Tuning-Guide/articles/extruder_calibration.html) to dial in your Stealthburner
5. Configure Input Shaping on both X and Y
* You can reference [this guide](https://www.klipper3d.org/Measuring_Resonances.html#bed-slinger-printers) for help
* Your X axis now has a built in accelerometer. You will still need to plug in the included accelerometer and mount it on the Y axis, the same way you would do on the stock T500.
* I use the SHAPER_CALIBRATE macro for automatic calibration. It will measure both X and Y sequentially and apply the best settings automatically. 

6. Retune your Pressure Advance
* Follow [this guide](https://github.com/SoftFever/OrcaSlicer/wiki/Calibration) from the Orca Slicer wiki. You shold set this number in the Orca Slicer filament profile settings, rather than setting it in the Klipper configs. It will be different for every filament and between brands. 
7. Pat yourself on the back for a job well done! Edit your slicer settings as shown below, and then do a test print!

### Slicer Configuration
1. Edit your printer settings "Machine G-code" tab as follows:
* PRINT_START EXTRUDER=[nozzle_temperature_initial_layer] BED=[bed_temperature_initial_layer_single] CHAMBER=[chamber_temperature]
* PRINT_END
* ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/38245c9d-8e7e-4014-8cb4-d94785227172)
2. In the 'Quality' tab, enable Arc Fitting
3. In the 'Others' tab, enable
* Label Objects
* Exclude Objects

### Troubleshooting Notes
There are two errors you may see in your console when starting a print:
1. Unknown command:"SAVE_LAST_FILE"
2. Error evaluating 'gcode_macro PRINT_START:gcode': jinja2.exceptions.UndefinedError: 'dict object' has no attribute 'BED'
These are caused by the non-standard version of klipperscreen that comes built into the T500, and makes calls to incorrectly labeled markers. **Thankfully, they don't actually impede the printing, so you can ignore them!** You can resolve this by installing the standard version of KlipperScreen using [KIAUH](https://github.com/dw-0/kiauh), which comes built into the T500

