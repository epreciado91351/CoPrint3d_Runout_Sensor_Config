# CoPrint3d_Runout_Sensor_Config
This is the configuration for runout sensors when using the CoPrint3D Color System on their 3D Printers.

**How it works:**  
When your runout sensor gets activated, it will begin keeping track of the amount of filament that is being dispensed, and how much there is left before it reaches a pre-determined amount that you choose.  Once that number is reached, the 3D Printer will put itself into "Pause" state and will cut the filament and retract it, so that can be removed from the ChromaHead by removing the bowden tube from the 8in1, and pulling out what's left of the filament.  It will then inform you that your 3D Printer is now ready to have you insert new filament into the extruder that ran out.

Once you have cleared out the filament, and put back the bowden tube into the 8in1, you will then feed fresh filament into the runout sensor, which will trigger a 30 second countdown on the console.  This should give you plenty of time to feed the filament past the runout sensor and onto the extruder feeding mechanism.  When the 30 seconds are up, the printer will move the Head to X-Position=0, and then begin loading your filament.  It will give you updates on how much filament is being loaded in maximum of 300mm/min, until the target amount is reached, which you have decided ahead of time.  You should optimally see filament coming out of the printer nozzle.  It will then tell you that the filament has been loaded, to clean the nozzle, and press the continue button on your console.  

Your printer will go back to printing where it left off, happily on its way.  Yay!

**There are 4 files in this repository:**  
#1) test_macro.cfg:  This is the macro to test out your runout sensors by simply typing in "_FILAMENT_RUNOUT" into your console command line during a 3D print.  This will simulate your runout sensor activating, so that you can evaluate if it will serve your needs.

#2) test_out_macro.cfg:  This is the macro to test out what happens when you insert filament into your runout sensor, and it begins to auto-load filament, up to the amount that you want to load.  You also run this macro by typing in "test_out_macro.cfg".  Again, this is a way to simulate your sensor activating, so that you can evaluate if it will serve your needs.

#3) runout.cfg:  This is the file that you will put in your config files directory.  This is what will get called when your runout sensor activates, either when it runs out of filament, or when you insert filament into the runout sensor.  

#4) save_variables.cfg:  This is where all of the variables are going to be stored to keep track of everything that all 3 of the files need in order to make the macros work.  Place this file in your config files directory as well.

**Configuration Changes Needed**  
There are some changes that will need to be made, in order for everything to work properly when you decide to use the runout.cfg file on our printer to have it installed.  

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
