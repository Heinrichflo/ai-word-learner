# ai-news-radar 深度学习 - 核心代码分析

*来自: github.com/LearnPrompt/ai-news-radar*

---

## 核心架构（1700行Python）

### 1. 数据结构

```python
@dataclass
class RawItem:
    site_id: str
    site_name: str
    source: str
    title: str
    url: str
    published_at: datetime | None
    meta: dict[str, Any]
```

### 2. 十大新闻源抓取

| 函数 | 数据源 |
|------|---------|
| fetch_techurls() | techurls.com |
| fetch_buzzing() | buzzing.cc |
| fetch_iris() | iris.findtruman.io |
| fetch_bestblogs() | bestblogs.dev |
| fetch_tophub() | tophub.today |
| fetch_zeli() | zeli.app (Hacker News) |
| fetch_ai_hubtoday() | ai.hubtoday.app |
| fetch_aibase() | aibase.com |
| fetch_aihot() | aihot.today |
| fetch_newsnow() | newsnow.busiyi.world |

### 3. 时间解析（关键！）

```python
def parse_date_any(value, now):
    # 支持格式：
    # - "X分钟前" / "X小时前" / "X天前"
    # - "刚刚" / "昨天"
    # - "今天 12:30" / "昨天 12:30"
    # - "2024年12月30日"
    # - Unix时间戳
    # - RSS标准时间格式
```

### 4. URL归一化（去重核心）

```python
def normalize_url(raw_url):
    # 1. 移除UTM参数
    # 2. 移除追踪参数(ref, spm, fbclid...)
    # 3. 统一小写
    # 4. 移除末尾斜杠
```

### 5. AI关键词过滤

```python
AI_KEYWORDS = [
    "aigc", "llm", "gpt", "claude", "gemini", 
    "deepseek", "openai", "anthropic", "transformer",
    "prompt", "diffusion", "agent",
    "大模型", "人工智能", "机器学习"...
]

# 判断是否AI相关内容
def is_ai_related_record(record):
    # 同时有AI或Tech关键词
    # 排除商业噪声（淘宝、京东促销）
    # 排除娱乐噪声
```

### 6. 双语翻译

```python
def translate_to_zh_cn(session, text):
    # 使用Google Translate API
    # 缓存翻译结果
    # 每次最多80条新翻译
```

### 7. RSS失败处理

```python
RSS_FEED_REPLACEMENTS = {
    "https://rsshub.app/infoq/recommend": "https://www.infoq.cn/feed",
    "https://rsshub.app/huggingface/blog-zh": "https://huggingface.co/blog/feed.xml",
    # 自动替换失效源
}

RSS_FEED_SKIP_PREFIXES = (
    # 跳过的源类型
)
```

### 8. 去重机制

```python
def dedupe_items_by_title_url(items, random_pick=False):
    # 按 title + url 去重
    # 随机选一个或选最新的
```

### 9. 并发抓取

```python
with ThreadPoolExecutor(max_workers=20) as executor:
    # 最多20个并发
```

### 10. 告警输出

```python
status_payload = {
    "failed_sites": [...],
    "zero_item_sites": [...],
    "skipped_feeds": [...],
    "replaced_feeds": [...]
}
```

---

## 关键算法

### 1. 新闻指纹
```python
def make_item_id(site_id, source, title, url):
    key = "||".join([site_id, source, title, url])
    return sha1(key).hexdigest()
```

### 2. 时间窗口
```python
window_start = now - timedelta(hours=24)
if event_time(record) >= window_start:
    # 纳入24小时最新
```

### 3. 归档管理
```python
# 保留45天数据
keep_after = now - timedelta(days=45)
```

---

## 应用到我身上

### 信息抓取技能

**我要实现的核心方法**：

1. **multi_source_fetch()** - 并发抓取多源
2. **parse_time_any()** - 智能时间解析
3. **normalize_url()** - URL归一化
4. **is_ai_related()** - AI内容过滤
5. **translate_bilingual()** - 双语翻译
6. **dedupe()** - 智能去重
7. **archive_manage()** - 归档管理

### 日报生成流程

```
1. 抓取24小时数据
2. AI关键词过滤
3. 去重
4. 双语翻译
5. 按时间排序
6. 生成摘要
7. 推送到公众号
```

---

## 代码特点总结

| 特点 | 实现 |
|------|------|
| 零API依赖 | ✅ 仅用requests |
| 高并发 | ThreadPoolExecutor |
| 智能去重 | SHA1指纹 |
| 失败容错 | RSS自动替换/跳过 |
| 双语支持 | Google Translate缓存 |
| 定时运行 | GitHub Actions |
