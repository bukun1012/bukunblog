---
title: "[SEO]如何讓 Google 搜尋到你的 GitHub Pages"
draft: false
description: "介紹如何讓 Google 索引你的 GitHub Pages，提升搜尋能見度。"
slug: "github-pages-GSC"
categories: ["SEO"]
tags: ["網站開發", "靜態網站", "GitHub Pages", "SEO"]
date: 2025-02-08T16:00:00+08:00
author: "Bukun"
---

很多人在使用 **GitHub Pages** 建立 Hugo 網站後，發現自己的網站在 Google 上搜尋不到。
這篇文章會介紹 **如何讓 Google 索引你的 GitHub Pages**，提升網站能見度，讓更多人能夠找到你的內容。

## 1. 確保 GitHub Pages 設定正確

首先，請確認你的 GitHub Pages 已經部署成功，並且網站可以正常訪問。

1. 進入你的 GitHub Repository → **Settings** → **Pages**
2. 確保 **Source** 設定為 `Deploy from a branch` 或 `GitHub Actions`
3. 網站 URL (如 `https://yourusername.github.io/`) 需要能夠正常開啟

如果你的 GitHub Pages 部署正常，但 Google 仍然無法找到你的網站，請繼續以下步驟。

---

## 2. 允許搜尋引擎索引網站

在 Hugo 的 `config.toml` 或 `hugo.toml` 內確認 **robots.txt** 允許 Google 抓取網站。

### **檢查 `robots.txt`**

Hugo 預設會產生 `robots.txt`，但如果不確定，可以手動設定。

新增 `static/robots.txt`，內容如下：

```txt
User-agent: *
Allow: /
Sitemap: https://yourusername.github.io/sitemap.xml
```

這樣會允許所有搜尋引擎索引你的網站，並提供 sitemap 方便 Google 抓取內容。

---

## 3. 產生 `sitemap.xml`

Google 會透過 **網站地圖 (sitemap.xml)** 來了解你的網站結構。
Hugo 內建支援 **sitemap.xml**，只需確保 `config.toml` 內啟用了 sitemap。

```toml
[outputs]
  home = ["HTML", "RSS", "SITEMAP"]
```

如果你已經啟用 sitemap，開啟 `https://yourusername.github.io/sitemap.xml` 確認是否正確生成。

---

## 4. 提交網站到 Google Search Console

### **1. 前往 Google Search Console**

開啟 [Google Search Console](https://search.google.com/search-console/) 並登入 Google 帳戶。

### **2. 新增你的網站**

- 選擇 **網址前綴** (URL Prefix)
- 輸入你的 GitHub Pages 網址 (例如 `https://yourusername.github.io/`)
- 按 **繼續**

### **3. 驗證網站所有權**

如果你的網址是 `https://yourusername.github.io/`，可以使用 **HTML 標籤驗證**

1. 在 Google Search Console 內選擇 **HTML 標籤驗證**
2. 取得 `meta` 標籤，例如：

```html
<meta name="google-site-verification" content="your-verification-code" />
```

3. 在 Hugo 的 `layouts/partials/head.html` 內加入這段程式碼：

```html
{{ if .Site.Params.googleSiteVerification }}
<meta
  name="google-site-verification"
  content="{{ .Site.Params.googleSiteVerification }}"
/>
{{ end }}
```

4. 在 `config.toml` 內新增：

```toml
[params]
  googleSiteVerification = "your-verification-code"
```

5. **部署網站後** 回到 Google Search Console 點選 **驗證**

---

## 5. 手動提交 `sitemap.xml`

在 Google Search Console 中：

1. 進入 **Sitemaps** (網站地圖)
2. 在輸入框內輸入：

```
sitemap.xml
```

3. 按 **提交**

Google 會開始抓取你的網站，並將其索引。

---

## 6. 加速索引：使用 URL 檢查工具

如果你的網站已經發佈，但搜尋不到，可以手動向 Google 請求索引。

1. 在 **Google Search Console** 選擇 **URL 檢查**
2. 貼上你的網站 URL，例如 `https://yourusername.github.io/`
3. 按 **請求索引**

Google 會優先處理這些請求，通常幾天內就會出現在搜尋結果中。

---

## 7. 確保內容具備 SEO 優化

除了技術層面的設定，也要確保你的內容符合 SEO 規範：

- **標題 (title)**：每篇文章應該有清楚的標題
- **描述 (meta description)**：可以在 `config.toml` 內設定
- **關鍵字 (keywords)**：使用適當的關鍵字，但不要過度堆疊
- **內部連結 (internal linking)**：在文章內部適當地連結其他內容

### **設定文章的 SEO 標籤**

```toml
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```

在 `config.toml` 內啟用 `unsafe = true`，確保 Hugo 正確渲染 HTML meta 標籤。

---

## 8. 總結

透過這幾個步驟，你的 GitHub Pages 就能讓 Google 正確索引：

1. 確保 GitHub Pages 部署成功
2. 設定 `robots.txt` 允許搜尋引擎索引
3. 確保 `sitemap.xml` 正常生成
4. 提交網站到 **Google Search Console**
5. 手動提交 `sitemap.xml`
6. 使用 **URL 檢查工具** 加速索引
7. 優化 SEO 內容，確保標題、描述與內部連結完整

完成這些步驟後，幾天內就能在 Google 上找到你的網站！ 🚀
