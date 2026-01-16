# Resource（资源管理）

用于加载和管理资源：图片、OCR 模型、Pipeline 配置；支持自定义识别/动作注册与覆盖。

## 创建资源

```python
from maa.resource import Resource

resource = Resource()
```

## 加载资源

```python
# 异步加载资源包
job = resource.post_bundle("./resource/path")
job.wait()

# 异步加载 OCR 模型
job = resource.post_ocr_model("./model/ocr")
job.wait()

# 异步加载 Pipeline
job = resource.post_pipeline("./pipeline")
job.wait()

# 异步加载图片资源
job = resource.post_image("./images")
job.wait()
```

## 覆盖与修改

```python
override_config = {
    "TaskName": {
        "action": "Click",
        "recognition": "OCR",
        "expected": "确认"
    }
}
resource.override_pipeline(override_config)

# 覆盖任务的 next 列表
resource.override_next("TaskName", ["NextTask1", "NextTask2"])

# 覆盖图片
import cv2
image = cv2.imread("image.png")
resource.override_image("ImageName", image)

# 获取任务定义
node_data = resource.get_node_data("TaskName")   # dict
node_object = resource.get_node_object("TaskName")  # JPipelineData
```

## 自定义识别器注册

```python
from maa.custom_recognition import CustomRecognition

# 方式1：装饰器
@resource.custom_recognition("MyRecognition")
class MyRecognition(CustomRecognition):
    def analyze(self, context, argv):
        return CustomRecognition.AnalyzeResult(
            box=(100, 100, 50, 50),
            detail={"text": "result"}
        )

# 方式2：手动注册
class MyRecognition2(CustomRecognition):
    def analyze(self, context, argv):
        return (100, 100, 50, 50)

resource.register_custom_recognition("MyRecog2", MyRecognition2())
resource.unregister_custom_recognition("MyRecog2")
resource.clear_custom_recognition()
```

## 自定义动作注册

```python
from maa.custom_action import CustomAction

@resource.custom_action("MyAction")
class MyAction(CustomAction):
    def run(self, context, argv):
        context.tasker.controller.post_click(100, 100).wait()
        return True

class MyAction2(CustomAction):
    def run(self, context, argv):
        return True

resource.register_custom_action("MyAct2", MyAction2())
resource.unregister_custom_action("MyAct2")
resource.clear_custom_action()
```

## Agent 自动注册

Agent 连接后会把 AgentServer 中注册的 `custom_action` / `custom_recognition`
自动同步到当前 `Resource`。需要先 `bind(resource)` 再 `connect()`。

```python
from maa.agent_client import AgentClient

agent = AgentClient("optional-identifier")
agent.bind(resource)
agent.connect()
```

## 推理设备配置

```python
resource.use_cpu()
resource.use_directml(device_id=0)  # Windows
resource.use_coreml()               # macOS
resource.use_auto_ep()
```

## 事件监听

```python
from maa.resource import ResourceEventSink

class MyResourceSink(ResourceEventSink):
    def on_resource_loading(self, resource, noti_type, detail):
        print(f"资源加载中: {detail.path}")

sink = MyResourceSink()
sink_id = resource.add_sink(sink)
resource.remove_sink(sink_id)
resource.clear_sinks()
```

## 状态与查询

```python
if resource.loaded:
    print("资源已加载")

hash_value = resource.hash
task_list = resource.node_list
recog_list = resource.custom_recognition_list
action_list = resource.custom_action_list

resource.clear()
```
