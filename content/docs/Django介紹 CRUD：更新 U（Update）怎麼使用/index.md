---
title: "[Django]介紹 CRUD：更新 U（Update），讓文章可以編輯吧！"
draft: false
description: "深入了解 Django 中的 CRUD 操作，這篇文章專注於 U（Update），學習如何在 Django 中更新資料。"
slug: "django-crud-update"
categories: ["Django"]
tags: ["Python", "Web 應用", "CRUD"]
date: 2025-02-12T16:00:00+08:00
author: "Bukun"
series: ["CRUD"]
series_order: 3
---

在 Web 開發中，**CRUD（Create、Read、Update、Delete）** 是最基本的操作，其中 **U（Update）** 代表「更新資料」。
這篇文章會帶你學習 **Django Form、View 與 Template**，並建立 **文章編輯功能**。

---

## 1. 設定 Model（資料庫模型）

在前一篇 [Django CRUD - Read](slug: "django-crud-read") 中，我們已經建立了 `Post` Model。
如果你還沒設定，請先參考前兩篇文章。

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

這裡新增了 `updated_at` 欄位，讓 Django 自動記錄資料更新時間。

---

## 2. 建立 Django 表單（Forms）

為了讓使用者能夠編輯文章，我們需要建立 Django Form。

在 `blog/forms.py` 加入：

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'content']
```

這樣 Django 會根據 `Post` Model **自動產生表單**。

---

## 3. 建立更新 View

我們需要建立一個 View，負責處理文章編輯。

在 `blog/views.py` 加入：

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post
from .forms import PostForm

def update_post(request, post_id):
    post = get_object_or_404(Post, id=post_id)

    if request.method == "POST":
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            form.save()
            return redirect('post_detail', post_id=post.id)
    else:
        form = PostForm(instance=post)

    return render(request, 'blog/update_post.html', {'form': form, 'post': post})
```

這個 View 的流程：

1. **取得要編輯的文章**，如果不存在則回傳 404。
2. **GET 請求**：顯示表單，並將 `Post` 的資料填入。
3. **POST 請求**：接收使用者輸入並儲存更新後的內容。
4. **重新導向**：編輯成功後，返回文章詳細頁面。

---

## 4. 建立 HTML 模板

在 `blog/templates/blog/update_post.html` 建立模板：

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>編輯文章</title>
  </head>
  <body>
    <h1>編輯文章 - {{ post.title }}</h1>
    <form method="post">
      {% csrf_token %} {{ form.as_p }}
      <button type="submit">更新</button>
    </form>
    <a href="{% url 'post_detail' post.id %}">取消</a>
  </body>
</html>
```

這樣就可以顯示表單，讓使用者編輯文章。

---

## 5. 設定 URL

我們需要設定 URL，讓 `update_post` View 可以被訪問。

在 `blog/urls.py` 加入：

```python
from django.urls import path
from .views import post_list, post_detail, update_post

urlpatterns = [
    path('', post_list, name='post_list'),
    path('post/<int:post_id>/', post_detail, name='post_detail'),
    path('post/<int:post_id>/edit/', update_post, name='update_post'),
]
```

現在可以在 `http://127.0.0.1:8000/post/1/edit/` 進行文章編輯。

---

## 6. 在文章列表加上「編輯」按鈕

為了讓使用者更方便進入編輯頁面，我們需要修改 `post_list.html`。

在 `blog/templates/blog/post_list.html` 更新：

```html
<ul>
  {% for post in posts %}
  <li>
    <a href="{% url 'post_detail' post.id %}">{{ post.title }}</a>
    <a href="{% url 'update_post' post.id %}">編輯</a>
  </li>
  {% endfor %}
</ul>
```

這樣每篇文章旁邊就會有 **「編輯」按鈕**。

---

## 7. 總結

這篇文章介紹了 **Django 中的 CRUD - Update（更新資料）**，並透過 **Model、Form、View、Template** 來完成 **文章編輯功能**。

✔ **建立 Django Form，讓使用者編輯資料**
✔ **設定 View，處理表單提交並更新資料庫**
✔ **建立 HTML 模板，顯示輸入表單**
✔ **設定 URL，讓功能可供訪問**
✔ **在文章列表頁面加上「編輯」按鈕**

下一篇文章會介紹 [**Django CRUD - Delete（刪除資料）**]({{< relref "docs/Django介紹 CRUD：刪除 D（Delete）最終章" >}})，讓我們學習如何刪除文章！

如果有任何問題歡迎下面留言討論！
