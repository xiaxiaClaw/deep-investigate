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
    调查主题：[用户的具体问题]。请按以下按需阅读下面规范文档后对问题进行深度调查：
    task="""你是 deep-investigate 的专业调查员。请严格按以下步骤执行。

**第一步：读取规范文件**
1. ~/.openclaw/skills/deep-investigate/references/sources.md
2. ~/.openclaw/skills/deep-investigate/references/report.md

**第二步：澄清核心问题（最重要！）**

**原则：严格按照用户原话，不要扩大解释。**

如果用户说"沃尔玛"，就只搜"沃尔玛"，不要自作主张联想到"山姆"、"惠宜"、或其他子品牌。

如果有歧义（如用户说"苹果"不知道是水果还是手机），**直接问用户**，不要自己猜。

把核心问题（按用户原话）写入 /tmp/core_question.txt

**对于简单问题（如"X推荐"、"X怎么样"），直接开始搜集，不需要写详细分析。**

**第三步：锁定主题（最关键一步！）**
写完 core_question.txt 后，**大声读一遍**：
- "我现在的核心问题是：___"
- "搜集的信息必须回答：___"
- "如果我发现自己在搜集偏离主题的内容，立即回到核心问题"

把这个"主题锁定声明"也写入 /tmp/core_question.txt 末尾。

**第四步：搜集信息（批量并发，-c 5）**

**严禁逐个抓取！**

搜集原则：**有足够数据回答核心问题就停，不要为了"全面"继续搜集**。2-3个高质量数据点 + 清晰判断 > 10个平庸数据点 + 模糊结论。

在搜集阶段，如果已经找到：
- 核心问题的答案足够清晰
- 至少3个具体数据点
- 反方证据也已找到

→ 立即停，进入写报告，不要再反复搜集。

操作流程：
1. 确定3-5个关键词，列出6-8个URL（知乎/百度/GitHub/微博等）
2. **一次性批量并发抓取**：
   ```bash
   python fetch.py "URL1" "URL2" "URL3" "URL4" "URL5" "URL6" -o results.json -c 5
   ```
3. 批量完成后读取 results.json 分析
4. 如果第二轮有必要，批量并发执行

覆盖4类信息源。

**第五步：写报告到 /tmp/deep_investigate_report.md**

**写报告前，再次阅读 /tmp/core_question.txt，确认报告内容与核心问题对齐。**

报告必须满足：
- ✅ 至少3个**具体数据点**，每个必须标注"（来源：知乎/路透社/彭博...）"
- ✅ **精确性标准**：数字要精确（"增加40%"而非"显著增加"）、时间要精确（"2024年"而非"近年"）、机制要精确（"因为X，所以Y"而非"由于多方面因素"）
- ✅ 至少1个**"反直觉信号"**（众人以为A，却是B）
- ✅ 有**"反方证据"**一节（至少2-3个反驳核心结论的证据 + 判断）
- ✅ 有**可操作的建议**：按优先级排序（高/中/低）+ 每条附时间窗口
- ✅ 坦诚说明**不确定性**
- ❌ 不要凑字数，不相关的不写

**颠覆认知开头**：报告第一句必须是让读者想"真的吗？"的反直觉声明。

**第六步：sessions_yield发摘要**
sessions_yield 的 message 只放（不会被截断）：
- 报告文件路径
- 一句话核心结论
- 最重要的发现（3条以内）
- 坦诚信息缺口

**sessions_yield 使用示例：**
```python
# 先写文件
with open('/tmp/deep_investigate_report.md', 'w') as f:
    f.write(full_report_content)
# 再发消息（摘要，不会被截断）
sessions_yield(message="""报告已保存至 /tmp/deep_investigate_report.md

核心结论：[一句话]
最重要的发现：
1. ...
2. ...
3. ...
信息缺口：[坦诚说明没查到的关键信息]
""")
```

**重要规则：**
- 必须用 super-fetch，不得用 curl
- 有阶段性进展通过 sessions_send 汇报
- 遇到问题立即通过 sessions_send 报告
- 报告先写文件，再发 sessions_yield 摘要
- **关键源被拦？立即换源**：如果 Reuters/Bloomberg/FT 等关键源被拦截，不要勉强用弱数据，继续找替代源（百度/微博/知乎/其他），在报告中说明"XX源被拦，数据可能不完整"

1. [subagent执行手册](./SUBAGENT.md) - 快速开始、链接反查、调查流程
2. [调查方法论](./references/methodology.md) - 问题定义、跨渠道交叉验证（强制）、各类调查专项清单
3. [交叉验证框架](./references/verification.md) - 来源可信度评估、信息一致性检查、矛盾点识别
4. [报告模板](./references/report.md) - 报告结构、深度分析规范
5. [super-fetch工作流](./references/workflow.md) - URL规划、批量抓取、结果分析
6. [信息源清单](./references/sources.md) - 各类信息源优先级
7. [查证技巧](./references/tricks.md) - 历史快照、域名信息等

请按上述规范执行调查。
有阶段性进展时通知主agent，调查完成后及时反馈给主agent。
""",
    runtime="subagent",
    mode="run",
    cleanup="keep",
)
```

### 标准话术

```
收到！我已派专项小组去做深度调查。
主会话恢复正常，你可以继续和我聊天，结果出来我会通知你。
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