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
    task="""你是 deep-investigate 的 subagent。
    调查主题：[用户的具体问题]。

请先阅读 [subagent执行手册](./SUBAGENT.md)，其中包含完整的调查流程和所有参考文档链接。请严格按照其中的规范执行调查：
- 调查方法论（问题定义、交叉验证）
- 交叉验证框架（来源可信度评估、信息一致性检查）
- 报告模板（报告结构、深度分析规范）
- super-fetch工作流（URL规划、批量抓取）
- 信息源清单（优先使用权威来源）
- 查证技巧（历史快照、域名信息等）

有阶段性进展时通知主agent，调查完成后及时反馈给主agent。
最终报告写在`~/.openclaw/deep-investigate/investigation`目录下
""",
    runtime="subagent",
    mode="session",
    cleanup="keep",
)
```

## 报告用户
在收到subagent完整的调查报告后，请将**完整的调查报告**发送给用户（如果很长可以发送文件）
如果收到subagent想要用户登录或者交互的情况，则也告知用户。

### 标准话术

```
收到！我正在就该问题进行深度调查。
深度调查需要一些时间，你可以继续和我聊天，结果出来我会通知你。
```

## 详细参考文档

详见 references/ 目录：
- [SUBAGENT执行手册](./SUBAGENT.md) 
- [调查方法论](./references/methodology.md) - 问题定义、信息源选择策略
- [信息源清单](./references/sources.md) - 权威媒体、社交平台、金融市场、预测市场、学术资源、官方渠道
- [super-fetch 工作流](./references/workflow.md) - 准备 URL 列表、批量抓取、结果分析
- [交叉验证框架](./references/verification.md) - 来源可信度评估、信息一致性检查、矛盾点识别
- [查证技巧](./references/tricks.md) - 历史快照、域名信息、元数据分析、其他技巧
- [报告模板](./references/report.md) - 总结报告的结构和 Markdown 模板