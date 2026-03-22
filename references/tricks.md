# 查证技巧

## 一、历史快照 (Wayback Machine)

网站内容可能被删除或修改，用历史快照查看"当时的样子"。

**访问方式：**

| 工具 | URL 格式 |
|------|---------|
| Wayback Machine | `https://web.archive.org/web/*/{url}` |
| archive.is | `https://archive.is/{url}` |
| archive.ph | `https://archive.ph/{url}` |

**示例：**

```bash
# 查看某公司官网的历史版本
# https://web.archive.org/web/*/https://www.example.com

# 批量抓取存档（如果需要）
python fetch.py \
  "https://web.archive.org/web/20240101/https://www.example.com/about" \
  "https://web.archive.org/web/20240201/https://www.example.com/about" \
```

## 二、域名/注册信息 (WHOIS)

查域名注册时间、所有者、变更记录，判断网站背景。

**常用工具：**

| 工具 | URL |
|------|-----|
| WHOIS.com | https://www.whois.com/whois/{domain} |
| DomainTools | https://whois.domaintools.com/{domain} |
| ViewDNS | https://viewdns.info/whois/?domain={domain} |
| ICANN | https://lookup.icann.org |

**查证要点：**
- 域名注册时间是否与"成立时间"吻合？
- 注册人/组织是否真实存在？
- 域名是否频繁更换注册商/NS？
- DNS 记录解析到哪里？

## 三、元数据分析

**图片 EXIF：**
- 拍摄时间、地点、设备型号
- 可用在线工具：https://exifinfo.org/

**PDF 元数据：**
- 作者、创建时间、修改历史
- 用 `pdfinfo` 命令查看

**视频元数据：**
- 编码信息、拍摄设备

## 四、404/内容缺失处理流程

当目标 URL 返回 404 或内容被移除时，按以下优先级处理：

| 优先级 | 方法 | 示例 |
|--------|------|------|
| 1 | 尝试 URL 变体（去掉路径最后一段、换 https/http） | `volcengine.com/doc/xxx` → `volcengine.com/act/xxx` |
| 2 | 用 `site:zhihu.com` 或 `site:36kr.com` 搜索 | `site:zhihu.com 火山引擎 Coding Plan` |
| 3 | 查 Wayback Machine 历史快照 | `https://web.archive.org/web/*/{url}` |
| 4 | 搜索"产品名 + 官方/官网" | `火山方舟 官网` |
| 5 | 查科技媒体替代报道 | 搜索"产品名 + 评测/体验/教程" |

### 批量 URL 存活检测

如果需要同时检测多个 URL 是否存活，可以用以下方式：

```bash
# 并发检测多个 URL 的 HTTP 状态码（简单版）
for url in "url1" "url2" "url3"; do
  status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
  echo "$url → $status"
done
```

### 反向搜索补全法

如果已知 A、B、C 三家平台都提供某产品，但找不到 D，可以：
1. 搜索"A B C D {产品名}" — D 可能出现在对比文章中
2. 搜索"{产品名} 对比 评测" — 对比文章通常会列举所有玩家
3. 搜索"{产品名} 知乎" — 知乎问答会有人整理

---

## 五、其他技巧

### 反向图片搜索
- Google 图片搜索：https://images.google.com
- TinEye：https://tineye.com
- 用于识别图片来源、是否被 PS、是否网图

### IP/服务器侦查
- `ping {domain}` - 看 IP
- `traceroute {domain}` - 看路由
- `nslookup -type=NS {domain}` - 查域名服务器
- Shodan：https://www.shodan.io - 设备搜索引擎

### 相关公司查证
- 天眼查 / 企查查 - 中国公司关系
- OpenCorporates - 全球公司数据
- SEC EDGAR - 美国公司备案

### 社交媒体侧写
- 查看发帖历史
- 关注/粉丝列表
- 点赞/转发记录
- 平台间关联（同头像、同用户名）
