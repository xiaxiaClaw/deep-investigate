# Deep Investigate - Subagent 深度调查指南

> **兼听则明，偏信则暗** — 多角度、多来源、交叉验证

## 核心原则

**事实是骨骼，分析是灵魂。**

- 不是新闻播报器——要给出独到判断，不人云亦云
- 不是资料库——要拎出关键，忽略噪音
- 分析基于事实 + 理论，不基于情绪或猜测
- 精准，不在长——3句打中要害 > 10句隔靴搔痒

---

## 快速开始

### 1. 定义调查问题

明确你要回答的核心问题。

### 2. 规划信息源

根据调查问题，规划需要抓取的 URL（搜索引擎、官方渠道、权威媒体、社交平台等）。

### 3. 批量抓取

> **建议使用会话** (`-s`) 访问需要登录的平台（微博、小红书、知乎等）
> **手动登录平台**（`-i`）首次访问需要登录的网站时可以弹出浏览器让用户手动登录平台（请通知主agent，让主agent与用户交互）

```bash
cd super-fetch

# 所有 fetch.py 调用必须带 -s 使用默认会话（保持登录态，提高反爬绕过成功率）
# ❌ 禁止使用无会话抓取（容易被拦截）
python fetch.py \
  "https://bing.com/search?q=关键词" \
  "https://www.baidu.com/s?wd=关键词" \
  -s

# 指定会话(不使用`-o`时直接显示到控制台)
python fetch.py \
  "https://bing.com/search?q=关键词" \
  "https://www.baidu.com/s?wd=关键词" \
  -s "mysession.json"
```

### 4. 分析并撰写报告

使用提供的报告模板整理结果。

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
# 1. 并发抓取搜索结果（假设搜索微博）- 必须带 -s
python fetch.py "https://s.weibo.com/weibo?q=关键词"  "https://bing.com/search?q=关键词" -s

# 2. 输出中链接（注意区分是网址链接还是图片链接）都是代号，如 @abc-12
#    复制需要反查的代号

# 3. 反查获取真实 URL
python get_link.py @abc-12 @abc-15 @abc-20

# 4. 输出：@abc-12 -> https://weibo.com/1234567890
#    用真实 URL 访问帖子详情 - 必须带 -s
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
