# 🌍 Django Hello World - From Scratch to Deploy with Render

This project is a step-by-step guide to creating a minimal Django web app that displays a single **"Hello World"** page, and deploying it online for free using [Render](https://render.com).

---

## ✅ Requirements

Before starting, ensure you have the following installed:

- Python 3.10+
- pip
- Git
- A GitHub account
- A Render account (we’ll create it below)
- (Optional but recommended) `venv` for a virtual environment

---

## 🛠️ 1. Project Setup

### 1.1. Create a project folder and virtual environment

```bash
mkdir django-hello-world
cd django-hello-world
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### Install Django

```bash
pip install django
```

### 1.3. Create Django project and app
```
django-admin startproject helloproject .
python manage.py startapp main
```

## ⚙️ 2. Django Configuration
2.1. Register the app in helloproject/settings.py
```bash 
INSTALLED_APPS = [
    ...
    'main',
]
```

### 2.2. Set ALLOWED_HOSTS
```bash 
ALLOWED_HOSTS = ['*']  # In production, we'll use '.onrender.com'
```


## 🧱 3. Hello World Page
### 3.1. Create the view in main/views.py
```bash 
from django.shortcuts import render

def home(request):
    return render(request, 'main/home.html')
```

## 3.2. Create the HTML template with styling
Create the file: main/templates/main/home.html

```html 
Modifica
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        h1 {
            font-size: 5rem;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
```

## 3.3. Set up routing

main/urls.py

```bash
from django.urls import path
from .views import home

urlpatterns = [
    path('', home),
]
```

helloproject/urls.py
```bash
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('main.urls')),
    path('admin/', admin.site.urls),
]
```

## 🧪 4. Test Locally
```bash
python manage.py runserver
```
Go to http://127.0.0.1:8000
✔️ You should see a beautiful "Hello World" centered on a gradient background.


## 📦 5. Prepare for Deployment (Render)
### 5.1. Install Gunicorn and WhiteNoise
```bash
pip install gunicorn whitenoise
```
### 5.2. Generate requirements.txt
```
pip freeze > requirements.txt
```
Or manually create it with:

```txt
Django>=5.1,<6.0
gunicorn>=21.2,<22.0
whitenoise>=6.6,<7.0
```

## ⚙️ 6. Configure Django for Production
### 6.1. Update helloproject/settings.py
```bash
import os

DEBUG = False
ALLOWED_HOSTS = ['.onrender.com']

STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
]
```
## 6.2. Create Procfile in the project root
```bash
web: gunicorn helloproject.wsgi
```
## 6.3. Create runtime.txt to specify Python version
```bash
python-3.12.1
```
## 6.4. Collect static files
```bash
python manage.py collectstatic
```
## 🧑‍💻 7. Push to GitHub
### 7.1. Initialize Git and commit
```bash
git init
git add .
git commit -m "Initial commit"
```
### 7.2. Create a repository on GitHub
👉 https://github.com/new

Then connect and push:

```bash
git remote add origin https://github.com/YOUR-USERNAME/django-hello-world.git
git push -u origin master
```
## ☁️ 8. Create a Render Account
Go to 👉 https://render.com

1. Click Sign Up

2. Sign up using your GitHub account (recommended)

3. Authorize Render to access your GitHub repositories

## 🚀 9. Deploy on Render

1. In the **Render dashboard**, click **New → Web Service**
2. Choose your GitHub repo: `django-hello-world`
3. Fill in the deployment configuration as follows:

| Field           | Value                                |
|----------------|--------------------------------------|
| **Runtime**     | Python                               |
| **Build Command** | `pip install -r requirements.txt`   |
| **Start Command** | `gunicorn helloproject.wsgi`        |
| **Branch**       | `main` or `master`                   |
| **Region**       | Closest to your location             |
| **Auto Deploy**  | ✅ Enabled (optional but recommended) |

4. Click **Create Web Service**

## 🌍 10. Your App is Live!
Once the build is complete, Render will give you a public URL like:

```bash
https://django-hello-world.onrender.com
```
Open it in your browser or on your phone. ✔️ Your app is now online!

⚠️ Render Free Tier Notes
Free services go to sleep after 15 minutes of inactivity

First access after sleep may take 30–50 seconds

To keep your app always on, upgrade to a paid plan (optional)

## 📂 Project Structure

```bash
django-hello-world/
├── helloproject/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── main/
│   ├── views.py
│   ├── urls.py
│   └── templates/
│       └── main/
│           └── home.html
│
├── manage.py
├── requirements.txt
├── Procfile
├── runtime.txt
└── staticfiles/  ← (created by collectstatic)
```

## ✅ Summary
You’ve created a Django project from scratch and deployed it for free using Render. You now have a live web app visible from any device!

## 📚 License
This project is licensed under the MIT License.
Feel free to use, modify, and share it.