# Tasker（任务执行引擎）

绑定 Resource 与 Controller，执行任务与动作/识别。

## 创建与绑定

```python
from maa.tasker import Tasker

tasker = Tasker()
tasker.bind(resource, controller)

if tasker.inited:
    print("Tasker 已初始化")
```

## 执行任务

```python
job = tasker.post_task("MyTask")
status = job.status
job.wait()
result = job.get()

pipeline_override = {
    "MyTask": {"action": "Click", "recognition": "OCR", "expected": "确认"}
}
result = tasker.post_task("MyTask", pipeline_override).wait().get()
```

## 执行识别

```python
from maa.pipeline import JRecognitionParam

reco_type = "OCR"
reco_param = JRecognitionParam(roi=[100, 100, 200, 300])
image = tasker.controller.post_screencap().wait().get()

result = tasker.post_recognition(reco_type, reco_param, image).wait().get()
detail = tasker.get_recognition_detail(reco_id)
```

## 执行动作

```python
from maa.pipeline import JActionParam

action_type = "Click"
action_param = JActionParam()
box = (100, 100, 50, 50)

result = tasker.post_action(action_type, action_param, box, "reco_detail").wait().get()
detail = tasker.get_action_detail(action_id)
```

## 任务控制与状态

```python
if tasker.running:
    print("任务器运行中")

tasker.post_stop().wait()

if tasker.stopping:
    print("任务器正在停止")

tasker.clear_cache()
```

## 查询结果详情

```python
node_detail = tasker.get_latest_node("TaskName")
task_detail = tasker.get_task_detail(task_id)
```

## 全局配置

```python
from maa.define import LoggingLevelEnum

Tasker.set_log_dir("./logs")
Tasker.set_save_draw(True)
Tasker.set_debug_mode(True)
Tasker.set_save_on_error(True)
Tasker.set_stdout_level(LoggingLevelEnum.Debug)
Tasker.set_draw_quality(85)
Tasker.set_reco_image_cache_limit(4096)
Tasker.load_plugin("./my_plugin.dll")
```

## 事件监听

```python
from maa.tasker import TaskerEventSink
from maa.context import ContextEventSink

class MyTaskerSink(TaskerEventSink):
    def on_tasker_task(self, tasker, noti_type, detail):
        print(f"任务执行: {detail.entry}, ID: {detail.task_id}")

    def on_raw_notification(self, tasker, msg, details):
        print(f"原始通知: {msg}")

sink = MyTaskerSink()
sink_id = tasker.add_sink(sink)

class MyContextSink(ContextEventSink):
    def on_node_next_list(self, context, noti_type, detail):
        print(f"节点下一步: {detail.name}")

context_sink_id = tasker.add_context_sink(MyContextSink())

tasker.remove_sink(sink_id)
tasker.remove_context_sink(context_sink_id)
tasker.clear_sinks()
tasker.clear_context_sinks()
```
