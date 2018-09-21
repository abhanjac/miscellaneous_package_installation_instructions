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
* Then select the free space memory (the unallocated space) that you previously created, and click on the check box kind of option or from the given options and click the **+** sign.
* You have to keep some space for a virtual ram or swap space (if your ram is not too big like 64gb or something). So keep it same as the size of the original ram or may be (e.g. 4GB or something like that).
* So reduce the size of the partition by that swap space amount and select the option **logical** and the mount point as **/**.
* Here you will also find option to select the option for swap space from a drop down.
* Use this 4GB or whatever space you have created, as **swap area** in the same manner using the **+** sign and then selecting the **use as** option as **swap area**.
* The select the **install now** option.

### Additional PARTITION Steps Required for Windows 10 PC with only one drive (C:\) (OPTIONAL):
* Turn off **Fast Startup** from **Power Settings** on windows side.
* Make sure you have **UEFI** as the **BIOS** type with **Secure Boot** enabled.
* When you try to partition Windows(C: drive), you first have to shrink the volume. However, this drive will be used crash dump, virtual memory (paging), etc. 
It will not allow you to shrink.

You can use MiniTool Partition Wizard software for this. Download it [here](https://www.partitionwizard.com/partitionmanager/not-enough-space-available-on-the-disk-to-complete-this-operation.html) and use it similar to the disk management. After you have done any changes you have to click the 'apply' button to make the changes permanent. 

In some cases when you want to partition the C drive it will ask you to restart the laptop to do that. Do restart and the partitioning will be done automatically. 
Sometimes when you also have ubuntu installed as dual boot, then while this restart is done, it will boot into ubuntu command line directly. In that case type **exit** and select the **windows** option for booting.

To fix this Disk Management error 'not enough space', you need to disable the system files as many as you can at this very moment. 
* Disable System Protection in **Control Panel\System and Security\System\System Protection** and then go to **configure** and **disable system protection** and **apply**.
* Run Disk Defragment. Type in **disk defragmenter** in the search box, and the defragment utility should show at the top of the search results. 

Optimize the c and d drives




