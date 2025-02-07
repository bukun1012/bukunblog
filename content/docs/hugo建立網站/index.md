---
title: "新手指南：如何使用 Hugo 建立網站"
weight: 10
draft: false
description: "從零開始學習如何使用 Hugo 建立靜態網站。"
slug: "getting-started-hugo"
categories: ["Hugo"]
tags: ["網站開發", "靜態網站", "部落格"]
date: 2025-02-06T10:00:00+08:00
author: "Bukun"
---

Hugo 是一款高效的靜態網站生成器，非常適合建立個人部落格或公司官網。本篇文章將帶領新手從零開始，逐步建立一個 Hugo 網站。

如果你有任何問題，可以在 [Hugo 官方論壇](https://discourse.gohugo.io/) 上尋求幫助。

## 安裝 Hugo

首先，你需要在電腦上安裝 Hugo，請根據你的作業系統選擇適合的方法。

### macOS

```bash
brew install hugo
```

### Windows

```powershell
scoop install hugo
```

或使用 Chocolatey 安裝：

```powershell
choco install hugo -confirm
```

### Linux

```bash
sudo apt install hugo
```

安裝完成後，請使用以下指令確認是否成功安裝：

```bash
hugo version
```

## 建立 Hugo 專案

要建立新的 Hugo 網站，請執行：

```bash
hugo new site my-blog
```

這將會建立一個新的 Hugo 專案，目錄結構如下：

```
my-blog/
├── archetypes/
├── content/
├── layouts/
├── static/
├── themes/
├── config.toml
```

## 選擇佈景主題

你可以從 [Hugo Themes](https://themes.gohugo.io/) 挑選適合的主題，例如 `ananke`：

```bash
cd my-blog
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

並在 `config.toml` 中設定主題：

```toml
theme = "ananke"
```

## 建立第一篇文章

使用以下指令建立一篇 Markdown 文章：

```bash
hugo new posts/my-first-post.md
```

然後用文字編輯器打開 `content/posts/my-first-post.md`，並填寫內容：

```markdown
---
title: "我的第一篇 Hugo 文章"
date: 2025-02-06
draft: true
---

這是我的第一篇 Hugo 文章！
```

## 啟動本地開發伺服器

當你有了佈景主題和文章後，可以使用以下指令啟動本地伺服器來預覽網站：

```bash
hugo server -D
```

然後打開瀏覽器，訪問 `http://localhost:1313/`，你將會看到你的網站。

## 部署網站

Hugo 生成的靜態網站可以輕鬆部署到 GitHub Pages、Netlify 或 Vercel。

### 部署到 GitHub Pages

1. 在 GitHub 上建立一個 Repository
2. 設定 `config.toml` 中的 `baseURL`
3. 生成靜態網站：
   ```bash
   hugo -D
   ```
4. 將 `public/` 目錄的內容推送到 GitHub Pages

## 總結

現在你已經成功建立了一個 Hugo 網站，並可以開始撰寫部落格文章。你可以進一步學習如何自訂主題、優化 SEO，讓你的網站更具特色！

測試
