# Python Best Practices
<!-- TOC -->
## Table of Contents
1. [Variable Naming](#variable-naming)
2. [Function Naming](#function-naming)
3. [Avoiding Magic Numbers](#avoiding-magin-numbers)
4. [List Comprehensions vs Loops](#list-comprehensions-vs-loops)
5. [Error Handling](#error-handling)
6. [Code Duplication](#code-duplication)
7. [Using Python’s Built-in Functions](#using-pythons-built-in-functions)
8. [Avoiding Mutable Default Arguments](#avoiding-mutable-default-arguments)
9. [Use of with for File Handling](#use-of-with-for-file-handling)
10. [Use of Itertools for Efficiency](#use-of-itertools-for-efficiency)
11. [Using is vs == Correctly](#using-is-vs--correctly)
12. [Properly Closing Database Connections](#properly-closing-database-connections)
13. [Using enumerate() Instead of Manual Indexing](#using-enumerate-instead-of-manual-indexing)
14. [Using defaultdict Instead of Checking for Keys Manually](#using-defaultdict-instead-of-checking-for-keys-manually)
15. [Using get() for Dictionary Access](#using-get-for-dictionary-access)
11. [Best Practices accepted by community](#best-practices-accepted-by-community)
12. [Python Naming Conventions](#python-naming-conventions)
13. [Interview Questions](#interview-questions)
    1. [Beginner](#beginner)
    2. [Intermediate](#intermediate)
    3. [Expert](#expert)
    4. [General](#general)
    5. [Miscellaneous Questions](#miscellaneous-questions)
<!-- /TOC -->

## Variable Naming
### Bad
```python
a = 10
b = 20
c = a + b
```

### Good
```python
num_apples = 10
num_oranges = 20
total_fruits = num_apples + num_oranges
```
The **Good** approach uses descriptive variable names that explain the purpose of the variable. It makes the code more readable and easier to maintain.

---

## Function Naming
### Bad
```python
def a(x, y):
    return x + y
```

### Good 
```python
def add_numbers(num1, num2):
    return num1 + num2
```
The **Good** approach has the function name add_numbers clearly indicates its purpose, and num1 and num2 make it clear what the function expects.

---

## Avoiding Magic Numbers
### Bad
```python
price = 10.5 * 1.2
```

### Good
```python
TAX_RATE = 1.2
price = 10.5 * TAX_RATE
```
The **Good** approach assigns 1.2 to a constant with a meaningful name, the code becomes self-explanatory. It’s now clear that TAX_RATE represents a tax multiplier.

---

## List Comprehensions vs Loops
### Bad
```python
squared_numbers = []
for num in range(10):
    squared_numbers.append(num * num)
```

### Good
```python
squared_numbers = [num * num for num in range(10)]
```
The **Good** approach ensures the List comprehensions are concise, more readable, and often faster than using for loops for simple tasks like creating a list.

---

## Error Handling
### Bad
```python
try:
    result = 10 / 0
except Exception as e:
    print("Something went wrong:", e)
```

### Good 
```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print("Error: Division by zero is not allowed")
```
The **Good** approach is better to catch specific exceptions, such as ZeroDivisionError in this case, so that the error is clear and it’s easier to handle appropriately.

---

## Code Duplication
### Bad
```python
def calculate_area_of_rectangle(length, width):
    return length * width

def calculate_area_of_square(side):
    return side * side
```

### Good 
```python
def calculate_area(length, width=None):
    if width is None:
        width = length  # For square
    return length * width
```
The **Good** approach reduces code duplication by allowing the same function to calculate the area of both a square and a rectangle, based on whether the width is provided.

---

## Using Python’s Built-in Functions
### Bad
```python
squares = []
for i in range(10):
    squares.append(i * i)
```

### Good
```python
squares = [i ** 2 for i in range(10)]
```
The **Good** approach is a simple task that can be done more efficiently with a built-in function. 

---

## Avoiding Mutable Default Arguments
### Bad
```python
def append_item(item, list=[]):
    list.append(item)
    return list
```

### Good
```python
def append_item(item, list=None):
    if list is None:
        list = []
    list.append(item)
    return list
```
The **Good** approach using the default value is set to None, and if no list is provided, a new list is created. This prevents the default list from being shared across multiple calls.

---

## Use of with for File Handling
### Bad
```python
file = open("file.txt", "r")
content = file.read()
file.close()
```

### Good
```python
with open("file.txt", "r") as file:
    content = file.read()
```
The **Good** approach uses with ensures that the file is properly closed, even if an error occurs during file reading. This is cleaner and more robust.

---

## Use of Itertools for Efficiency
### Bad
```python
def get_unique_items(lst):
    unique_items = []
    for item in lst:
        if item not in unique_items:
            unique_items.append(item)
    return unique_items
```

### Good
```python
from itertools import tee

def get_unique_items(lst):
    return list(set(lst))
```
The **Good** approach uses set automatically removes duplicates in a more efficient way. This is faster and simpler.

--- 
## Using is vs == Correctly
### Bad
```python
a = 256
b = 256
print(a is b)  # True (small integers are cached)

a = 257
b = 257
print(a is b)  # False (different objects)
```

### Good
```python
a = 257
b = 257
print(a == b)  # True (correct way to compare values)
```
The **Bad** approach uses is to compare values, which checks object identity instead of equality. This can lead to unexpected results when comparing integers beyond the caching range or other objects like lists and strings.

---
## Properly Closing Database Connections
### Bad
```python
import sqlite3

conn = sqlite3.connect('example.db')
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
data = cursor.fetchall()
conn.close()  # Can fail if an exception occurs
```

### Good 
```python
import sqlite3

with sqlite3.connect('example.db') as conn:
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM users")
    data = cursor.fetchall()
```

Using with ensures that the connection is closed properly even if an exception occurs, making the code more robust.

---
## Using enumerate() Instead of Manual Indexing
### Bad 
```python
names = ["Alice", "Bob", "Charlie"]
for i in range(len(names)):
    print(i, names[i])
```

### Good 
```python
names = ["Alice", "Bob", "Charlie"]
for i, name in enumerate(names):
    print(i, name)
```
The **Good** approach is more readable and avoids manually handling indices.

---
## Using defaultdict Instead of Checking for Keys Manually
### Bad
```python
word_count = {}
words = ["apple", "banana", "apple", "orange"]

for word in words:
    if word in word_count:
        word_count[word] += 1
    else:
        word_count[word] = 1
```

### Good 
```python
from collections import defaultdict

word_count = defaultdict(int)
words = ["apple", "banana", "apple", "orange"]

for word in words:
    word_count[word] += 1
```
The **Good** approach eliminates the need to check for key existence manually and provides a default value.

---
## Using get() for Dictionary Access
### Bad 
```python
config = {"timeout": 30}
timeout = config["timeout"] if "timeout" in config else 60
```

### Good 
```python
config = {"timeout": 30}
timeout = config.get("timeout", 60)
```
The **Good** approach is cleaner and more idiomatic.

---
## Best Practices accepted by community
| Task | Standard Tools | 3rd Party Tools |
|---|---|---|
| API Authentication | `Flask-Login`, `Django Authentication`, `Authlib` | `Flask-JWT-Extended`, `PyJWT`, `Auth0 `|
| API Client | `requests` | `httpx`, `pycurl` |
| API Documentation | `Swagger UI with Flask-RESTPlus`, `Django REST Framework (DRF) Auto Documentation` | `Postman collections`, `FastAPI (with automatic OpenAPI docs)` |
| Authentication (OAuth) | `OAuthLib`, `requests-oauthlib` | `Authlib`, `Flask-OAuthlib` |
| Background Jobs | `Celery` | `Authlib`, `Flask-OAuthlib` |
| Caching | `functools.lru_cache`, `memcached` | `Flask-Caching`, `Django-Redis`, `cachetools` |
| CLI Application | `argparse` | `Click`, `fire`, `plac` |
| Data Processing | `pandas`, `numpy` | `Dask`, `Vaex` |
| Data Serialization | `json`, `pickle` | `Marshmallow`, `Pydantic` |
| Database Migrations | `Alembic (SQLAlchemy)` | `Flyway`, `Django South` |
| Database ORM | `SQLAlchemy`, `Django ORM` | `Peewee`, `Tortoise ORM` |
| Environment Configuration | `os.environ`, `configparser` | `python-dotenv`, `dynaconf` |
| File Storage | `Django Storages`, `Python's shutil` | `Boto3 (AWS S3)`, `pyFilesystem2` |
| File Uploads | `Django built-in file handling`, `Flask File upload` | `django-storages`, `Flask-Uploads`, `python-dotenv` |
| Form Handling | `WTForms` | `django-forms`, `Flask-WTF`, `FormEncode` |
| GraphQL | `Graphene` | `Ariadne`, `Strawberry GraphQL` |
| Image Processing | `Pillow` | `OpenCV`, `imageio` |
| Logging | `logging module (built-in)` | `Loguru`, `structlog` |
| Real-time Communication | `Django Channels`, `Flask-SocketIO` | `Socket.IO`, `websockets` |
| Request Handling | `Flask`, `Django` | `FastAPI`, `Bottle` |
| Security | `ssl`, `hashlib`, `hmac`, `cryptography` | `Flask-Security`, `Django-Defender`, `PyCryptodome` |
| Task Queues | `Celery` | `RQ` , `Huey` , `Dramatiq`  |
| Testing | `unittest`, `pytest` , `nose2`  | `tox` , `pytest-django`  |


---

## Python Naming Conventions
| What | How | Good | Bad |
|---|---|---|---|
| Class Attributes | Lowercase words separated by underscores (snake_case) | `user_name`, `car_model`, `account_balance` | `UserName`, `CarModel`, `AccountBalance` |
| Classes | Capitalized words (PascalCase) | `UserProfile`, `CarModel`, `DataProcessor` | `userprofile`, `car_model`, `dataprocessor` |
| Constants | Uppercase words separated by underscores (UPPER_SNAKE_CASE) | `MAX_RETRIES`, `PI`, `DEFAULT_TIMEOUT` | `MaxRetries`, `pi`, `defaultTimeout` |
| Enum Names | Capitalized words separated by underscores (UPPER_SNAKE_CASE) | `DAY_ONE`, `STATUS_OK`, `ERROR_CODE` | `DayOne`, `StatusOk`, `ErrorCode` |
| Exceptions | Class name in PascalCase with Error suffix | `InvalidDataError`, `AuthenticationError` | `invaliddataerror`, `authenticationerror` |
| File Uploads | Lowercase words separated by underscores (snake_case) | `user_upload`, `image_file`, `uploaded_files` | `UserUpload`, `ImageFile`, `UploadedFiles` |
| Function Arguments | Descriptive names, in lowercase and snake_case | `user_id`, `start_time`, `end_time` | `id`, `start`, `end` |
| Function/Methods | Lowercase words separated by underscores (snake_case) | `get_user_data()`, `calculate_average()` | `GetUserData()`, `calculateAverage()` |
| Global Variables | Use all lowercase with underscores (snake_case) | `global_user`, `global_config` | `GlobalUser`, `GlobalConfig` |
| Instance Methods | Lowercase words separated by underscores (snake_case) | `get_user_info()`, `process_data()` | `GetUserInfo()`, `ProcessData()` |
| Modules | Lowercase words separated by underscores (snake_case) | `data_processor.py`, `user_manager.py` | `DataProcessor.py`, `UserManager.py` |
| Packages | Lowercase words, no underscores | `mypackage`, `utils`, `dataprocessing` | `MyPackage`, `DataProcessing` |
| Private Variables | Leading underscore for "private" variables | `_user_data`, `_car_model`, `_total_count` | `user_data`, `car_model`, `total_count` |
| Properties | Lowercase words separated by underscores (snake_case) | `user_age`, `car_model`, `account_balance` | `UserAge`, `CarModel`, `AccountBalance` |
| Test Functions | Start with test_ and lowercase words separated by underscores (snake_case) | `test_user_creation()`, `test_api_response()` | `TestUserCreation()`, `TestAPIResponse()` |
| Variables | Lowercase words separated by underscores (snake_case) | `user_age`, `car_model`, `total_count` | `UserAge`, `CarModel`, `TotalCount` |

---

## Interview Questions
### Beginner
- What are Python’s key features?
    - Interpreted: Python is an interpreted language, which means the code is executed line by line.
	- Dynamically Typed: You don’t need to declare the type of a variable explicitly.
	- Easy to Learn: Python has a simple and readable syntax.
	- Object-Oriented: Python supports object-oriented programming principles.
    - Cross-Platform: Python is available on various platforms, such as Windows, Linux, and macOS.
- What is the difference between list and tuple in Python?
    - List: A list is mutable, meaning its elements can be changed after creation. It uses square brackets [] (e.g., my_list = [1, 2, 3]).
	- Tuple: A tuple is immutable, meaning its elements cannot be modified after creation. It uses parentheses () (e.g., my_tuple = (1, 2, 3)).
- What are Python’s data types?
    - `int` (integer)
	- `float` (floating-point number)
	- `str` (string)
	- `list` (list of elements)
	- `tuple` (immutable list)
	- `set` (unordered collection of unique elements)
	- `dict` (dictionary, key-value pairs)
	- `bool` (boolean: True or False)
	- `None` (represents absence of value)
- What is the purpose of the self keyword in Python?
    - `self` is used to refer to the instance of the current object within a class. It is a reference to the current object and allows access to its attributes and methods. It’s the first parameter in instance methods of a class.
### Intermediate
- What is a lambda function in Python?
    - A lambda function is a small anonymous function defined using the lambda keyword. It can take any number of arguments but can only have one expression. It’s often used for short, throwaway functions.
- What is list comprehension in Python?
    - List comprehension is a concise way to create lists in Python. It allows you to generate a new list by applying an expression to each item in an existing list or iterable.
- Explain the difference between `deepcopy()` and `copy()` in Python.
    - `copy()`: Creates a shallow copy of an object. For mutable objects, changes made to nested elements in the copied object will affect the original object.
	- `deepcopy()`: Creates a deep copy of an object, meaning it recursively copies all nested objects. Changes to the copied object will not affect the original object.
- What is a decorator in Python?
    - A decorator is a function that modifies the behavior of another function or method. It is applied using the @decorator_name syntax.
### Expert
- What are Python’s memory management strategies?
    - Memory Manager: Python has an automatic memory management system, which includes the Python Memory Manager, Garbage Collection, and Reference Counting.
	- Reference Counting: Python keeps track of the number of references to an object, and when the reference count drops to zero, the object is deleted.
	- Garbage Collection: Python automatically removes unused objects through a process known as garbage collection using the cyclic garbage collector.
- What is the Global Interpreter Lock (GIL) in Python?
    - The Global Interpreter Lock (GIL) is a mutex that protects access to Python objects, ensuring that only one thread can execute Python bytecode at a time. This can limit the performance of multi-threaded Python programs, especially on multi-core machines.
- Explain Python’s yield keyword and how it works.
    - The `yield` keyword is used in a function to turn it into a generator. A generator is a special type of iterator that yields values one at a time using the `yield` keyword instead of returning them all at once.
- What are metaclasses in Python?
    - A metaclass is a class of a class. It defines the behavior of classes themselves, controlling the creation and customization of class objects. You can use metaclasses to modify class instantiation, inheritance, or method resolution order.
### General
- What is the difference between `is` and `==` in Python?
    - `is`: Checks if two variables point to the same object in memory (identity comparison).
	- `==`: Checks if the values of two variables are equal (value comparison).
- What is the purpose of `__init__()` in Python classes?
    - `__init__()` is the constructor method in Python classes. It is automatically called when an instance of the class is created and is used to initialize the object’s attributes.
- What is the purpose of the pass statement in Python?
    - The pass statement is a placeholder that does nothing. It’s used when a statement is syntactically required but you don’t want to perform any action.
### Miscellaneous Questions
- What are Python’s built-in data structures?
    - `List`: Ordered collection of items, can hold different types of data.
	- `Tuple`: Immutable sequence of elements.
	- `Set`: Unordered collection of unique items.
	- `Dictionary`: Collection of key-value pairs.
- How do you handle exceptions in Python?
    - Python uses `try`, `except`, `else`, and `finally` blocks to handle exceptions. The try block is used to wrap code that may raise an exception, and the except block catches and handles the exception.
- What is the difference between `range()` and `xrange()` in Python 2.x?
    - 	In Python 2.x, `range()` returns a list, while `xrange()` returns an iterator (generating values one at a time). In Python 3.x, range() behaves like `xrange()` from Python 2.x and returns an iterator, so `xrange()` no longer exists.
- What is Python’s with statement?
    - The `with` statement is used to wrap the execution of a block of code within methods defined by a context manager. It ensures that resources are properly managed (like file handling or network connections) and automatically cleans up after execution.
---
