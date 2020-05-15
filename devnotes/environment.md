## Repo setup

```
$ git remote add upstream https://github.com/solokeys/solo-python.git
```


## Python environment setup

We need to create a dev virtual environment. We follow the [venv documentation](https://docs.python.org/3/library/venv.html#module-venv).

```
$ pwd
/xtras/Repositories/solo-python
$ mkdir venv
# create custom environment
$ python3 -m venv ./venv/solo
# activate environment
$ source venv/solo/bin/activate
# deactivate environment
$ deactivate
```

Let's install solo from local source:

```
(solo) $ pip install --upgrade pip
(solo) $ pip install .
...
Successfully installed certifi-2020.4.5.1 cffi-1.14.0 chardet-3.0.4 click-7.1.2 cryptography-2.9.2 ecdsa-0.15 fido2-0.8.1 idna-2.9 intelhex-2.2.1 pycparser-2.20 pyserial-3.4 pyusb-1.0.2 requests-2.23.0 six-1.14.0 solo-python-0.0.24.1 urllib3-1.25.9
# We see that we installed the local version we changed!
(solo) $ which solo
/xtras/Repositories/solo-python/venv/solo/bin/solo
(solo) $ solo version
0.0.24.1

```

Let's see our device's properties:

```
(solo) $ python
Python 3.8.2 (default, Feb 28 2020, 00:00:00) 
[GCC 10.0.1 20200216 (Red Hat 10.0.1-0.8)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import solo
>>> solos = solo.client.find()
>>> print(solos.dev.descriptor)
{'path': '/dev/hidraw1', 'usage_page': 61904, 'usage': 1, 'vendor_id': 1155, 'product_id': 41674, 'product_string': 'SoloKeys Solo 4.0.0'}

```

### python-fido2 library:

```
$ tail -n +1 /sys/class/hidraw/hidraw*/device/uevent
==> /sys/class/hidraw/hidraw0/device/uevent <==
DRIVER=hid-generic
HID_ID=0003:00001BCF:00000005
HID_NAME=USB Optical Mouse
HID_PHYS=usb-0000:00:14.0-1/input0
HID_UNIQ=
MODALIAS=hid:b0003g0001v00001BCFp00000005

==> /sys/class/hidraw/hidraw1/device/uevent <==
DRIVER=hid-generic
HID_ID=0003:00000483:0000A2CA
HID_NAME=SoloKeys Solo 4.0.0
HID_PHYS=usb-0000:00:14.0-6/input0
HID_UNIQ=207636905548
MODALIAS=hid:b0003g0001v00000483p0000A2CA

==> /sys/class/hidraw/hidraw2/device/uevent <==
DRIVER=hid-generic
HID_ID=0003:00001050:00000407
HID_NAME=Yubico YubiKey OTP+FIDO+CCID
HID_PHYS=usb-0000:00:14.0-11/input0
HID_UNIQ=
MODALIAS=hid:b0003g0001v00001050p00000407

==> /sys/class/hidraw/hidraw3/device/uevent <==
DRIVER=hid-generic
HID_ID=0003:00001050:00000407
HID_NAME=Yubico YubiKey OTP+FIDO+CCID
HID_PHYS=usb-0000:00:14.0-11/input1
HID_UNIQ=
MODALIAS=hid:b0003g0001v00001050p00000407

```


After patching fido2 we get:
```
>>> print(solos.dev.descriptor)
{'path': '/dev/hidraw1', 'usage_page': 61904, 'usage': 1, 'vendor_id': 1155, 'product_id': 41674, 'product_string': 'SoloKeys Solo 4.0.0', 'serial_number': '207636905548'}
```
