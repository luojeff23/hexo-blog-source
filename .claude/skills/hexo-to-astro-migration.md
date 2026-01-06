# Hexo 迁移到 Astro + Retypeset 主题指南

## 概述

将 Hexo 博客迁移到 Astro 框架 + Retypeset 主题，部署到 Cloudflare Pages。

## 技术栈

- **框架**: Astro (替代 Hexo)
- **主题**: Retypeset - 极简排版风格
- **部署**: Cloudflare Pages
- **包管理**: pnpm

## 迁移步骤

### 1. 克隆 Retypeset 主题

```bash
git clone https://github.com/radishzzz/astro-theme-retypeset.git astro-blog
cd astro-blog
npm install -g pnpm
pnpm install
```

### 2. 配置站点信息

编辑 `src/config.ts`:

```typescript
site: {
  title: "博客标题",
  subtitle: '副标题',
  description: '描述',
  i18nTitle: false,  // 禁用多语言标题
  author: '作者名',
  url: 'https://your-domain.pages.dev',
},
global: {
  locale: 'zh',      // 默认中文
  moreLocales: [],   // 只保留中文
},
comment: {
  enabled: false,    // 禁用评论（可选）
},
```

### 3. 清理多语言文件

如果只需要中文，删除其他语言的 about 页面：

```bash
# 保留 about-zh.md，删除其他
rm src/content/about/about-en.md
rm src/content/about/about-es.md
rm src/content/about/about-ja.md
rm src/content/about/about-ru.md
rm src/content/about/about-zh-tw.md
```

### 4. 迁移文章

将 Hexo 文章从 `source/_posts/` 复制到 `src/content/posts/`

**Front-matter 格式转换**:

```yaml
# Hexo 格式
---
title: 文章标题
date: 2025-03-15 18:15:52
tags:
  - 标签1
categories:
  - 分类1
---

# Retypeset 格式
---
title: 文章标题
published: 2025-03-15
tags:
  - 标签1
---
```

注意：Retypeset 不使用 categories，只用 tags。

### 5. 删除示例文章

```bash
rm -rf src/content/posts/examples
rm -rf src/content/posts/guides
rm "src/content/posts/Universal Post.md"
```

### 6. 本地测试

```bash
pnpm dev
# 访问 http://localhost:4321
```

### 7. 推送到 GitHub

```bash
rm -rf .git
git init
git add .
git commit -m "Migrate to Astro + Retypeset theme"
git branch -M main
git remote add origin https://github.com/username/repo.git
git push -u origin main --force
```

### 8. Cloudflare Pages 构建设置

| 设置项 | 值 |
|-------|-----|
| 构建命令 | `pnpm install && pnpm build` |
| 构建输出目录 | `dist` |
| 环境变量 NODE_VERSION | `18` |

## 关键文件

| 文件 | 用途 |
|-----|------|
| `src/config.ts` | 站点配置 |
| `src/content/posts/` | 文章目录 |
| `src/content/about/about-zh.md` | 关于页面 |
| `astro.config.ts` | Astro 配置 |

## 常见问题

### 构建失败：Invalid enum value for lang

原因：多语言配置与文件不匹配。
解决：删除不需要的语言文件，或在 `moreLocales` 中添加对应语言。

### SSH 推送失败

改用 HTTPS：
```bash
git remote set-url origin https://github.com/username/repo.git
```

## 主题资源

- Retypeset GitHub: https://github.com/radishzzz/astro-theme-retypeset
- Astro 官方主题市场: https://astro.build/themes/
- Retypeset Demo: https://retypeset.radishzz.cc/
