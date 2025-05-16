# CoPrint3d_Runout_Sensor_Config
This is the configuration for runout sensors when using the CoPrint3D Color System on their 3D Printers.


There are 3 files in this repository:

#1) test_macro.cfg:  This is the macro to test out your runout sensors by simply typing in "test_macro.cfg" into your console command line when you during a print.  This will simulate your runout sensor activating, so that you can evaluate if it will serve your needs.

#2) test_out_macro.cfg:  This is the macro to test out what happens when you insert filament into your runout sensor, and it begins to auto-load filament, up to the amount that you want to load.  You also run this macro by typing in "test_out_macro.cfg".  Again, this is a way to simulate your sensor activating, so that you can evaluate if it will serve your needs.

#3) runout.cfg:  This is the file that you will put in your config files directory.  This is what will get called when your runout sensor activates, either when it runs out of filament, or when you insert filament into the runout sensor.  
