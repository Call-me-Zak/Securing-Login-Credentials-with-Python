![image](https://github.com/user-attachments/assets/0d9f1124-db4f-4598-aba6-7b11f901d73f)# Securing-Login-Credentials-with-Python
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

### Run the server locally

```python manage.py runserver```

![image](https://github.com/user-attachments/assets/7933423e-7649-450d-bd2c-28557f394259)

![image](https://github.com/user-attachments/assets/cb644240-981a-4b02-89b9-66f8cb3d9a36)


![image](https://github.com/user-attachments/assets/334f0554-2eef-4859-86f2-3ada1d1409e8)

To login we must create an account using : ```python manage.py createsuperuser```

![image](https://github.com/user-attachments/assets/3b6abf83-b8f6-4e82-9a90-2abd631e43ad)

Enter Username, Email and Password when prompted :

![image](https://github.com/user-attachments/assets/9aca5bf9-3b9f-48a3-a86e-6acdba2b0a48)

As you can see I get 02 warning messages saying that the password is too short and that it's too similar to the email, this is because I integrated the built-in password validators modules. The password I typed was "zak".

Let's see what happens when I type a common password such as "azerty"

![image](https://github.com/user-attachments/assets/3ec2591e-b9a1-472d-a6d0-d43a5f78f6d0)

And lastly, if the passwords don't match, we get a message for that as well.

![image](https://github.com/user-attachments/assets/52f2a915-c5cf-42cd-9cec-b27262b8a1a4)


Ofc. you can always bypass the validators by typing ```y``` but I'm not gonna do that, I'll just make a password that meets the criteria.
As we can see in the picture, superuser was created successfully.

![image](https://github.com/user-attachments/assets/66e4c08b-6985-498e-9a34-72ab251c4fee)


And always keep in mind when creating passwords we must follow the NIST password guidlines and this is how I've done that by integrating the following code in ```settings.py``` :

![image](https://github.com/user-attachments/assets/5b458787-bdb6-45af-a362-f3844c2541bd)

```django.contrib.auth.password_validation```:
This module contains the classes that django checks to verify that the password meets these specific conditions, as for ```auth``` it stands for django "authentication" built-in framework.

Let's break down the code line by line :

```django.contrib.auth.password_validation.UserAttributeSimilarityValidator``` is for validating that the user and the password differ and aren't very similar, this is defined by a similarity coefficient (e.g. 0.7 means that the password and username can be similar up to 70%, the higher the value the more similar the password and username can be, and the lower the value, say we set it to 0 then the password and username must be completely different)

Why ? you might ask, well to deter guessers from guessing your password based on the available info your username might giveaway (although uncommon not impossible).
And this is what the module used looks like :
```python
from django.contrib.auth.password_validation import UserAttributeSimilarityValidator

class UserAttributeSimilarityValidator:
    def __init__(self, user_attributes=None, max_similarity=0.7):
        self.user_attributes = user_attributes or ['username', 'first_name', 'last_name', 'email']
        self.max_similarity = max_similarity

    def validate(self, password, user=None):
        for attribute in self.user_attributes:
            value = getattr(user, attribute, '') or ''
            if value and self.is_similar(password, value):
                raise ValidationError("The password is too similar to your other personal information.")
    
    def is_similar(self, password, value):
        # Simplified similarity check code
        return True if some_similarity_metric(password, value) > self.max_similarity else False
```

Ofc. it's not that simple, and personally I didn't dive into it all that deep, rather I have a general idea of how it works to better know what to expect so I can spot any shortcomings, and to verify that it works as intended, which I'll show you how we'll verify using the built-in django-admin GUI.

Link for those who want to read more about password validators :
```
https://docs.djangoproject.com/en/5.1/ref/settings/#auth-password-validators
```

Moving on to line 2 :
```django.contrib.auth.password_validation.MinimumLengthValidator``` just like the name suggests, this verifies that the password does consist of at least 8 characters

Line 3 :
```django.contrib.auth.password_validation.CommonPasswordValidator``` this checks that the password isn't common from a list of pre-established common passwords (I'm not the one who set the list)

And finally, line 4 :
```django.contrib.auth.password_validation.NumericPasswordValidator``` checks that the password isn't numeric, meaning only contains numbers.

Now as promised, to verify that these're working as intended, we can go ahead and visit the django-admin interface.
We can see that the account I've just created is there.

![image](https://github.com/user-attachments/assets/bc37c847-f4d1-4b93-8ae2-9d92ce50642f)

Staff status allows the user to log into the django-admin page.
Afterwards I'm gonna go ahead and create new user accounts from the django-admin interface by clicking ```+Add``` on ```Users```

![image](https://github.com/user-attachments/assets/5da4d236-7751-4d25-b1aa-6d6edcdfc3e6)

I went ahead and created a username

![image](https://github.com/user-attachments/assets/d4a5e57e-cab3-4dad-9d5c-105f3bbb4a90)

And below the password we can see the 04 password validators conditions below :

![image](https://github.com/user-attachments/assets/25a0348f-5788-4064-a1ae-b71def702cfe)

Password °1 : zak (too short)

![image](https://github.com/user-attachments/assets/abb7fbf0-c6bc-45b6-9d84-85815dea7a40)
![image](https://github.com/user-attachments/assets/c290bba4-a63e-4174-94b8-447e37220795)

Password °2 : azertyuiop (too common)

![image](https://github.com/user-attachments/assets/6db64144-c976-4f35-91f2-16631fa6484d)

Password °3 : 123456789111 (numeric)

![image](https://github.com/user-attachments/assets/6c68d675-c022-495d-8a46-4860753a4c86)

Password °4 : (same as username)

![image](https://github.com/user-attachments/assets/74776a3d-8956-46e4-a16c-063558b32e2b)
![image](https://github.com/user-attachments/assets/d4851a10-6934-4c6a-9b75-67935d5a4ed9)

Password °5 : r0ngPass1-rd (meets all conditions) 

![image](https://github.com/user-attachments/assets/7dd3ef43-9e24-4bec-92d0-5aa08f96196b)

We can see here that the user was created succefully and the password has been stored, and to add an extra layer of security in case of a breach, it has been hashed (sha256 encryption) and salted (basically adding random characters to make it harder to read), so with the password encrypted it makes it harder for a threat actor to directly access the raw password, ofc. it could be decrypted with the right tools but that's for another topic.

Now we can go ahead and add the necessary info to complete the account, although optional but I'm gonna do it for the sake of the project.
But before that let's go back to the users menu to see if it was added or not :
As we can see there's no info at the moment, so let's add it.

![image](https://github.com/user-attachments/assets/88a0a169-e86f-4ec3-8cf6-a626121ba55b)

After adding the info and clicking on ```Save``` :

![image](https://github.com/user-attachments/assets/da43eaf8-8199-4214-86e7-747356c90f11)

We can see that the email and staff status have been updated :

![image](https://github.com/user-attachments/assets/34e51171-a9d8-45c4-9735-fd638acc2e12)

Let's go ahead and logout from my current superuser account "Zakaria" and login with "newly_made_account" to see if it works or not :

![image](https://github.com/user-attachments/assets/360166f0-bb56-40e6-a160-c63b000082f8)

As you can see I'm able to login but I don't have any permissions, so let's fix that by logging back into my superuser account "Zakaria" and assigning permissions to "newly_made_account"

![image](https://github.com/user-attachments/assets/e329ada9-e384-4eae-9394-0d4a26b3bf2a)


