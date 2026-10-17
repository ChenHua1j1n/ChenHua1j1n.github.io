# 我的博客（Hugo + PaperMod）

Hugo Extended v0.168.0 ｜ PaperMod 主题 ｜ 简体中文

---

## 一、环境信息

| 项目 | 值 |
| --- | --- |
| Hugo 可执行文件 | `E:\blog\tools\hugo\hugo.exe` |
| Hugo 缓存目录 | `E:\blog\tools\hugo-cache`（已写入用户环境变量 `HUGO_CACHEDIR`） |
| 站点根目录 | `E:\blog\myblog` |
| 主题 | PaperMod（`themes/papermod`，Git Submodule） |
| 构建输出 | `E:\blog\myblog\public` |

> ⚠️ **本机特殊说明**：机器级环境变量 `TEMP` 指向 `C:\WINDOWS\TEMP`，普通用户无权创建子目录，会导致 Hugo 启动时报
> `mkdir ...hugo_cache: Access is denied`。已通过设置用户级 `HUGO_CACHEDIR` 解决，**该变量请勿删除**。

---

## 二、常用命令

所有命令请在站点根目录执行：

```powershell
cd E:\blog\myblog
```

| 用途 | 命令 |
| --- | --- |
| 本地预览（含草稿，热重载） | `hugo server -D` |
| 本地预览（不含草稿） | `hugo server` |
| 指定端口 | `hugo server -D --port 1314` |
| 生成静态文件 | `hugo --minify` |
| 清理后再生成 | `hugo --gc --minify` |
| 新建文章 | `hugo new content posts/文章名.md` |
| 更新主题 | `git submodule update --remote --merge` |
| 查看 Hugo 版本 | `hugo version` |

预览地址默认：<http://localhost:1313>

---

## 三、写文章

### 1. 新建

```powershell
hugo new content posts/my-first-post.md
```

新文件默认是**草稿**（`draft: true`），只有加 `-D` 预览时才可见。发布时把 `draft` 改为 `false`。

### 2. Front Matter 模板

```yaml
---
title: "文章标题"
date: 2026-10-17T20:00:00+08:00
draft: false
summary: "列表页和搜索结果显示的摘要"
description: "SEO 描述"
categories: ["技术"]
tags: ["Hugo", "教程"]
ShowToc: true      # 是否显示目录
TocOpen: false     # 目录默认展开
cover:
  image: "images/cover.png"
  alt: "封面"
  hidden: false    # 列表页是否隐藏封面
---
```

> ⚠️ **日期不要写成未来时间**：Hugo 默认不生成 `date` 晚于当前时间的文章，且**没有任何报错**，页面会静默消失。
> 若文章没出现，先执行 `hugo list future` 检查是否有“未来文章”。
> 临时预览可用 `hugo server -D --buildFuture`。

### 3. 目录规范

```
content/
├── posts/           # 文章（自动出现在 /posts/ 和首页）
│   ├── _index.md    # 文章列表页的标题
│   └── hello-hugo.md
├── about.md         # 关于页
├── archives.md      # 归档页（layout: archives）
├── search.md        # 搜索页（layout: search）
├── categories/      # 分类页标题
└── tags/            # 标签页标题
```

---

## 四、站点配置

配置文件：`E:\blog\myblog\hugo.toml`

**上线前务必修改：**

```toml
baseURL = 'https://你的域名/'   # 或 https://用户名.github.io/
title = '我的博客'
```

其他常用项：

- 每页文章数：`pagination.pagerSize = 10`
- 深色模式：`params.defaultTheme`（`auto` / `light` / `dark`）
- 开关目录、阅读时间、分享按钮：`params.ShowToc` / `ShowReadingTime` / `ShowShareButtons`
- 社交图标：`[[params.socialIcons]]`
- 导航菜单：`[[menu.main]]`

---

## 五、部署（已配置好，自动发布）

**线上地址：<https://ChenHua1j1n.github.io/>**

| 项目 | 值 |
| --- | --- |
| 源码仓库 | `git@github.com:ChenHua1j1n/ChenHua1j1n.github.io.git`（`main` 分支） |
| 发布分支 | `gh-pages`（由 Actions 自动生成，**请勿手动修改**） |
| 工作流文件 | `.github/workflows/hugo.yaml` |
| Pages 构建源 | 分支 `gh-pages` / 根目录 |

### 日常发布流程

```powershell
cd E:\blog\myblog
hugo server -D          # 先本地预览

git add .
git commit -m "写完了新文章"
git push                # 推送后 ~1-2 分钟自动上线
```

推送后可在仓库 **Actions** 标签页查看构建进度；成功即表示已发布。

### 手动重新发布

仓库 → **Actions** → 左侧 `Deploy Hugo site to GitHub Pages` → 右侧 **Run workflow**。

### 首次克隆

```powershell
git clone --recursive git@github.com:ChenHua1j1n/ChenHua1j1n.github.io.git
```

> ⚠️ 注意：本仓库 `main` 分支存放的是 **Hugo 源码**，不是静态站点。
> 静态站点在 `gh-pages` 分支，GitHub Pages 直接读取它，无需任何手动上传。
>
> 若改用 Netlify / Vercel：构建命令 `hugo --minify`，发布目录 `public`，环境变量 `HUGO_VERSION=0.168.0`。
> PaperMod 的图片处理需要 **Hugo Extended** 版本。

### 常见问题：推送成功后线上仍是旧页面

**现象**：Actions 显示 success，但浏览器打开还是旧内容，或某些页面 404。

**原因**：GitHub Pages 的 CDN 缓存，以及切换到 `gh-pages` 源后未自动重建。

**处理**：以仓库 **Settings → Pages** 页面显示的 “Your site is live at ...” 为准，并可强制重建：

```powershell
# 需要 GitHub Token（含 repo 权限）
curl -X POST -H "Authorization: Bearer <TOKEN>" `
  https://api.github.com/repos/ChenHua1j1n/ChenHua1j1n.github.io/pages/builds
```

或在 **Actions** 中手动 Run workflow。通常 1–2 分钟内生效。

---

## 六、主题管理

主题以 Git Submodule 方式管理。克隆本仓库后需要初始化：

```powershell
git clone --recursive <你的仓库地址>
# 若已克隆但主题为空：
git submodule update --init --recursive
```

主题更新：

```powershell
git submodule update --remote --merge
git add themes/papermod
git commit -m "更新主题"
```

---

## 七、验证过的功能

- ✅ 首页、文章列表、文章详情、关于、归档、搜索页
- ✅ 分类 / 标签自动聚合
- ✅ 目录、代码高亮与复制按钮、阅读时长、字数统计
- ✅ 站内搜索（`index.json` 索引已生成）
- ✅ 中文本地化（菜单、归档月份、阅读时长）
- ✅ RSS、sitemap.xml、robots.txt
