---
title: "你好，Hugo"
date: 2026-10-17T22:30:00+08:00
draft: false
summary: "本博客的第一篇文章，介绍一下这个站点是怎么搭起来的。"
description: "Hugo 博客环境搭建完成后的第一篇示例文章。"
categories: ["随笔"]
tags: ["Hugo", "博客", "入门"]
ShowToc: true
TocOpen: true
cover:
  image: ""
  alt: "封面图"
  caption: ""
  relative: false
  hidden: true
---

## 欢迎

这是使用 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题搭建的第一篇文章。

## 常用命令

在站点根目录 `E:\blog\myblog` 下执行：

```powershell
# 本地预览（带草稿），默认 http://localhost:1313
hugo server -D

# 新建文章
hugo new content posts/my-new-post.md

# 构建静态站点到 public/
hugo --minify
```

## Markdown 展示

### 列表

- 第一项
- 第二项
  - 嵌套项

### 表格

| 功能 | 命令 | 说明 |
| --- | --- | --- |
| 预览 | `hugo server -D` | 热重载 |
| 构建 | `hugo --minify` | 输出到 public |

### 代码块

```javascript
const greet = (name) => {
  console.log(`你好, ${name}!`);
};
greet('Hugo');
```

### 引用

> 保持记录，是长期主义者的习惯。

## 下一步

1. 修改 `hugo.toml` 里的 `baseURL` 和 `title`
2. 在 `content/posts/` 下写自己的文章
3. 选择部署方式（GitHub Pages / Netlify / Vercel 等）
