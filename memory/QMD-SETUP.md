# qmd 配置

## 安装
```bash
# 安装 bun
curl -fsSL https://bun.sh/install | bash

# 安装 qmd
git clone https://github.com/tobi/qmd.git
cd qmd && bun install && bun run build
```

## 使用
```bash
~/.bun/bin/bun /tmp/qmd/dist/qmd.js query "关键词" -c main-workspace
```

## 索引路径
/root/.openclaw/workspace/*.md

## 状态
- qmd 已安装
- 模型待下载
