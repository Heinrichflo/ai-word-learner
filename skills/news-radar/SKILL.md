# news-radar 技能

> AI新闻雷达 - 自动抓取AI/科技热点，生成每日简报

## 技能描述

定时抓取多源AI/科技新闻，生成中英双语每日简报。

## 核心方法

### 1. 并发抓取多源

```javascript
async function fetchAllSources() {
    const sources = [
        { name: 'TechURLs', url: 'https://techurls.com/' },
        { name: 'Buzzing', url: 'https://www.buzzing.cc/feed.json' },
        { name: 'HackerNews', url: 'https://hacker-news.firebaseio.com/v0/topstories.json' },
        // ...
    ];
    
    const results = await Promise.all(
        sources.map(s => fetchSource(s))
    );
    return results.flat();
}
```

### 2. 时间解析

支持格式：
- "X分钟前" → now - X minutes
- "X小时前" → now - X hours  
- "X天前" → now - X days
- Unix timestamp → Date

### 3. URL归一化

```javascript
function normalizeUrl(url) {
    try {
        const u = new URL(url);
        // 移除UTM参数
        u.searchParams.delete('utm_source');
        u.searchParams.delete('utm_medium');
        u.searchParams.delete('utm_campaign');
        // 移除末尾斜杠
        return u.toString().replace(/\/$/, '');
    } catch {
        return url;
    }
}
```

### 4. AI关键词过滤

```javascript
const AI_KEYWORDS = [
    'ai', 'aigc', 'llm', 'gpt', 'claude', 'gemini',
    'deepseek', 'openai', 'anthropic', 'transformer',
    '大模型', '人工智能', '机器学习', '智能体'
];

function isAIRelated(title, source) {
    const text = `${title} ${source}`.toLowerCase();
    return AI_KEYWORDS.some(k => text.includes(k));
}
```

### 5. 去重

```javascript
function dedupe(items) {
    const seen = new Set();
    return items.filter(item => {
        const key = normalizeUrl(item.url) + '||' + item.title;
        if (seen.has(key)) return false;
        seen.add(key);
        return true;
    });
}
```

### 6. 双语翻译

使用翻译API将英文标题翻译为中文。

### 7. 生成简报

```javascript
function generateReport(items) {
    return `# 每日AI简报
    
## 今日热点 (${items.length}条)

${items.map((item, i) => `${i+1}. ${item.title}`).join('\n')}

---
Generated at ${new Date().toISOString()}
`;
}
```

## 触发方式

- 定时任务（每30分钟/每小时）
- 手动触发：`帮我抓取今天的AI新闻`

## 输出

- JSON格式新闻列表
- Markdown格式简报
- 推送到公众号/飞书
