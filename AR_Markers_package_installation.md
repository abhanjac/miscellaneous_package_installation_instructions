# Installing AR_Markers package for reading and detecting ARcodes:

**ar_markers** package can be used to detect ar codes with computer vision. **OpenCV 3** should be installed for this to work.

The **ar_markers** package is obtained in this [link](https://pypi.org/project/ar-markers/)

#### Install ar_markers:
```
sudo pip install ar_markers
```

But this often gives the following error:

```
Traceback (most recent call last):
  File "/usr/local/bin/pip", line 11, in <module>
    sys.exit(main())
TypeError: 'module' object is not callable
```

So if this error is encountered, use the following command instead as suggested in this [link](https://stackoverflow.com/questions/58451650/pip-no-longer-working-after-update-error-module-object-is-not-callable):

```
sudo python -m pip install ar_markers
```

But even this may encounter the following error sometimes:

```
ERROR: Cannot uninstall 'docutils'. It is a distutils installed project and thus we cannot accurately 
determine which files belong to it which would lead to only a partial uninstall.
```

In that case, downgrade pip by the following command as suggested in this [link](https://github.com/pypa/pip/issues/5247):

```
sudo pip install pip==9.0.3
```

Now use the earlier command again:

```
sudo python -m pip install ar_markers
```

Everything should be installed properly this time.

#### Run and Check:
Once the package has been installed, you can try to import it in a python script and see if it works properly.

```
python
>>> import ar_markers
>>>




