# 🚀 Vercel部署快速指南

## 前置要求

- 有Vercel账号（使用GitHub账号登录）
- 安装了Node.js和npm

## 📦 方式1: Vercel CLI部署（最快）

### 1. 安装Vercel CLI

```bash
npm install -g vercel
```

### 2. 进入文档目录

```bash
cd /Users/hzy/Code/zhuilai/aaaa/api面板/docs/mintlify
```

### 3. 登录Vercel（首次需要）

```bash
vercel login
```

按提示完成登录验证。

### 4. 部署到生产环境

```bash
vercel --prod
```

首次部署会询问：

```
? Set up and deploy "docs/mintlify"? [Y/n] y
? Which scope do you want to deploy to? (选择你的团队/个人)
? Link to existing project? [y/N] n
? What's your project's name? z-image-api-docs
? In which directory is your code located? ./
```

部署完成后会显示URL：
```
✅  Production: https://z-image-api-docs.vercel.app
```

### 5. 配置自定义域名

1. 访问 https://vercel.com/dashboard
2. 选择项目 `z-image-api-docs`
3. Settings → Domains
4. 添加域名: `docs.z-image.vip`
5. 配置DNS:

```
Type: CNAME
Name: docs
Target: cname.vercel-dns.com
Proxy Status: DNS only（关闭Cloudflare代理）
```

等待DNS生效（5-30分钟），然后访问 https://docs.z-image.vip

---

## 🔄 方式2: GitHub自动部署（推荐生产）

### 1. 推送代码到GitHub

```bash
git push origin features/api-docs-admin
```

### 2. 导入项目到Vercel

1. 访问 https://vercel.com/new
2. 选择你的GitHub仓库
3. 配置项目：

```
Project Name: z-image-api-docs
Root Directory: docs/mintlify  (重要！)
Framework Preset: Other
Build Command: (留空，使用vercel.json配置)
Output Directory: (留空，使用vercel.json配置)
```

4. 点击 "Deploy"

### 3. 配置自定义域名

同方式1的第5步。

---

## ✅ 验证部署

部署成功后访问：

1. **临时域名**: https://z-image-api-docs.vercel.app
2. **自定义域名**: https://docs.z-image.vip

检查清单：

- [ ] 首页正常显示
- [ ] 左侧导航正确
- [ ] 所有文档页面可访问
- [ ] 搜索功能正常
- [ ] Logo和favicon显示正常
- [ ] 代码高亮正常
- [ ] 移动端显示正常

---

## 🔧 后续更新

### 自动部署（如果用GitHub集成）

只需推送代码到GitHub：

```bash
git add .
git commit -m "docs: 更新API文档"
git push
```

Vercel会自动部署。

### 手动部署（如果用CLI）

```bash
cd docs/mintlify
vercel --prod
```

---

## 🐛 常见问题

### Q: 部署失败显示 "Build failed"

检查：
1. `vercel.json` 配置是否正确
2. 文档MDX文件格式是否有误
3. 查看Vercel部署日志

### Q: 自定义域名无法访问

检查：
1. DNS配置是否生效（用 `nslookup docs.z-image.vip`）
2. Vercel域名状态是否为"Valid"
3. 如果用Cloudflare，确保关闭代理（DNS only）

### Q: Logo不显示

检查：
1. `logo/logo.webp` 文件是否存在
2. `docs.json` 中logo路径是否正确
3. 浏览器控制台是否有404错误

### Q: 想本地预览部署效果

```bash
npm install -g mintlify
cd docs/mintlify
mintlify dev
```

访问 http://localhost:3000

---

## 📞 需要帮助？

- Vercel文档: https://vercel.com/docs
- Mintlify文档: https://mintlify.com/docs
- Vercel状态: https://vercel-status.com
