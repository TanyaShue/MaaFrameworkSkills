---
name: MaafwSkillsPython
description: MaaFramework(简称maafw或者maa) Python API 文档与用法参考。用于理解maafw的架构、接口与实现自动化方案；包含资源、控制器、任务器、上下文、扩展与 Agent 模式的用法与示例。
---

# MaaFramework Python Skill

用于理解 MaaFramework Python 绑定的架构与接口，并实现自动化方案与扩展。该技能将内容拆分为多个 reference 文件，便于按需加载。

## 何时使用

- 需要理解 MaaFramework 的核心组件与调用流程
- 需要编写自动化任务、控制设备、处理识别与动作
- 需要扩展自定义识别或自定义动作
- 需要使用 Agent 模式进行分布式处理

## 使用方式（示例提问）

- “如何加载资源并执行任务？”
- “怎么注册自定义识别器并返回识别结果？”
- “ADB 控制器支持哪些截图/输入方式？”
- “Tasker 的 post_task 和 post_action 用法是什么？”

## 文档结构（按需加载）

- `reference-quickstart.md`：安装与最小可用示例
- `reference-resource.md`：Resource 资源管理与扩展注册
- `reference-controller.md`：Controller 设备控制与各平台实现
- `reference-tasker.md`：Tasker 任务执行与结果获取
- `reference-context.md`：Context 上下文与流程覆盖
- `reference-toolkit.md`：Toolkit 设备发现与配置初始化
- `reference-extensions.md`：自定义识别/动作的完整定义
- `reference-agent.md`：Agent Server/Client 使用
- `reference-types-enums.md`：核心数据类型与枚举
- `reference-faq.md`：常见问题与最佳实践

---

*Last updated: 2026-01-16*
