<!-- 
 Copyright 2014, NICTA

 This software may be distributed and modified according to the terms of
 the BSD 2-Clause license. Note that NO WARRANTY is provided.
 See "LICENSE_BSD2.txt" for details.

 @TAG(NICTA_BSD) 
-->

# CAmkES and seL4 on Odroid-XU Hardware

In order to run on hardware you will need an Odroid-XU from [hardkernel](http://www.hardkernel.com/).  You will need a appropriate serial to USB cable and a micro-USB cable to connect the Odroid-XU to your host computer.  

## Prerequisites

<a name="hostsoftware"></a>
### Host Software

#### Install minicom, fastboot. and u-boot-tools:

	sudo apt-get install minicom android-tools-fastboot u-boot-tools

#### Configure minicom:

Figure out what port you will connect your Odroid-XUs UART to.

  * typically this is `/dev/ttyUSB0`
  * if not, then plugin in the UART cable to your Odroid-XU and the host computer
  * run `dmesg` and see what device name was assigned to it
  * use this value below for the `pu port` configuration option

Create a minicom configuration file:

	cat << EOF > ~/.minirc.odroid
	pu port             /dev/ttyUSB0
	pu baudrate         115200
	pu bits             8
	pu parity           N
	pu stopbits         1
	pu rtscts           No
	EOF

If you need to (re)configure from within minicom these settings are:

	115200 8N1, No HW/SW flow control

#### Configure fastboot

	echo SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", MODE=="0666", GROUP=="users" | sudo tee /etc/udev/rules.d/40-odroidxu-fastboot.rules


### Flash u-boot on Odroid-XU

The Odroid-XU seL4 kernel requires an updated u-boot bootloader on the hardware.  You will have to reflash a new image before you run on the Odroid-XU.

Use the provided bl2 and u-boot images.

 * bl2: `odroid/smdk5410-spl.bin.signed`
 * u-boot: `odroid/u-boot.bin`

*Note:* You will need to install and configure *minicom* and *fastboot* on your host.  See [Prerequisites: Host Software](#hostsoftware).

The procedure to update u-boot is:

1. Connect a UART cable from the Odroid-XU to your host machine.  
2. Start `minicom odroid` in the host in a separate window
3. Connect a micro-USB cable from the Odroid-XU to your host machine
4. Boot the Odroid-XU into u-boot and enter the command `fastboot` in minicom
  * On the host machine, run `sudo fastboot devices` to check the
    connection. If it doesn't work, try restarting the Odroid-XU.
5. On the host, run:

		sudo fastboot flash bl2 smdk5410-spl.bin.signed
 		sudo fastboot flash bootloader u-boot.bin
	 	sudo fastboot reboot

6. When the Odroid-XU reboots, in minicom press Space or Enter to stop it from automatically booting into Linux.  You can type `fastboot` to start fastboot at this point instead.  However doing this everytime you reboot can get annoying, so reconfigure the boot command such that it automatically runs fastboot instead.  At the u-boot prompt do the following:

	setenv bootcmd fastboot
	saveenv

Note, if you do boot into Linux, some versions of Linux come with a boot.ini script that automatically replaces u-boot. Disabling of this feature requires some simple
modification of the boot.ini file which lives on the SD/eMMC. This file
can be modified with any text editor.


## Connect Odroid-XU to Host

Connect a UART cable to the Odroid-XU's UART port and to the host computer.

Connect a micro-USB cable to the Odroid-XU's USB port and the host computer.

Start minicom on host in separate window

	minicom odroid

*Note:* if your user is not already in the `dialout` group you may need to run minicom with `sudo` or add your user to the `dialout` group (for example, by editing `/etc/group`).


## Build for Odroid-XU and make image

Build a system for the Odroid-XU. For example:

	make arm_simple_defconfig
	make silentoldconfig
	make menuconfig   # edit config to choose the Odroid-XU kernel
	make

Convert to an Odroid image

	cd images/
	mkimage -a 0x48000000 -e 0x48000000 -C none -A arm -T kernel -O qnx -d capdl-loader-experimental-image-arm-exynos5 odroid-image


## Load image onto Odroid-XU and run

Turn on Odroid-XU

Watch the Odroid-XU start in minicom window.  If necessary execute fastboot on the Odroid-XU by typing into minicom:

	fastboot

Execute fastboot on the host:

	sudo fastboot boot odroid-image

Watch your system run.

Restart the Odroid-XU to start again.

