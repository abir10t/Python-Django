# .........................................django................................................
### terminal :
  
     1. to know current location : Pwd
     2. what file are loccated in current position : ls
     3.for go to back_up : cd ..
     
  ### for setup python 3.7 in ubuntu
     https://phoenixnap.com/kb/how-to-install-python-3-ubuntu
     
     
 ### install django && terminal work
 
    ##### create environment->
      
         1. conda create --name twitter django(django provides that in twitter we need django packages)
         2. conda info --envs (list of environment)
         3. source activate twitter (for activate)
         4. conda install django=2.2 (for latest version use just django)
         5. deactivate(for deactivate the environment)
         6. python -m django --version(for cheacking the django version)
      
      
      
     mkdir django
     cd django
     conda activate myDjangoEnv
     django-admin startproject first_project
     cd first_project
     python manage.py startapp calc(for making a application)
     #### migration: python manage.py migrate
     #### admin user & pass : python manage.py createsuperuser
     #### for changing password : python manage.py changepassword username
  
     
     
### django-media-images :
     #### settings.py :
      MEDIA_URL='/media/'
      MEDIA_ROOT=os.path.join(BASE_DIR, 'media')

     #### Urls.py
       from django.conf.urls.static import static
       urlpatterns +=static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
       
       
### grabs from database:
  
    ##### views.py:
    from.models import Project
    projects=Project.objects.all()
    return render(request,'pro/home.html',{'projects':projects})
       
     
     
  ### another set of urls
  
    ##### urls.py :
    from django.urls import path,include
        path('blog/', include('blog.urls')),
        
    create urls.py folder into apps  
    
    #####  apps urls.py:
        from django.urls import path
        from . import views
        urlpatterns = [
                path('',views.all_blogs, name='all_blogs'),
              ]

   
 ### create signup form (3.0):
 
 
    ##### urls.py :
          from django.conf.urls import url
          from django.contrib import admin
          from calc import views
          urlpatterns = [
              url(r'^admin/', admin.site.urls),
              url(r'^signup/', views.signupuser , name='signupuser'),
          ]
        
        
    ##### views.py :
           from django.shortcuts import render
           from django.contrib.auth.forms import UserCreationForm

           def signupuser(request):
              return render(request, 'todo/signupuser.html' ,  {'form':UserCreationForm()})
    
    
    ##### signupuser.html :
               {{ form.as_p }}  #as_p makes text as <p>
               
               
               
               
 ### make signup button & user can create account & save them into database  
     ##### create super user for admin by terminal
              python manage.py createsuperuser-> give username,pass 
            
            
     ##### signupuser.html :
     
              <h1>Sign Up</h1>
              <h2>{{ error }}</h2>
              <form method="POST"> #  <!post are using creating new data ->
                {% csrf_token %}   # showing text <p>
                {{ form.as_p }}    #for using submit type button
                <button type="submit">sign Up</button>
              </form>
              
        
    ##### views.py:   
        from django.shortcuts import render
        from django.contrib.auth.forms import UserCreationForm
        from django.contrib.auth.models import User  # User.objects.create_user() -> for using it
        
        def signupuser(request):
            if request.method == 'GET':  # for showing just signup/ page.
                 return render(request, 'todo/signupuser.html' ,  {'form':UserCreationForm()})
                 
             else:                      # for 'POST' ->when some one using 
                                        #signup button for create account  
                                        
                     if request.POST['password1'] == request.POST['password2']: #cheacking password & password confirm field same or not.
                                                                                
                                                                                
                        user=User.objects.create_user(request.POST['username'] , password=request.POST['password1'])#for  making new user object,pass the username & password                                                                                               
                                                                                            
                        user.save()   #save new account into database
                     else:
                     
                        return render(request, 'calc/signupuser.html' ,  {'form':UserCreationForm() , 'error':'password did  not match'})

           
            
   ### cheack user name unique when create account (IntegrityError) :
     ##### views.py:
        
           from django.shortcuts import render
           from django.contrib.auth.forms import UserCreationForm
           from django.contrib.auth.models import User  
           from django.db import IntegrityError
    
         def signupuser(request):
            if request.method == 'GET':  
                 return render(request, 'todo/signupuser.html' ,  {'form':UserCreationForm()})
                 
             else:                                              
                     if request.POST['password1'] == request.POST['password2']:
                        try:    #if user name not registered than try
                           user=User.objects.create_user(request.POST['username'] , password=request.POST['password1'])                                user.save() 
                           
                        except IntegrityError:
                           return render(request, 'todo/signupuser.html' ,  {'form':UserCreationForm() ,'error':'thisusername alredy registered'})
                           
                     else:
                        return render(request, 'todo/signupuser.html' ,  {'form':UserCreationForm() , 'error':'password did not match'})
                        
                        
                        
   ### showing logged into account : 
          ### urls.py :
              urlpatterns = [
                                url(r'^current/', views.currenttodos , name='currenttodos'),
                            ]
                            
   
         ##### views.py:
                from django.shortcuts import render, redirect
                from django.contrib.auth import login
                
                try:
                   user=User.objects.create_user(request.POST['username'] , password=request.POST['password1'])
                   user.save()
                   login(request , user) #after login we have to send them somewhere,making currenttodos page.
                   return redirect('currenttodos') #redirect into currenttodos
                   
                def currenttodos(request):
                   return render(request, 'todo/currenttodos.html' )
                
                
                
                
### show if a user is logged in:
    ##### currenttodos.html:
           {% extends 'todo/base.html' %}
           {% block content %}
              Current
           {% endblock %}
           
     
    #####  base.html :
                {% if user.is_authenticated %} <! cheacked if someone is logged in ->
                Logged in as {{ user.username }}
                <a href="logout">logout</a>
                
                {% else %}
                <a href="logout">Signup</a>
                <a href="logout">login</a>

                {% endif %}
                {% block content %}{% endblock %}
                
                
     ### make this change also in our signup page:
     
           ### signupuser.html:
              {% extends 'calc/base.html' %}
              {% block content %}
              <h1>Sign Up</h1>
              <h2>{{ error }}</h2>
              <form method="POST">  <!post are using creating new data ->
                {% csrf_token %}
                {{ form.as_p }}
              <button type="submit">sign Up</button>
              </form>
              {% endblock %}

    

### logout:
     
     ##### urls.py:
           urlpatterns = [
      path('logout/', views.logoutuser, name='logoutuser'),
    
    ]
    
    
    #####views.py:
      from django.contrib.auth import login, logout    #for account logout 
      
       def logoutuser(request):
           if request.method == 'POST':  # it can kicking user out , because browser are doing this work automaticly, so we logged out if it is post request. in base.html -> <a href="{% url 'logoutuser' %}">LOGOUT</a> -> 'GET' statement
              logout(request)   #() -> pass what things are logged out
              return redirect('home')  # redirect use for go to the another function in views.py, and it's must .
              
           
    ### base.html :
           <a href="{% url 'logoutuser' %}">LOGOUT</a>



      
      
      
     


     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     ##### urls.py:
           urlpatterns = [
    path('', views.home, name='home'),
    
    ]


     ##### views.py:
        def home(request):
           return render(request, 'todo/home.html')    
           
           
    ##### home.html:
      {% extends 'todo/base.html' %}
      {% block content %}
         Home
      {% endblock %}

           
      


                       
           
           
   
   
    
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
 # .........................................python................................................
     
     
### Lambda Expressions Map and Filter
          def square(num):
      return num**2
  my_nums=[1,2,3,4,5]
  for item in map (square,my_nums):
      print(item)
      
  or we can make a list:
  list(map(square,my_nums))
  
  
  easy syntax:
    mynum=[1,2,3,4,5]
  print(list(map(lambda num:num**2,mynum)))
  
  
  ### OOP
  
  ### classmethod 
    class PlayerCharacter:
      membership=True
      def __init__(self,name,age):
          self.name=name
          self.age=age
      def shout(self):
          print(f'my name is {self.name}')
      @classmethod
      def adding_things(cls,num1,num2):
          return cls('Teddy',num1+num2)

    player3=PlayerCharacter.adding_things(2,3)
    print(player3.name)
    
   ### Super
      class User(object):
    def __init__(self,email):
        self.email=email

    class Wizard(User):
          def __init__(self,name,power,email):
            super().__init__(email)
            self.name=name
            self.power=power


    wizard1=Wizard('merlin',60,"abc@gmail.com ")
    print(wizard1.email)//we can now access the email
    
    
  ### Introspection:
      class User():
      def __init__(self,email):
          self.email=email

    class Wizard(User):
        def __init__(self,name,power,email):
            super().__init__(email)
            self.name=name
            self.power=power


    wizard1=Wizard('merlin',60,"abc@gmail.com ")
    print(dir(wizard1))//give us list of all method or attributes 
    
  ### power of dunder method
    class Superlist(list):
    def __len__(self):
        return 100;

    Superlist1=Superlist()
    print(len(Superlist1))
    Superlist1.append(50)
    print(Superlist1[0])





  

  
      
 



