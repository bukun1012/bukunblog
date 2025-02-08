---
title: "Hugo：自訂主題"
weight: 10
draft: false
description: "介紹如何設定Hugo的theme，以Blowfish為例"
slug: "hugo-simple-theme"
categories: ["Hugo"]
tags: ["網站開發", "靜態網站", "Blowfish"]
date: 2025-02-07T20:00:00+08:00
author: "Bukun"
---

本篇文章將介紹如何調整 Hugo 主題，從基本設定到進階自訂，讓網站符合需求。
我會以`Blowfish`為此篇文章的範例主題，也是我正在使用的！
如果想進一步了解`Blowfish`，請參考 [Blowfish 官方文件](https://blowfish.page/)。

## 1. 設定 `config.toml`

Hugo 的主要設定檔是 `config.toml`，這裡定義了網站的基本資訊、語言、主題等。

### **基本設定**

```toml
baseURL = "https://example.com/" #未來部署需更改
languageCode = "zh-TW"
title = "我的 Hugo 網站"
theme = "blowfish" # 根據主題改變
paginate = 10  # 設定每頁顯示的文章數量
```

### **啟用進階功能**

Blowfish 支援許多內建功能，可以在 `config.toml` 內啟用：

```toml
[params]
  showTableOfContents = true  # 在文章側邊顯示目錄
  showDate = true  # 顯示文章日期
  math = true  # 啟用 LaTeX 數學公式渲染
  customCSS = ["css/custom.css"]  # 引入自訂 CSS 樣式
```

## 2. 自訂 `params.toml`

`params.toml` 主要控制 Blowfish 主題的細節，如標題、副標題、配色等。

### **修改網站標題與副標題**

```toml
[params]
  title = "Bukun 的技術部落格"
  subtitle = "紀錄學習與開發經驗"
```

### **更改顏色主題**

Blowfish 內建 `light` 和 `dark` 模式，但你也可以進一步調整顏色。

```toml
[params]
  colorScheme = "dark"
```

## 3. 設定導覽列 (`menus.toml`)

如果想要修改網站導覽列，可以在 `config/_default/menus.toml` 內加入：

```toml
[[main]]
  identifier = "home"
  name = "首頁"
  url = "/"
  weight = 1

[[main]]
  identifier = "blog"
  name = "部落格"
  url = "/posts/"
  weight = 2
```

這樣就能在導覽列顯示「首頁」和「部落格」的連結。

## 4. 自訂樣式 (`custom.css`)

如果你想要進一步修改 Blowfish 主題的樣式，可以在 `assets/css/custom.css` 內加入 CSS 來覆蓋預設樣式。

```css
body {
  background-color: #222;
  color: #f5f5f5;
}
```

## 總結

這篇文章介紹了如何調整 Hugo Blowfish 主題，包括 `config.toml`、`params.toml`、自訂 CSS、修改導覽列，以及變更網站佈局。如果想要進一步優化，還可以學習如何客製化 `layouts/` 內的檔案，打造完全符合需求的 Hugo 網站！
