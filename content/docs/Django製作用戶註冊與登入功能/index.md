---
title: "[Django]建立網站第一步：製作用戶註冊與登入功能"
draft: false
description: "學習如何使用 Django 建立用戶註冊與登入功能，包含表單、驗證與登入狀態管理。"
slug: "django-user-authentication"
categories: ["Django"]
tags: ["Python", "Web 應用", "用戶驗證"]
date: 2025-02-25T21:00:00+08:00
author: "Bukun"
series: ["註冊登入"]
series_order: 1
---

前面文章已經大致了解 Django 運作模式，且也有了基礎設置
而在 Web 應用中，**用戶註冊與登入** 是常見且重要的功能。
這篇文章將一起完成 **Django** 的用戶註冊和登入功能
使用 **Django 的內建驗證系統**，實現完整的帳號管理。

---

## 1. 設定 Django 專案

首先，建立一個新的 Django 專案與應用：

```bash
django-admin startproject myproject
cd myproject
python manage.py startapp accounts
```

接著，將 **accounts** 加入 **settings.py**：

```python
# myproject/settings.py 目前的檔案位置

INSTALLED_APPS = [
    # 其他應用
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'accounts',  # 新增 accounts 應用
]
```

---

## 2. 使用 Django 內建的 UserCreationForm

在 accounts/forms.py 建立表單：

```python
from django import forms
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User

class SignUpForm(UserCreationForm):
    email = forms.EmailField(required=True)

    class Meta:
        model = User
        fields = ('username', 'email', 'password1', 'password2')
```

這個表單包含使用者名稱、Email、以及兩次密碼（用來確認密碼正確）。

Django 提供了內建的 **用戶驗證系統 (Authentication System)**
讓開發者能快速建立用戶註冊、登入、權限管理等功能。
這個系統內建在 django.contrib.auth 中
包含**User 模型**、**表單(forms)**、**視圖（views**）和**工具函示**

現在使用的**UserCreationForm**也有內建一些重要功能

### 欄位設定

- username：使用者名稱。
- password1：密碼輸入欄位。
- password2：再次輸入密碼，用於確認兩次密碼相同。

### 內建驗證

- 密碼匹配：自動檢查 password1 和 password2 是否一致。
- 密碼強度：如果有設定密碼政策（例如最少字元、特殊字元等），UserCreationForm 會進行檢查。
- 使用者名稱唯一性：檢查 username 是否已經被使用。

而我們在其中手動加入了 email 欄位
並且使用**forms.EmailField(required=True)**設定為必填

---

## 3. 建立用戶註冊 View

在 accounts/views.py 中建立：

```python
from django.shortcuts import render, redirect
from django.contrib.auth import login
from .forms import SignUpForm

def signup_view(request):
    if request.method == 'POST':
        form = SignUpForm(request.POST)
        if form.is_valid():
            user = form.save()
            login(request, user)  # 註冊後自動登入
            return redirect('home')
    else:
        form = SignUpForm()
    return render(request, 'accounts/signup.html', {'form': form})
```

### 匯入的模組

- **render**：用來將資料與模板結合，返回 HTML 頁面
- **redirect**：用來在成功註冊後，導向使用者到其他頁面（例如 home 頁面）
- **login**：Django 內建的登入方法，會將用戶標記為已登入狀態並建立 session
- **SignUpForm**：自訂的註冊表單(上一步我們建立的表單)

### 視圖邏輯

- 如果是 POST 請求，驗證表單並檢查是否符合規範後註冊新用戶
- 註冊成功後，使用 login() 自動登入新用戶並導向 home 頁面
- 如果是 GET 請求，顯示註冊表單

---

## 4. 建立登入與登出 View

在 accounts/views.py 中使用內建的登入與登出 View：

```python
from django.contrib.auth.views import LoginView, LogoutView

class CustomLoginView(LoginView):
    template_name = 'accounts/login.html'

class CustomLogoutView(LogoutView):
    next_page = 'home'  # 登出後導向首頁
```

---

## 5. 設定 URL

在 accounts/urls.py 中設定路由：

```python
from django.urls import path
from .views import signup_view, CustomLoginView, CustomLogoutView

urlpatterns = [
    path('signup/', signup_view, name='signup'),
    path('login/', CustomLoginView.as_view(), name='login'),
    path('logout/', CustomLogoutView.as_view(), name='logout'),
]

```

再把 accounts/urls.py 加到專案核心的 urls.py：

```python
from django.contrib import admin
from django.urls import path, include
from django.shortcuts import render

def home(request):
    return render(request, 'home.html')

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', home, name='home'),
    path('accounts/', include('accounts.urls')),
]

```

---

## 6. 建立模板

接下來就可以依照自己想要的風格
創建各個頁面的模板了！
下面我會簡單製作幾個當作範例

### 註冊頁面

頁面放在 **accounts/templates/accounts/signup.html**

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>用戶註冊</title>
  </head>
  <body>
    <h1>註冊帳號</h1>
    <form method="post">
      {% csrf_token %} {{ form.as_p }}
      <button type="submit">註冊</button>
    </form>
    <p>已經有帳號了？<a href="{% url 'login' %}">登入</a></p>
  </body>
</html>
```

### 登入頁面

頁面放在 **accounts/templates/accounts/login.html**

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>用戶登入</title>
  </head>
  <body>
    <h1>登入</h1>
    <form method="post">
      {% csrf_token %} {{ form.as_p }}
      <button type="submit">登入</button>
    </form>
    <p>還沒有帳號？<a href="{% url 'signup' %}">註冊</a></p>
  </body>
</html>
```

### 首頁

頁面放在**templates/home.html**

```html
<!DOCTYPE html>
<html lang="zh-TW">
  <head>
    <meta charset="UTF-8" />
    <title>首頁</title>
  </head>
  <body>
    <h1>歡迎來到我的網站！</h1>

    {% if user.is_authenticated %}
    <p>嗨，{{ user.username }}！<a href="{% url 'logout' %}">登出</a></p>
    {% else %}
    <p>
      <a href="{% url 'login' %}">登入</a> 或
      <a href="{% url 'signup' %}">註冊</a>
    </p>
    {% endif %}
  </body>
</html>
```

---

## 7. 設定登入後導向

最後我們在**settings.py**中加入登入登出後的導向頁面

```python
LOGIN_REDIRECT_URL = 'home'  # 登入成功後導向首頁
LOGOUT_REDIRECT_URL = 'home'  # 登出後導向首頁
```

---

## 8. 總結

這樣我們就完成了基本的用戶註冊、登入功能
如果有任何問題或想法，歡迎在留言區討問！
下一篇文章將繼續實作 **忘記密碼功能**，讓我們的網站開始運作！
