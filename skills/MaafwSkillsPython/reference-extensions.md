# Custom Extensions（自定义扩展）

## Custom Recognition

```python
from maa.custom_recognition import CustomRecognition
from maa.context import Context

class MyRecognition(CustomRecognition):
    def analyze(self, context: Context, argv: CustomRecognition.AnalyzeArg):
        reco_detail = context.run_recognition(
            "MyOCR",
            argv.image,
            pipeline_override={"MyOCR": {"roi": [100, 100, 200, 300]}}
        )

        if reco_detail and reco_detail.hit:
            box = reco_detail.best_result.box
            context.override_next(argv.node_name, ["TaskA", "TaskB"])
            return CustomRecognition.AnalyzeResult(
                box=box,
                detail={"text": reco_detail.best_result.text}
            )

        return CustomRecognition.AnalyzeResult(box=None, detail={})
```

### AnalyzeArg 参数

```python
@dataclass
class AnalyzeArg:
    task_detail: TaskDetail
    node_name: str
    custom_recognition_name: str
    custom_recognition_param: str
    image: numpy.ndarray
    roi: Rect
```

### AnalyzeResult 返回

```python
@dataclass
class AnalyzeResult:
    box: Optional[RectType]
    detail: dict

# 可选返回类型:
# - AnalyzeResult
# - (x, y, w, h)
# - [x, y, w, h]
# - numpy.ndarray(shape=(4,))
# - None
```

## Custom Action

```python
from maa.custom_action import CustomAction
from maa.context import Context

class MyAction(CustomAction):
    def run(self, context: Context, argv: CustomAction.RunArg):
        context.tasker.controller.post_click(100, 100).wait()
        context.override_next(argv.node_name, ["NextTask1", "NextTask2"])
        if context.tasker.stopping:
            return False
        return True
```

### RunArg 参数

```python
@dataclass
class RunArg:
    task_detail: TaskDetail
    node_name: str
    custom_action_name: str
    custom_action_param: str
    reco_detail: RecognitionDetail
    box: Rect
```

### RunResult 返回

```python
@dataclass
class RunResult:
    success: bool

# 可选返回类型:
# - RunResult
# - bool
# - None (默认 True)
```
