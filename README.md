# OOP in python

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
# Queues

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
# XML Processing

## The XML file

```xml
<group>
	<person id="1">
		<name>Mike Smith</name>
		<age>34</age>
		<weight>90</weight>
		<height>175</height>
	</person>
	<person id="2">
		<name>Anna Smith</name>
		<age>54</age>
		<weight>91</weight>
		<height>188</height>
	</person>
	<person id="3">
		<name>Bob Johnson</name>
		<age>25</age>
		<weight>76</weight>
		<height>190</height>
	</person>
</group>
```

## SAX

Useful for large xml files or little RAM memory.

```python
import xml.sax

class GroupHandler(xml.sax.ContentHandler):
	
	def startElement(self, name, attrs):
		self.current = name 
		if self.current == "person":
			print("---Person---")
			print("ID: {}".format(attrs["id"]))

	def character(self, content):
		if self.current == "name":
			self.name = content
		elif self.current == "age":
			self.age = content
		elif self.current == "weight":
			self.weight = content
		elif self.current == "height":
			self.height = content
		else:
			print("no match")

	def endElement(self, name)
		if self.current == "name":
			print("Name: {}".format(self.name))
		elif self.current == "age":
			print("Age: {}".format(self.age))
		elif self.current == "weight":
			print("Weight: {}".format(self.weight))
		elif self.current == "height":
			print("height: {}".format(self.height))
		else:
			print("no match")
			
		self.current = ""

handler = GroupHandler()
parser = xml.sax.make_parser()
parser.setContentHandler(handler)
parser.parse("data.xml")
```

## DOM

Read from files

```python
import xml.dom.minidom

domtree = xml.dom.minidom.parse("data.xml")

# refering to <group> in .xml file
group = domtree.documentElement

persons = group.getElementByTagname("person")

for person in persons:
	print("---Person---")
	if person.hasAttribute("id"):
		print("ID: {}".format(person.getAttribute("id")))
	
	print("Name: {}".format(person.getElementsByTagName("name")[0].childNodes[0].data))
	print("Age: {}".format(person.getElementsByTagName("age")[0].childNodes[0].data))
	print("Weight: {}".format(person.getElementsByTagName("weight")[0].childNodes[0].data))
	print("height: {}".format(person.getElementsByTagName("height")[0].childNodes[0].data))
```

Edit xml file

```python
import xml.dom.minidom

domtree = xml.dom.minidom.parse("data.xml")

# refering to <group> in .xml file
group = domtree.documentElement

persons = group.getElementByTagname("person")

persons[2].getElementsByTagName("name")[0].childNodes[0].nodeValue = "Changed name"
persons[0].setAttribute("id", "100")
persons[3].getElementsByTagname("age")[0].childNodes[0].nodeValue = "-10"

# Save changes to file
domtree.writexml(open("data.xml", "w"))
```

Create and add new elements

```python
import xml.dom.minidom

domtree = xml.dom.minidom.parse("data.xml")

newperson = domtree.createElement("person")
newperson.setAttribute("id", "4")

name = domtree.createElement("name")
name.appendChild(domtree.createTextNode("Paul Green"))

age = domtree.createElement("age")
age.appendChild(domtree.createTextNode("19"))

weight = domtree.createElement("weight")
weight.appendChild(domtree.createTextNode("80"))

height = domtree.createElement("height")
height.appendChild(domtree.createTextNode("176 cm"))

newperson.appendChild(name)
newperson.appendChild(weight)
newperson.appendChild(age)
newperson.appendChild(height)

group.appendChild(newperson)

domtree.writexml(open("data.xml", 'w'))
```
# Logging

```python
import logging

# Change security level to DEBUG
logging.basicConfig(level=logging.DEBUG)

# Change security leve to CRITICAL
logging.basicConfig(level=logging.CRITICAL)

# Change security level to INFO
logging.basicConfig(level=loggin.INFO)

logging.info("You have got 20 mails in your inbox!")
logging.warning("You're running low on space: 28.8 GB of 29.2 in use")
logging.critical("All components failed!")
```

Creating custom logger

```python
import logging

# Create
logger = logging.getLogger("Youtube Channel Logger")

# Create error message
logger.info("The logger was just created")
logger.critical("Your YouTube channel was deleted"
logger.log(logging.ERROR, "An error occured")
```

Saving log message to a file

```python
import logging

# Create
logger = logging.getLogger("Youtube Channel Logger")
logger.setLevel(logging.DEBUG)

handler = logging.FileHandler("mylog.log")
handler = setLevel(logging.INFO)


# Create Format
formatter = logging.Formatter("%(levelname)s - %(asctime)s: %(message)s")
handler.setFormatter(formatter)

logger.addHandler(handler)

logger.debug("This is a debug message!")
logger.info("This is important information!")
```

# Asyncio 

Writing programs that do something while waiting on tasks

```python
import asyncio

async def main():
	# task: what you do when waiting
	task = asyncio.create_task(other_function())
	print("A")
	await asyncio.sleep(1)
	print("B")
	# make sure task is completed
	await task

async def other_function():
	print("1")
	await asyncio.sleep(2)
	print("2")

asyncio.run(main())
```

Dealing with return values

```python
import asyncio

async def main():
	task = asyncio.create_task(other_function())
	print("A")
	await asyncio.sleep(1)
	print("B")
	return_value = await task
	print(f"Return value was {return_value}")

async def other_function():
	print("1")
	await asyncio.sleep(2)
	print("2")
	return 10

asyncio.run(main())
```
