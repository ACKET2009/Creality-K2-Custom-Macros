# Creality-K2-Custom-Macros
Creality K2 macros for better ORCA Slicer compatibility and other improvements\
Based on ideas: https://github.com/jamincollins great thanks!\
Special thanks: https://github.com/Tartarianec for Creality libs functions extraction \
GPL-3.0 license\
Author: Mikhail Ivanov masluf@gmail.com 
> [!NOTE]
> Macros tested on 1.1.2.10 Printer firmware and 1.1.3 CFS Firmware.
> I have information that Tool.cfg does not work with the 1.1.3.5 firmware and it may not work with 1.1.3.1 either.

> [!WARNING]
> Using custom macros could damage your printer and void your warranty, or cause unexpected behavior.

## Key Features:
### main.cfg
- Fix the exhaust fan to improve Orca's (and Creality Print) "Activate Air Filtration" function.
- Implement dynamic control of the exhaust fan for improved heating, cooling, and air filtration.
- Bed mesh store for different bed temperatures
- Better start printing with an additional fast start option for printing the same model (through the virtual pin). This would be useful for printing smaller parts or testing and soak time up to 10 minutes (through virtual pins as well) for better bed stabilization and the best first layer for large parts or printing many parts at once.
- The toolhead moves quickly after RESUME printing to prevent oozing.
- `AUTO_MESH` is a macro that creates a mesh for each desired temperature set under `variable_bed_mesh_temps` in `main.cfg`. Just fill comma separated list, save, push the button and wait. 

### overrides.cfg
- Increased accuracy of Z_TILT_ADJUST
- Default bed mesh calibration is turned off after printer restart
- Disabled prtouch console flood
- Max bed temp increased to 140c
- Heater fan speed increased to full. It's ok for heating but you can get best result with my Ultimate Exhaust system https://www.crealitycloud.com/model-detail/67c9f8b07f0b8c17944c377b?source=22
### tool.cfg
> [!NOTE]
> I am not sure about all the commands in this module, as they are the result of reverse engineering and require a lot of testing.

- added Toolhead buttons for manual load and unload filaments. T17 for unload any. Do NOT press any buttons except T17 to unload filaments. \
![изображение](https://github.com/user-attachments/assets/afea66c2-4f16-4baf-859d-b6a7c3ac8330)
- Unloading CFS filament is disabled after printing. This is great for reusing the same filament.
- Spoolholder can be used from ORCA without unplugging CFS and feed filament through buffer! Just add +1 filament to your project (if you have 1 CFS connected - fifth, if 2 CFS-ninth etc.), set it for parts and push print! \
![изображение](https://github.com/user-attachments/assets/f3d3497c-8c7c-4c29-9110-13ea197c1ac1)
- All loading and purging parameters, such as temperature and velocity, are used from the slicer, and not from the printer's filament database or printer's on-screen menu.
- Added pressure stabilization to the last purging step. This allows you to print without a skirt, a brim, a priming tower, or any other garbage objects. However, in some cases, such as when changing filament types or opposite colours changes, you may want to add a priming tower for better print quality.
- Loading and unloading CFS filament as quickly as possible.

To start printing from the spool holder after using the CFS, follow these steps:
1. Open the Fluidd web interface.
2. Press the T17 button or type the T17 command into the console and press Enter (see screenshot below). The CFS filament will be unloaded. 
3. If you don't have a Y-splitter, disconnect the CFS tube and connect the spoolholder tube. 
4. Manually load filament from the spoolholder. 
5. Go to the filament tab in the printer screen. 
6. Set the right filament type for the spoolholder. 
7. Press the "Extrude" button. 
8. Push filament to the extruder for a better catch.

![изображение](https://github.com/user-attachments/assets/ba56b4d1-2272-4d52-b366-ec12b5d96024) 

To start printing from the CFS after the spool holder:
1. Open the Fluidd web interface.
2. Press the "T17" button or type "T17" into the console and press "Enter". The spool holder filament will be unloaded.
3. Manually retract the filament from the extruder and tubes.
4. If you do not have a Y-splitter, disconnect the spool holder tube and connect the CFS tube.

> [!WARNING]
> !!! DO NOT USE RETRACT BUTTON IN PRINTER SCREEN FOR UNLOADING FILAMENT!!! USE ONLY T17 FOR IT !!!

### better_Z.cfg (already included in overrides.cfg)
- The Z-axis homing position has been moved to the left back corner of the bed, as this is a more temperature-stable point than the center of the bed.
- prtouch tuned for accuracy \
!!!REMOVE ALL BED MESHES AFTER INSTALL THIS COMPONENT, OR USE AUTO_MESH WITH ALL BED TEMPERATURES BEFORE FIRST PRINT!!!

## Installation:

### Install these scripts to your K2

> [!NOTE]
> Before using main.cfg, please open it, read the comments on the user-defined section, set the desired values, save, and restart firmware. 
> Also, there is an explanation of each virtual pins in the comments in main.cfg.

 1. Install Klipper Virtual Pins https://github.com/pedrolamas/klipper-virtual-pins: Just copy `virtual_pins.py` to you printer `/usr/share/klipper/klippy/extras/` via SSH (find the SSH password from touch UI > Cogwheel > General > Root account)
 1. Upload the `custom` directory of this repo to your printer using the Fluidd web interface.
 1. Edit your `printer.cfg`:
     1. Add after the `[includes ...]` block at the top:
        ```diff
        ...
         [include sensorless.cfg]
         [include gcode_macro.cfg]
         [include printer_params.cfg]
         [include box.cfg]
        +[include custom/main.cfg]
        +[include custom/tool.cfg]

        ...
        ```
     1. Add a the bottom, just before the `SAVE_CONFIG` commented section:
        ```diff
        ...

        +[include custom/overrides.cfg]

         #*# <---------------------- SAVE_CONFIG ---------------------->
         #*# DO NOT EDIT THIS BLOCK OR BELOW. The contents are auto-generated.
         ...
        ```

Example:
![изображение](https://github.com/user-attachments/assets/d2adb77c-587f-4844-a844-545f4fd42174)
![изображение](https://github.com/user-attachments/assets/9f2b6c62-a756-42e8-a3e8-70fc86d4d4e8)
![изображение](https://github.com/user-attachments/assets/aa353b06-e271-4759-b018-69a6830509f7)

Note: `tool.cfg` is optional. Also `overrides.cfg` can be used as is or just partially. Annotation will be added soon.

![изображение](https://github.com/user-attachments/assets/331bd7bf-287d-4d6c-9f20-7ea7645a218d)

### Update your slicer settings

You need change some start g-codes in slicer:

  - Machine start g-code ([recommended](https://github.com/MasterLufier/Creality-K2-Custom-Macros/pull/7)):
    ```
    START_PRINT EXTRUDER_TEMP=[nozzle_temperature_initial_layer] BED_TEMP=[bed_temperature_initial_layer_single] CHAMBER_TEMP=[overall_chamber_temperature]
    T[initial_no_support_extruder] TEMP=[first_layer_temperature] MAX_FLOWRATE=[filament_max_volumetric_speed] FILAMENT_TYPE=[filament_type]
    ```

  - (Machine) Change filament g-code
    ```diff
    G1 E-[old_retract_length] F2400
    G2 Z{z_after_toolchange + 0.4} I0.86 J0.86 P1 F10000 ; spiral lift a little from second lift
    G1 X0 Y345 F30000 ;GO_TO_CUT_POS
    T[next_extruder] TEMP=[new_filament_temp] MAX_FLOWRATE=[filament_max_volumetric_speed]  FILAMENT_TYPE=[filament_type]
    ```

  - In Multimaterial tab in Printer settings you need to switch on *Manual Filament Change*
    ![изображение](https://github.com/user-attachments/assets/c69695b4-2daa-42a4-8690-5e2150cb7631)   

  - In Filament start g-code you need remove or comment:
    ```diff
    -M109 S[nozzle_temperature]
    ```

## For developers:
Extraction of `box_wrapper.cpython-39.so` attributes here:  
https://docs.google.com/spreadsheets/d/16-dBGIGJ-zMNRc8hM-vnQLuDPJgmWSqQlyrfId-jeRs/edit?usp=sharing  

Extraction of `filament_rack_wrapper.cpython-39.so` attributes here:  
https://docs.google.com/spreadsheets/d/1BUP6k6tMjnTPiEdz6MP2wp-nE-Sxzs840_5d1FgZd7s/edit?usp=sharing  
