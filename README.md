# DNDC-Training
Training Repository containing the Model executable, example site files and training modules

DNDC_HPC (WINDOWS and LINUX versions)

This is the High Performance Cluster Version of CANADA DNDC (DNDCv.CAN). It can be run in LINUX with DNDCgo.exe or Windows depending on the Cmake compile. There is no GUI available with the HPC version of the model and it requires the user to understand the intended structure of the DNDC model input file (.dnd). If you need to make a new .dnd file and are unsure how to edit it then use the windows GUI version of the model (see below) to create and save a dnd file and then run with the HPC version.

DNDCv.CAN (DNDC_GUI).

This has a GUI version built off the legacy version of DNDC95 and has been updated to support DNDC.vCAN. The model can be run using windows for a single site and for multiple sites at once using either site run or batch run commands. There is also a command line version of this windows version that can be run but it has been made obsolete by the DNDC_HPC command line version of the model. The DNDCv.CAN (DNDC_Windows) will be slowly shelved in developement and only the DNDC_HPC version will be supported for new development additions.


Please unzip DNDC_HPC_NOV2026.zip to C:\  
It should extract to C:\DNDC_WIN
the executable for windows will be named C:\DNDC\DNDCgo.exe

Please unzip DNDC_GUI_NOV2026.zip to C:\
It should extract to C:\DNDC
the excutable for windows will be name C:\DNDC\DNDC95.exe
