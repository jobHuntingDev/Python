# Notes on classes and objects

## Basic class defining

A basic person class with a class variable, methods and a constructor.

```python
class Person:
    amount = 0					#Class variable

    def __init__(self,name,surname,age):	#Constructor
        self.name = name
        self.surname = surname
        self.age = age
	Person.amount += 1			#Adding to class variable

    def Great(self,name):			#Class method
        print("Hello {}".format(name))
        print("my name is {}".format(self.name))

    def __str__(self):				#Response on print
        return f"Name: {self.name}\nSurname: {self.surname}\nAge: {self.age}"

    def __del__(self):				#Response on delete
	Person.amount -= 1			#Subtracting from class varible
        print("object deleted")
```

## Inheritance

Creating a Worker class that inherits from the Person class.

```python
class Worker(Person):

    def __init__(self,name,surname,age,salary):
        super(Worker,self).__init__(name,surname,age)
        self.salary = salary

    def __str__(self):
        text = super(Worker,self).__str__()
        text += f"\nSalary: {self.salary}"
        return text

    def Calculate_Annual_Salary(self):
        return self.salary * 12
```

## Operator Overloading

A way to define what happens when arithmetic is performed with objects.In this example we'll be creating vector objects and defining vector arithmetic.

```pyhton
class Vector:

    def __init__(self,x,y):
        self.X_value = x
        self.Y_value = y

    def __add__(self,other):
        return Vector(self.X_value + other.X_value,self.Y_value + other.Y_value)

    def __str__(self):
        return f"x: {self.X_value} y: {self.Y_value}"

v1 = Vector(3,5)
v2 = Vector(5,3)
v3 = v1 + v2

print(v1)
print(v2)
print(v3)
```

output:
```
x: 3 y: 5
x: 5 y: 3
x: 8 y: 8
```
