# Context（执行上下文）

用于自定义识别/动作中执行子任务、覆盖流程、访问当前任务信息。

## 基本用法

```python
from maa.context import Context

result = context.run_task("TaskName")
reco_detail = context.run_recognition("RecogName", image)
action_detail = context.run_action("ActionName", box=(0, 0, 100, 100))

task_id = context.tasker.get_task_job()
job = context.get_task_job()
detail = job.wait().get()

if context.tasker.stopping:
    pass
```

## 覆盖与修改

```python
context.override_pipeline({
    "TaskName": {"action": "Click", "recognition": "OCR", "expected": "确认"}
})
context.override_next("TaskName", ["NextTask1", "NextTask2"])

import numpy as np
image = np.zeros((720, 1280, 3), dtype=np.uint8)
context.override_image("ImageName", image)

context.set_anchor("MyAnchor", "TaskName")
anchor_node = context.get_anchor("MyAnchor")

count = context.get_hit_count("TaskName")
context.clear_hit_count("TaskName")

new_context = context.clone()
new_context.override_pipeline({"TaskName": {"roi": [100, 200, 300, 400]}})
```

## 事件监听

```python
from maa.context import ContextEventSink

class MyContextSink(ContextEventSink):
    def on_node_next_list(self, context, noti_type, detail):
        print(f"节点下一步: {detail.name}, 列表: {detail.next_list}")

    def on_node_recognition(self, context, noti_type, detail):
        print(f"识别节点: {detail.name}, 识别ID: {detail.reco_id}")

    def on_node_action(self, context, noti_type, detail):
        print(f"动作节点: {detail.name}, 动作ID: {detail.action_id}")
```
