# Installing Anbox package for using Android apps in Ubuntu:

Parts of the **Anbox** package installation instructions are there in these links [1](https://www.fosslinux.com/13176/how-to-install-and-run-android-apps-on-ubuntu-using-anbox.htm), [2](https://www.youtube.com/watch?v=rvsni3AIB9I)

#### System Update and Add Anbox Repo to your system:

In this section, we will add the PPA to your Linux system and install the essential and appropriate anbox-modules-dkms package, which contains the kernel modules.

```
sudo apt update
sudo add-apt-repository ppa:morphis/anbox-support
sudo apt update
```

#### Install Kernel Modules:

Install appropriate kernel modules using the following commands:

```
sudo apt install anbox-modules-dkms
```

The last command may ask for passwords inside the terminal itself and you have to enter it if asked.

Now run the following commands to start kernel modules manually:

```
sudo modprobe ashmem_linux
```

You may get errors like the following if you run the first command.

```
modprobe: ERROR: could not insert 'ashmem_linux': Required key not available 
```

So for this command to work, you have to disable the secure boot.
So reboot the system and go to the BIOS or UEFI and then disable the Secure Boot, as suggested by this [link](https://askubuntu.com/questions/762254/why-do-i-get-required-key-not-available-when-install-3rd-party-kernel-modules).

Next run the following.

```
sudo modprobe binder_linux
```

#### Verify Kernel Modules:

Now, let's verify that new kernel modules have been installed successfully.
Now you should have two new nodes in your systems **/dev** directory:

```
ls -1 /dev/{ashmem,binder}
```

This will output:

```
/dev/ashmem
/dev/binder
```

#### Anbox Installation using Snap:

Now we will install the Anbox using the snap command. First, ensure that you have snap installed. 
If it is not there then install snap using this [guide](https://www.fosslinux.com/10380/what-are-snaps-and-how-to-install-it-on-various-linux-distributions.htm).

```
snap --version
```

If it is already there, then you will get an output like the following:

```
snap    2.39.3
snapd   2.39.3
series  16
ubuntu  16.04
kernel  4.15.0-55-generic
```

Install Anbox. Note that since it is still in the development phase, we will download the beta version.

```
sudo snap install --devmode --beta anbox
```

After the installation completes the output successfully should look like below:

```
anbox (beta) 4-e1ecd04 from morphis installed
```

Information about anbox can be obtained with the following command:

```
snap info anbox
```

#### To install android app store:

In Debian, Ubuntu or Linux Mint, use this command to install the required dependencies and then clone the script for installing the playstore.

```
sudo apt install wget lzip unzip squashfs-tools curl
git clone https://github.com/geeks-r-us/anbox-playstore-installer.git
```

This will create a directory called 'anbox-playstore-installer'.

```
cd anbox-playstore-installer 
sudo ./install-playstore.sh 
```

Some apps like instagram potentially relies on a dynamic library built for ARM CPU rather than x86 as suggested by this [link](https://www.linuxuprising.com/2018/07/anbox-how-to-install-google-play-store.html).
So enable the ARM applications. This is a reference [link](https://github.com/anbox/anbox/issues/800).

```
sudo ./install-houdini-only.sh 
sudo reboot
```

#### Configure the play store:

After the play store is installed, go to **Settings -> Apps -> Google Play Services -> Permissions** and enable all available permissions. Do the same for **Google Play Store** app as well.

Now you can use the playstore to install apps just like you do on a phone.

#### Another way of installing apps using ADB:

References: [1](https://www.maketecheasier.com/run-android-apps-on-ubuntu/), [2](https://www.youtube.com/watch?v=rvsni3AIB9I)

This is still very rough. You need to use the ADB (Android Debug Bridge).
Open a terminal and install the necessary packages with apt.

```
sudo apt install android-tools-adb android-tools-fastboot
```

After they're done installing, you can go to this [website](https://www.apkmirror.com), to pick up some Android app packages. 
You can't export them from your phone because Anbox is running as an x86 computer, not ARM. That's an important thing to keep in mind as you're looking for apps.

It's also important to remember that not every app will work. Currently, there's no way to get the Play Store or Google Play Services working in Anbox. 
As a result, no apps that require Play Services to work will.

Once you have an app to install, you can use ADB to do it. While Anbox is running, open a terminal and type the following command. The app will be installed in Anbox.

```
sudo su         [ become root ]
adb install <name-of.apk>
```

