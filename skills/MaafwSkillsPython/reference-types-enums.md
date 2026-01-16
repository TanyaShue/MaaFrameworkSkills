# 核心数据类型与枚举

## 结构体与详情对象

- `Rect`: `(x, y, w, h)`
- `Point`: `(x, y)`
- `RecognitionDetail`: 识别结果详情
- `ActionDetail`: 动作执行详情
- `TaskDetail`: 任务执行详情

### RecognitionDetail（示例字段）

```python
from maa.define import RecognitionDetail

detail = RecognitionDetail(
    reco_id=123,
    name="MyRecognition",
    algorithm="OCR",
    hit=True,
    box=Rect(x=100, y=200, w=50, h=50),
    all_results=[...],
    filtered_results=[...],
    best_result=...,
    raw_detail={},
    raw_image=np.array(...),
    draw_images=[...]
)
```

### ActionDetail（示例字段）

```python
from maa.define import ActionDetail

detail = ActionDetail(
    action_id=123,
    name="MyAction",
    action="Click",
    box=Rect(x=100, y=200, w=50, h=50),
    success=True,
    result=...,
    raw_detail={}
)
```

### TaskDetail / Status

```python
from maa.define import TaskDetail, Status

task_detail = TaskDetail(
    task_id=123,
    entry="MyTask",
    nodes=[...],
    status=Status(...)
)
```

## 状态枚举

```python
from maa.define import MaaStatusEnum

MaaStatusEnum.invalid
MaaStatusEnum.pending
MaaStatusEnum.running
MaaStatusEnum.succeeded
MaaStatusEnum.failed
```

## 日志级别

```python
from maa.define import LoggingLevelEnum

LoggingLevelEnum.Off
LoggingLevelEnum.Fatal
LoggingLevelEnum.Error
LoggingLevelEnum.Warn
LoggingLevelEnum.Info
LoggingLevelEnum.Debug
LoggingLevelEnum.Trace
LoggingLevelEnum.All
```

## 算法与动作枚举

```python
from maa.define import AlgorithmEnum, ActionEnum

AlgorithmEnum.DirectHit
AlgorithmEnum.TemplateMatch
AlgorithmEnum.FeatureMatch
AlgorithmEnum.ColorMatch
AlgorithmEnum.OCR
AlgorithmEnum.NeuralNetworkClassify
AlgorithmEnum.NeuralNetworkDetect
AlgorithmEnum.And
AlgorithmEnum.Or
AlgorithmEnum.Custom

ActionEnum.DoNothing
ActionEnum.Click
ActionEnum.LongPress
ActionEnum.Swipe
ActionEnum.MultiSwipe
ActionEnum.ClickKey
ActionEnum.LongPressKey
ActionEnum.InputText
ActionEnum.StartApp
ActionEnum.StopApp
ActionEnum.Scroll
ActionEnum.TouchDown
ActionEnum.TouchMove
ActionEnum.TouchUp
ActionEnum.KeyDown
ActionEnum.KeyUp
ActionEnum.StopTask
ActionEnum.Command
ActionEnum.Shell
ActionEnum.Custom
```

## ADB 截图/输入枚举

```python
from maa.controller import MaaAdbScreencapMethodEnum, MaaAdbInputMethodEnum

MaaAdbScreencapMethodEnum.EncodeToFileAndPull
MaaAdbScreencapMethodEnum.Encode
MaaAdbScreencapMethodEnum.RawWithGzip
MaaAdbScreencapMethodEnum.RawByNetcat
MaaAdbScreencapMethodEnum.MinicapDirect
MaaAdbScreencapMethodEnum.MinicapStream
MaaAdbScreencapMethodEnum.EmulatorExtras
MaaAdbScreencapMethodEnum.All
MaaAdbScreencapMethodEnum.Default

MaaAdbInputMethodEnum.AdbShell
MaaAdbInputMethodEnum.MinitouchAndAdbKey
MaaAdbInputMethodEnum.Maatouch
MaaAdbInputMethodEnum.EmulatorExtras
MaaAdbInputMethodEnum.All
MaaAdbInputMethodEnum.Default
```

## Win32 截图/输入枚举

```python
from maa.controller import MaaWin32ScreencapMethodEnum, MaaWin32InputMethodEnum

MaaWin32ScreencapMethodEnum.GDI
MaaWin32ScreencapMethodEnum.FramePool
MaaWin32ScreencapMethodEnum.DXGI_DesktopDup
MaaWin32ScreencapMethodEnum.DXGI_DesktopDup_Window
MaaWin32ScreencapMethodEnum.PrintWindow
MaaWin32ScreencapMethodEnum.ScreenDC

MaaWin32InputMethodEnum.Seize
MaaWin32InputMethodEnum.SendMessage
MaaWin32InputMethodEnum.PostMessage
MaaWin32InputMethodEnum.LegacyEvent
MaaWin32InputMethodEnum.PostThreadMessage
MaaWin32InputMethodEnum.SendMessageWithCursorPos
MaaWin32InputMethodEnum.PostMessageWithCursorPos
```

## Gamepad 枚举

```python
from maa.controller import MaaGamepadTypeEnum, MaaGamepadButtonEnum, MaaGamepadContactEnum

MaaGamepadTypeEnum.Xbox360
MaaGamepadTypeEnum.DualShock4

MaaGamepadButtonEnum.A
MaaGamepadButtonEnum.B
MaaGamepadButtonEnum.X
MaaGamepadButtonEnum.Y
MaaGamepadButtonEnum.DPAD_UP
MaaGamepadButtonEnum.DPAD_DOWN
MaaGamepadButtonEnum.DPAD_LEFT
MaaGamepadButtonEnum.DPAD_RIGHT
MaaGamepadButtonEnum.START
MaaGamepadButtonEnum.BACK
MaaGamepadButtonEnum.LEFT_THUMB
MaaGamepadButtonEnum.RIGHT_THUMB
MaaGamepadButtonEnum.LB
MaaGamepadButtonEnum.RB
MaaGamepadButtonEnum.GUIDE
MaaGamepadButtonEnum.CROSS
MaaGamepadButtonEnum.CIRCLE
MaaGamepadButtonEnum.SQUARE
MaaGamepadButtonEnum.TRIANGLE
MaaGamepadButtonEnum.PS
MaaGamepadButtonEnum.TOUCHPAD

MaaGamepadContactEnum.LEFT_STICK
MaaGamepadContactEnum.RIGHT_STICK
MaaGamepadContactEnum.LEFT_TRIGGER
MaaGamepadContactEnum.RIGHT_TRIGGER
```

## 调试控制器与特性枚举

```python
from maa.controller import MaaDbgControllerTypeEnum, MaaControllerFeatureEnum

MaaDbgControllerTypeEnum.CarouselImage
MaaDbgControllerTypeEnum.ReplayRecording

MaaControllerFeatureEnum.Null
MaaControllerFeatureEnum.UseMouseDownAndUpInsteadOfClick
MaaControllerFeatureEnum.UseKeyboardDownAndUpInsteadOfClick
```
