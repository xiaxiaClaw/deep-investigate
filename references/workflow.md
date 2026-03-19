# super-fetch 实战工作流

## 一、准备 URL 列表

根据调查问题，规划需要抓取的 URL。

**示例：调查"X 公司裁员"事件**

需要抓取的 URL：
- 搜索引擎：Bing、百度
- 官方渠道：公司官网、IR 页面
- 权威媒体：财新等
- 社交平台：微博、知乎
- 财经数据：Yahoo Finance、东方财富

## 二、批量抓取

使用 super-fetch 的批量并发功能，直接传 URL 参数：

```bash
# 进入 super-fetch 目录
cd super-fetch

# 基础批量抓取（推荐）
python fetch.py \
  "https://cn.bing.com/search?q=小米裁员" \
  "https://www.baidu.com/s?wd=小米裁员" \
  "https://www.xiaomi.com/about/news" \
  "https://ir.xiaomi.com" \
  "https://www.caixin.com/search/search.jsp?keyword=小米裁员" \
  "https://s.weibo.com/weibo/小米裁员" \
  "https://www.zhihu.com/search?q=小米裁员" \
  "https://finance.yahoo.com/quote/XIACF" \
  "https://quote.eastmoney.com/01810.html" \
  -o investigation.json

# 保守模式（防爬严格的网站，降低并发）
python fetch.py \
  "https://cn.bing.com/search?q=小米裁员" \
  "https://www.baidu.com/s?wd=小米裁员" \
  -c 2 \
  --domain-delay-min 5 \
  --domain-delay-max 10 \
  -o investigation.json

# 需要 JS 渲染的动态页面（默认就是 playwright）
python fetch.py \
  "https://cn.bing.com/search?q=小米裁员" \
  "https://www.baidu.com/s?wd=小米裁员" \
  -e playwright \
  -o investigation.json
```

### 防反爬虫配置

| 参数 | 默认 | 说明 |
|------|------|------|
| `-c, --concurrency` | 3 | 最大并发数（建议 2-5） |
| `--domain-delay-min` | 2.0 | 同一域名最小间隔（秒） |
| `--domain-delay-max` | 5.0 | 同一域名最大间隔（秒） |
| `--jitter` | 0.5 | 额外随机抖动上限（秒） |

```bash
# 保守模式（防爬严格的网站）
python fetch.py \
  "https://url1.com" \
  "https://url2.com" \
  -c 2 \
  --domain-delay-min 5 \
  --domain-delay-max 10 \
  -o investigation.json

# 激进模式（内部网站或不限制的网站）
python fetch.py \
  "https://url1.com" \
  "https://url2.com" \
  -c 10 \
  --domain-delay-min 0.5 \
  --domain-delay-max 1 \
  -o investigation.json
```

## 三、结果分析

输出的 JSON 格式：

```json
[
  {
    "url": "https://example.com/page1",
    "success": true,
    "markdown": "# 标题\n\n内容...",
    "error": null
  },
  {
    "url": "https://example.com/page2",
    "success": true,
    "markdown": "...",
    "error": null
  }
]
```

### 快速查看结果

```bash
# 用 Python 查看
python -c "import json; d=json.load(open('investigation.json')); [print(f'=== {x[\"url\"]} ===\n{x[\"markdown\"][:500]}...\n') for x in d if x['success']]"
```
