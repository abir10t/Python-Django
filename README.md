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
       TEMPLATE_DIR = os.path.join(BASE_DIR,"templates")
       
       TEMPLATES = [
      
        'DIRS': [ TEMPLATE_DIR,],
      
       ]
       
  
  
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
        topic = models.ForeignKey(Topic,on_delete=models.CASCADE, ) # on_delete=models.CASCADE -> When the referenced object is deleted, also delete the objects that have references to it,it's rule of Foreignkey.https://www.youtube.com/watch?v=uaRZy-rflRo
        name = models.CharField(max_length=264,unique=True)
        url = models.URLField(unique=True)

        def __str__(self):
            return self.name


    class AccessRecord(models.Model):
         name = models.ForeignKey(webpage,on_delete=models.CASCADE, )
         date = models.DateField()

         def __str__(self):
             return str(self.date) # this is a date time object so it's need be cast to string. if we unsure somewhere we can use str for typecasting.



##### now, initiate this entire process into the project folder by migrate :
       terminal  ->  python manage.py migrate
 

##### registered the changes to our application
    terminal -> python manage.py makemigrations first_app
    terminal -> python manage.py migrate
    
    
##### confirmed all of this are working by shell :
         terminal -> python manage.py shell
         from first_app.models import Topic
         print(Topic.objects.all())
         t = Topic(top_name="Social Network")
         t.save()
         print(Topic.objects.all())
         
         
 #### admin interface with the models -> admin.py :
    from django.contrib import admin
    from first_app.models import AccessRecord, Topic,webpage
    admin.site.register(AccessRecord)
    admin.site.register(Topic)
    admin.site.register(webpage)
    
 ##### Super user :
        terminal -> python manage.py createsuperuser
        
        
        
        
        
        
        
 ### Population Scripts :
     
      1. terminal -> pip install Faker
      2. create a new file in top level project folder ( populate_first_app.py -> name user define)
      3. ##### populate_first_app.py :
      
        import os
        os.environ.setdefault('DJANGO_SETTINGS_MODULE','first_project.settings') # configuring the settings for the project

        import django
        django.setup()


        ## FAKE POP SCRIPT
        import random
        from first_app.models import AccessRecord,webpage,Topic
        from faker import Faker


        fakegen = Faker() #create a instance of Faker object
        topics = ['Search','Social','Marketplace','News','Games']

        def add_topic():
             t = Topic.objects.get_or_create(top_name=random.choice(topics))[0]
             #get_or_create() -> Returns a tuple of (object, created), where object is the retrieved or created object and
             # created is a boolean specifying whether a new object was created.
             #[0] -> Any keyword arguments passed to get_or_create() — except an optional one called defaults — will be used in a get() call.
             #If an object is found, get_or_create() returns a tuple of that object and False.
             t.save()
             return t


        def populate(N=5):
           for entry in range(N):
               top = add_topic()

               #create the fake data for the entry
               fake_url = fakegen.url()
               fake_date =fakegen.date()
               fake_name = fakegen.company()

               # create the new  webpage entry
               webpg = webpage.objects.get_or_create(topic=top,url=fake_url,name=fake_name)[0]

               # create a fake accessrecord for that webpage

               acc_rec = AccessRecord.objects.get_or_create(name=webpg,date=fake_date)[0]



        if __name__ == '__main__':
            print('populating script')
            populate(20)
            print("populating complete")
            
            
            
            
            
 ### Models - Templates - views :
      
   ##### models.py:
    
    from django.db import models
    class Topic(models.Model): #inheret from Model class
             top_name = models.CharField(max_length=264,unique=True)

             def __str__(self):
                 return self.top_name


    class webpage(models.Model):
            topic = models.ForeignKey(Topic,on_delete=models.CASCADE, )# When the referenced object is deleted, also delete the objects that have references to it,

            name = models.CharField(max_length=264,unique=True)
            url = models.URLField(unique=True)

            def __str__(self):
                return self.name


    class AccessRecord(models.Model):
             name = models.ForeignKey(webpage,on_delete=models.CASCADE, )
             date = models.DateField()

             def __str__(self):
                 return str(self.date) # this is a date time object so it's need be cast to string. if we unsure somewhere we can use str for typecasting.
                 
                 
                 
   #####  views.py :
    
        from first_app.models import Topic,webpage,AccessRecord
        def index(request):
          webpage_list = AccessRecord.objects.order_by('date')
          date_dict = {'access_records':webpage_list}
          return render(request,'first_app/index.html',context=date_dict)
          
          
          
   ##### index.html
         
    <!DOCTYPE html>
    {% load staticfiles %}
    <html >
      <head>
        <meta charset="utf-8">
        <title>Django level 2</title>
        <link rel="stylesheet" href="{% static "css/mystyle.css" %}"/>
      </head>
      <body>
            <h1> wlcome to </h1>
            <h2>here are your access records:</h2>
             <div class="djangtwo">
              {% if access_records %}
               <table>
                 <thead>
                  <th>site name</th>
                  <th>date access</th>
                 </thead>
                 {% for acc in access_records %}
                 <tr>
                   <td>{{ acc.name }}</td>
                   <td>{{ acc.date }}</td>
                 </tr>
                 {% endfor %}
               </table>
              {% else %}
                 <p>not found </p>
              {% endif  %}
            </div>
    </html>

                
         
### create form and print the given input :

   ##### views.py :
    from django.shortcuts import render
    from django.http import HttpResponse
    from . import forms

    def index(request):
        return render(request,'basicapp/index.html' )

    def form_name_view(request):
        form = forms.FormName() #that will create the form page
        if request.method =='POST':
            form =forms.FormName(request.POST)

        if form.is_valid():
            print("validation success!")
            print("Name: "+form.cleaned_data['name'])
            print("Email: "+form.cleaned_data['email'])
            print("Text: "+form.cleaned_data['text'])
    return render(request,'basicapp/form_page.html',{'form':form})
    
    
   ##### form_page.html:
    <!DOCTYPE html>
    <html lang="en" dir="ltr">
      <head>
        <meta charset="utf-8">
        <title>Forms</title>
      </head>
      
      <body>
        <div class ="container">
          <form method="POST">

             {{ form.as_p }}
             {% csrf_token %}

             <input type ="submit" class="btn btn-primary" value="submit">

            </form>

      </body>
    </html>


 ### form validation :
   #### custom valitadors :
   #####  forms.py:
    
      from django import forms
    

    class FormName(forms.Form):
        name = forms.CharField()
        email = forms.EmailField()
        text = forms.CharField(widget=forms.Textarea)
        botcatcher = forms.CharField(required=False,widget=forms.HiddenInput,validators=[validators.MaxLengthValidator(0)])
        
        
  #### django provieds validator :
  ##### forms.py : 
    from django.core import validators
    class FormName(forms.Form):
      name = forms.CharField()
      email = forms.EmailField()
      text = forms.CharField(widget=forms.Textarea)
      botcatcher = forms.CharField(required=False,widget=forms.HiddenInput,validators=[validators.MaxLengthValidator(0)])
      
    
   #### custom clean method for our particular class method :
   #### forms.py:
     from django import forms
     from django.core import validators


    def check_for_z(value):
        if value[0].lower() !='z':
            raise forms.ValidationError("name needs to start with z")

    class FormName(forms.Form):
        name = forms.CharField(validators=[check_for_z])
        email = forms.EmailField()
        text = forms.CharField(widget=forms.Textarea)
        botcatcher = forms.CharField(required=False,widget=forms.HiddenInput,validators=[validators.MaxLengthValidator(0)])
        
        
   #### one method that clean the all field :
   ##### forms.py :
    
    from django import forms
    from django.core import validators

    class FormName(forms.Form):
        name = forms.CharField()
        email = forms.EmailField()
        verify_email = forms.EmailField(label="Enter your email again :")
        text = forms.CharField(widget=forms.Textarea)

        def clean(self):
            all_clean_data = super().clean() # this is goiong to return all our clean data
            email = all_clean_data['email']
            vmail = all_clean_data['verify_email']

            if email != vmail :
                 raise forms.ValidationError("Make sure email match")
     


  ### create model form and save them into database :
   #### models.py :
   
     from django.db import models
     class UserF(models.Model):
        first_name = models.CharField(max_length=128)
        last_name = models.CharField(max_length=128)
        email = models.EmailField(max_length=265, unique=True)
        
   #### admin.py :
    from django.contrib import admin
    from first_app.models import UserF
    admin.site.register(UserF)
    
   #### forms.py :
         from django import forms
         from first_app.models import UserF

    class NewUserForm(forms.ModelForm): #here we actually want to connect with model  so we use forms.ModelForm
             class Meta():
                 model = UserF
                 fields ='__all__'
                 
   #### views.py :
   
    from django.shortcuts import render
    from first_app.forms import NewUserForm

    def index(request):
        return render(request,'basicapp/index.html' )

    def users(request):
        form = NewUserForm()
        
        if request.method == "POST":
            form =NewUserForm(request.POST)

            if form.is_valid():
                form.save(commit=True)
                return index(request)
            else :
                print("form invalid")
        return render(request,'basicapp/users.html',{'form':form})
        
        
   #### idex.html :
     <!DOCTYPE html>
      <html >
        <head>
          <meta charset="utf-8">
          <title>Home</title>
        </head>
        <body>
          <h1>welcome</h1>
          <h2>go to /users to sign up!</h2>
        </body>
      </html>
      
   #### users.html :
    <!DOCTYPE html>
    <html lang="en" dir="ltr">
      <head>
        <meta charset="utf-8">
        <title>Forms</title>

      </head>
      <body>

    <h1>please sign up here !</h1>
    <form method="post">
    
      {{ form.as_p }}
      {% csrf_token %}
      <input type="submit" class='btn btn-primary' value="Submit">
      
    </body>
     </html>




### Relative urls :
  #### urls.py : 
       app_name = 'basicapp'

       urlpatterns=[

           path('relative/',views.relative,name='relative'),
           path('other/',views.other,name='other'),
     ]
     
  #### views.py :
    def other(request):
    return render(request,'basicapp/other.html')

    def relative(request):
        return render(request,'basicapp/relative_url_templates.html')
        
        
 #### relative_url_templates.html :
      <!DOCTYPE html>
    <html lang="en" dir="ltr">
      <head>
        <meta charset="utf-8">
        <title></title>
      </head>
      <body>
          <h1>welcome to RELATIVEU</h1>
          <a href="{% url 'basicapp:other' %}">The other page</a>
      </body>
    </html>
    
    
 #### other.html :
     <!DOCTYPE html>
    <html lang="en" dir="ltr">
      <head>
        <meta charset="utf-8">
        <title></title>
      </head>
      <body>
      <h1>welcome to other</h1>
      </body>
    </html>









    
        
    
        

      


    



     
      

    
     
       
       
   
       
       
   
      
      
                   
           
           
   
   
    
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
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



  

  
      
 



