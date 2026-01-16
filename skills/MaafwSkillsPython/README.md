# MaaFramework Python Skill Library

该目录已整理为标准 Claude Agent Skill 结构，入口文件为 `SKILL.md`，其它内容拆分到多个 `reference-*.md`，便于按需加载。

## 📁 目录结构

```
Maafw-Skills-Python/
├── SKILL.md
├── reference-quickstart.md
├── reference-resource.md
├── reference-controller.md
├── reference-tasker.md
├── reference-context.md
├── reference-toolkit.md
├── reference-extensions.md
├── reference-agent.md
├── reference-types-enums.md
└── reference-faq.md
```

## 使用方式

- 入口：`SKILL.md`
- 需要细节时加载对应的 `reference-*.md`
# MaaFramework Python Skill Library

> **Skill Description**: This skill library contains comprehensive documentation for MaaFramework Python bindings. AI assistants should use this skill when they need to understand MaaFramework's architecture, interfaces, or implement automation solutions using Python bindings.

## 📁 File Structure

```
Maafw-Skills-Python/
├── maafw_python_skill.md    # Complete MaaFramework Python API documentation (SKILL format)
└── README.md                # This file
```

## 🎯 Skill Purpose

This skill should be invoked when AI assistants need to understand:

1. **MaaFramework Python Binding Interfaces**
   - Resource management for assets and configurations
   - Controller management for device interaction
   - Tasker engine for workflow execution
   - Context handling for custom modules
   - Toolkit utilities for device discovery

2. **Custom Extension Development**
   - CustomRecognition for specialized image recognition
   - CustomAction for custom automation actions

3. **Agent Mode Architecture**
   - AgentServer for distributed processing
   - AgentClient for inter-process communication

4. **Data Types and Enumerations**
   - Core data structures (Rect, Point, RecognitionDetail, etc.)
   - Enumeration types (algorithms, actions, screencap methods, etc.)

## 📖 Usage Guide

### As an AI Skill

When AI assistants need to understand MaaFramework Python interfaces, they should invoke this skill:

```
How do I use Resource class methods in MaaFramework Python?
```

```
How to register custom recognition modules in MaaFramework Python?
```

```
What screencap methods are supported by ADB controllers in MaaFramework Python?
```

### As Documentation Reference

Developers can use this documentation for:

1. **Quick Start**
   - Install MaaFramework Python bindings
   - Create basic automation workflows

2. **API Reference**
   - View all available classes and methods
   - Understand parameter and return types
   - See usage examples

3. **Custom Development**
   - Implement custom recognition modules
   - Implement custom action modules
   - Use Agent mode for distributed processing

## 📦 Dependencies

- Python 3.9+
- maafw package (from PyPI)
- numpy (for image processing)
- opencv-python (optional, for advanced image processing)

## 🚀 Quick Start

### Installation

```bash
python -m pip install maafw
```

### Basic Usage

```python
from maa.resource import Resource
from maa.controller import AdbController
from maa.tasker import Tasker
from maa.toolkit import Toolkit

# Initialize toolkit
Toolkit.init_option("./")

# Find and connect to device
devices = Toolkit.find_adb_devices()
device = devices[0]

controller = AdbController(
    adb_path=device.adb_path,
    address=device.address,
    screencap_methods=device.screencap_methods,
    input_methods=device.input_methods,
)

# Connect to device
controller.post_connection().wait()

# Load resources
resource = Resource()
resource.post_bundle("./resource").wait()

# Create tasker and bind components
tasker = Tasker()
tasker.bind(resource, controller)

# Execute task
result = tasker.post_task("MyTask").wait().get()
```

## 📚 Core Concepts

### 1. Resource (Resource Management)

Handles loading and managing resource files:
- Image assets
- OCR models
- Pipeline configurations
- Custom recognition/action module registration

### 2. Controller (Device Controller)

Manages device interaction:
- Screenshots
- Clicks, swipes, input simulation
- Supports multiple device types (ADB, Win32, Gamepad, PlayCover, etc.)

### 3. Tasker (Task Execution Engine)

Executes automation workflows:
- Binds Resource and Controller components
- Asynchronous task execution
- Task status and result querying

### 4. Context (Execution Context)

Used within custom actions/recognition modules:
- Execute subtasks
- Override task flows dynamically
- Access current task information

### 5. Toolkit (Utility Toolkit)

Provides helper functionality:
- Device discovery (ADB devices, desktop windows)
- Configuration initialization

## 🔧 Custom Extensions

### Custom Recognition

```python
from maa.resource import Resource
from maa.custom_recognition import CustomRecognition
from maa.context import Context

@resource.custom_recognition("MyRecognition")
class MyRecognition(CustomRecognition):
    def analyze(self, context: Context, argv: CustomRecognition.AnalyzeArg):
        # Implement custom recognition logic
        return CustomRecognition.AnalyzeResult(
            box=(100, 100, 50, 50),
            detail={"text": "recognition result"}
        )
```

### Custom Action

```python
from maa.resource import Resource
from maa.custom_action import CustomAction
from maa.context import Context

@resource.custom_action("MyAction")
class MyAction(CustomAction):
    def run(self, context: Context, argv: CustomAction.RunArg):
        # Execute custom logic
        context.tasker.controller.post_click(100, 100).wait()
        return True
```

## 🎮 Agent Mode

Agent mode enables running custom recognition/action modules in separate processes with IPC communication.

### Agent Server

```python
from maa.agent.agent_server import AgentServer
from maa.custom_recognition import CustomRecognition
from maa.context import Context

@AgentServer.custom_recognition("MyReco")
class MyRecognition(CustomRecognition):
    def analyze(self, context: Context, argv: CustomRecognition.AnalyzeArg):
        # Custom recognition logic
        return CustomRecognition.AnalyzeResult(box=None, detail={})

def main():
    socket_id = sys.argv[-1]
    AgentServer.start_up(socket_id)
    AgentServer.join()
    AgentServer.shut_down()
```

### Agent Client

```python
from maa.agent.agent_client import AgentClient

client = AgentClient("socket_id")
client.connect()

# Get list of registered custom recognition modules
recog_list = client.get_custom_recognition_list()
```

## 📋 枚举类型参考

### 截图方式

- **ADB**: `MaaAdbScreencapMethodEnum`
  - `EncodeToFileAndPull` - 慢速，高兼容
  - `Encode` - 慢速，高兼容
  - `RawWithGzip` - 中速，高兼容
  - `RawByNetcat` - 快速，低兼容
  - `MinicapDirect` - 快速，低兼容，有损编码
  - `MinicapStream` - 极快，低兼容，有损编码
  - `EmulatorExtras` - 极快，低兼容，仅模拟器

- **Win32**: `MaaWin32ScreencapMethodEnum`
  - `GDI` - 快速，中兼容
  - `FramePool` - 极快，中兼容 (需要 Windows 10 1903+)
  - `DXGI_DesktopDup` - 极快，低兼容
  - `DXGI_DesktopDup_Window` - 极快，低兼容
  - `PrintWindow` - 中速，中兼容
  - `ScreenDC` - 快速，高兼容

### 输入方式

- **ADB**: `MaaAdbInputMethodEnum`
  - `AdbShell` - 慢速，高兼容
  - `MinitouchAndAdbKey` - 快速，中兼容
  - `Maatouch` - 快速，中兼容
  - `EmulatorExtras` - 快速，低兼容，仅模拟器

- **Win32**: `MaaWin32InputMethodEnum`
  - `Seize` - 高兼容，占用鼠标
  - `SendMessage` - 中兼容
  - `PostMessage` - 中兼容
  - `LegacyEvent` - 低兼容，占用鼠标
  - `SendMessageWithCursorPos` - 中兼容 (检查光标位置)
  - `PostMessageWithCursorPos` - 中兼容 (检查光标位置)

### Algorithm Types

- `DirectHit` - Direct hit
- `TemplateMatch` - Template matching
- `FeatureMatch` - Feature matching
- `ColorMatch` - Color matching
- `OCR` - Optical character recognition
- `NeuralNetworkClassify` - Neural network classification
- `NeuralNetworkDetect` - Neural network detection
- `And` - And (all sub-recognitions must hit)
- `Or` - Or (any sub-recognition must hit)
- `Custom` - Custom algorithm

### Action Types

- `DoNothing` - No operation
- `Click` - Click
- `LongPress` - Long press
- `Swipe` - Swipe
- `MultiSwipe` - Multiple swipes
- `ClickKey` - Click key
- `LongPressKey` - Long press key
- `InputText` - Input text
- `StartApp` - Start application
- `StopApp` - Stop application
- `Scroll` - Scroll
- `TouchDown` - Touch down
- `TouchMove` - Touch move
- `TouchUp` - Touch up
- `KeyDown` - Key down
- `KeyUp` - Key up
- `StopTask` - Stop task
- `Command` - Command
- `Shell` - Shell command
- `Custom` - Custom action

## 📝 Frequently Asked Questions

### Q: How to wait for asynchronous operations to complete?

A: All `post_*` methods return `Job` objects:
- `job.status` - Check current status (no waiting)
- `job.wait()` - Wait for completion and return status
- `job.get()` - Wait for completion and get result

### Q: How to handle task failures?

A: Check the returned `Status` object:
```python
result = tasker.post_task("MyTask").wait().get()
if result.status.failed:
    print("Task failed")
```

### Q: How to dynamically modify task flow?

A: Use `override_next` or `override_pipeline`:
```python
# In custom actions/recognition modules
context.override_next("CurrentTask", ["NextTask1", "NextTask2"])

# Override entire pipeline
resource.override_pipeline({
    "TaskName": {
        "action": "Click",
        "recognition": "OCR",
        "expected": "target text"
    }
})
```

### Q: How to get screenshots and recognition results?

A: Use debug mode:
```python
Tasker.set_debug_mode(True)

# Get screenshot
image = controller.post_screencap().wait().get()

# Get recognition details
detail = tasker.get_recognition_detail(reco_id)
raw_image = detail.raw_image  # Raw image
draw_images = detail.draw_images  # Visualization images
```

## 🔗 Related Resources

- [MaaFramework Official Repository](https://github.com/MaaXYZ/MaaFramework)
- [Python Bindings Source](https://github.com/MaaXYZ/MaaFramework/tree/main/source/binding/Python)
- [Integration Documentation](../maafw_doc/2.1-集成文档.md)
- [API Overview](../maafw_doc/2.2-集成接口一览.md)
- [Pipeline Protocol](../maafw_doc/3.1-任务流水线协议.md)

## 📄 Complete Documentation

For complete API documentation, see the `maafw_python_skill.md` file (formatted as a Claude skill).

## 📅 Changelog

### 2026-01-16
- Initial version in standard Claude skill format
- Complete MaaFramework Python API documentation
- Covers Resource, Controller, Tasker, Context, Toolkit core modules
- Includes custom recognition, custom actions, Agent mode extensions
- Comprehensive enumeration types reference
- FAQ section and usage examples

---

*This skill library is generated by Claude Code, based on MaaFramework Python binding source code and official documentation.*
