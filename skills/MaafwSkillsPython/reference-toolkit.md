# Toolkit（工具包）

用于配置初始化与设备发现。

## 配置初始化

```python
from maa.toolkit import Toolkit

Toolkit.init_option("./user_config")

default_config = {
    "logging": True,
    "save_draw": True,
    "stdout_level": 2
}
Toolkit.init_option("./user_config", default_config)
```

## 设备发现

```python
devices = Toolkit.find_adb_devices()
for device in devices:
    print(device.name, device.adb_path, device.address)

specified_devices = Toolkit.find_adb_devices(specified_adb="C:/Android/adb.exe")
```

```python
windows = Toolkit.find_desktop_windows()
for window in windows:
    print(window.hwnd, window.class_name, window.window_name)
```

## 数据类

```python
from maa.toolkit import AdbDevice, DesktopWindow

# AdbDevice:
# - name, adb_path, address, screencap_methods, input_methods, config

# DesktopWindow:
# - hwnd, class_name, window_name
```
