# terminal :
  
     1. to know current location : Pwd
     2. what file are loccated in current position : ls
     3.for go to back_up : cd ..
  ### for setup python 3.7 in ubuntu
     <a href="https://phoenixnap.com/kb/how-to-install-python-3-ubuntu"></a>
     
     
     
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

  

  
      
 



