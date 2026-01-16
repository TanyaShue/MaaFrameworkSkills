# FAQ 与最佳实践

## 异步操作如何等待完成？

所有 `post_*` 方法返回 `Job`：

```python
job.status  # 不等待，仅查询状态
job.wait()  # 等待完成并返回状态
job.get()   # 等待完成并获取结果
```

## 如何处理任务失败？

```python
result = tasker.post_task("MyTask").wait().get()
if result.status.failed:
    detail = tasker.get_task_detail(result.task_id)
    print("任务失败")
```

## 如何动态修改任务流程？

```python
context.override_pipeline({"TaskName": {"action": "Click", "recognition": "OCR"}})
```

## 如何获取截图与识别结果？

```python
from maa.tasker import Tasker

Tasker.set_debug_mode(True)
image = controller.post_screencap().wait().get()

detail = tasker.get_recognition_detail(reco_id)
raw_image = detail.raw_image
draw_images = detail.draw_images
```

## Agent 连接是否需要 identifier？

可选。如果需要匹配特定 AgentServer，请传入 `identifier`：

```python
from maa.agent_client import AgentClient

agent = AgentClient("my-agent-id")
agent.connect()
```
