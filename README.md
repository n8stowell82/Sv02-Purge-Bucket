# Sv02-Purge-Bucket with Cura Gcode
Purge bucket model and gcode for Sovol Sv02 3d printer

Before you do anything, create a new printer profile in Cura

## Create new printer profile
Create a new profile so that if you decide this isn't for you, it will be really easy to just ignore these changes by slicing with the original printer profile

- Settings -> Printer -> Add Printer
<img width="534" alt="image" src="https://user-images.githubusercontent.com/1544580/221338994-4e6a092f-2766-43a0-ae89-4118466c4975.png">

- Select Sovol Sv02 from the list of available printers
<img width="786" alt="image" src="https://user-images.githubusercontent.com/1544580/221339032-10e752c0-faf5-44e0-b418-9200ea528f5b.png">

- Name the new printer something unique I.E. "sv02-purge-bucket"

## Modify Printer Gcode
Update the printer's unique gcode to make use of the new purge bucket

- Settings -> Printer -> Manage Printers
<img width="529" alt="image" src="https://user-images.githubusercontent.com/1544580/221339289-801400d7-2ceb-4be4-9402-08c9d1f1c54a.png">

- Select "Machine Settings"
<img width="780" alt="image" src="https://user-images.githubusercontent.com/1544580/221339316-da6dc3fb-a06b-4d9c-aa75-421c611a48a2.png">

- insert the following gcode in the appropriate sections:

## Printer start code
```
G21 ; Metric values
M201 X500.00 Y500.00 Z100.00 E5000.00 ;Setup machine max acceleration
M203 X500.00 Y500.00 Z10.00 E50.00 ;Setup machine max feedrate
M204 P500.00 R1000.00 T500.00 ;Setup Print/Retract/Travel acceleration
M205 X8.00 Y8.00 Z0.40 E5.00 ;Setup Jerk
M220 S100 ;Reset Feedrate
M221 S100 ;Reset Flowrate
G28 ; Home all
G29 ; Level Bed
G90 ; Absolute positioning ON
M107 ; Fan OFF
G1 Z15 F2400 ; Raise nozzle 15mm
T0 ; Extruder 1 ON
M211 X1 S0 ; Disable soft endstops
G1 X315 F6000 ; Move to bucket
G92 E0 ; Zero extruder
G1 E90 F500 ; Load filament
G92 E0 ; Zero extruder
G1 E60 F100 ; Purge
G92 E0 ; Zero extruder
G1 E-3 F3000 ; Short retract
G92 E0 ; Zero extruder
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G1 X2.0 Y40 Z0.28 F5000.0 ;Move to start position
G1 X2.0 Y200.0 Z0.28 F1500.0 E15 ;Draw the first line
G1 X2.4 Y200.0 Z0.28 F5000.0 ;Move to side a little
G1 X2.4 Y40 Z0.28 F1500.0 E30 ;Draw the second line
G1 Y20 F5000 ; Wipe
G92 E0 ;Reset Extruder
G1 Z2.0 F3000 ;Move Z Axis up
```

## Printer end code
```
G91 ;Relative positioning
G1 E-6 F2700 ;Retract a bit
G1 E0 Z0.2 F2400 ;Retract and raise Z
G1 X5 Y5 F3000 ;Wipe out
G1 Z10 ;Raise Z more
G1 E-84 F2000 ;Withdraw filament
G90 ;Absolute positionning
G1 X Y230 ;Present print
M106 S0 ;Turn-off fan
M104 S0 ;Turn-off hotend
M140 S0 ;Turn-off bed
M84 X Y E ;Disable all steppers but Z
```

## Left Extruder start code
```
;Remove default gcode
;Leave blank
;You don't need anything here
```

## Left Extruder end code
```
M104 S216 ; Set temp to 216 (one degree over print temp)
M211 X1 S0 ; Disable endstops
G91 ; Relative positioning ON
G1 E-6 F2700 ;Retract a bit
M400 ; Wait for finish
G1 E-4 Z0.1 F2400 ;Retract and raise Z
G1 X0.4 Y0.6 F3000 ;Wipe out
G1 Z0.9 F1000 ; Raise nozzle by 1mm
G90 ; Absolute positioning ON
G92 E0 ; Zero extruder
G1 E-80 F2000 ; Withdraw filament
G1 X315 F6000 ; Go to Purge Bucket
G92 E0 ; Zero extruder
T1 ; Extruder 2 ON
G92 E0 ; Zero extruder
G0 E90 F1800 ; Reload filament
G92 E0 ; Zero extruder
G0 E60 F180 ; Purge
G92 E0 ; Zero extruder
G1 E-0.00 F3000 ; Short retract
G92 E0 ; Zero extruder
M400 ; Wait for finish
G4 P1500 ; Delay 1.5 seconds
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G92 E0 ; Zero extruder
G91 ; Relative positioning ON
G1 Z-1 F1000 ; Lower nozzle by 1mm
G90 ; Absolute positioning ON
M400 ; Wait for finish
```

## Right Extruder start code
```
;Remove default gcode
;Leave blank
;You don't need anything here
```

## Right Extruder end code
```
M104 S216 ; Set temp to 216 (one degree over print temp)
M211 X1 S0 ; Disable endstops
G91 ; Relative positioning ON
G1 E-6 F2700 ;Retract a bit
M400 ; Wait for finish
G1 E-4 Z0.1 F2400 ;Retract and raise Z
G1 X0.4 Y0.4 F3000 ;Wipe out
G1 Z0.9 F1000 ; Raise nozzle by 1mm
G90 ; Absolute positioning ON
G92 E0 ; Zero extruder
G1 E-80 F2000 ; Withdraw filament
G1 X315 F6000 ; Go to Purge Bucket
G92 E0 ; Zero extruder
T0 ; Extruder 1 ON
G92 E0 ; Zero extruder
G0 E90 F1800 ; Reload filament
G92 E0 ; Zero extruder
G0 E60 F180 ; Purge
G92 E0 ; Zero extruder
G1 E-0.00 F3000 ; Short retract
G92 E0 ; Zero extruder
M400 ; Wait for finish
G4 P1500 ; Delay 1.5 seconds
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G1 X295 F6000 ; Wipe
G1 X315 F6000 ; Wipe
G92 E0 ; Zero extruder
G91 ; Relative positioning ON
G1 Z-1 F1000 ; Lower nozzle by 1mm
G90 ; Absolute positioning ON
M400 ; Wait for finish
```
