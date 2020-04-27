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
           <a href="{% url 'logoutuser' %}">LOGOUT</a> # now it's GET
           
    
     ### make a home page
     
     ### solve error:  'todo.views.logoutuser didn't return an HttpResponse object'
            error occur because this function has a 'POST' but no action is happend
            
         ##### base.html:
              <form action="{% url 'logoutuser' %}" method="POST"> 
                   {% csrf_token %}
                   <button type="submit">Logout</button>
              </form>
              
      
    
   ### user can login with their existing account:
        
        
      #####urls.py:
         urlpatterns = [
           path('login/', views.loginuser, name='loginuser'),
          ]
           

     #####views.py:
        from django.contrib.auth.forms import  AuthenticationForm
        from django.contrib.auth import authenticate
        
        def loginuser(request):
          if request.method =='GET':
            return render(request, 'todo/loginuser.html' , {'form':AuthenticationForm()})

          else:
            user=authenticate(request, username=request.POST['username'] ,password=request.POST['password'])
              if user is None:
                 return render(request, 'todo/loginuser.html' , {'form':AuthenticationForm(),'error':'username or password did not match'})
                 
              else:
                login(request, user)
                return redirect('currenttodos')
                
      
    #####  loginuser.html
       {% extends 'todo/base.html' %}
       {% block content %}
        <h1>Login</h1>
        <h2>{{ error }}</h2>
        <form method="POST">
          {% csrf_token %}
          {{form.as_p}}
          <button type="submit">Login</button>
        </form>
       {% endblock %}
       
       
 
 
### creating models for todo: 
    ##### models.py:
      from django.db import models
      from django.contrib.auth.models import User #for one to many relationship
      class Todo(models.Model): #search django model field
        title = models.CharField(max_length=100)
        memo = models.TextField(blank=True)
        created = models.DateTimeField(auto_now_add=True)
        datecompleted = models.DateTimeField(null=True)
        important =  models.BooleanField(default=False)
        user = models.ForeignKey(User, on_delete=models.CASCADE)#ForeignKey store the relationship between this todo and that.
    #python3 manage.py makemigrations
    #python3 manage.py migrate
       
    #####admin.py:
      from django.contrib import admin
      from .models import Todo
      admin.site.register(Todo)

    ### created field not showing , let's make it clear:
        ##### admin.py:
             from django.contrib import admin
             from .models import Todo
             
            #specifying 'created' as read only file
            class TodoAdmin(admin.ModelAdmin):
               readonly_fields=('created',)
               
            admin.site.register(Todo,TodoAdmin)

      
     

  ### user can now crteate todo object:
       ##### urls.py:
       
           path('create/', views.createtodo, name='createtodo'), 
     
     
      ##### views.py:
      
     def createtodo(request):
         if request.method =='GET':
             return render(request, 'todo/createtodo.html' , {'form':TodoForm()})
         else:
             #we have to get the information from the 'POST' request and connect with our form
             form=TodoForm(request.POST) #whatever they send us like title,memo we are gona pass that into Todoform. and we are collecting this  data in form
             newtodo=form.save(commit=False) # cammit=false stop saving data into database
             #we have to handel which user are saving this data
             newtodo.user=request.user
             newtodo.save()
             return redirect('currenttodos')

           
      ##### forms.py:
      
         from django.forms import ModelForm #TodoForm needs inheret, so we import it
         from .models import Todo
         
         class TodoForm(ModelForm):
          class Meta: #specify what class it is,what model,fields we want ,so we need to use meta class
            model =Todo #specifying the model ,and we need to import from .models import Todo
            fields=['title','memo','important'] #specify what we want
         
     ##### createtodo.html:
           {% extends 'todo/base.html' %}
           {% block content %}
            <h1>create</h1>
            <h2>{{ error }}</h2>
            <form method="POST">
              {% csrf_token %}
              {{form.as_p}}
              <button type="submit">Create</button>
            </form>
           {% endblock %}

     
    ### title size is 100 char, so 101 char found error (ValueError).Handel this by try and exception
     ##### views.py:
    def createtodo(request):
       if request.method =='GET':
           return render(request, 'todo/createtodo.html' , {'form':TodoForm()})
       else:
        try:
           #we have to get the information from the 'POST' request and connect with our form
           form=TodoForm(request.POST) #whatever they send us like title,memo we are gona pass that into Todoform. and we are collecting this  data in form
           newtodo=form.save(commit=False) # cammit=false stop saving data into database
           #we have to handel which user are saving this data
           newtodo.user=request.user
           newtodo.save()
           return redirect('currenttodos')
           
        except ValueError:
            return render(request, 'todo/createtodo.html' , {'form':TodoForm(),'error':'Bad data passed in. T'})

          
         
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
    
           
      


                       
           
           
   
   
    
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
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





  

  
      
 



