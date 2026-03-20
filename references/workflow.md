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
  --domain-delay-min 3 \
  --domain-delay-max 6 \
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

### 登录态抓取

部分网站（如微博、知乎、付费媒体等）需要登录后才能查看完整内容。使用 `-i` 交互模式先登录：

```bash
# 1. 先在浏览器中登录（交互模式）
python fetch.py "https://weibo.com" -i

# 2. 登录成功后，后续抓取使用保存的会话
python fetch.py "https://weibo.com/search" -s
```

## 三、补全策略：当搜索结果不够全面时

如果第一次搜索结果有明显遗漏，按以下顺序补全：

### 第一步：多轮搜索补全

```bash
# 已知：腾讯云、阿里云有 Coding Plan
# 目标：找"还有谁"

# 第二轮搜索：找更多平台
python fetch.py \
  "https://www.baidu.com/s?wd=Coding+Plan+还有哪些平台" \
  "https://www.baidu.com/s?wd=火山引擎+Coding+Plan" \
  "https://www.baidu.com/s?wd=月暗+Kimi+Coding+Plan" \
  "https://www.baidu.com/s?wd=智谱+GLM+Coding+Plan" \
  -o round2.json

# 第三轮搜索：对比类文章
python fetch.py \
  "https://www.baidu.com/s?wd=Coding+Plan+对比+哪个好" \
  "https://www.zhihu.com/search?type=content&q=Coding+Plan+订阅+对比" \
  -o round3.json
```

### 第二步：换平台绕过 404

如果官方页面 404，用以下替代来源：
- 科技媒体（36kr、虎嗅、极客公园）
- 知乎问答
- 搜索引擎缓存 `cache:url`
- Wayback Machine

### 第三步：批量存活检测

当有一堆 URL 要验证时，先快速检测哪些还活着：

```bash
# 检测 URL 列表的 HTTP 状态
for url in \
  "https://volcengine.com/product/ark-claw" \
  "https://volcengine.com/pricing/ark" \
  "https://ark.cn-beijing.volces.com" \
  "https://www.volcengine.com/document/detail/PAuPpD8Q1683368812"; do
  status=$(curl -s -o /dev/null -w "%{http_code}" --max-time 10 "$url" 2>/dev/null)
  echo "$status $url"
done
```

---

## 四、结果分析

输出的 JSON 格式：

```json
{
  "total": 10,
  "success": 8,
  "failed": 2,
  "results": [
    {
      "url": "https://example.com/page1",
      "success": true,
      "title": "页面标题",
      "content": "# 标题\n\n内容...",
      "namespace": "abcd",
      "error": ""
    },
    {
      "url": "https://example.com/page2",
      "success": false,
      "title": "",
      "content": "",
      "namespace": "",
      "error": "连接超时"
    }
  ]
}
```

### 快速查看结果

```bash
# 用 Python 查看成功的结果
python -c "import json; d=json.load(open('investigation.json')); [print(f'=== {x[\"url\"]} ===\n{x[\"content\"][:500]}...\n') for x in d['results'] if x['success']]"

# 查看失败的情况
python -c "import json; d=json.load(open('investigation.json')); [print(f'{x[\"url\"]}: {x[\"error\"]}') for x in d['results'] if not x['success']]"
```
