# Z-Image API Documentation

使用Mintlify构建的API文档站点。

## 本地开发

```bash
# 安装Mintlify CLI
npm install -g mintlify

# 启动开发服务器
mintlify dev

# 访问 http://localhost:3000
```

## 部署到Vercel

### 方法1: Vercel CLI（推荐）

```bash
# 1. 安装Vercel CLI（如果还没安装）
npm install -g vercel

# 2. 在此目录运行部署
cd docs/mintlify
vercel

# 3. 首次部署会提示：
# - 登录Vercel账号
# - 选择scope（团队/个人）
# - 链接到项目或创建新项目
# - 确认设置

# 4. 生产环境部署
vercel --prod
```

### 方法2: GitHub集成（自动部署）

1. 推送代码到GitHub
2. 访问 [Vercel Dashboard](https://vercel.com/dashboard)
3. 点击 "Import Project"
4. 选择你的仓库
5. 配置：
   - **Root Directory**: `docs/mintlify`
   - **Framework Preset**: Other
   - 其他保持默认

### 配置自定义域名

部署成功后：

1. 进入项目 Settings → Domains
2. 添加域名: `docs.z-image.vip`
3. 配置DNS:
   ```
   Type: CNAME
   Name: docs
   Target: cname.vercel-dns.com
   ```

## 文档结构

```
docs/mintlify/
├── docs.json              # Mintlify配置
├── overview.mdx          # 首页
├── authentication.mdx    # 认证说明
├── quickstart.mdx        # 快速开始
├── reference/            # API参考
│   ├── api-overview.mdx
│   ├── create-task.mdx
│   ├── get-task-status.mdx
│   └── webhooks.mdx
├── guides/              # 使用指南
│   ├── image-generation.mdx
│   ├── video-generation.mdx
│   └── error-handling.mdx
└── logo/                # 品牌资源
    └── logo.webp
```

## Logo和图标

当前使用的是webp格式的logo。如需优化，可以提供SVG格式：

```
logo/
├── dark.svg   # 深色主题logo（120x40px推荐）
└── light.svg  # 浅色主题logo（120x40px推荐）
```

然后更新 `docs.json`:

```json
{
  "logo": {
    "dark": "/logo/dark.svg",
    "light": "/logo/light.svg"
  }
}
```

## 环境变量

部署时可以在Vercel设置环境变量：

- `NEXT_PUBLIC_SITE_URL` - 主站URL（用于返回主站链接）
- 其他可选变量...

## 更新文档

1. 编辑对应的 `.mdx` 文件
2. 提交到GitHub
3. Vercel自动部署（如果配置了GitHub集成）
4. 或运行 `vercel --prod` 手动部署

## 技术支持

- Mintlify文档: https://mintlify.com/docs
- Vercel文档: https://vercel.com/docs
