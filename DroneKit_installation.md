# DroneKit installation instruction in Ubuntu 16.04 LTS on laptop or Odroid XU4:

Official instructions are given in [Instructions link](https://dev.px4.io/en/robotics/dronekit.html)
But the following instruction also have added information about some errors encountered during installation.

#### Install Dependencies
1. `pip install future`
2. `sudo apt-get install python-dev libxml2-dev libxslt1-dev zlib1g-dev`
3. `sudo apt-get install build-essential autoconf libtool pkg-config python-opengl python-imaging python-pyrex python-pyside.qtopengl idle-python2.7 qt4-dev-tools qt4-designer libqtgui4 libqtcore4`
4. `sudo apt-get install libqt4-xml libqt4-test libqt4-script libqt4-network libqt4-dbus python-qt4 python-qt4-gl libgle3 python-dev libssl-dev`
5. `sudo pip install greenlet`
6. `sudo pip install gevent`

#### Clone the Repository:
```
sudo apt-get update
git clone https://github.com/dronekit/dronekit-python.git
```

#### Build Repository:
```
cd ./dronekit-python
sudo python setup.py build
```

#### Run Installation:
```
sudo python setup.py install
```

If there is an error saying:
```
ERROR: /bin/sh: 1: xslt-config: not found
```
[ **NOTE:** This error is written not at the very end of the set of printed messages in the terminal, but a little before that. ]

Then that means you do not have all the modules of the dependencies **2** as mentioned earlier under **Install Dependencies**.
More help is available in this [link](https://stackoverflow.com/questions/5178416/pip-install-lxml-error).

If there is an error saying:
```
error: Setup script exited with error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
```

Then this means you do not have all the modules of the dependencies **3** or **4** as mentioned earlier under **Install Dependencies**.
More help is available in this [link](https://stackoverflow.com/questions/26053982/setup-script-exited-with-error-command-x86-64-linux-gnu-gcc-failed-with-exit).
This link also asks to install the packages **greenlet** and **gevent** by **easy_install**, 
but you may also do it using **pip** (suggested in this [link](https://askubuntu.com/questions/350312/i-am-not-able-to-install-easy-install-in-my-ubuntu)) as shown in **Install Dependencies**.

After resolving the errors, try running `sudo python setup.py install` again.

#### Run and Check:
Now after installation is done without errors, try to check if it can be imported into python without any errors
