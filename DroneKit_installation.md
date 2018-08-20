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

