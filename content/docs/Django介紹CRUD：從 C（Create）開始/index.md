---
title: "[Django]介紹CRUD：從 C（Create）開始"
draft: false
description: "深入了解 Django 中的 CRUD 操作，這篇文章專注於 C（Create），學習如何在 Django 中建立資料。"
slug: "django-crud-create"
categories: ["Django"]
tags: ["Python", "Web 應用", "CRUD"]
date: 2025-02-12T01:00:00+08:00
author: "Bukun"
series: ["CRUD"]
---

在 Web 開發中，**CRUD（Create、Read、Update、Delete）** 是最基本的操作，其中 **C（Create）** 代表「建立資料」。
這篇文章會帶你從 **Django Model、Form 到 View**，完整學習如何在 Django 中建立資料，並透過 **Django Admin 與表單** 來處理新增資料的需求。

---

## 1. 設定 Model（資料庫模型）

Django 使用 **ORM（Object-Relational Mapping）** 來處理資料庫，所有的資料表都是透過 `models.py` 來定義的。

假設我們要建立「部落格文章」，在 `blog/models.py` 加入：

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)  # 文章標題
    content = models.TextField()  # 文章內容
    created_at = models.DateTimeField(auto_now_add=True)  # 自動存入建立時間

    def __str__(self):
        return self.title  # 在 Django Admin 顯示文章標題
```

---

## 2. 建立資料庫遷移並套用

當 Model 設定完成後，Django 需要同步資料庫。

### **步驟 1：產生遷移檔**

```bash
python manage.py makemigrations
```

這個指令會在 `blog/migrations/` 內產生遷移檔，記錄我們對資料庫的變更。

### **步驟 2：套用變更**

```bash
python manage.py migrate
```

這樣 `Post` Model 會正式建立成 **資料表**，可以開始儲存資料了。

---

## 3. 在 Django Admin 內新增資料

Django 內建 **Admin 管理後台**，讓我們可以方便地透過網頁介面新增資料。

### **步驟 1：在 `admin.py` 註冊 Model**

在 `blog/admin.py` 加入：

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)  # 註冊 Post 模型
```

### **步驟 2：建立管理員帳號**

如果還沒有管理員帳號，可以執行：

```bash
python manage.py createsuperuser
```

依照指示輸入 **使用者名稱、電子郵件、密碼**，然後啟動 Django 伺服器：

```bash
python manage.py runserver
```

打開 `http://127.0.0.1:8000/admin/`，輸入剛剛建立的帳號密碼，就能在後台看到 **Post** 模型，並直接新增文章！

---

## 4. 建立 Django 表單（Forms）

雖然 Django Admin 很方便，但通常會在 **網站前台** 提供「新增文章」的表單，讓使用者輸入內容。

在 `blog/forms.py` 建立 Django 表單：

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post  # 指定對應的 Model
        fields = ['title', 'content']  # 設定顯示的欄位
```

這樣 Django 就會幫我們產生 **HTML 表單**，讓使用者輸入標題與內容。

---

## 5. 建立 View 來處理表單提交

我們需要在 `views.py` 設定一個 View，負責顯示表單並處理提交的資料。

在 `blog/views.py` 加入：

```python
from django.shortcuts import render, redirect
from .forms import PostForm

def create_post(request):
    if request.method == "POST":
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()  # 存入資料庫
            return redirect('post_list')  # 重新導向到文章列表頁
    else:
        form = PostForm()

    return render(request, 'blog/create_post.html', {'form': form})
```

這個 View 的流程：

1. **GET 請求**：顯示空的表單。
2. **POST 請求**：接收使用者輸入的標題與內容。
3. **驗證表單**：確保輸入內容有效。
4. **存入資料庫**：儲存新文章。
5. **重新導向**：導回文章列表頁面。

---

## 6. 建立 HTML 模板

在 `blog/templates/blog/` 內建立 `create_post.html`，讓表單能顯示在前端。

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>新增文章</title>
  </head>
  <body>
    <h1>新增文章</h1>
    <form method="post">
      {% csrf_token %} {{ form.as_p }}
      <button type="submit">提交</button>
    </form>
  </body>
</html>
```

這樣就完成了一個基本的「新增文章」功能！

---

## 7. 設定 URL

最後，我們要在 `urls.py` 內設定 URL，讓 `create_post` View 能夠被訪問。

在 `blog/urls.py` 加入：

```python
from django.urls import path
from .views import create_post

urlpatterns = [
    path('create/', create_post, name='create_post'),
]
```

這樣我們就可以在 `http://127.0.0.1:8000/create/` 訪問 **新增文章的頁面**。

---

## 8. 總結

這篇文章介紹了 **Django 中的 CRUD - Create（新增資料）**，並透過 **Model、Admin、Form、View、Template** 來完成完整的「建立文章」功能。

✔ **建立 Model，定義資料庫結構**
✔ **使用 Django Admin 直接新增資料**
✔ **建立 Django Form，讓使用者填寫資料**
✔ **設定 View，處理表單提交與存入資料庫**
✔ **建立 HTML 模板，顯示輸入表單**
✔ **設定 URL，讓功能可供訪問**

下一篇文章會繼續介紹 **Django CRUD - Read（讀取資料）**，讓我們的文章能夠在前端正確顯示！ 🚀
如果有任何問題歡迎下面留言討論！
