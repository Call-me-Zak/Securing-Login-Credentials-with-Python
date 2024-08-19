# Securing-Login-Credentials-with-Python
# Units :


## 01 : Creating login interface : HTML, CSS, JavaScript
## 02 : Mapping URLs to views : Python, Django
## 03 : 


#### Unit 01 :

* Installing Django (Python library)

  ````pip install django```
  ![image](https://github.com/user-attachments/assets/b6dd4fa2-2bd9-4502-adb0-8706a6a04f49)

* Creating a Django project :

Browse to the desired directory then use the following command :
  
```django-admin startproject <project_name>```

![image](https://github.com/user-attachments/assets/b8b1977f-07d3-494c-a284-d18efdc7301d)

New directory "myproject" created under root directory

![image](https://github.com/user-attachments/assets/9df4382d-2343-4da5-8792-9e7716a9ee29)

Let's navigate to the directory using ```cd myproject``` then run the command ```python manage.py startapp myapp``` to set up our app files

![image](https://github.com/user-attachments/assets/db9eec93-89f6-4a94-acd8-600d14e56e5b)

Update settings.py to include our app

![image](https://github.com/user-attachments/assets/093ba516-2db3-451a-bce5-a21ece6f26f2)

Update middleware by adding ```myapp.middleware.WindowsAuthMiddleware```

![image](https://github.com/user-attachments/assets/a293660c-f3b6-4e7a-a8e5-b6977de1e23f)


Created views.py, urls.py and windows_auth_backend_win.py in myapp directory (check files)






