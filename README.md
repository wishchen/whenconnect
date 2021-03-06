# whenconnect

[中文版README](https://github.com/williamfzc/whenconnect/blob/master/README_zh.md)

[![Maintainability](https://api.codeclimate.com/v1/badges/c6e406c3416bbbcbd898/maintainability)](https://codeclimate.com/github/williamfzc/whenconnect/maintainability)
[![PyPI version](https://badge.fury.io/py/whenconnect.svg)](https://badge.fury.io/py/whenconnect)
[![Downloads](https://pepy.tech/badge/whenconnect)](https://pepy.tech/project/whenconnect)

> when your android connected, do sth :)

## What For

A better way to handle things when connect android device, such as install an app, launch an app, and something else you wish.

- Have same lifecycle with your python script.
- And when your python script end, it will end too.

## Usage

### Base

If you want to call function A when device '123456F' connected:

```python
from whenconnect import when_connect, start_detect


def A(device):
    print('call function A', device)


# Start when_connect detect
start_detect()

# register event
when_connect(device=['123456F'], do=A)
```

After that, when 123456F connected, function A will be called!

Of course, you can choose to detect 'any' devices.

```python
when_connect(device='any', do=A)
```

### More

Use dead loop or server to keep when_connect alive for a long time if you want.

For example, after device connected, check its device info every 5 seconds：

```python
from whenconnect import when_connect, start_detect
import os
import threading


def check_device_info(device):
    cmd = 'adb -s {} shell getprop ro.product.model'.format(device)
    device_model = os.popen(cmd).read()
    print(device_model)

    global timer
    timer = threading.Timer(5, lambda: check_device_info(device))
    timer.start()


start_detect()

when_connect(device='any', do=check_device_info)

while True:
    pass
```

Can not become easier.

## API

See `whenconnect/api.py` for detail.

## Install

```
pip install whenconnect
```

- Only tested on python3.
- and ADB installed.

## License

MIT
