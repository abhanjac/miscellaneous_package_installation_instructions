# Installing pyzbar package for reading QRcodes:

This is a much better package for decoding QR code compared to **Zbar**.

The **Pyzbar qrcode decoder** package is obtained in this [link](https://pypi.org/project/pyzbar/)

This package can decode the information inside a qrcode when it is fed into the functions in this package using a python script.
You can even tilt the qrcode, and it can still detect. And it does the detection in realtime without lags.

#### Install Dependencies:
```
sudo apt install python-zbar libzbar-dev python-qrtools
```

#### Install Pyzbar:
```
sudo pip install pyzbar
```

#### Install Pyzbar for python 3:

First install **pip3** if not already there.

```
sudo apt install python3-pip
```

Now install **Pyzbar**.

```
sudo pip install pyzbar
```

#### Run and Check:
Once the package has been installed, you can try to import it in a python script and see if it works properly.
You can check if it works by reading a qrcode file (if available) using the following code.

To read the qrcode from an **image**

```
import cv2, numpy as np
import pyzbar.pyzbar as pyzbar

image = cv2.imread("pysource_qrcode.png")

decodedObjects = pyzbar.decode(image)
for obj in decodedObjects:
    print("Type:", obj.type)
    print("Data: ", obj.data, "\n")

cv2.imshow("Frame", image)
cv2.waitKey(0)
```

To read the qrcode in a **video**

```
import cv2, numpy as np
import pyzbar.pyzbar as pyzbar

cap = cv2.VideoCapture(0)
font = cv2.FONT_HERSHEY_PLAIN

while True:
    _, frame = cap.read()

    decodedObjects = pyzbar.decode(frame)
    for obj in decodedObjects:
        #print("Data", obj.data)
        cv2.putText(frame, str(obj.data), (50, 50), font, 2,
                    (255, 0, 0), 3)

    cv2.imshow("Frame", frame)

    key = cv2.waitKey(1)
    if key == 27:
        break
```



