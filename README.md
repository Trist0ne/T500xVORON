# T500xVORON
A Comgrow T500 Rework to install an RC8 Voron Stealthburner, complete with ChaoticLabs CNC Tap for nozzle based probing and Auto-Z offset, and CANBUS single cable intergration. This should be considered a moderatly advanced build; you will need experience configuring Klipper, crimping/soldering wire, using heat-set inserts, and be comfortable dissasembling/reassembling the T500. If your printer blows up in a million pieces (or otherwise doesn't work like you want it to), you are responsible.

The guide below will cover the full T500 conversion; you may be able to deviate if you just want parts of it (for example; enabling sensorless homing on your stock T500) or you want to use different components (such as a different toolhead board or CANBUS adapter), but you're on your own if you do so. 

## Things to Buy
* [ChaoticLabs CNC TAP **V2**](https://chaoticlab.xyz/products/cnc-voron-tap). This is the nozzle probing system. It MUST be the V2
* [BTT U2C V2.1 CANBUS USB adapter](https://www.amazon.com/BIGTREETECH-Adapter-Supports-Connection-Interface/dp/B0B1X47319). This is the USB to CANBUS adapter
* [BTT SB 2209 RP2040](https://biqu.equipment/products/bigtreetech-ebb-sb2209-can-v1-0?variant=40377392431202) This is the toolhead control board. You should get the 2209 **RP2040** variant.
* [Everything from Fasteners & Electronics section of the BOM of the latest VORON Stealthburner](https://vorondesign.com/voron_stealthburner). Do not cheap out on the BMG extruder kit!
* [A Stealthburner compatible Hotend of your choosing](https://us.store.bambulab.com/products/complete-hotend-assembly-x1-series) I use the Bambu Labs hardened steel hotend assembly, but any compatible hotend will work. 

## Things to Print
**THESE MUST ALL BE PRINTED FROM ABS OR ASA. THEY ARE DESIGNED FOR ABS. OTHER MATERIALS ARE NOT SUITABLE EITHER DUE TO THEIR TEMPERATURE RESISTANCE OR STRUCTURE CREEP OVER TIME; YOU HAVE BEEN WARNED**
* [1x Complete Voron Stealthburner Assembly](https://github.com/VoronDesign/Voron-Stealthburner/tree/main)
  * Optionally use the inline filament runout sensor located [here](https://www.printables.com/model/292186-stealthburner-clockwork-2-filament-sensor/files). This replaces the main_body of the CW2 extruder.
  * Ensure you use the Cable_Cover_for_PCB and the toolhead PCB spacer!
* [1x Toolhead Adapter Plate](https://github.com/Trist0ne/T500xVORON/blob/main/STL/Toolhead%20Adapter%20Plate.stl)
* [2x Bed Spacers](https://github.com/Trist0ne/T500xVORON/blob/main/STL/Bed%20Spacer%20T500%20v3.stl)
* [1x CW2 Cable Bridge](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2240_2209%20CAN/STL/CW2%20Cable%20Bridge.STL)
* [1x CAN Cable Strain Relief](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2240_2209%20CAN/STL/Printed_Part_for_CAN_Cable_V1.2.stl)
* [1x U2C Adapter Plate](https://www.printables.com/model/422274-bigtreetech-u2c-mount/files)
  * You will also need some double sided mounting tape to secure this in the electronics bay

## The Build
You can do the following sections in any order, they are just listed in the order I did them. Some of the steps will be linked to other guides for the non-conversion specific parts as they cover them more in depth, and there's no need to reinvent the wheel. 

### Prerequisites
* Flash KATAPULT and Klipper to the U2C CANBUS adapter AND the SB2209 Toolhead board
  * To make your life easier, do this before assembling anything.
  * An excellent resource for CANBUS flashing is located [here](https://github.com/Esoterical/voron_canbus). There are subfolders for the U2C and the SB2209 rp2040 
* Assemble the VORON Stealthburner
  * Follow the build guide located [here](https://github.com/VoronDesign/Voron-Stealthburner/tree/main/Manual)
  * Attach and wire the SB2209 to the Stealthburner  

### Mounting the Toolhead
1. Start by inserting {4x} M3 5x4mm heat set inserts into the adapter plate. Take care to ensure they are perfectly level with the surface of the plastic.
   [Inset image]

2. Unclip the stock toolhead umbilical and drag chain mount, and move them out of the way. Unmount the stock T500 toolhead and drag chain plate. There are 4 screws around the outside of the plastic casing (keep these!), several screws holding the drag chain plate to the rail. You should be left with a metal rectangular sliding rail.
   [Insert Images]

3. Using the 4 casing screws from the stock toolhead, attach the Adapter Plate to the rectangular sliding rail. Check to ensure the rail still slides back and forth. 
   [Insert Images]

4. Attach the ChaoticLabs CNC TAP to the Adapter Plate
   * The attachment process is the same as the one located in the [CNC TAP build guide](https://github.com/Chaoticlab/CNC-Tap-for-Voron/blob/master/Manual/CNC_Voron_Tap_V2_Build_Guide_20231013.pdf)

5. Attach the Stealthburner assembly to the CNC TAP. Check to make sure the whole assembly can slide up and down (the 'TAP' function)
   * The attachment process for the Stealthburner is the same as the one located in the [CNC TAP build guide](https://github.com/Chaoticlab/CNC-Tap-for-Voron/blob/master/Manual/CNC_Voron_Tap_V2_Build_Guide_20231013.pdf)
