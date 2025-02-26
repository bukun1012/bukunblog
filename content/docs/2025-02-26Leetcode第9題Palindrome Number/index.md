---
title: "[Leetcode]第9題的Palindrome Number解法與思路"
draft: false
description: "深入解析 LeetCode 第9題 Palindrome Number，探討解題思路與不同解法的時間複雜度。"
slug: "leetcode-palindromenumber"
categories: ["Leetcode"]
tags: ["Python", "演算法", "數組", "優化"]
date: 2025-02-26T17:00:00+08:00
author: "Bukun"
series: ["leetcode"]
series_order: 2
---

題目：
給定一個整數 x，如果 x 是回文數，則返回 true，否則返回 false。
回文數是指**正著讀和倒著讀都一數**。

範例

```python
Input: x = 121
Output: true
Explanation: 121 正著讀和倒著讀都一樣

Input: x = -121
Output: false
Explanation: -121 反轉後是 121-，不相等

Input: x = 10
Output: false
Explanation: 10 反轉後是 01，不相等
```

---

## 初步解析

看完題目後我有幾個想法

- 1. 直接將數字**int**轉成字串**str**，再用切片反轉比較是否相同
- 2. 負數絕對不是回文數，例如-121 反轉後為 121-
     有了想法後我就嘗試轉字串解法

---

## 解法 1：轉換字串解法

```
def isPalindrome(x: int) -> bool:
    return str(x) == str(x)[::-1]
```

### 步驟解析

- **str(x)**: 先將數字 x 轉成字串，例如 x = 121 變成 ”121“。
- **str(x)[::-1]**: 反轉字串，例如 ”121“ 反轉後還是 ”121“，但 ”-121“ 反轉後是 ”121-“，不同於原來 的字串。
- **return str(x) == str(x)[::-1]**: 比較原始字串和反轉後的字串是否相等。

### 時間與空間複雜度

- 時間複雜度：O(n)，因為字串反轉需要 O(n) 的時間。
- 空間複雜度：O(n)，因為建立了新的字串。

### 小結

沒想到測試之後這題就解開了...
完全出乎我的意料
於是我再思考該怎麼不轉字串來解出題目，並且減少計算量

---

## 解法 2：數學解法

### 再次分析

- 1. 負數不可能是回文數，因為 - 會影響正反讀的結果，例如 -121 反轉後變成 121-，所以直接返回 False。
- 2. 如果數字以 0 結尾（但不是 0 本身），它不可能是回文數，例如 10 反轉後變成 01，不同於原來的 10，所以直接返回 False。
- 3. 只需要反轉數字的一半。例如 1221 反轉一半後會變成 12，然後可以直接比較這兩部分是否相等。這樣可以避免反轉整個數字，減少運算量。

### 實作解法

```
def isPalindrome(x: int) -> bool:
    if x < 0 or (x % 10 == 0 and x != 0):
        return False

    reversed_half = 0
    while x > reversed_half:
        reversed_half = reversed_half * 10 + x % 10
        x //= 10

    return x == reversed_half or x == reversed_half // 10
```

### 詳細步驟解析

我先以 x = 1221 來做接下來的舉例

#### step1 排除特殊情況

```
if x < 0 or (x % 10 == 0 and x != 0):
    return False
```

這一行判斷的是

- x < 0？ False（1221 不是負數）。
- x % 10 == 0 and x != 0？ False（1221 不是以 0 結尾）。
  那我們繼續往下執行

#### step2 逐步反轉數字的一半

```
reversed*half = 0
while x > reversed_half:
reversed_half = reversed_half * 10 + x % 10
x //= 10
```

先用變數**reversed_half**存放反轉後的數字的一半
且先初始化為 0
每次只處理 x 的最後一位數
第一輪迴圈：
• reversed*half = 0 * 10 + 1221 % 10 = 1
• x = 1221 // 10 = 122
• 現在 x = 122，reversed_half = 1，還沒過半，繼續執行迴圈。

第二輪迴圈：
• reversed_half = 1 \* 10 + 122 % 10 = 12
• x = 122 // 10 = 12
• 現在 x = 12，reversed_half = 12，此時 x 和 reversed_half 相等，代表數字已處理一半。

迴圈結束條件：
• 當 x <= reversed_half 時，代表數字已經處理一半，可以跳出迴圈。

#### step3 判斷是否為回文

```
return x == reversed_half or x == reversed_half // 10
```

• 為什麼需要 x == reversed_half？
• 如果數字長度是偶數，例如 1221，此時 x = 12，reversed_half = 12，直接相等，所以 return True。
• 為什麼還要 x == reversed_half // 10？
• 如果數字長度是奇數，例如 12321：
• 迴圈結束時 x = 12，但 reversed_half = 123。
• reversed_half 多了一位數，所以我們可以**去掉最後一位（// 10）**來做比較。
• reversed_half // 10 = 123 // 10 = 12，剛好等於 x，所以 return True。

#### 時間與空間複雜度

- 時間複雜度：O(log n)，因為 x 每次都減少 1 位數，所以迴圈大約執行 log10(n) 次。
- 空間複雜度：O(1)，只用了 reversed_half 這個額外變數。

---

## 總結

如果只是解題，那一開始的字串解法較簡單快速
若考慮到優化，則應該使用數字解法
