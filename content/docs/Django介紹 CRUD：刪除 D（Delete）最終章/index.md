---
title: "[Django]介紹 CRUD：刪除 D（Delete）最終章！"
draft: false
description: "深入了解 Django 中的 CRUD 操作，這篇文章專注於 D（Delete），學習如何在 Django 中刪除資料。"
slug: "django-crud-delete"
categories: ["Django"]
tags: ["Python", "Web 應用", "CRUD"]
date: 2025-02-12T17:00:00+08:00
author: "Bukun"
series: ["CRUD"]
series_order: 4
---

在 Web 開發中，**CRUD（Create、Read、Update、Delete）** 是最基本的操作，其中 **D（Delete）** 代表「刪除資料」。
這篇文章會帶你學習 **Django View、Template**，並建立 **文章刪除功能**。

---

## 1. 設定 Model（資料庫模型）

在前一篇 [Django CRUD - Update](slug: "django-crud-update") 中，我們已經建立了 `Post` Model。
如果你還沒設定，請先參考前面的文章。

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```

---

## 2. 建立刪除 View

我們需要建立一個 View，負責處理文章刪除。

在 `blog/views.py` 加入：

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post

def delete_post(request, post_id):
    post = get_object_or_404(Post, id=post_id)

    if request.method == "POST":
        post.delete()
        return redirect('post_list')

    return render(request, 'blog/delete_post.html', {'post': post})
```

這個 View 的流程：

1. **取得要刪除的文章**，如果不存在則回傳 404。
2. **GET 請求**：顯示確認刪除的頁面。
3. **POST 請求**：刪除文章，並導向文章列表。

---

## 3. 建立 HTML 模板

在 `blog/templates/blog/delete_post.html` 建立模板：

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>刪除文章</title>
  </head>
  <body>
    <h1>刪除文章 - {{ post.title }}</h1>
    <p>確定要刪除此文章嗎？</p>
    <form method="post">
      {% csrf_token %}
      <button type="submit">確認刪除</button>
    </form>
    <a href="{% url 'post_list' %}">取消</a>
  </body>
</html>
```

這樣使用者會看到刪除確認頁面，避免誤刪資料。

---

## 4. 設定 URL

我們需要設定 URL，讓 `delete_post` View 可以被訪問。

在 `blog/urls.py` 加入：

```python
from django.urls import path
from .views import post_list, post_detail, update_post, delete_post

urlpatterns = [
    path('', post_list, name='post_list'),
    path('post/<int:post_id>/', post_detail, name='post_detail'),
    path('post/<int:post_id>/edit/', update_post, name='update_post'),
    path('post/<int:post_id>/delete/', delete_post, name='delete_post'),
]
```

現在可以在 `http://127.0.0.1:8000/post/1/delete/` 進行文章刪除。

---

## 5. 在文章列表加上「刪除」按鈕

為了讓使用者更方便刪除文章，我們需要修改 `post_list.html`。

在 `blog/templates/blog/post_list.html` 更新：

```html
<ul>
  {% for post in posts %}
  <li>
    <a href="{% url 'post_detail' post.id %}">{{ post.title }}</a>
    <a href="{% url 'update_post' post.id %}">編輯</a>
    <a href="{% url 'delete_post' post.id %}">刪除</a>
  </li>
  {% endfor %}
</ul>
```

這樣每篇文章旁邊就會有 **「刪除」按鈕**。

---

## 6. 總結

這篇文章介紹了 **Django 中的 CRUD - Delete（刪除資料）**，並透過 **Model、View、Template** 來完成 **文章刪除功能**。

✔ **設定 View，處理刪除請求**
✔ **建立 HTML 模板，顯示刪除確認頁面**
✔ **設定 URL，讓功能可供訪問**
✔ **在文章列表頁面加上「刪除」按鈕**

至此，我們已經完成了 **Django CRUD（Create、Read、Update、Delete）** 的完整功能！

如果有任何問題歡迎下面留言討論！
