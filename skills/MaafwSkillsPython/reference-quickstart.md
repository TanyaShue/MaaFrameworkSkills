# Quick Start

## 安装

```bash
python -m pip install maafw
```

## 基本用法

```python
from maa.controller import AdbController
from maa.resource import Resource
from maa.tasker import Tasker
from maa.toolkit import Toolkit

# 1. 初始化配置目录
Toolkit.init_option("./config")

# 2. 查找并连接设备
devices = Toolkit.find_adb_devices()  # 或 Toolkit.find_desktop_windows()
device = devices[0]
controller = AdbController(
    adb_path=device.adb_path,
    address=device.address,
    screencap_methods=device.screencap_methods,
    input_methods=device.input_methods,
)
controller.post_connection().wait()

# 3. 加载资源（图片、模型、pipeline）
resource = Resource()
resource.post_bundle("./resource").wait()

# 4. （可选）连接 Agent 并自动注册
from maa.agent_client import AgentClient

agent = AgentClient("optional-identifier")
agent.bind(resource)  # 必须先绑定 Resource
agent.connect()       # 连接后自动注册 custom_action/custom_recognition

# 5. 创建任务器并绑定
tasker = Tasker()
tasker.bind(resource, controller)

# 6. 执行任务
result = tasker.post_task("TaskName").wait().get()
```
