# Installing Moveit package for ROS in python:

The **Moveit** package tutorial is obtained in this [link](http://docs.ros.org/melodic/api/moveit_tutorials/html/doc/getting_started/getting_started.html)

This package is used for robotic arm manipulation. 

#### Install Moveit:
```
sudo apt install ros-kinetic-moveit
```
If you are using ros melodic, then just replace the word kinetic to melodic in the above command.

#### Run and Check:
Once the package has been installed, you can try to import it in a python script and see if it works properly.
Just see if you can import moveit_commander in python.

```
python
>>> import moveit_commander
```

