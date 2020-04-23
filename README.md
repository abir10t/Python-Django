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
     #### for know to the system : python manage.py makemigrations
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
              
      ##### views.py: 
    from django.contrib.auth.forms import UserCreationForm
      def signupuser(request):
        return render(request, 'todo/signupuser.html' , {'form':UserCreationForm()})
        
    ##### templates:
             {{ form }} #({{ form.as_p }} turns into paragraph)
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
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





  

  
      
 



