---
name: deep-investigate
description: 深度调查指南 — 使用 super-fetch 进行多源信息搜集、交叉验证、去伪存真，并给出独到分析。覆盖：事实核查、趋势判断、多方博弈、市场信号。强调：报告不是新闻播报，是决策工具。
user-invocable: true
dependencies:
  - super-fetch
---

# Deep Investigate - 深度调查指南

> **兼听则明，偏信则暗** — 多角度、多来源、交叉验证

## 核心原则

**事实是骨骼，分析是灵魂。**

- 不是新闻播报器——要给出独到判断，不人云亦云
- 不是资料库——要拎出关键，忽略噪音
- 分析基于事实 + 理论，不基于情绪或猜测
- 精准，不在长——3句打中要害 > 10句隔靴搔痒

---

## 调查方式

| 情况 | 方式 |
|------|------|
| **默认** | subagent 后台调查 |
| 需要 `-i` 交互登录 | 主会话直接执行（subagent 无法处理交互） |
| 简单问题 | 可选主会话直接执行 |

---

## 快速开始

### 1. 定义调查问题

明确你要回答的核心问题。

### 2. 规划信息源

根据调查问题，规划需要抓取的 URL（搜索引擎、官方渠道、权威媒体、社交平台等）。

### 3. 批量抓取

> **建议使用会话** (`-s`) 访问需要登录的平台（微博、小红书、知乎等）
> **手动登录平台**（`-i`）首次访问需要登录的网站时可以弹出浏览器让用户手动登录平台

```bash
cd super-fetch

# 使用默认会话抓取（推荐，可访问登录内容）
python fetch.py \
  "https://bing.com/search?q=关键词" \
  "https://www.baidu.com/s?wd=关键词" \
  -s 

# 指定会话(不使用`-o`时直接显示到控制台)
python fetch.py \
  "https://bing.com/search?q=关键词" \
  "https://www.baidu.com/s?wd=关键词" \
  -s "mysession.json"

# 无会话抓取（无法访问需登录才能获取的内容）
python fetch.py \
  "https://bing.com/search?q=关键词" \
  "https://www.baidu.com/s?wd=关键词" \
```

### 4. 分析并撰写报告

使用提供的报告模板整理结果。

---

## Subagent 后台调查

### 启动命令

```python
sessions_spawn(
    task="""你是 deep-investigate 的专业调查员。请严格按以下步骤执行。

**第一步：读取规范文件**
1. ~/.openclaw/skills/deep-investigate/references/sources.md
2. ~/.openclaw/skills/deep-investigate/references/report.md

**第二步：澄清核心问题（最重要！）**
在搜集之前，先明确：
- 用户问这个问题的**背景**是什么？
- 用户真正想知道的是什么？（不是表面问题，而是"知道了能用来做什么"）
把核心问题陈述写入 /tmp/core_question.txt

**第三步：锁定主题（最关键一步！）**
写完 core_question.txt 后，**大声读一遍**：
- "我现在的核心问题是：___"
- "搜集的信息必须回答：___"
- "如果我发现自己在搜集偏离主题的内容，立即回到核心问题"

把这个"主题锁定声明"也写入 /tmp/core_question.txt 末尾。

**第四步：搜集信息（必须用 super-fetch）**
围绕核心问题搜集，**拒绝搜集看起来相关但不是核心问题的内容**。
每次抓取URL之前，自问："这个URL回答的是我的核心问题吗？"
至少覆盖4类信息源。

**第五步：写报告到 /tmp/deep_investigate_report.md**

**写报告前，再次阅读 /tmp/core_question.txt，确认报告内容与核心问题对齐。**

报告必须满足：
- ✅ 至少3个**具体数据点**（数字、案例、事实）
- ✅ 至少1个**"反直觉信号"**（大多数人以为X，但其实是Y）
- ✅ 有**可操作的建议**（针对不同对象，给出具体行动建议）
- ✅ 坦诚说明**不确定性**（什么是有根据的，什么是猜测的）
- ❌ 不要凑字数，不相关的不写
- ❌ 不要为了"结构完整"而堆砌内容

**"反直觉信号"是什么？**
- 不是"XX增长了10%"这种普通数据
- 而是：**"与直觉相反的事实"**——众人以为A，却是B
- 例子：美国人以为TikTok威胁国家安全 → 数据：TikTok在青少年中渗透率创历史新高（完全相反）
- 找到1个强力反直觉信号，比10个普通数据更有价值

在报告深度分析中加入"反直觉信号"一节，专门说明这个发现为什么与直觉相悖。

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

调查主题：[用户的具体问题]""",
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

---

## ⚠️ 链接反查（必须掌握的流程）

### 什么时候需要反查？

抓取搜索结果后，所有链接都会变成代号，必须反查才能拿到真实 URL。

典型场景：
- 抓取微博/小红书/知乎搜索结果后，想访问具体帖子
- 批量抓取后处理结果文件，想知道每个链接指向什么
- 想抓取特定帖子详情，但只有搜索结果页

### 反查链接

```bash
# 反查单个链接
python get_link.py @abcd-1

# 反查多个（推荐，一次性反查多个）
python get_link.py @abcd-1 @abcd-2 @abcd-3
```

### 完整使用流程

```bash
# 1. 并发抓取搜索结果（假设搜索微博）
python fetch.py "https://s.weibo.com/weibo?q=关键词"  "https://bing.com/search?q=关键词" -s

# 2. 输出中链接（注意区分是网址链接还是图片链接）都是代号，如 @abc-12
#    复制需要反查的代号

# 3. 反查获取真实 URL
python get_link.py @abc-12 @abc-15 @abc-20

# 4. 输出：@abc-12 -> https://weibo.com/1234567890
#    用真实 URL 访问帖子详情
python fetch.py "https://weibo.com/1234567890" -s

# 如果清理链接数据库
python get_link.py --clear abc
```

**注意**：代号 `@{namespace}-{number}` 中的 namespace 是每次抓取随机生成的，旧的代号在新的抓取中可能失效（但数据库中已保存的映射不受影响）。

### 数据库清理

反查获取真实URL后请及时清理数据库。

```bash
# 清空全部（谨慎使用）
python get_link.py --clear

# 删除特定链接
python get_link.py --clear @abcd-1

# 删除某命名空间下所有链接
python get_link.py --clear abcd
```


**清理要求：**
- `links.db` 会自动清理 7 天前的旧数据（5% 概率触发）
- 抓取大量页面后，可手动执行 `--clear` 释放空间
- 清理后无法反查之前的链接代号

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
- 如果发现有网站不能正确访问，请告知用户，考虑使用参数-i进行手动登录
---


## 详细参考文档

详见 references/ 目录：

- [调查方法论](./references/methodology.md) - 问题定义、信息源选择策略
- [信息源清单](./references/sources.md) - 权威媒体、社交平台、金融市场、预测市场、学术资源、官方渠道
- [super-fetch 工作流](./references/workflow.md) - 准备 URL 列表、批量抓取、结果分析
- [交叉验证框架](./references/verification.md) - 来源可信度评估、信息一致性检查、矛盾点识别
- [查证技巧](./references/tricks.md) - 历史快照、域名信息、元数据分析、其他技巧
- [报告模板](./references/report.md) - 总结报告的结构和 Markdown 模板
