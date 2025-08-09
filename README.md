# Django To-Do App 📝

A simple, elegant To-Do application built with Django and styled with Tailwind CSS.

![To-Do App](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)

## Features ✨

- ➕ Add new tasks
- ✅ Mark tasks as complete
- 🗑️ Delete tasks
- 📱 Responsive design
- 🎨 Modern UI with Tailwind CSS

## Prerequisites 📋

Before you begin, ensure you have the following installed:
- Python 3.8 or higher
- pip (Python package installer)

## Step-by-Step Setup Guide 🚀

### Step 1: Create Project Directory
```bash
mkdir django-todo-app
cd django-todo-app
```

### Step 2: Install Django
```bash
pip install django
```

### Step 3: Create Django Project
```bash
django-admin startproject project1
cd project1
```

### Step 4: Create Django App
```bash
python manage.py startapp myapp
```

### Step 5: Configure Settings
Edit `project1/settings.py` and add your app to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'myapp',  # Add this line
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

### Step 6: Create the Task Model
Edit `myapp/models.py`:

```python
from django.db import models

class Task(models.Model):
    title = models.CharField(max_length=200)
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title
```

### Step 7: Define Task model in admin.py Create and Apply Migrations
Edit `myapp/admin.py`:
```python
from django.contrib import admin
from .models import Task

admin.site.register(Task)
```
Then run this in terminal
```bash
python manage.py makemigrations
python manage.py migrate
```

### Step 8: Create Views
Edit `myapp/views.py`:

```python
from django.shortcuts import render, redirect
from .models import Task

def index(request):
    tasks = Task.objects.all()

    if request.method == 'POST':
        title = request.POST.get('title')
        if title:
            Task.objects.create(title=title)
        return redirect('index')

    return render(request, 'index.html', {'tasks': tasks})

def delete_task(request, task_id):
    task = Task.objects.get(id=task_id)
    task.delete()
    return redirect('index')

def complete_task(request, task_id):
    task = Task.objects.get(id=task_id)
    task.completed = True
    task.save()
    return redirect('index')
```

### Step 9: Create URL Configuration
Create `myapp/urls.py`:

```python
from django.urls import path
from .views import index, delete_task, complete_task

urlpatterns = [
    path('', index, name='index'),
    path('delete/<int:task_id>/', delete_task, name='delete_task'),
    path('complete/<int:task_id>/', complete_task, name='complete_task'),
]
```

### Step 10: Update Main URL Configuration
Edit `project1/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
]
```

### Step 11: Create Template Directory
Create the following directory structure:
```
myapp/
└── templates/
    └── index.html
```

### Step 12: Create the HTML Template
Create `myapp/templates/index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>To-Do App</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
    <h1 class="font-extrabold text-2xl">To-Do List</h1>

    <!-- Add Task Form -->
    <form method="POST" class="my-4 bg-white p-4 rounded shadow-md w-1/3">
        {% csrf_token %}
        <input type="text" name="title" placeholder="Enter a task" required class="border border-gray-300 p-2 w-full rounded mb-2">
        <button type="submit" class="bg-green-500 text-white px-4 py-2 w-full">Add Task</button>
    </form>

    <ul class="p-4 bg-white rounded shadow-md w-1/3">
        {% for task in tasks %}
        <li class="flex justify-between items-center border-b p-2 bg-gray-300 rounded mb-2">
            {% if not task.completed %}
                {{ task.title }}
                <div class="p-2">
                    <a href="{% url 'complete_task' task.id %}" class="bg-green-500 px-4 py-2">Complete</a>
                    <a href="{% url 'delete_task' task.id %}" class="bg-red-500 px-4 py-2">Delete</a>
                </div>
            {% else %}
                {{ task.title }}
                <a href="{% url 'delete_task' task.id %}" class="bg-red-500 px-4 py-2">Delete</a>
            {% endif %}
        </li>
        {% endfor %}
    </ul>
</body>
</html>
```

### Step 13: Run the Development Server
```bash
python manage.py runserver
```

Visit `http://127.0.0.1:8000/` in your browser to see your To-Do app!

## Project Structure 📁

```
project1/
├── db.sqlite3
├── manage.py
├── myapp/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   ├── views.py
│   ├── migrations/
│   │   ├── __init__.py
│   │   └── 0001_initial.py
│   └── templates/
│       └── index.html
└── project1/
    ├── __init__.py
    ├── asgi.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

## Key Concepts Learned 🎓

1. **Django Project Structure**: Understanding how Django organizes projects and apps
2. **Models**: Creating database models with Django ORM
3. **Views**: Handling HTTP requests and responses
4. **Templates**: Rendering HTML with Django template engine
5. **URL Routing**: Mapping URLs to views
6. **Forms**: Processing form data in Django
7. **CRUD Operations**: Create, Read, Update, Delete functionality
8. **CSS Framework**: Using Tailwind CSS for styling

## Common Issues and Solutions 🔧

### Issue 1: "No such table" error
**Solution**: Run migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### Issue 2: Template not found
**Solution**: Make sure the template is in the correct path: `myapp/templates/index.html`

### Issue 3: CSS not loading
**Solution**: Ensure you have internet connection for Tailwind CDN, or download it locally

## Enhancements Ideas 💡

- Add user authentication
- Add task categories/tags
- Add due dates
- Add task priorities
- Implement search functionality
- Add task editing capability
- Deploy to production (Heroku, AWS, etc.)

## Contributing 🤝

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License 📄

This project is open source and available under the [MIT License](LICENSE).

## Author ✍️

Created with ❤️ by Joseph Liyon 
Visit liyon.xyz

---

**Happy Coding!** 🚀
