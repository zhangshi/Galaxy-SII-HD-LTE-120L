 ========================================
 
   How to build ICS kernel for Galaxy-SII-HD-LTE-120L
 
 ========================================
 
 
 (1) Create a directory for CM9 and initialize it:  

 repo init -u  git://github.com/CyanogenMod/android.git -b ics


(2) Update ~/<cm9_folder>/.repo/local_manifest.xml with this line:

  <project name="CyanogenMod/android_kernel_samsung_msm8660-common" path="kernel/samsung/msm8660-common" remote="github" revision="ics" />


(3) Under your CM9 root folder, type:  repo sync -j4


(4) Get Toolchain (i.e. a set of compiler tools meant for your device).  You may already have the correct one under ~/<cm9_folder>/prebuilt/linux-x86/toolchain/arm-eabi-4.4.3/bin/arm-eabi-
	    	
   	Feature : ARM
	Target OS : "EABI"
        Release : "Sourcery G++ Lite 2009q3-68"
        package : "IA32 GNU/Linux TAR"


(5) Under kernel/samsung/msm8660-common, edit Makefile for compile.

    edit "CROSS_COMPILE" to the correct toolchain path which you downloaded.

    ex) ARCH  ?= arm
        CROSS_COMPILE	?= /home/dsixda/android/system/prebuilt/linux-x86/toolchain/arm-eabi-4.4.3/bin/arm-eabi-   //You have to modify this path!


(6) Copy "cyanogenmod_celoxhd_defconfig" to kernel/samsung/msm8660-common/arch/arm/configs/

    You can modify it if you want.


Skip the rest of the steps if you are building the kernel with the ROM (BoardConfig.mk: TARGET_KERNEL_CONFIG and TARGET_KERNEL_SOURCE have been set).  Step 8 may be important however.

If you want to build the kernel by itself, then follow the rest of the steps.


(7) From kernel/samsung/msm8660-common, compile as follows:

    $ make clean
    $ make mrproper
    $ make cyanogenmod_celoxhd_defconfig
    $ make


(8) If you get a compilation error for 'melfas_ts', open up drivers/input/touchscreen/melfas_ts.c and change the following line:

	#include <mcs8000_download.h>

	to:

	#include "mcs8000_download.h"


(9) If successfully built, get the zImage at: kernel/samsung/msm8660-common/arch/arm/boot/zImage.  Remember also to get the module files (*.ko), which are shown in the output at the end of the build with their full paths.  These normally go under /system/lib/modules of your ROM.
 