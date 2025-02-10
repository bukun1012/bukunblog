---
title: "[Hugo]如何在網站加入評論功能"
draft: false
description: "介紹如何在 Hugo 網站中加入評論系統，以提升與讀者的互動。"
slug: "hugo-comments"
categories: ["Hugo"]
tags: ["靜態網站", "評論系統", "Disqus", "網站開發"]
date: 2025-02-09
author: "Bukun"
---

Hugo 本身是一個靜態網站生成器，不具備內建的評論系統，但可以透過第三方服務來加入評論功能。
本篇文章會介紹幾種常見的方法，可以在你的 Hugo 網站上留言與互動。

## 1. 使用 Disqus

[Disqus](https://disqus.com/) 是最常見的靜態網站評論系統之一，支援即時留言、垃圾訊息過濾與社群互動。

### **步驟 1: 註冊 Disqus 帳號並建立網站**

1. 前往 [Disqus](https://disqus.com/) 註冊帳號。
2. 建立一個新的網站，並記下 **Shortname** (例如 `my-hugo-site`)。

### **步驟 2: 設定 Hugo**

在 `config.toml` 中加入以下內容：

```toml
[params]
  disqusShortname = "my-hugo-site"
[article]
  showComments = true
```

### **步驟 3: 驗證評論是否顯示**

執行 Hugo 本地伺服器：

```bash
hugo server -D
```

> 注意：Disqus 需要 **正式部署後** 才能正常顯示。

---

## 2. 使用 Utterances (GitHub Issue 評論系統)

如果你的 Hugo 網站是放在 GitHub Pages 上，**Utterances** 是一個很棒的免費選擇。
它會將每篇文章的評論儲存在 GitHub Issue 中，適合技術部落格。

### **步驟 1: 安裝 Utterances**

1. 前往 [Utterances](https://utteranc.es/) 並安裝 GitHub App。
2. 選擇一個用於儲存評論的 GitHub Repository。

### **步驟 2: 在 Hugo 加入 Utterances**

在 `layouts/partials/comments.html` 新增以下內容：

```html
<script
  src="https://utteranc.es/client.js"
  repo="your-username/your-repository"
  issue-term="title"
  theme="github-light"
  crossorigin="anonymous"
  async
></script>
```

### **步驟 3: 在文章模板內引入評論**

編輯 `layouts/_default/single.html`，在適當位置加入：

```html
{{ if .Params.comments }} {{ partial "comments.html" . }} {{ end }}
```

然後在 `config.toml` 內啟用：

```toml
[params]
  enableComments = true
```

---

## 3. 使用 Giscus (GitHub Discussions 評論系統)

[Giscus](https://giscus.app/) 是一個基於 GitHub Discussions 的評論系統，運作方式類似 Utterances。

### **步驟 1: 設定 Giscus**

1. 前往 [Giscus](https://giscus.app/) 設定你的 GitHub Repository。
2. 選擇 **Discussion Category**。
3. 複製生成的 `<script>` 代碼。

### **步驟 2: 在 Hugo 中加入 Giscus**

在 `layouts/partials/comments.html` 新增：

```html
<script
  src="https://giscus.app/client.js"
  data-repo="your-username/your-repository"
  data-repo-id="YOUR_REPO_ID"
  data-category="YOUR_CATEGORY"
  data-category-id="YOUR_CATEGORY_ID"
  data-mapping="pathname"
  data-strict="0"
  data-reactions-enabled="1"
  data-emit-metadata="0"
  data-input-position="bottom"
  data-theme="light"
  crossorigin="anonymous"
  async
></script>
```

然後在文章內啟用：

```toml
[params]
  enableComments = true
```

---

## 4. 總結

- **Disqus**：適合一般部落格，簡單好用，但可能有廣告。
- **Utterances**：適合技術部落格，留言儲存在 GitHub Issue，完全免費。
- **Giscus**：基於 GitHub Discussions，適合開發者社群使用。

我個人是使用 Disqus，因為他設定簡單且快速好用。
這些方法都能讓 Hugo 網站擁有留言功能
選擇適合你的解決方案，讓網站與讀者有更多互動吧！
