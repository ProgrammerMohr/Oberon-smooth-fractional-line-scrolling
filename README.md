# Oberon-smooth-fractional-line-scrolling
Smooth fractional scrolling of displayed texts with variable line spaces for the Project Oberon 2013 operating system

Note: In this repository, the term "Project Oberon 2013" refers to a re-implementation of the original "Project Oberon" on an FPGA development board around 2013, as published at www.projectoberon.com.

**PREREQUISITES**: A current version of Project Oberon 2013 (see http://www.projectoberon.com). If you use Extended Oberon (see http://github.com/andreaspirklbauer/Oberon-extended), the functionality is already implemented.

------------------------------------------------------

The code in this repository enables completely smooth, fractional line scrolling of displayed texts with variable lines spaces and dragging of entire viewers with continuous content refresh. Both *far* (positional) scrolling and *near* (pixel-based) scrolling are realized. The system automatically switches back and forth between the two scrolling modes based on the horizontal position of the mouse pointer.

------------------------------------------------------

**Preparing your Oberon system to support smooth fractional line scrolling**

If *Extended Oberon* is used, smooth fractional line scrolling is already implemented on your system.

If *Project Oberon 2013* is used, follow the instructions below:

------------------------------------------------------

**STEP 1**: Download and import the files to implement smooth fractional line scrolling on your Project Oberon 2013 system

Download all files from the [**Sources**](Sources/) directory of this repository. Convert the *source* files to Oberon format (Oberon uses CR as line endings) using the command [**dos2oberon**](dos2oberon), also available in this repository (example shown for Linux or MacOS):

     for x in *.Mod ; do ./dos2oberon $x $x ; done

Import the files to your Oberon system. If you use an emulator, click on the *PCLink1.Run* link in the *System.Tool* viewer, copy the files to the emulator directory, and execute the following command on the command shell of your host system:

     cd oberon-risc-emu
     for x in *.Mod ; do ./pcreceive.sh $x ; sleep 0.5 ; done

------------------------------------------------------

**STEP 2:** Build the s slightly modified Oberon-07 compiler as follows:

     ORP.Compile ORG.Mod/s ~
     System.Free ORTool ORP ORG ORB ORS ~

This merely sets the constants *maxCode" and *maxStrx* in module *ORG* as follows:

     CONST ...
       maxCode = 8800; maxStrx = 3200; ...

This step is (unfortunately) necessary since the official Oberon-07 compiler has a tick too restrictive constants. To compile the new version of module *TextFrames*, one needs slightly more space (in the compiler) for both *code* and *string constants*.

------------------------------------------------------

**STEP 4:** Use the new compiler rebuild the Outer Core of your system

Compile the following modules of the Oberon system:

     ORP.Compile Display.Mod/s Viewers.Mod/s ~
     ORP.Compile Oberon.Mod/s ~
     ORP.Compile MenuViewers.Mod/s TextFrames.Mod/s ~
     ORP.Compile System.Mod/s Edit.Mod/s Tools.Mod/s ~

Re-compile the enhanced Oberon-07 compiler itself before (!) restarting the system:

     ORP.Compile ORS.Mod/s ORB.Mod/s ~
     ORP.Compile ORG.Mod/s ORP.Mod/s ~
     ORP.Compile ORL.Mod/s ORX.Mod/s ORTool.Mod/s ~

If you don't re-compile the compiler before restarting the Oberon system, you won't be able to start it afterwards!

------------------------------------------------------

**STEP 5:** Restart the Oberon system

You are now running a modified Oberon system with smooth fractional line scrolling.

Re-compile any other modules that you may have on your system.


