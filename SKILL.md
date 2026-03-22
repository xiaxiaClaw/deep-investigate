---
name: deep-investigate
description: 深度调查指南 — 使用 super-fetch 进行多源信息搜集、交叉验证、去伪存真，并给出独到分析。覆盖：事实核查、趋势判断、多方博弈、市场信号。强调：报告不是新闻播报，是决策工具。
user-invocable: true
dependencies:
  - super-fetch
---

# Deep Investigate - 主 Agent 路由指南

## 调查方式

| 情况 | 方式 |
|------|------|
| **默认** | subagent 后台调查 |
| 需要 `-i` 交互登录 | 主会话直接执行（subagent 无法处理交互） |
| 简单问题 | 可选主会话直接执行 |

---

## Subagent 后台调查

### 启动命令

```python
sessions_spawn(
    task="""你是 deep-investigate 的subagent。请读取 /home/xunbu/.openclaw/skills/deep-investigate/SUBAGENT.md，按其中规范执行调查。

调查主题：[用户的具体问题]""",
    runtime="subagent",
    mode="session",
    cleanup="keep",
)
```

### 标准话术

```
收到！我已派专项小组去做深度调查。
主会话恢复正常，你可以继续和我聊天，结果出来我会通知你。
```
