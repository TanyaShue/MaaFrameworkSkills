# MaaFramework Skills

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.9+-green.svg)](https://www.python.org/)
[![MaaFramework](https://img.shields.io/badge/MaaFramework-5.0+-orange.svg)](https://github.com/MaaXYZ/MaaFramework)

这是一个Claude市场插件仓库，包含用于指导AI掌握MaaFramework的技能集合。该技能库提供了完整的MaaFramework Python API文档、使用示例和最佳实践，帮助AI助手理解和使用MaaFramework进行自动化开发。

> **当前仅支持 Python binding**

---

## 安装步骤

### 前置要求

- **Claude Code** 或支持Claude市场插件的环境
- **Python 3.9+** (运行MaaFramework所需)
- **Git** (用于克隆仓库)

### 在claude code中使用


1. **启动claude**
   ```bash
   claude
   ```
2. **添加市场源**
   ```bash
   /plugin marketplace add TanyaShue/MaaFrameworkSkills
   ```

3. **安装技能**
   ```bash
   /plugin install MaafwSkillsPython@MaaFrameworkSkills
    ```  

## 使用方法

### 基本使用

#### 1. 通过对话使用

在Claude对话中直接提问，技能会自动被调用：

```text
用户: 如何使用MaaFramework的Resource类？
AI: [自动调用MaafwSkillsPython技能]
```

#### 2. 主动调用技能

```bash
# 询问资源管理相关问题
"Resource的post_bundle方法如何使用？"

# 询问控制器相关问题
"ADB控制器支持哪些截图方式？"

# 询问任务执行相关问题
"Tasker的post_task和post_action有什么区别？"
```

### 技能触发条件

当AI助手遇到以下情况时，会自动调用该技能：

1. **用户询问MaaFramework相关内容**
   - API使用方法
   - 架构说明
   - 开发指南

2. **代码中包含MaaFramework相关代码**
   - 需要理解代码功能
   - 需要提供使用建议
   - 需要调试帮助

## 许可证

本项目采用 [MIT License] 许可证。


---

## 相关链接

- [MaaFramework 官方仓库](https://github.com/MaaXYZ/MaaFramework)
- [Python Bindings 源码](https://github.com/MaaXYZ/MaaFramework/tree/main/source/binding/Python)
- [Claude Code 官方文档](https://claude.com/claude-code)
