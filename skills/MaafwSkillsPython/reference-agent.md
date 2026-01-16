# Agent Mode

用于将自定义识别/动作模块运行在独立进程中，进行分布式处理。

## Agent Server

```python
from maa.agent.agent_server import AgentServer
from maa.custom_recognition import CustomRecognition
from maa.context import Context

@AgentServer.custom_recognition("MyReco")
class MyRecognition(CustomRecognition):
    def analyze(self, context: Context, argv: CustomRecognition.AnalyzeArg):
        return CustomRecognition.AnalyzeResult(box=None, detail={})

def main():
    import sys
    if len(sys.argv) < 2:
        print("Usage: python agent_main.py <socket_id>")
        return
    socket_id = sys.argv[-1]
    AgentServer.start_up(socket_id)
    AgentServer.join()
    AgentServer.shut_down()
```

## Agent Client（连接与自动注册）

Agent 连接后会自动将 AgentServer 中注册的 `custom_action` / `custom_recognition`
同步到当前 `Resource` 中，因此需要先创建并绑定 `Resource`。

```python
from maa.agent_client import AgentClient
from maa.resource import Resource

resource = Resource()

agent = AgentClient()  # 可选 identifier
agent.bind(resource)  # 先绑定资源
agent.connect()       # 连接后自动注册到 resource

recog_list = agent.get_custom_recognition_list()
action_list = agent.get_custom_action_list()
```

## identifier（可选连接标识）

`AgentClient` 构造函数支持传入 `identifier`，用于匹配特定的 AgentServer。

```python
from maa.agent_client import AgentClient

agent = AgentClient("my-agent-id")
agent.connect()
```

## 推荐顺序（示例）

```python
from maa.resource import Resource
from maa.agent_client import AgentClient

resource = Resource()
resource.post_bundle("./resource").wait()

agent = AgentClient()
agent.bind(resource)
agent.connect()
```
