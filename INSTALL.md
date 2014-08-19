<!-- @LICENSE(NICTA_CORE) -->

# Quick Start

## 0. Make sure you have [Prerequisites](#prerequisites)

## 1. [Checkout](#checkout) CAmkES and the kernel	

   	mkdir camkes-project
	cd camkes-project
	repo init -u https://github.com/smaccm/camkes-smaccm-manifest.git
	repo sync

## 2. [Build](#build) example

	make clean
	make arm_simple_defconfig
	make silendtoldconfig
	make

## 3. [Run](#run) on qemu

	qemu-system-arm -M kzm -nographic -kernel images/capdl-loader-experimental-image-arm-imx31

# Introduction

This is a release of the CAmkES platform and seL4 kernel for the Odroid-XU (Cortex A15) platform with HYP support (as well as all other supported seL4 platforms).  It allows you to develop, build, and run CAmkES-based systems on seL4 on these platforms.

In order to use it, you must first install the Prerequisites.  Then you must checkout the CAmkES repositories.  After that, in order to get a feeling for how things work, you can choose a prepared configuration, build, and run a system either in an emulator or on hardware.  Finally, you can develop your own CAmkES-based system.

<a name="prerequisites"></a>
# Prerequisites

Before building and running CAmkES-based systems you must have several prerequisite software packages installed on the machine you will be using to build.  

## Linux

First of all we assume that you are running Linux.  We typically use some flavour of Ubuntu or other Debian-based system.  You could try using other Unix systems (including Mac OS) but your mileage may vary.  The following instructions assume Ubuntu.

## Toolchains

Building CAmkES reguires: an appropriate cross compiler, Python, Python packages (Jinja2, PLY, pyelftools), Haskell, Cabal (a Haskell package manager) and Haskell packages (MissingH, Data.Ordlist, Split).

### Install a GCC cross-compiler

Do the following to install an appropriate GCC cross-compiler:

	cd /tmp
	sudo mkdir -p /opt/local
 
	wget https://sourcery.mentor.com/public/gnu_toolchain/arm-none-eabi/arm-2013.11-24-arm-none-eabi-i686-pc-linux-gnu.tar.bz2
	tar xf arm-2013.11-24-arm-none-eabi-i686-pc-linux-gnu.tar.bz2
	sudo mv arm-2013.11 /opt/local/
	export PATH=/opt/local/arm-2013.11/bin:$PATH
	echo "export PATH=/opt/local/arm-2013.11/bin:\$PATH" >> ~/.bashrc

Since this compiler is a 32-bit binary, on 64-bit Linux, you should also do:

	sudo apt-get install ia32-libs

Some versions of Ubuntu no longer have `ia32-libs`, in which case you should do:

	sudo apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0

### Install Python and packages

	sudo apt-get install python python-pip python-tempita
	sudo pip install --upgrade pip
	sudo pip install jinja2 ply pyelftools

### Install Haskell and packages

	sudo apt-get install cabal-install
	cabal update
	cabal install MissingH data-ordlist split

## Install git and repo
   	sudo apt-get install git curl
	curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
	chmod a+x ~/bin/repo

### Miscellaneous requirements
	
	sudo apt-get install realpath libxml2-utils libncurses5-dev

## Emulator

Install qemu.

	sudo apt-get install qemu


<a name="checkout"></a>
# Checkout CAmkES

   	mkdir camkes-project
	cd camkes-project
	repo init -u https://github.com/smaccm/camkes-smaccm-manifest.git
	repo sync

<a name="build"></a>
# Building Examples

There are many example configurations in the `configs/` directory.  To build one (for example `arm_simple_defconfig` will build the 'simple' example system) do the following from the CAmkES directory:

	make clean
	make arm_simple_defconfig
	make silendtoldconfig
	make

<a name="run"></a>
# Running Examples 

This runs the system on an emulator.  To run on hardware see the separate `HARDWARE.md` documt.

	qemu-system-arm -M kzm -nographic -kernel images/capdl-loader-experimental-image-arm-imx31

*Note:* quit qemu with `Ctrl-A X`

# More configuration

To have more fine-grained control of the system configuration run:

	make menuconfig

and setup configuration parameters to your liking, exit and save the config file, then simply run 'make' to build with your modified configuration.

# Making your own

See the `README.md` document in camkes-project for more information about making your own CAmkES system.

