---
title: "[Hugo]外觀重要文件,params修改!"
draft: false
description: "詳細介紹如何修改 Hugo 的 params.toml，讓網站更具個人化與特色，以 Blowfish 為範例。"
slug: "hugo-params-configuration"
categories: ["Hugo"]
tags: ["網站開發", "靜態網站", "Blowfish"]
date: 2025-02-08
author: "Bukun"
series: ["Hugo"]
series_order: 3
---

上一篇文章我們介紹了 `hugo.toml`、`languages.toml` 以及 `menus.toml` 的設定。
這次我們要進一步調整 **`params.toml`**，這是影響 Hugo 網站外觀與功能的關鍵設定檔。

如果你還沒完成前一步的設定，請先參閱 [自訂主題]({{< relref "docs/hugo自訂主題,從config出發！" >}})。

---

## 1. 重要全局設定

這些參數決定網站的基本外觀與功能，例如顏色、背景圖等。
Blowfish 主題已內建多種配色方案，可參閱 [全局配置](https://blowfish.page/zh-cn/docs/configuration/#%E5%85%A8%E5%B1%80-1)。

```toml
colorScheme = "blowfish"
defaultAppearance = "dark" # valid options: light or dark  #預設亮色或暗色模式
autoSwitchAppearance = true  #根據系統設定自動切換
defaultBackgroundImage = "/img/background.png" #全站背景圖
highlightCurrentMenuArea = true      #高亮選中的導航區域
smartTOC = true  # 啟用智慧目錄
```

{{< alert >}}確保背景圖片放置在 `assets/img` 內，否則無法正確載入！{{< /alert >}}

---

## 2. 修改頁腳

頁腳控制網站底部的顯示內容，如版權、滾動返回頂部等。

```toml
[footer]
  showMenu = true  # 顯示頁腳選單
  showCopyright = true  # 顯示版權資訊
  showThemeAttribution = true  # 顯示主題版權標示
  showAppearanceSwitcher = true  # 允許使用者切換亮/暗模式
  showScrollToTop = true  # 顯示返回頂部按鈕
```

---

## 3. 修改主頁配置

設定首頁顯示的內容與佈局風格。

```toml
[homepage]
  layout = "background" # 選擇首頁樣式 (可選 page, profile, hero, card, background, custom)
  showRecent = true  # 顯示最新文章
  showRecentItems = 12  # 最新文章數量
  showMoreLink = true  # 顯示「更多文章」按鈕
  showMoreLinkDest = "docs"  # 「更多文章」按鈕導向的頁面
  cardView = true  # 啟用卡片視圖
  layoutBackgroundBlur = false  # 背景模糊效果（適用於 background 佈局）
```

---

## 4. 修改文章配置

這些設定影響文章的顯示方式，例如顯示作者、日期、面包屑導航等。

```toml
[article]
  showDate = true  # 顯示發佈日期
  showViews = false  # 不顯示閱讀數
  showLikes = false  # 不顯示點讚數
  showAuthor = true  # 顯示作者資訊
  showTableOfContents = true  # 顯示文章目錄
  showRelatedContent = true  # 顯示相關文章
  relatedContentLimit = 3  # 相關文章顯示數量
  showPagination = true  # 啟用文章分頁
  heroStyle = "background" # 文章標題樣式 (可選 basic, big, background, thumbAndBackground)
```

---

## 5. 修改類別配置

調整分類頁面的外觀與內容。

```toml
[taxonomy]
  showTermCount = true  # 顯示分類數量
  showHero = true  # 啟用分類標題背景
  heroStyle = "background" # 分類標題樣式 (可選 basic, big, background, thumbAndBackground)
  showBreadcrumbs = false  # 不顯示面包屑導航
  showTableOfContents = true  # 啟用分類頁面的目錄
  cardView = true  # 啟用卡片視圖
```

---

## 6. 總結

`params.toml` 是 Hugo Blowfish 主題最關鍵的設定檔之一。
透過它，你可以輕鬆調整網站的標題、配色、導航選單、文章顯示方式等。

下一篇文章將會深入探討 **`layouts/` 自訂佈局**，讓你的網站更加符合個人需求！
如果有任何問題也歡迎下面留言討論！
