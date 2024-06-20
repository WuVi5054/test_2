# test_2

<response>
# Building a Webapp with Django Backend and React Frontend

Here's a step-by-step guide to building a web application using Django for the backend and React for the frontend.

## 🛠️ Steps and Code

### 1. **Set Up Django Backend**

#### Install Django and Django REST Framework
```sh
pip install django djangorestframework
```

#### Create a Django Project and App
```sh
django-admin startproject myproject
cd myproject
django-admin startapp myapp
```

#### Configure Django Settings
Add `myapp` and `rest_framework` to `INSTALLED_APPS` in `myproject/settings.py`:
```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'myapp',
]
```

#### Create a Model
In `myapp/models.py`:
```python
from django.db import models

class Item(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()

    def __str__(self):
        return self.name
```

#### Create a Serializer
In `myapp/serializers.py`:
```python
from rest_framework import serializers
from .models import Item

class ItemSerializer(serializers.ModelSerializer):
    class Meta:
        model = Item
        fields = '__all__'
```

#### Create a View
In `myapp/views.py`:
```python
from rest_framework import viewsets
from .models import Item
from .serializers import ItemSerializer

class ItemViewSet(viewsets.ModelViewSet):
    queryset = Item.objects.all()
    serializer_class = ItemSerializer
```

#### Configure URLs
In `myproject/urls.py`:
```python
from django.contrib import admin
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from myapp.views import ItemViewSet

router = DefaultRouter()
router.register(r'items', ItemViewSet)

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include(router.urls)),
]
```

#### Migrate and Run Server
```sh
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

### 2. **Set Up React Frontend**

#### Create a React App
```sh
npx create-react-app myfrontend
cd myfrontend
```

#### Install Axios for HTTP Requests
```sh
npm install axios
```

#### Create a Component to Fetch Data
In `src/App.js`:
```javascript
import React, { useEffect, useState } from 'react';
import axios from 'axios';

function App() {
    const [items, setItems] = useState([]);

    useEffect(() => {
        axios.get('http://localhost:8000/api/items/')
            .then(response => {
                setItems(response.data);
            })
            .catch(error => {
                console.error("There was an error fetching the items!", error);
            });
    }, []);

    return (
        <div className="App">
            <h1>Items</h1>
            <ul>
                {items.map(item => (
                    <li key={item.id}>{item.name}: {item.description}</li>
                ))}
            </ul>
        </div>
    );
}

export default App;
```

### 3. **Handle CORS**

#### Install and Configure `django-cors-headers`
```sh
pip install django-cors-headers
```

In `myproject/settings.py`:
```python
INSTALLED_APPS = [
    ...
    'corsheaders',
]

MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    ...
]

CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
]
```

### 4. **Run Both Servers**

#### Run Django Server
```sh
python manage.py runserver
```

#### Run React Server
```sh
npm start
```

Your React frontend should now be able to communicate with your Django backend.

## 🌐 Sources
1. [medium.com - Django (Backend) + React (Frontend) Basic Tutorial](https://medium.com/@gazzaazhari/django-backend-react-frontend-basic-tutorial-6249af7964e4)
2. [digitalocean.com - How To Build a To-Do application Using Django and React](https://www.digitalocean.com/community/tutorials/build-a-to-do-application-using-django-and-react)
3. [ordergroup.co - Build a web application with Django and React](https://ordergroup.co/resources/article-business-and-technical/build-a-web-application-with-django-and-react/)
4. [logrocket.com - Using React with Django to create an app: Tutorial](https://blog.logrocket.com/using-react-django-create-app-tutorial/)
5. [reddit.com - Learning to write production level Django code + React.JS](https://www.reddit.com/r/django/comments/k6plfa/learning_to_write_production_level_django_code/)
</response>