Django Custom User Model (Email Authentication)

This project demonstrates how to create a **custom user model in Django** using `AbstractBaseUser` and `PermissionsMixin`, where **email is used as the primary authentication field** instead of username.

---

## 🚀 Features

- Custom User Model
- Email as `USERNAME_FIELD`
- Custom User Manager
- Superuser creation
- Secure password hashing
- Django best practices

---

## 🛠 Tech Stack

- Python
- Django
- SQLite (default)

---

## 📂 Project Structure

project/
│
├── app/
│ ├── models.py
│ └── managers.py
│
└── manage.py


---

## 🧑‍💻 Custom User Model Code

```python
from django.db import models
from django.contrib.auth.models import AbstractBaseUser, PermissionsMixin, BaseUserManager

class CustomUserManager(BaseUserManager):
    def create_user(self, email, first_name, last_name, password=None):
        if not email:
            raise ValueError('Users must have an email address')

        email = self.normalize_email(email).lower()
        user = self.model(
            email=email,
            first_name=first_name,
            last_name=last_name
        )
        user.set_password(password)
        user.save()
        return user

    def create_superuser(self, email, first_name, last_name, password):
        user = self.create_user(email, first_name, last_name, password)
        user.is_staff = True
        user.is_superuser = True
        user.save()
        return user

class CustomUser(AbstractBaseUser, PermissionsMixin):
    email = models.EmailField(primary_key=True)
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)

    objects = CustomUserManager()

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['first_name', 'last_name']
⚙️ Setup Instructions
git clone https://github.com/your-username/django-custom-user-model.git
cd django-custom-user-model
pip install django
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
📌 Notes
Set AUTH_USER_MODEL = 'app.CustomUser' in settings.py

This model replaces Django’s default User model

