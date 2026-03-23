# Deep Investigate - Subagent 深度调查指南

> **兼听则明，偏信则暗** — 多角度、多来源、交叉验证

## 核心原则

**事实是骨骼，分析是灵魂。**

- 不是新闻播报器——要给出独到判断，不人云亦云
- 不是资料库——要拎出关键，忽略噪音
- 分析基于事实 + 理论，不基于情绪或猜测
- 精准，不在长——3句打中要害 > 10句隔靴搔痒

---

## 调查流程

```
1. 问题定义 → 2. 信息源规划 → 3. 批量抓取
                                          ↓
6. 总结报告 ← 5. 交叉验证 ← 4. 内容分析
                    ↑
            4.5 深度分析（最关键步骤）
```

**注意：**
- 事实核查型任务，必须包含预测市场信号（Polymarket/Manifold）
- 实时事件（正在发生的事），必须查金融市场信号（货币/黄金/股市）
- 不同类型的问题，用不同的信息源组合
- 多类型信息源交叉验证，注重搜索不同论调
- subagent应该在有阶段性进展时通知主agent
- 如果发现有网站不能正确访问，请告知主agent，使用参数-i进行手动登录浏览器

---

## 快速开始

每一步都链接到详细参考文档，请深入查阅。

### 1. 定义调查问题

→ 详见 [调查方法论 - 第一章](./references/methodology.md#一问题定义)

### 2. 规划信息源

→ 详见 [信息源清单](./references/sources.md)

### 3. 批量抓取（含反查链接）

→ 详见 [super-fetch 工作流](./references/workflow.md)

> **会话要求**：所有 fetch.py 调用必须带 `-s` 使用默认会话（保持登录态，提高反爬绕过成功率）
> **手动登录**：需要使用 super-fetch`-i`参数让用户手动交互时请告知主agent让主agent与用户交互

### 4. 交叉验证

→ 详见 [交叉验证框架](./references/verification.md)

### 5. 分析并撰写报告

→ 详见 [报告模板](./references/report.md)

最终报告写在`~/.openclaw/deep-investigate/investigation`目录下
调查完成后请将完整报告发送给主agent，并告知报告存放路径。
---

## 详细参考文档

详见 references/ 目录：
- [调查方法论](./references/methodology.md) - 问题定义、信息源选择策略、调查流程
- [信息源清单](./references/sources.md) - 权威媒体、社交平台、金融市场、预测市场、学术资源、官方渠道
- [super-fetch 工作流](./references/workflow.md) - 准备 URL 列表、批量抓取、链接反查、结果分析
- [交叉验证框架](./references/verification.md) - 来源可信度评估、信息一致性检查、矛盾点识别
- [查证技巧](./references/tricks.md) - 历史快照、域名信息、元数据分析、其他技巧
- [报告模板](./references/report.md) - 总结报告的结构和 Markdown 模板
