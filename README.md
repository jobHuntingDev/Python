# Python

## OOP in python

### Basic class definition

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

### Inheritance

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

### Operator Overloading

A way to define what happens when arithmetic is performed with objects.In this example we'll be creating vector objects and defining vector arithmetic.

```python
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
## Queues

Where we'd use a que: one list is  being operated on by multiple threads so to prevent values being used more than one we use a que

**First in first out**

```python
import queue

q = queue.Queue()

number = [10,20,30,40,50,60,70]

for number in numbers:
	q.put(number)

print(q.get())	# get first element
print(q.get())	# get next element
```

**Last in first out**

```python
import queue

q = queue.LifoQueue()

numbers = [1,2,3,4,5,6,7]
for x in numbers:
	q.put(x)

print(q.get())
```

**Priority Que**

Specifying a custom order values leave the que using priority.

```python
import queue

q = queue.PriorityQueue()

q.put((2, "Hello World!"))
q.put((11,99))
q.put((5, 7.5))
q.put((1, True))

while not q.empty():
	print(q.get())
```

# Sockets

socket: The endpoint of communication channel.

**Server**

```python
import socket

# TCP socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# s.bind((IP address, port))
s.bind(('127.0.0.1', 8000))

# listen for connection
s.listen()

# access loop that accepts connections
while True:
	client, address = s.accept()	
	print("Connected to {}".format(address))
	client.send("200 you're connected".encode())
	client.close()
```

**Client**

```python
import socket

# TCP socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# connect to server
s.connect(('127.0.0.1', 8000))

# recieve 1024 bytes
message = s.recv(1024)

# close connection
s.close()

print(message.decode())
```
# Database connection

```python
import sqlite3

connection = sqlite3.connect('Test.db')

cursor = connection.cursor()

# Writing SQL statements

cursor.execute("""
	 CREATE TABLE persons (
		first_name TEXT,
		last_name TEXT,
		age INTEGER
	)
""")

# Retrive data

cursor.execute("""
	SELECT * FROM persons
	WHERE last_name = 'smith'
""")

rows = cursor.fetchall()
print(rows)
one_row = cusor.fetchone()
print(one_row)

connection.commit()
connection.close()
```

# Recursion 

```python
def factorial(n):
	if n < 1:
		return 1
	else:
		number = n * factorial(n-1)
		return number

print(factorial(7))
```
Notes on the python programing language by L.G Mngadi
