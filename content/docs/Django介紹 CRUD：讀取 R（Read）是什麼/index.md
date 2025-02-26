---
title: "[Django]介紹 CRUD：讀取 R（Read）是什麼？"
draft: false
description: "深入了解 Django 中的 CRUD 操作，這篇文章專注於 R（Read），學習如何在 Django 中讀取並顯示資料。"
slug: "django-crud-read"
categories: ["Django"]
tags: ["Python", "Web 應用", "CRUD"]
date: 2025-02-12T15:00:00+08:00
author: "Bukun"
series: ["CRUD"]
series_order: 2
---

在 Web 開發中，**CRUD（Create、Read、Update、Delete）** 是最基本的操作，其中 **R（Read）** 代表「讀取資料」。
這篇文章會帶你學習 **Django QuerySet、ListView、DetailView**，並在前端顯示資料。

---

## 1. 設定 Model（資料庫模型）

在前一篇 [Django CRUD - Create](slug: "django-crud-create") 中，我們已經建立了 `Post` Model。
如果你還沒設定，請先參考上一篇文章。

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

---

## 2. 在 Django Shell 測試讀取資料

如果你的資料庫已經有資料，可以使用 **Django Shell** 來測試讀取：

```bash
python manage.py shell
```

進入 Django Shell 後，輸入：

```python
from blog.models import Post

# 取得所有文章
posts = Post.objects.all()
print(posts)

# 取得單一文章
post = Post.objects.get(id=1)
print(post.title, post.content)
```

這樣就能透過 Django ORM 來 **讀取資料**。

---

## 3. 建立文章列表頁面（ListView）

現在我們來建立一個頁面，顯示所有的文章列表。

### **步驟 1：設定 View**

在 `blog/views.py` 加入：

```python
from django.shortcuts import render
from .models import Post

def post_list(request):
    posts = Post.objects.all()  # 取得所有文章
    return render(request, 'blog/post_list.html', {'posts': posts})
```

這個 View 會：

1. **從資料庫取得所有文章**
2. **傳遞給模板（post_list.html）來渲染頁面**

### **步驟 2：建立 HTML 模板**

在 `blog/templates/blog/post_list.html` 建立模板：

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>文章列表</title>
  </head>
  <body>
    <h1>文章列表</h1>
    <ul>
      {% for post in posts %}
      <li>
        <a href="{% url 'post_detail' post.id %}">{{ post.title }}</a>
      </li>
      {% endfor %}
    </ul>
  </body>
</html>
```

這樣每篇文章的標題都會顯示在頁面上，並且加上 **連結** 指向詳細內容頁面。

### **步驟 3：設定 URL**

在 `blog/urls.py` 加入：

```python
from django.urls import path
from .views import post_list

urlpatterns = [
    path('', post_list, name='post_list'),
]
```

現在可以在 `http://127.0.0.1:8000/` 查看文章列表。

---

## 4. 建立文章詳細頁面（DetailView）

### **步驟 1：設定 View**

在 `blog/views.py` 加入：

```python
from django.shortcuts import render, get_object_or_404
from .models import Post

def post_detail(request, post_id):
    post = get_object_or_404(Post, id=post_id)
    return render(request, 'blog/post_detail.html', {'post': post})
```

這個 View 會：

1. **根據 post_id 取得特定文章**
2. **如果文章不存在，回傳 404 錯誤**
3. **傳遞給模板（post_detail.html）來渲染頁面**

### **步驟 2：建立 HTML 模板**

在 `blog/templates/blog/post_detail.html` 建立模板：

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>{{ post.title }}</title>
  </head>
  <body>
    <h1>{{ post.title }}</h1>
    <p>{{ post.content }}</p>
    <a href="{% url 'post_list' %}">回到文章列表</a>
  </body>
</html>
```

這樣我們就可以顯示文章的詳細內容。

### **步驟 3：設定 URL**

在 `blog/urls.py` 加入：

```python
from django.urls import path
from .views import post_list, post_detail

urlpatterns = [
    path('', post_list, name='post_list'),
    path('post/<int:post_id>/', post_detail, name='post_detail'),
]
```

現在可以在 `http://127.0.0.1:8000/post/1/` 查看 **第一篇文章的詳細內容**。

---

## 5. 總結

這篇文章介紹了 **Django 中的 CRUD - Read（讀取資料）**，並透過 **Model、View、Template** 來完成資料的讀取與顯示。

✔ **使用 Django ORM 讀取資料**
✔ **建立文章列表頁面，顯示所有文章**
✔ **建立文章詳細頁面，顯示單篇文章內容**
✔ **設定 URL 來對應不同頁面**

下一篇文章會介紹 [**Django CRUD - Update（更新資料）**]({{< relref "docs/Django介紹 CRUD：更新 U（Update）怎麼使用" >}})，讓我們學習如何編輯與修改文章內容！

如果有任何問題歡迎下面留言討論！
