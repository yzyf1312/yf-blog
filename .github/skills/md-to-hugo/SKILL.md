---
name: md-to-hugo
description: '将普通 Markdown 文件转换为 Hugo 博客文章格式。使用场景：有新写的 Markdown 笔记想发布为 Hugo 博文时，自动添加 front matter、转英文文件名、做内容兼容性检查。'
argument-hint: "Markdown 文件路径或内容"
user-invocable: true
---

# md-to-hugo

将普通 Markdown 文件转换为 Hugo 博客文章。

## 何时使用

- 写了一篇 Markdown 笔记，想发布到 Hugo 博客
- 需要批量将外部 Markdown 转为 Hugo 格式
- 需要统一博客文章的 front matter 格式

## 前置：收集已有标签和分类

在执行转换前，先扫描 `content/post/` 下所有 `.md` 文件，收集博客已有的 `tags` 和 `categories`。**优先复用现有标签/分类，不要无依据地新建。**

## 转换步骤

### 1. 添加 Hugo Front Matter

在文件顶部插入标准 front matter：

```toml
+++
title = '<文章标题>'
date = '<时间，格式 YYYY-MM-DDTHH:MM:SS+08:00>'
draft = false
tags = ['<标签1>', '<标签2>']
categories = ['<分类>']
+++
```

- `title`：从原文第一个 `#` 标题提取，无标题则根据内容与文件名推断
- `date`：优先使用文件修改时间，否则用当前时间
- `tags`：从已收集的标签列表中，选出与内容主题最匹配的 1-3 个。如果现有标签都无法完整覆盖内容，**主动提议新标签**并询问用户是否采纳
- `categories`：从已收集的分类列表中选取。如果现有分类无法完整覆盖内容，**主动提议新分类**并询问用户是否采纳

### 2. 文件名转英文 Slug

- 中文文件名 → 英文 kebab-case（短横线命名）
- 保留有意义的英文词汇，中文部分意译或音译
- 例如：`WSA 杂谈.md` → `wsa-notes.md`
- 如果文件已在博客 `content/post/` 目录下，修改 front matter 后**直接使用系统对应的移动命令完成重命名**

### 3. 内容兼容性检查

- 不修改原文内容

## 输出

返回转换后的完整文件内容和建议的文件名。
