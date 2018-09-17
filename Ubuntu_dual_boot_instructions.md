# Ubuntu 14.04 LTS OR 16.04 LTS dual boot installation instruction (with windows) on laptop:

#### Create bootable USB drive:
* First you have to create a ubuntu bootable usb drive. So, format a (minimum size 2GB) usb drive.
* Download the [Rufus USB Installer](https://rufus.akeo.ie/downloads/rufus-2.12.exe) to create a bootable usb drive.

#### Download Ubuntu image:
* Download the Ubuntu image (64 bit i.e. the amd64) from this [link for Ubuntu 14](http://releases.ubuntu.com/14.04/) or from this [link for Ubunut 16](http://releases.ubuntu.com/16.04/).

#### Convert the USB into bootable drive:
* Then convert the usb stick into a bootable one, using the Rufus software. Open it, select the device and the option for ISO image and select the image.
* The instructions are given [here](https://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows)

#### Settings on laptop:
* In the laptop where you want to have the dual boot, go to disk management in windows, shrink an existing drive so as to create an unallocated drive.
* The instructions to create a disk partition is given [here](http://www.wikihow.com/Create-a-Partition).
* Then plug in the usb stick and restart laptop. Press F12 continuously or repeatedly to get into boot options while starting. 
* Then select the usb stick from the boot options.
* Then select the option of **install ubuntu**. Instructions are given [here](https://www.youtube.com/watch?v=uGdrQxA0E6g).
* Select language and check the options of **installing updates** and **3rd party software** during installation.
* Select **something else** option when it comes to installation type menu.
* Then select the free space memory (the unallocated space) that you previously created, and click on the check box kind of option or from the given options and click the '+' sign.
* You have to keep some space for a virtual ram or swap space (if your ram is not too big like 64gb or something). So keep it same as the size of the original ram or may be (e.g. 4GB or something like that).
* So reduce the size of the partition by that swap space amount and select the option **logical** and the mount point as **/**.



