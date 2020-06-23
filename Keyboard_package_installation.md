# Installing Keyboard package:

The **Keyboard** package can be used to detect keys which are pressed on the keyboard or anything else related to the keyboard actions.
But here we will only site the example for detecting key presses on the keyboard. 

This package is obtained in this [link](https://pypi.org/project/keyboard/) and the installation instructions are also given in details in there.

This package can work in both Python 2 and Python 3, but needs **sudo** permissions to work. 

#### Install Keyboard:
```
pip install keyboard
```

If it asks for permissions, then use **sudo**

```
sudo pip install keyboard
```

If you want to specifically install it for Python 2 then use

```
sudo pip2 install keyboard
```

#### Run and Check:
Once the package has been installed, you can try to import it in a python script and see if it works properly.
You can check if it works by reading a qrcode file (if available) using the following code.

This will throw an error saying **you need sudo permissions to import this package keyboard** if you don't use **sudo python**
```
sudo python
>>> import keyboard
>>> t=0
>>> while True:
...     print (t)
...     t=t+1
...     if keyboard.is_pressed('esc'):
...         print('STOP prog')
...         break
```

This will keep printing the values of **t** from 0 to some huge number, till you stop the code by pressing **esc**.

#### Keyboard press detection without Keyboard package:

But sometimes it happens, that you have some packages that needs sudo and some that does not work with sudo, so just running all the python scripts as **sudo python** is not always the solution.
Hence, there is this second way in which you can find what key is pressed on the keyboard.

Check this [link](https://www.jonwitts.co.uk/archives/896) for more details.

Make a **test.py** file.

```
touch test.py
```

Now write the following in that.

```
import sys, termios, tty, os, time
 
def getch():
    fd = sys.stdin.fileno()
    old_settings = termios.tcgetattr(fd)
    try:
        tty.setraw(sys.stdin.fileno())
        ch = sys.stdin.read(1)
 
    finally:
        termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
    return ch
 
button_delay = 0.2
 
while True:
    char = getch()
 
    if (char == "p"):
        print("Stop!")
        exit(0)
 
    if (char == "a"):
        print("Left pressed")
        time.sleep(button_delay)
 
    elif (char == "d"):
        print("Right pressed")
        time.sleep(button_delay)
 
    elif (char == "w"):
        print("Up pressed")
        time.sleep(button_delay)
 
    elif (char == "s"):
        print("Down pressed")
        time.sleep(button_delay)
 
    elif (char == "1"):
        print("Number 1 pressed")
        time.sleep(button_delay)
```

#### Run and Check second method:
```
python test.py
```

This will print whatever key you type in. But you cannot stop this code by **Ctrl+c**, you have to press **p** for that.

A more concise version is given below. This code is stopped by pressing **esc**.

```
import sys, termios, tty, os, time

def getch():
    fd = sys.stdin.fileno()
    old_settings = termios.tcgetattr(fd)
    tty.setraw(sys.stdin.fileno())
    ch = sys.stdin.read(1)
    termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
    return ch
 
while True:
    char = getch()
    print(char)
    if ord(char) & 0xFF == 27:   break
```
