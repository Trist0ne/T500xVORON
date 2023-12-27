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
   * Ensure you attach the cable bridge shown at the end of the build guide. We will not use the steel wire, but the bridge will provide strain relief and stability to the CANBUS cable port. 
 
6. Check clearences, and appreicate how nice the Stealthburner looks mounted on the T500

### Raising the Bed
The stock T500 gantry and bed are mounted in such a way that the gantry cannot drop low enough to touch the nozzle to the bed. Obviously, this is problematic. Because of the way the gantry is designed, there is no easy way to gracefully drop the toolhead low enough; thus, we have to go the other direction and raise the bed!
* "Wont I lose Z height??"
  * Nope! In it's stock form, the T500 Z rails can raise the nozzle up to 550mm. With these spacers in place, you will still have 520mm of Z clearance (though it will stil be software limited to 500mm), so there is no lost Z build height!
* "Wont these spacers melt so close to the bed? Are they strong enough?"
  * Nope! They're far enough from the bed that they are well within the longterm operating temperaturs of ABS. ABS also does not stress creep over time, and should remain durable for the life of the printer
 
1. Remove the magnetic sheet, and remove the 8x M4 socket head screws using the access holes
   * [Insert Image]
   * If you have a difficult time getting a tool to fit in those holes, you can unbolt every M4 countersunk screw and remove the headbed entirely.
   * If you remove the bed from the bed frame, remove the screws holding the bed drag chain to the bed frame
   * [Insert image]

2. Carefully raise the bed frame, and align the bed spacers to the long rectangular rails
   * Be very careful not to damage the heatbed wiring!
   * Depending on how much your ABS print shrunk, you may need to slightly drill out the holes in the printed bed spacers; you'll never get them properly aligned unless the screws can slide through the spacers with only light resistance
   * [Inset Image]

3. Using 8x M4x40mm screws, bolt the bed frame to the Y axis rails through the spacers. Ensure everything is very tight and does not have any play; you dont want your bed level changing or your bed flying off its rails!
   * [Insert Image]

4. If you removed it in step 1, reattach the drag chain to the bed frame

5. Check to ensure the bed is able to slide all the way back and forth across its rails. If you did not use M4x40mm screws to attach the spacers, they may stick out from the rails and contact the Y Stepper drivers.

### Wiring Wiring Wiring
Thankfully for the most part we are SIMPLIFYING the wiring than making it more complicated. It should go without saying that your printer should be fully powered off and unplugged for this step especially.

#### U2C Wiring and Mounting
1. Remove the electronics bay cover
  * There are 6 screws, 4 in the front and two in the back
  * [Insert image]

2. Carefully unplug and remove the following items from the printer. You will need to cut zip ties and undo screws where appropriate:
* The stock toolhead umbilical (you will need to cut open the Z axis wiring loom to remove this fully. Do NOT cut or remove the X axis stepper wires)
  * Remove the umbilical from the X axis drag chain; we will still utilize the drag chain. Unscrew the mounts on the drag chain and move it as close to the Z axis rails as you can, then reattach it 
* The X and Y axis endstops (you will need to cut open the Z axis wiring loom to remove this fully. Do NOT cut or remove the X axis stepper wires)

3. Using at least 20 AWG wire, connect the U2C to the T500 power supply
* There are two open attachment points for power and ground on the stock power supply you can utilize
* Connect the wires from the power supply to the green VIN/GND points on the U2C
* ![image](https://github.com/Trist0ne/T500xVORON/assets/41755299/df421d74-ece0-4e10-b0ab-56382d2021f4)
* Do not get them backwards unless you to fry your boards!!!

4. Insert the jumper included with the U2C on the pins labled 120R, near the USB-C port

5. Mount the U2C in the electronics bay using the printed mount and double sided tape to secure it
* I chose to mount it here

6. Run a USB-C to USB-A cable from the U2C to the USB port on the T500 screen, following the existing cable routing path
* [Insert Image]

#### Toolhead CANBUS Cable

1. Crimp the end of the long cable included with the SB2209 with the Molex Microfit 4 pin connector that came with the U2C. Take great care that you align CAN_L, CAN_H, GND, and VIN appropriatly (see pinout above; the black 4 pin connector next to the large white one), or you will pop your toolhead board.
* You do not need to shorten the cable; there is room in the electronics back to tuck the slack away nicely.

2. Using the screws on top of the Z axis, move the toolhead mount as far up the Z axix and as far to the right of the X axis as it will go.
* This will determine how much cable length you need

3. Thread the CANBUS cable from the toolhead port through the X axis drag chain, following the same path the stock umbilical cable did. Use the same anchor points to attach the cable to the edge of the X axis and the bottom of the Z axis
* LEAVE SOME ROOM FOR SLACK. While you want these cables managed nicely, you dont want them routed so tightly that they're damaging the things they're attached to, or the internal wiring.  
