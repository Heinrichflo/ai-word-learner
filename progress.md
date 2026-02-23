# AI 背单词小程序 - 开发进度

## 2026-02-23

### ✅ 已完成功能

| 功能 | 状态 | 说明 |
|------|------|------|
| 背单词模式 | ✅ | 闪卡展示 + 认识/不认识/困难 |
| AI 出题 | ✅ | 选择题形式 + MiniMax API 预留 |
| 发音测评 | ✅ | 录音 + 评分（Web Speech API） |
| 用户登录 | ✅ | 本地存储用户名和级别 |
| 学习记录 | ✅ | 历史记录保存和展示 |
| 级别筛选 | ✅ | 根据用户级别筛选单词 |

### MiniMax API 配置

如需启用 AI 智能出题功能，在代码中设置：
```javascript
const MINIMAX_API_KEY = '你的API密钥';
```

### 文件位置
- `/root/.openclaw/workspace-dev/word-ai/index.html`

### 运行方式
直接在浏览器打开 `index.html` 即可。
