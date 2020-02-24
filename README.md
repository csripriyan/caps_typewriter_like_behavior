#Bringing typewriter's CAPS behavior to Linux PC

The objective of this blog is to make the caps lock button behave like the typewriter's caps key. After doing the below modification the following behavior is expected.

Action                - Expected output
-----------------------------------
Press Caps lock       - Caps is ON
Press Caps lock again - No Change. 
Press a Shift key     - Caps is OFF.

However note that the Shift key's original functionality remains intact.

This has been tested on Debian 8 version.

This solution involves modifying xserver's input driver. So you need to download the following package and compile it.

Download xserver-xorg-input-evdev_2.10.5.orig.tar.gz from http://old-releases.ubuntu.com/ubuntu/pool/universe/x/xserver-xorg-input-evdev/

Prerequisites:

The following packages need to be installed to compile the code.

apt install make
apt install build-essential
apt install pkg-config
apt install libevdev-dev
apt install xserver-xorg-dev
apt install libmtdev-dev
apt install libudev-dev

Now we are ready to compile the code. 
Let's decompress, configure and apply the patch before compiling.

Assuming the code package is in the Downloads folder, just change directory to ~/Downloads and extract,
$ tar xvfz xserver-xorg-input-evdev_2.10.5.orig.tar.gz

Now change directory to the new folder and run configure,
$ cd ~/Downloads/xf86-input-evdev-2.10.5/
$ ./configure --prefix=/usr

Just issue make to ensure it compiles,
$ make

Assuming the patch file is available in the Downloads folder, issue the below command to apply the patch. 
$ patch -b < ~/Downloads/typewriter_caps_exact_windows_behavior.patch

Now that the patch is applied; recompile the code again,
$ make

Use this compiled driver to overwrite the original driver file. (Make a copy of the original evdev_drv.so, just in case!)
cp -a ~/Downloads/xf86-input-evdev-2.10.5/src/.libs/evdev_drv.so /usr/lib/xorg/modules/input/evdev_drv.so



