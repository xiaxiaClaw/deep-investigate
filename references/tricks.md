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
  -o archive_results.json
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

## 四、其他技巧

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
