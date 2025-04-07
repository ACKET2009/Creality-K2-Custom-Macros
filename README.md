# Creality-K2-Custom-Macros
Creality K2 macros for better ORCA Sliser compatibility and other improvements\
Based on ideas: https://github.com/jamincollins great thanks!\
GPL-3.0 license\
Author: Mikhail Ivanov masluf@gmail.com  

> [!WARNING]
> Using custom macros could damage your printer and void your warranty, or cause unexpected behavior.

## Key Features:
### main.cfg
- Fix the exhaust fan to improve Orca's (and Creality Print) "Activate Air Filtration" function.
- Implement dynamic control of the exhaust fan for improved heating, cooling, and air filtration.
- Bed mesh store for different bed temperatures
- Better start printing with an additional fast start option for printing the same model (through the virtual pin). This would be useful for printing smaller parts or testing and soak time up to 10 minutes (through virtual pins as well) for better bed stabilization and the best first layer for large parts or printing many parts at once.
- The toolhead moves quickly after RESUME printing to prevent oozing.
### overrides.cfg
- Increased accuracy of Z_TILT_ADJUST
- Default bed mesh calibration is turned off after printer restart
- prtouch:
  - Disabled console flood
- Max bed temp increased to 120c
- Heater fan speed increased to full. It's ok for heating but you can get best result with my Ultimate Exhaust system https://www.crealitycloud.com/model-detail/67c9f8b07f0b8c17944c377b?source=22
### tool.cfg Please do not use this module if you are not sure what it does.
under development now, but some features work:
- added Toolhead buttons for manual load and unload filaments. T17 for unload any.  \
![изображение](https://github.com/user-attachments/assets/afea66c2-4f16-4baf-859d-b6a7c3ac8330)
- Unloading CFS filament is disabled after printing. This is great for reusing the same filament.
- Spoolholder can be used from ORCA without unplugging CFS and feed filament through buffer! Just add +1 filament to your project (if you have 1 CFS connected - fifth, if 2 CFS-ninth etc.), set it for parts and push print! \
![изображение](https://github.com/user-attachments/assets/f3d3497c-8c7c-4c29-9110-13ea197c1ac1)
#### !!!
You need change some start g-codes in slicer:  
Machine start g-code:  
```
START_PRINT EXTRUDER_TEMP=[nozzle_temperature_initial_layer] BED_TEMP=[bed_temperature_initial_layer_single] CHAMBER_TEMP=[overall_chamber_temperature]
T[initial_no_support_extruder] TEMP=[first_layer_temperature] MAX_FLOWRATE=[filament_max_volumetric_speed]  FILAMENT_TYPE=[filament_type]
```
Change filament g-code
```
G1 E-[old_retract_length] F2400
G2 Z{z_after_toolchange + 0.4} I0.86 J0.86 P1 F10000 ; spiral lift a little from second lift
G1 X0 Y200 F30000
T[next_extruder] TEMP=[first_layer_temperature] MAX_FLOWRATE=[filament_max_volumetric_speed]  FILAMENT_TYPE=[filament_type]
```
In Multimaterial tab in Printer settings you need to switch on Manual Filament Change

![изображение](https://github.com/user-attachments/assets/c69695b4-2daa-42a4-8690-5e2150cb7631)   

In Filament start g-code you need remove or comment all strings!

#### !!!

### better_Z.cfg
- The Z-axis homing position has been moved to the left back corner of the bed, as this is a more temperature-stable point than the center of the bed.
- prtouch tuned for accuracy
- AUTO_MESH is a macro that creates a mesh for each desired temperature. Just fill comma separated list, save, push the button and wait. \
!!!REMOVE ALL BED MESHES AFTER INSTALL THIS COMPONENT, OR USE AUTO_MESH WITH ALL BED TEMPERATURES BEFORE FIRST PRINT!!!

## Installation:
For use you need install Klipper Virtual Pins https://github.com/pedrolamas/klipper-virtual-pins \
Just copy virtual_pins.py to you printer /usr/share/klipper/klippy/extras/ via SSH\
Than copy main.cfg to you pinter config directory Fluidd web interface and include to printer.cfg after all includes.\
you can use tool.cfg or not. If you want, just copy tool.cfg and custom_vars.cfg to printer config directory via Fluidd web interface and include to printer.cfg after main.cfg\
\
Example:
![изображение](https://github.com/user-attachments/assets/d2adb77c-587f-4844-a844-545f4fd42174)
![изображение](https://github.com/user-attachments/assets/9f2b6c62-a756-42e8-a3e8-70fc86d4d4e8)
![изображение](https://github.com/user-attachments/assets/aa353b06-e271-4759-b018-69a6830509f7)

overrides.cfg need to be included in printer.cfg  after all blocs before SAVE_CONFIG section \
you can use overrides.cfg partialy or full. Annotation will be added soon.

![изображение](https://github.com/user-attachments/assets/331bd7bf-287d-4d6c-9f20-7ea7645a218d) \
## For developers:
Extraction of box_wrapper.cpython-39.so attributes here:  
https://docs.google.com/spreadsheets/d/16-dBGIGJ-zMNRc8hM-vnQLuDPJgmWSqQlyrfId-jeRs/edit?usp=sharing  
Extraction of filament_rack_wrapper.cpython-39.so attributes here:  
https://docs.google.com/spreadsheets/d/1BUP6k6tMjnTPiEdz6MP2wp-nE-Sxzs840_5d1FgZd7s/edit?usp=sharing  
