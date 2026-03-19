---
name: deep-investigate
description: 深度调查指南 — 使用 super-fetch 进行多源信息搜集、交叉验证、去伪存真的方法论
user-invocable: true
dependencies:
  - super-fetch
---

# Deep Investigate - 深度调查指南

> **兼听则明，偏信则暗** — 多角度、多来源、交叉验证

## 快速开始

### 1. 定义调查问题

明确你要回答的核心问题。

### 2. 规划信息源

根据调查问题，规划需要抓取的 URL（搜索引擎、官方渠道、权威媒体、社交平台等）。

### 3. 批量抓取

```bash
cd super-fetch
python fetch.py \
  "https://cn.bing.com/search?q=关键词" \
  "https://www.baidu.com/s?wd=关键词" \
  -o investigation.json
```

### 4. 分析并撰写报告

使用提供的报告模板整理结果。

---

## 调查流程

```
1. 问题定义 → 2. 信息源规划 → 3. 批量抓取
                                              ↓
6. 总结报告 ← 5. 交叉验证 ← 4. 内容分析
```

---

## 搜索引擎速查表

只使用 Bing 和百度：

| 类型 | 引擎 | URL 模板 |
|------|------|---------|
| 通用 | Bing | `https://cn.bing.com/search?q={q}` |
| 通用 | 百度 | `https://www.baidu.com/s?wd={q}` |

---

## 详细参考文档

详见 references/ 目录：

- [调查方法论](./references/methodology.md) - 问题定义、信息源选择策略
- [信息源清单](./references/sources.md) - 权威媒体、社交平台、金融市场、预测市场、学术资源、官方渠道
- [super-fetch 工作流](./references/workflow.md) - 准备 URL 列表、批量抓取、结果分析
- [交叉验证框架](./references/verification.md) - 来源可信度评估、信息一致性检查、矛盾点识别
- [查证技巧](./references/tricks.md) - 历史快照、域名信息、元数据分析、其他技巧
- [报告模板](./references/report.md) - 总结报告的结构和 Markdown 模板
