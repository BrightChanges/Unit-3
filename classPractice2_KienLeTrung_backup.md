## Class Practice
### 1.Code and create the UML class diagram for:
  -Create a Python class Person with attributes: name and age of type string.
  -Create a display() method that displays the name and age of an object created via the Person class.
  -Create a child class Student which inherits from the Person class and which also has a section attribute.
  -Create a method displayStudent() that displays the name, age and section of an object created via the Student class.
  -Create a student object via an instantiation on the Student class and then test the displayStudent method.
Image of the UML class for class Person and class Student with the problem's methods in them:
![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4525.jpg)

Codes:
```.py

class Person():

  def __init__(self,name,age):
    self.name = name
    self.age = age

  def get_name(self):
    print(self.name)

  def get_age(self):
    print(self.age)

class Student(Person):

  def __init__(self,name,age,grade,gender):
    Person.__init__(self,name,age)
    self.grade=grade
    self.gender=gender

  def get_student(self):
     print(f"Student {self.name} is {self.age} years old, grade {self.grade}, and {self.gender}")

student1=Student("Jack",16,11,"male")
student1.get_student()

```

### 2. Write a Python class to convert an integer to a roman numeral.

```.py

class Int_Roman_num_converter():

    def int_to_Roman(self, num):
        val = [
            1000, 900, 500, 400,
            100, 90, 50, 40,
            10, 9, 5, 4,
            1
            ]
        syb = [
            "M", "CM", "D", "CD",
            "C", "XC", "L", "XL",
            "X", "IX", "V", "IV",
            "I"
            ]
        roman_num = ''
        i = 0
        while  num > 0:
            for _ in range(num // val[i]):
                roman_num += syb[i]
                num -= val[i]
            i += 1
        return roman_num


print(Int_Roman_num_converter().int_to_Roman(1))
print(Int_Roman_num_converter().int_to_Roman(4000))

```

### 3.Describe with words the program below.
Create the UML class diagram for the program below.

```.py

class BankAccount:
    def __init__(self):
        self.balance = 0

    def withdraw(self, amount):
        self.balance -= amount
        return self.balance

    def deposit(self, amount):
        self.balance += amount
        return self.balance

class MinimumBalanceAccount(BankAccount):
    def __init__(self, minimum_balance):
        BankAccount.__init__(self)
        self.minimum_balance = minimum_balance

    def withdraw(self, amount):
        if self.balance - amount < self.minimum_balance:
            print('Sorry, minimum balance must be maintained.')
        else:
            BankAccount.withdraw(self, amount)

```
We have two classes BankAccount and MinimumBalanceAccount, which is a class that is inherited from the class BankAccount. To initialize an object for the class BankAccount, we don't need to input anything. With 1 input, we can use the 2 methods of class BankAccount: withdraw() and deposit(). To initialize an object for the class MinimumBalanceAccount, we need to input 1 variable: minimum_balance. To use its 1 method withdraw(), we also need to input 1 variable: amount.

Below is the UML class diagram for the codes:
![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4526.jpg)
