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
  
   
  ### URL Mapping (create urls.py in project folder) :
     
   ##### urls.py :
         from django.conf.urls import include
           path('first_app/',include('first.urls')),   
       
   ##### urls.py :
          now create urls.py in apps folder
          
         from django.urls import path
         from first_app import views
         urlpatterns=[
         
            path('',views.index,name='index'),
       
              ]

           
   ##### views.py :
          from django.shortcuts import render
          from django.http import HttpResponse
          def index(request):
             return HttpResponse("hello world")
             
             
             

   ### Templates :
   
   ##### issue of operating system, so we will make a default path for templates.now if we transfer this file to another pc no error will be held(windows to linux or linux to windows).
   
   ##### settings.py:
   
       BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
       TEMPLATE_DIR = os.path.join(BASE_DIR,"templates"))
       
  
  
   #### Templates tag :
   
   ##### views.py:
     def index(request):
       my_dict={'insert_me':"hello i am from views.py "}
       return render(request,'first_app/index.html',context=my_dict)

   ##### templates-> first_app -> index.html :
      <body>
      <h1>this is index </h1>
          {{ insert_me }}
      </body>
       
  
  
  
  
  ### static :
  
  ##### settings.py:
     STATIC_URL = '/static/'
     STATIC_DIR = os.path.join(BASE_DIR,"static")
     STATICFILES_DIRS = [
      STATIC_DIR,

     ]
     
  ##### create new file at parrent folder named = static -> images 
  
     <!DOCTYPE html>
     {% load staticfiles %}
     <html>
     ....
     <img src="{% static "images/im.jpeg" %}" alt="Uh oh, didn't show!">
     </html>
    
  ##### we need to handel css file from static folder.
  
  
  
  
  
  ### Creating Models:
  
   ##### Special Methods ( oop python: __str__  ) :
      class Book():

      def__init__(self,title,author,pages):
             self.title = title
             self.author = author
             self.pages = pages

      def __str__(self): #that's the speacial method.
            return "Title: {}, Author: {}, pages: {}".format(self.title,self.author,self.pages)
            
    b = book("python","jose",200)
    print(b) # when we calling print function it's looking for string representation. we need to use what's known as a special method to do that.
    
    
  ##### models.py :
    from django.db import models
    class Topic(models.Model): #inheret from Model class
         top_name = models.CharField(max_length=264,unique=True)

         def __str__(self):
             return self.top_name


    class webpage(models.Model):
        topic = models.ForeignKey(Topic,on_delete=models.CASCADE, ) ## When the referenced object is deleted, also delete the objects that have references to it,it's rule of Foreignkey.https://www.youtube.com/watch?v=uaRZy-rflRo
        name = models.CharField(max_length=264,unique=True)
        url = models.URLField(unique=True)

        def __str__(self):
            return self.name


    class AccessRecord(models.Model):
         name = models.ForeignKey(webpage,on_delete=models.CASCADE, )
         date = models.DateField()

         def __str__(self):
             return str(self.date) # this is a date time object so it's need be cast to string. if we unsure somewhere we can use str for typecasting.



##### now, initiate this entire process into the project folder :


      

    
     
       
       
   
       
       
   
      
      
                   
           
           
   
   
    
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
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



  

  
      
 



