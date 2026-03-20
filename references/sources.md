# 信息源清单

## 一、搜索引擎（起点）

| 引擎 | URL 模板 | 特点 |
|------|---------|------|
| Bing | `https://bing.com/search?q={query}` | 全球信息，可用性好 |
| 百度 | `https://www.baidu.com/s?wd={query}` | 中文内容优势 |

使用示例：

```bash
# Bing 搜索
python fetch.py "https://bing.com/search?q=关键词"

# 百度搜索
python fetch.py "https://www.baidu.com/s?wd=关键词"
```

## 二、权威媒体

### 中文权威

| 媒体 | 首页 | 搜索/新闻页 |
|------|------|------------|
| 新华社 | https://www.xinhuanet.com | https://so.news.cn/#search/0/{query}/1 |
| 人民网 | https://www.people.com.cn | http://search.people.cn/s?keyword={query} |
| 央视网 | https://www.cctv.com | https://search.cctv.com/search.php?qtext={query} |
| 澎湃新闻 | https://www.thepaper.cn | https://www.thepaper.cn/searchResult.jsp?index=0&k={query} |
| 财新网 | https://www.caixin.com | https://search.caixin.com/search/search.jsp?keyword={query} |
| 第一财经 | https://www.yicai.com | https://www.yicai.com/search/?keywords={query} |

### 英文权威

| 媒体 | 首页 | 备注 |
|------|------|------|
| Reuters | https://www.reuters.com | 路透社，客观中立 |
| AP News | https://apnews.com | 美联社 |
| BBC | https://www.bbc.com | 英国广播公司 |
| NYT | https://www.nytimes.com | 纽约时报 |
| WSJ | https://www.wsj.com | 华尔街日报（财经） |
| FT | https://www.ft.com | 金融时报 |
| Bloomberg | https://www.bloomberg.com | 彭博（财经） |

## 三、社交平台

| 平台 | URL/搜索方式 | 特点 |
|------|-------------|------|
| X (Twitter) | https://x.com/search?q={query} | 全球热点最快 |
| 微博 | https://s.weibo.com/weibo/{query} | 中国舆论场 |
| 知乎 | https://www.zhihu.com/search?q={query} | 深度讨论 |
| 小红书 | https://www.xiaohongshu.com/search?keyword={query} | 消费产品/生活方式测评首选 |
| Reddit | https://www.reddit.com/search/?q={query} | 西方社区 |
| 抖音/ TikTok | 应用内搜索 | 视频信息 |
|---------|-----------|
| 地缘政治、金融市场、重大事件 | 权威媒体（Reuters/Bloomberg/FT）+ 预测市场 |
| 学术/技术问题 | Google Scholar / arXiv |
| **消费产品/生活方式/购物攻略** | **小红书**（真实用户测评）|
| 中国舆论场、名人话题 | 微博 |
| 深度讨论/问答 | 知乎 |
| 全球热点/实时新闻 | X (Twitter) |

## 四、金融市场

| 类型 | 平台 | URL |
|------|------|-----|
| 美股 | Yahoo Finance | https://finance.yahoo.com/quote/{symbol} |
| 美股 | TradingView | https://www.tradingview.com/symbol/{exchange}:{symbol} |
| A股 | 东方财富 | https://quote.eastmoney.com/{symbol}.html |
| A股 | 同花顺 | http://stock.10jqka.com.cn/{symbol}/ |
| 港股 | 腾讯自选股 | https://gu.qq.com/hk/{symbol} |
| 加密 | CoinGecko | https://www.coingecko.com/en/coins/{coin} |

### 地缘危机专用金融市场信号（调查实时事件时必查）

| 类型 | 为什么重要 | 关键平台 |
|------|----------|---------|
| **货币汇率** | 事件国的货币立即反映信心 | bonbast.com（伊朗里亚尔）、Tgju.org（伊朗黄金/外汇） |
| **黄金价格** | 地缘危机晴雨表 | Kitco.com、BullionVault |
| **原油价格** | 中东危机直接影响能源 | CME Group、OilPrice.com |
| **VIX恐慌指数** | 全球风险偏好 | CBOE |
| **美元指数** | 避险资金流向 | DXY 指数 |

```bash
# 伊朗里亚尔（实时）
python fetch.py "https://www.tgju.org/"  # 伊朗黄金外汇市场
python fetch.py "https://www.bonbast.com/"  # 里亚尔黑市汇率

# 黄金/原油
python fetch.py "https://www.kitco.com/charts/livegold.html"
python fetch.py "https://oilprice.com/"
```

## 五、预测市场

预测市场反映"用钱投票"的观点，有时比媒体更准确。

| 平台 | URL | 特点 |
|------|-----|------|
| Polymarket | https://polymarket.com | 美国政治、事件预测 |
| Manifold | https://manifold.markets | 各类预测，加密货币结算 |
| Kalshi | https://kalshi.com | 受监管的预测市场 |
| PredictIt | https://www.predictit.org | 政治预测市场 |

### 预测市场使用技巧

```bash
# 搜索相关问题
python fetch.py "https://polymarket.com/events/{关键词}"
python fetch.py "https://manifold.markets/search?q={关键词}"

# 分析框架
# 1. 找相关问题（不一定完全匹配）
# 2. 记录概率 + 交易量（交易量越大越可信）
# 3. 特别注意：低概率 + 高交易量 = 聪明钱在押注
# 4. 对比不同时间点的概率变化
```

## 六、学术资源

| 平台 | URL | 特点 |
|------|-----|------|
| Google Scholar | https://scholar.google.com/scholar?q={query} | 全球学术文献 |
| Semantic Scholar | https://www.semanticscholar.org/search?q={query} | AI 驱动的学术搜索 |
| arXiv | https://arxiv.org/search/?query={query} | 预印本（CS/物理等） |
| 知网 | https://kns.cnki.net/kns8s/ | 中文学术 |

## 七、官方/公司渠道

| 类型 | 查找方式 |
|------|---------|
| 公司官网 | `https://www.{company}.com` |
| IR（投资者关系） | `ir.{company}.com` 或 `investor.{company}.com` |
| 新闻稿 | 官网"新闻"、"媒体"栏目 |
| SEC 备案（美股） | https://www.sec.gov/edgar |
| 香港联交所 | https://www.hkex.com.hk |
| 上交所/深交所 | 官网"披露"栏目 |

### 各国官方媒体（实时事件必查）

当调查涉及某国的实时事件时，该国官方媒体是"ground truth"——即使被封锁也要尝试。

| 国家 | 官方媒体 | 备注 |
|------|---------|------|
| 伊朗 | IRNA (irna.ir)、FARS News (farsnews.ir)、Tasnim News | 可能被封锁，尝试英语版 |
| 俄罗斯 | RT (rt.com)、TASS | 官方口径 |
| 以色列 | IBA (iba.org.il)、Jerusalem Post | 战争期间重要 |
| 中国 | 新华社、人民网、CCTV | 官方立场 |

```bash
# 伊朗官方媒体（可能需要备用域名）
python fetch.py "https://www.irna.ir/"
python fetch.py "https://www.farsnews.ir/"
```
