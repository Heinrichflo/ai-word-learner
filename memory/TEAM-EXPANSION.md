# OPC 团队扩展计划

## 当前团队（6人）

| ID | 名字 | 职责 |
|---|------|------|
| main | 大总管小爱 | 统筹 |
| dev | 开发助理 | 代码开发 |
| content | 内容助理 | 公众号文章 |
| ops | 运营增长 | 推广 |
| law | 法务助理 | 法务 |
| finance | 财务助理 | 财务 |

## 扩展计划

### 产品线扩展（需要新的飞书账号）

| 代理ID | 名字 | 职责 | 产品 |
|--------|------|------|------|
| dev-product1 | 前端dev | 前端开发 | AI背单词 |
| dev-product2 | 后端dev | 后端开发 | 用户系统 |
| dev-product3 | AI dev | AI功能 | 智能功能 |
| content-blog | 博客写手 | 博客文章 | 博客运营 |
| content-social | 社交运营 | 小红书/抖音 | 社媒运营 |

### 激活方式

每个新代理需要：
1. 在飞书开放平台创建新应用
2. 获取 App ID 和 Secret
3. 在 openclaw.json 添加 bindings
4. 发消息激活

### 任务分配示例

```
产品线1（AI背单词）→ dev-product1
产品线2（待定）→ dev-product2  
内容（公众号）→ content-blog
社媒（小红书）→ content-social
```

## 当前可用的并行任务

目前 main 可以同时给 dev 和 content 布置任务，实现一定程度的并行工作。
