# Controller（设备控制器）

负责与设备交互：截图、点击、滑动、输入、启动/停止应用等。

## 基础控制器操作

```python
# 连接设备
job = controller.post_connection()
job.wait()
is_connected = controller.connected

# 截图
image = controller.post_screencap().wait().get()

# 点击/滑动/输入
controller.post_click(100, 200).wait()
controller.post_swipe(100, 200, 300, 400, duration=500).wait()
controller.post_input_text("Hello MAA!").wait()

# 启动/停止应用（ADB）
controller.post_start_app("com.example.app").wait()
controller.post_stop_app("com.example.app").wait()

# Shell 命令（ADB）
output = controller.post_shell("ls /sdcard").wait().get()
```

## 截图配置与属性

```python
controller.set_screenshot_target_long_side(1920)
controller.set_screenshot_target_short_side(1080)
controller.set_screenshot_use_raw_size(True)

image = controller.cached_image
width, height = controller.resolution
uuid = controller.uuid
```

## 事件监听

```python
from maa.controller import ControllerEventSink

class MyControllerSink(ControllerEventSink):
    def on_controller_action(self, controller, noti_type, detail):
        print(f"控制器操作: {detail.action}, UUID: {detail.uuid}")

sink = MyControllerSink()
sink_id = controller.add_sink(sink)
controller.remove_sink(sink_id)
controller.clear_sinks()
```

## ADB 控制器

```python
from maa.controller import AdbController
from maa.toolkit import Toolkit

devices = Toolkit.find_adb_devices()
device = devices[0]

controller = AdbController(
    adb_path=device.adb_path,
    address=device.address,
    screencap_methods=device.screencap_methods,
    input_methods=device.input_methods,
    config=device.config,
    agent_path="./MaaAgentBinary"
)
```

## Win32 控制器

```python
from maa.controller import Win32Controller, MaaWin32ScreencapMethodEnum, MaaWin32InputMethodEnum
from maa.toolkit import Toolkit

windows = Toolkit.find_desktop_windows()
window = windows[0]

controller = Win32Controller(
    hWnd=window.hwnd,
    screencap_method=MaaWin32ScreencapMethodEnum.FramePool,
    mouse_method=MaaWin32InputMethodEnum.PostMessage,
    keyboard_method=MaaWin32InputMethodEnum.PostMessage,
)
```

## Dbg 控制器

```python
from maa.controller import DbgController, MaaDbgControllerTypeEnum

controller = DbgController(
    read_path="./record/input",
    write_path="./record/output",
    dbg_type=MaaDbgControllerTypeEnum.CarouselImage,
    config={}
)
```

## PlayCover 控制器（macOS）

```python
from maa.controller import PlayCoverController

controller = PlayCoverController(
    address="127.0.0.1:1717",
    uuid="com.hypergryph.arknights"
)
```

## Gamepad 控制器（Windows）

```python
from maa.controller import GamepadController, MaaGamepadTypeEnum, MaaGamepadButtonEnum, MaaGamepadContactEnum
from maa.controller import MaaWin32ScreencapMethodEnum

controller = GamepadController(
    hWnd=None,
    gamepad_type=MaaGamepadTypeEnum.Xbox360,
    screencap_method=MaaWin32ScreencapMethodEnum.FramePool
)

controller.post_click_key(MaaGamepadButtonEnum.A).wait()
controller.post_touch_move(0, 32767, 0, contact=MaaGamepadContactEnum.LEFT_STICK).wait()
```

## 自定义控制器

```python
from maa.controller import CustomController
import numpy as np

class MyCustomController(CustomController):
    def connect(self) -> bool:
        return True
    def request_uuid(self) -> str:
        return "my-device-uuid"
    def start_app(self, intent: str) -> bool:
        return True
    def stop_app(self, intent: str) -> bool:
        return True
    def screencap(self) -> np.ndarray:
        return np.zeros((720, 1280, 3), dtype=np.uint8)
    def click(self, x: int, y: int) -> bool:
        return True
    def swipe(self, x1: int, y1: int, x2: int, y2: int, duration: int) -> bool:
        return True
    def touch_down(self, contact: int, x: int, y: int, pressure: int) -> bool:
        return True
    def touch_move(self, contact: int, x: int, y: int, pressure: int) -> bool:
        return True
    def touch_up(self, contact: int) -> bool:
        return True
    def click_key(self, keycode: int) -> bool:
        return True
    def input_text(self, text: str) -> bool:
        return True
    def key_down(self, keycode: int) -> bool:
        return True
    def key_up(self, keycode: int) -> bool:
        return True
    def scroll(self, dx: int, dy: int) -> bool:
        return True

controller = MyCustomController()
```
