# Z-Image API 文档规范

## 目录结构一致性原则

**核心原则：Tab名 = 目录名 = URL路径**

```
Tab: "Getting Started" → 目录: getting-started/ → URL: /getting-started/*
Tab: "Seedream API"    → 目录: seedream/        → URL: /seedream/*
Tab: "Wan 2.2 API"     → 目录: wan22/           → URL: /wan22/*
Tab: "Common"          → 目录: common/          → URL: /common/*
Tab: "Resources"       → 目录: resources/       → URL: /resources/*
```

**禁止**：
- ❌ Tab名和目录名不一致（如Tab叫"Market"但目录是market/）
- ❌ 创建中间层目录（如guides/、examples/）
- ❌ 在根目录放内容页面（如overview.mdx、index.mdx）

## 链接格式规范

### 相对路径（必须使用）

```markdown
✅ 正确：[Quick Start](getting-started/quickstart)
✅ 正确：[Seedream API](seedream/image-generation)
✅ 正确：[同目录](support)
✅ 正确：[上级目录](../common/authentication)

❌ 错误：[Quick Start](/quickstart)
❌ 错误：[Seedream](/guides/image-generation)
```

### 计算相对路径规则

从当前文件位置计算：

```
当前文件：resources/logs.mdx
目标文件：common/reference/create-task.mdx
相对路径：../common/reference/create-task

当前文件：common/reference/webhooks.mdx
目标文件：common/reference/get-task-status.mdx
相对路径：get-task-status（同目录）

当前文件：seedream/image-generation.mdx
目标文件：common/authentication.mdx
相对路径：../common/authentication
```

### 检查链接

提交前运行：
```bash
npm run check:links
```

## 内容规范

### Frontmatter标准

```yaml
---
title: 动词开头，50字以内（Getting Started / Create Task）
description: 一句话说清页面用途，用于SEO和预览
---
```

### 页面长度限制

- **Getting Started**: ≤200行（5分钟能读完）
- **API Reference**: 每个endpoint独立页面，≤300行
- **其他页面**: ≤400行
- **硬性限制**: 单页不超过500行（超过必须拆分）

### 代码示例标准

每个API endpoint必须提供3种语言示例：

```markdown
<CodeGroup>
```bash cURL
curl -X POST "https://z-image.vip/api/v1/jobs/createTask" \
  -H "Authorization: Bearer $API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"seedream-text-to-image","inputs":{...}}'
```

```javascript Node.js
const response = await fetch('https://z-image.vip/api/v1/jobs/createTask', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${process.env.API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({...})
});
```

```python Python
import requests
response = requests.post(
    'https://z-image.vip/api/v1/jobs/createTask',
    headers={'Authorization': f'Bearer {os.getenv("API_KEY")}'},
    json={...}
)
```
</CodeGroup>
```

**要求**：
- 示例必须可运行（真实endpoint，虚构ID用`task_clxxxxxx`格式）
- 同时提供成功和失败响应示例
- 包含常见错误处理

### API端点页面模板

每个endpoint页面必须包含：

1. **一句话描述**（What it does）
2. **完整请求示例**（3种语言）
3. **完整响应示例**（成功+失败）
4. **参数表格**（required/optional明确标注）
5. **常见错误FAQ**（前3个高频问题）

示例结构：
```markdown
---
title: Create Task
description: 'Submit a new AI generation task'
---

## Overview
One sentence explaining what this endpoint does.

## Request

<CodeGroup>
[3 language examples]
</CodeGroup>

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| model | string | ✅ Yes | Model identifier |
| inputs | object | ✅ Yes | Model-specific parameters |

## Response

### Success (200)
```json
{
  "code": 200,
  "data": {...}
}
```

### Error (400)
```json
{
  "code": 400,
  "message": "Invalid model"
}
```

## Common Issues

**Q: Why do I get "Insufficient credits"?**
A: Check your balance at [dashboard](https://z-image.vip/admin)

[More FAQs...]
```

## 同步机制

### API变更追踪

在 `packages/api/CHANGELOG.md` 记录所有API变更：

```markdown
## 2025-02-04
- Wan 2.2: 新增12秒选项（duration: 12）
- Seedream: 4K分辨率正式支持

## 2025-01-15
- 新增webhook支持
```

### 文档更新流程

1. **API代码变更时**：同步更新CHANGELOG.md
2. **每周检查**：对比CHANGELOG和文档，同步更新
3. **发版前checklist**：确认文档已更新所有API变化

### Discord/Telegram FAQ同步

每月第一周：
1. 收集Discord/Telegram高频问题（support频道）
2. 添加到对应文档的FAQ部分
3. 更新resources/support.mdx的"常见问题"章节

## 主题配置

**固定配置**（不要改）：

```json
{
  "theme": "maple",
  "colors": {
    "primary": "#FF9A56",
    "light": "#FFB380",
    "dark": "#E67E3C"
  },
  "modeToggle": { "default": "dark" }
}
```

**原因**：与z-image.vip官网保持一致

## 部署前检查清单

```bash
# 1. 链接检查
npm run check:links

# 2. 本地预览
npm run dev
# 访问 http://localhost:3001 检查：
# - 所有导航可点击
# - Home跳转到z-image.vip
# - 深色模式默认启用
# - 橙色主题正确显示

# 3. 构建测试
npm run build
```

## 维护责任

| 内容区域 | 负责人 | 更新频率 |
|---------|--------|----------|
| getting-started/ | 产品负责人 | 每次定价/流程变化 |
| seedream/ | Seedream API owner | 每次模型更新 |
| wan22/ | Wan 2.2 API owner | 每次模型更新 |
| common/reference/ | 后端负责人 | 每次API变更 |
| resources/ | 支持负责人 | 每月（根据FAQ） |

## 常见问题

**Q: 为什么不用绝对路径链接？**
A: Mintlify在不同环境下base path可能变化，相对路径更可靠。

**Q: 可以添加新的tab吗？**
A: 可以，但必须：
1. 在docs/mintlify/下创建对应目录
2. 目录名 = tab名（小写+连字符）
3. 更新docs.json
4. 更新本文档

**Q: 示例代码必须都可运行吗？**
A: 是的。用户复制粘贴应该能直接工作（只需替换API_KEY）。

**Q: 多长时间同步一次Discord FAQ？**
A: 每月第一周。高频问题立即同步。

---

**规范版本**：v1.0
**最后更新**：2025-02-04
**维护人**：API文档团队
