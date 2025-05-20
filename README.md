# CoPrint3d_Runout_Sensor_Config
This is the configuration for runout sensors when using the CoPrint3D Color System on their 3D Printers.

**How it works:**  
When your runout sensor gets activated, it will begin keeping track of the amount of filament that is being dispensed, and how much there is left before it reaches a pre-determined amount that you choose.  Once that number is reached, the 3D Printer will put itself into "Pause" state and will cut the filament and retract it, so that can be removed from the ChromaHead by removing the bowden tube from the 8in1, and pulling out what's left of the filament.  It will then inform you that your 3D Printer is now ready to have you insert new filament into the extruder that ran out.

Once you have cleared out the filament, and put back the bowden tube into the 8in1, you will then feed fresh filament into the runout sensor, which will trigger a 30 second countdown on the console.  This should give you plenty of time to feed the filament past the runout sensor and onto the extruder feeding mechanism.  When the 30 seconds are up, the printer will move the Head to X-Position=0, and then begin loading your filament.  It will give you updates on how much filament is being loaded in maximum of 300mm/min, until the target amount is reached, which you have decided ahead of time.  You should optimally see filament coming out of the printer nozzle.  It will then tell you that the filament has been loaded, to clean the nozzle, and press the continue button on your console.  

Your printer will go back to printing where it left off, happily on its way.  Yay!

**There are 4 files in this repository:**  
#1) **test_macro.cfg**:  This is the macro to test out your runout sensors by simply typing in "_FILAMENT_RUNOUT" into your console command line during a 3D print.  This will simulate your runout sensor activating, so that you can evaluate if it will serve your needs.

#2) **test_out_macro.cfg**:  This is the macro to test out what happens when you insert filament into your runout sensor, and it begins to auto-load filament, up to the amount that you want to load.  You also run this macro by typing in "test_out_macro.cfg".  Again, this is a way to simulate your sensor activating, so that you can evaluate if it will serve your needs.

#3) **runout.cfg**:  This is the file that you will put in your config files directory.  This is what will get called when your runout sensor activates, either when it runs out of filament, or when you insert filament into the runout sensor.  

#4) **save_variables.cfg**:  This is where all of the variables are going to be stored to keep track of everything that all 3 of the files need in order to make the macros work.  Place this file in your config files directory as well.

**Configuration Changes Needed**  
There are some changes that will need to be made, in order for everything to work properly when you decide to use the runout.cfg file on your printer to have it installed.  

**CP_MACRO.CFG**  
add the following lines to the sections that are in brackets[ ].  
`[gcode_macro START_PRINT]`  
`variable_started_printing: False`  
`gcode:`  
`    SAVE_VARIABLE VARIABLE=started_printing VALUE=True  ;  turns off filament runout sensor from auto-loading filament`  

`[gcode_macro END_PRINT]`    
`gcode:`  
`    SAVE_VARIABLE VARIABLE=started_printing VALUE=False   ;  turns off filament runout sensor from auto-loading filament`  

`[gcode_macro CANCEL_PRINT]`  
`gcode:`  
`    SAVE_VARIABLE VARIABLE=started_printing VALUE=False   ;  turns off filament runout sensor from auto-loading filament`  

`[gcode_macro END_PRINT]`  
`gcode:`  
`    SAVE_VARIABLE VARIABLE=started_printing VALUE=False   ;  turns off filament runout sensor from auto-loading filament`  

**PRINTER.CFG**  
`[include runout.cfg]`  
`[include test_macro.cfg]`  ; only put this in, if you're going to be using this file to test out the macro runout sensor  
`[include test_out_macro.cfg]  ; only put this in, if you're going to be using this file to test out the macro runout sensor`  

**Frequently Asked Questions**  
***Question 1:***  What happens if the printer calls for a filament change before it completes the filament extrusion countdown  
***Answer 1:***  The program will detect a filament change.  It will inform you that there is a filament change scheduled, will pause the printer prematurely and cut and retract the filament so that it can be changed out, before the filament change command happens.  

***Question 2:***  What happens when the printer loads the runout filament while there is a pending change of filament scheduled?  
***Answer 2:***  The program will load your runout filament as usual, then it will detect whether there is an upcoming change to the filament.  It will then retract the new filament and set it up to be called.  It will then extrude the upcoming filament so that it can be ready to print with the filament that didn't run out immediately as you hit the resume button again.  

***Question 3:***   Where do I change the amount of filament that is extruded when the runout sensor is triggered?  
***Answer 3:***  You will find it in the macro below.  The default is 800mm.  Simply change the number to the amount of millimeters you want to be extruded before your 3D printer goes into Pause State.  
`[gcode_macro _TRACK_TARGET_FILAMENT_RUNOUT]`  
`SAVE_VARIABLE VARIABLE=target_filament_runout VALUE=800`  

***Question 4:***  Where do I change the amount of filament that is loaded when the runout sensor detects me loading filament?  
***Answer 4:***  You will find it in the macro below.  This is done by a series of G1 commands.  E300 shows how much filament to load and the F300 is the speed at which to load it.  You can load up to 300mm per instance.  Simply copy the 4 lines in the macro as many times as you want to load up filament and add it together.  The default I made was 1050mm.  You can change this by adding or removing those 4 lines to your hearts content.  
`[gcode_macro _LOAD_FILAMENT_AFTER_RUNOUT]`  
`G92 E0        ;  resets extruder position`  
`G1 E300 F300  ;  load filament 300mm at 300mm/min`  
`M400          ;  ensures all movements are completed before proceeding`  
`RESPOND MSG="🔄 300mm of filament loaded 🔄 - ⏳ 750mm remaining to target (1050mm) ⏳"   ;  gives a tally of filament loaded`  
