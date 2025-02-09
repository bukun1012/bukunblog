---
title: "[Hugo]如何初步建立網站"
draft: false
description: "從零開始學習如何使用 Hugo 建立靜態網站。"
slug: "getting-started-hugo"
categories: ["Hugo"]
tags: ["網站開發", "靜態網站", "部落格"]
date: 2025-02-06
author: "Bukun"
---

Hugo 是一款高效的靜態網站生成器，非常適合建立個人部落格或公司官網。
本篇文章將從零開始，逐步建立一個 Hugo 網站。
如果有任何問題，可以在 [Hugo 官方論壇](https://discourse.gohugo.io/) 上尋求幫助。

## 安裝 Hugo

首先，需要在電腦上安裝 Hugo，根據作業系統選擇適合的方法。

### macOS

#### 使用 <span style="color: #ff6600">Homebrew</span> 安裝

```bash
brew install hugo
```

如果需要 Hugo 的 **extended 版本**（用於處理 SCSS/SASS），使用：

```bash
brew install hugo_extended
```

### Windows

#### 使用 <span style="color: #ff6600">Scoop</span> 安裝

```powershell
scoop install hugo
```

#### 或使用 <span style="color: #ff6600">Chocolatey</span> 安裝

```powershell
choco install hugo -confirm
```

### 驗證安裝

安裝完成後，使用以下指令確認是否成功安裝：

```bash
hugo version
```

若能看到 Hugo 的版本資訊，表示安裝成功。

```bash
hugo v0.142.0
```

## 建立 Hugo 專案

要建立新的 Hugo 網站，執行：

```bash
hugo new site my-blog
```

這將會建立一個新的 Hugo 專案，目錄結構如下：

```
my-blog/
├── archetypes/      # 預設文章模板
├── content/         # 文章內容
├── layouts/         # 版型檔案
├── static/          # 靜態資源 (圖片、CSS、JS)
├── themes/          # 佈景主題
├── config.toml      # 站點設定檔案
```

## 選擇佈景主題

Hugo 可以使用不同的佈景主題來快速建立網站。從 [Hugo Themes](https://themes.gohugo.io/) 挑選適合的主題，例如 `blowfish`，然後執行下載指令：

```bash
cd my-blog
git init
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

並在 `config.toml` 中設定主題：

```toml
theme = "blowfish"
```

## 設定網站基本資訊

在 `config.toml` 中，新增以下內容來設定網站基本資訊：

```toml
baseURL = "http://localhost:1313/"
languageCode = "zh-TW"
title = "我的 Hugo 網站"
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

當有了佈景主題和文章後，可以使用以下指令啟動本地伺服器來預覽網站：

```bash
hugo server -D
```

然後打開瀏覽器，訪問 `http://localhost:1313/`，你將會看到目前快速建立的網站！

## 總結

現在已經成功建立了一個 Hugo 網站，可以開始撰寫部落格文章。
下面文章將更深入設定[自訂主題]({{< relref "docs/hugo自訂主題,從config出發！" >}})、優化 SEO 等，讓網站更具特色！
