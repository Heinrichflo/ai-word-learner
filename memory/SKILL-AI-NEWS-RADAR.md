# AI News Radar 技能 - 深入学习

*来自: github.com/LearnPrompt/ai-news-radar*

---

## 项目概述

**AI Signal Board** - 高质量 AI/科技新闻聚合项目

### 核心特点
- **无需 API** 即可运行
- 静态网页展示
- 24小时增量更新
- 支持 OPML RSS 批量接入
- 失败源自动处理与告警

---

## 核心功能

### 1. 多源聚合（10个网页源）
- TechURLs / Buzzing / Info Flow
- BestBlogs / TopHub / Zeli
- AI HubToday / AIbase
- AI今日热榜 / NewsNow

### 2. RSS 接入
- 支持 OPML 批量导入
- 私有 RSS（通过 GitHub Secrets）
- 自动失败源替换

### 3. 24小时双视图
- `AI强相关` 视图
- `全量` 视图

### 4. 去重机制
- AI模式默认去重
- 全量模式可开关

### 5. 数据输出
- `latest-24h.json` - 24小时最新
- `archive.json` - 归档
- `source-status.json` - 源状态

---

## 技术实现

### 核心脚本
```bash
# 本地运行
python scripts/update_news.py --output-dir data --window-hours 24 --rss-opml feeds/follow.opml

# 启动静态网页
python -m http.server 8080
```

### GitHub Actions 自动更新
```yaml
# .github/workflows/update-news.yml
- 每30分钟自动执行
- 自动提交data/*到仓库
```

### 依赖
- feedparser - RSS解析
- requests - HTTP请求
- beautifulsoup4 - 网页解析

---

## 告警机制

| 类型 | 说明 |
|------|------|
| failed_feeds | 抓取失败的源 |
| zero_item_feeds | 无内容的源 |
| skipped_feeds | 跳过的源 |
| replaced_feeds | 替换的源 |

---

## 应用到我的技能系统

### 1. 信息抓取技能
- 定时抓取AI/科技新闻
- 聚合多源信息
- 自动去重

### 2. 日报生成技能
- 提取24小时热点
- 生成摘要
- 推送到公众号

### 3. 竞品监控技能
- 监控竞品动态
- 自动告警

---

## 关键代码片段

### 定时抓取
```python
import schedule
import time

def job():
    os.system('python scripts/update_news.py --output-dir data --window-hours 24')

schedule.every(30).minutes.do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

### RSS 解析
```python
import feedparser

def parse_rss(url):
    feed = feedparser.parse(url)
    return [{
        'title': entry.title,
        'link': entry.link,
        'published': entry.published
    } for entry in feed.entries]
```

---

## 总结

这个项目的核心价值：
1. **零API成本** - 不需要额外付费
2. **自动化** - GitHub Actions 定时运行
3. **去重** - 智能过滤重复内容
4. **告警** - 失败源自动处理

**我要集成的技能**：
- 定时新闻抓取
- 日报自动生成
- 竞品动态监控
