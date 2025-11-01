# Python Data Types - ডেটা টাইপ

## 📚 Data Type কী?

Data Type নির্ধারণ করে কোন ভ্যারিয়েবল কী ধরনের ডেটা ধারণ করবে এবং তার উপর কী কী operation করা যাবে।

Python-এ ভ্যারিয়েবল ডিক্লেয়ার করার সময় টাইপ বলে দিতে হয় না (Dynamically Typed), Python নিজেই বুঝে নেয়।

---

## 🗂️ Python-এর প্রধান Data Types

### 1️⃣ **Numeric Types (সংখ্যা)**
- `int` - পূর্ণ সংখ্যা
- `float` - দশমিক সংখ্যা
- `complex` - জটিল সংখ্যা

### 2️⃣ **String (স্ট্রিং)**
- `str` - টেক্সট/শব্দ

### 3️⃣ **Boolean (বুলিয়ান)**
- `bool` - True/False

### 4️⃣ **Sequence Types (ক্রমিক)**
- `list` - পরিবর্তনযোগ্য তালিকা
- `tuple` - অপরিবর্তনযোগ্য তালিকা
- `range` - সংখ্যার পরিসীমা

### 5️⃣ **Mapping Type**
- `dict` - Key-Value জোড়া

### 6️⃣ **Set Types**
- `set` - অনন্য উপাদানের সেট
- `frozenset` - অপরিবর্তনযোগ্য সেট

### 7️⃣ **None Type**
- `None` - কোন মান নেই

---

## 💻 বিস্তারিত উদাহরণ

### 1. Integer (int) - পূর্ণ সংখ্যা

```python
# Integer examples
age = 25
year = 2024
negative = -10
big_num = 1000000

print(age)           # Output: 25
print(type(age))     # Output: <class 'int'>

# Operations
a = 10
b = 3
print(a + b)         # 13 (যোগ)
print(a - b)         # 7 (বিয়োগ)
print(a * b)         # 30 (গুণ)
print(a // b)        # 3 (ভাগফল)
print(a % b)         # 1 (ভাগশেষ)
print(a ** b)        # 1000 (ঘাত)
```

---

### 2. Float - দশমিক সংখ্যা

```python
# Float examples
price = 99.99
pi = 3.14159
temperature = -5.5

print(price)         # Output: 99.99
print(type(price))   # Output: <class 'float'>

# Operations
x = 10.5
y = 2.5
print(x + y)         # 13.0
print(x / y)         # 4.2
print(x * y)         # 26.25

# Int to Float conversion
num = 10
float_num = float(num)
print(float_num)     # 10.0
```

---

### 3. String (str) - টেক্সট

```python
# String examples
name = "Sahin"
city = 'Dhaka'
message = """This is a
multi-line string"""

print(name)          # Output: Sahin
print(type(name))    # Output: <class 'str'>

# String operations
first_name = "Md"
last_name = "Rahman"
full_name = first_name + " " + last_name
print(full_name)     # Md Rahman

# String methods
text = "python programming"
print(text.upper())          # PYTHON PROGRAMMING
print(text.capitalize())     # Python programming
print(text.title())          # Python Programming
print(len(text))             # 18 (দৈর্ঘ্য)

# String indexing
word = "Hello"
print(word[0])       # H (প্রথম অক্ষর)
print(word[-1])      # o (শেষ অক্ষর)
print(word[1:4])     # ell (slicing)
```

---

### 4. Boolean (bool) - সত্য/মিথ্যা

```python
# Boolean examples
is_student = True
is_adult = False

print(is_student)        # Output: True
print(type(is_student))  # Output: <class 'bool'>

# Boolean from comparisons
x = 5
print(x > 3)            # True
print(x < 2)            # False
print(x == 5)           # True

# Boolean operations
a = True
b = False
print(a and b)          # False (উভয় True হতে হবে)
print(a or b)           # True (একটি True হলেই হবে)
print(not a)            # False (বিপরীত)
```

---

### 5. List - তালিকা (পরিবর্তনযোগ্য)

```python
# List examples
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]

print(fruits)            # ['apple', 'banana', 'cherry']
print(type(fruits))      # <class 'list'>

# Accessing elements
print(fruits[0])         # apple
print(fruits[-1])        # cherry (শেষ element)

# List methods
fruits.append("orange")  # শেষে যোগ
print(fruits)            # ['apple', 'banana', 'cherry', 'orange']

fruits.insert(1, "mango")  # নির্দিষ্ট স্থানে যোগ
print(fruits)              # ['apple', 'mango', 'banana', 'cherry', 'orange']

fruits.remove("banana")    # নির্দিষ্ট item মুছে দিন
print(fruits)              # ['apple', 'mango', 'cherry', 'orange']

print(len(fruits))         # 4 (তালিকার দৈর্ঘ্য)

# List slicing
nums = [0, 1, 2, 3, 4, 5]
print(nums[2:5])          # [2, 3, 4]
print(nums[:3])           # [0, 1, 2]
print(nums[3:])           # [3, 4, 5]
```

---

### 6. Tuple - টিউপল (অপরিবর্তনযোগ্য)

```python
# Tuple examples
coordinates = (10, 20)
colors = ("red", "green", "blue")
single = (5,)  # একটি উপাদানের tuple

print(coordinates)       # (10, 20)
print(type(coordinates)) # <class 'tuple'>

# Accessing elements
print(colors[0])         # red
print(colors[-1])        # blue

# Tuple unpacking
x, y = coordinates
print(x)                 # 10
print(y)                 # 20

# Tuples are immutable (পরিবর্তন করা যায় না)
# colors[0] = "yellow"   # Error দিবে!

# Tuple methods
nums = (1, 2, 3, 2, 2, 4)
print(nums.count(2))     # 3 (2 কতবার আছে)
print(nums.index(3))     # 2 (3 এর index)
```

---

### 7. Dictionary (dict) - অভিধান

```python
# Dictionary examples
student = {
    "name": "Rahim",
    "age": 20,
    "city": "Dhaka"
}

print(student)              # {'name': 'Rahim', 'age': 20, 'city': 'Dhaka'}
print(type(student))        # <class 'dict'>

# Accessing values
print(student["name"])      # Rahim
print(student.get("age"))   # 20

# Adding/Updating
student["grade"] = "A"      # নতুন key-value যোগ
student["age"] = 21         # update করা
print(student)

# Dictionary methods
print(student.keys())       # dict_keys(['name', 'age', 'city', 'grade'])
print(student.values())     # dict_values(['Rahim', 21, 'Dhaka', 'A'])
print(student.items())      # key-value pairs

# Checking key existence
print("name" in student)    # True
print("email" in student)   # False

# Removing items
del student["grade"]        # মুছে ফেলা
print(student)
```

---

### 8. Set - সেট (অনন্য উপাদান)

```python
# Set examples
numbers = {1, 2, 3, 4, 5}
fruits = {"apple", "banana", "cherry"}
mixed = {1, "hello", 3.14}

print(numbers)           # {1, 2, 3, 4, 5}
print(type(numbers))     # <class 'set'>

# Duplicate items automatically removed
duplicate = {1, 2, 2, 3, 3, 3}
print(duplicate)         # {1, 2, 3}

# Set operations
set1 = {1, 2, 3}
set2 = {3, 4, 5}

print(set1.union(set2))        # {1, 2, 3, 4, 5} (সব)
print(set1.intersection(set2)) # {3} (common)
print(set1.difference(set2))   # {1, 2} (শুধু set1 এ আছে)

# Set methods
fruits.add("orange")     # যোগ করা
print(fruits)

fruits.remove("banana")  # মুছে ফেলা
print(fruits)
```

---

### 9. None Type - কিছু নেই

```python
# None examples
result = None

print(result)            # None
print(type(result))      # <class 'NoneType'>

# None is used when
def greet():
    print("Hello")
    # return statement নেই

x = greet()              # Hello
print(x)                 # None

# Checking None
if result is None:
    print("No value")    # Output: No value
```

---

## 🔄 Type Conversion (টাইপ রূপান্তর)

```python
# String to Integer
num_str = "123"
num_int = int(num_str)
print(num_int + 10)      # 133

# Integer to String
age = 25
age_str = str(age)
print("Age: " + age_str) # Age: 25

# String to Float
price_str = "99.99"
price = float(price_str)
print(price)             # 99.99

# List to Tuple
my_list = [1, 2, 3]
my_tuple = tuple(my_list)
print(my_tuple)          # (1, 2, 3)

# String to List
text = "hello"
char_list = list(text)
print(char_list)         # ['h', 'e', 'l', 'l', 'o']
```

---

## 🔍 Type Checking

```python
# type() function
x = 10
print(type(x))           # <class 'int'>

y = "Hello"
print(type(y))           # <class 'str'>

# isinstance() function
num = 100
print(isinstance(num, int))      # True
print(isinstance(num, str))      # False

data = [1, 2, 3]
print(isinstance(data, list))    # True
```

---

## 📊 Data Types তুলনা

| Type | Mutable? | Example | Use Case |
|------|----------|---------|----------|
| `int` | ❌ | `10` | গণনা, বয়স, সংখ্যা |
| `float` | ❌ | `3.14` | দাম, তাপমাত্রা, পরিমাপ |
| `str` | ❌ | `"Hello"` | নাম, ঠিকানা, টেক্সট |
| `bool` | ❌ | `True` | শর্ত, পরীক্ষা |
| `list` | ✅ | `[1,2,3]` | পরিবর্তনশীল ডেটা |
| `tuple` | ❌ | `(1,2,3)` | স্থায়ী ডেটা |
| `dict` | ✅ | `{"a":1}` | Key-value mapping |
| `set` | ✅ | `{1,2,3}` | অনন্য উপাদান |

---

## 💡 কোন Data Type কখন ব্যবহার করবেন?

### **List ব্যবহার করুন যখন:**
- ডেটা পরিবর্তন করতে হবে
- ক্রম (order) গুরুত্বপূর্ণ
- Duplicate values থাকতে পারে

```python
shopping_list = ["rice", "fish", "oil"]
shopping_list.append("salt")
```

### **Tuple ব্যবহার করুন যখন:**
- ডেটা স্থায়ী রাখতে হবে
- পরিবর্তন করার প্রয়োজন নেই
- Memory efficient চাই

```python
coordinates = (23.8103, 90.4125)  # Dhaka এর location
```

### **Dictionary ব্যবহার করুন যখন:**
- Key-value pair দরকার
- দ্রুত lookup চাই
- Structured data

```python
user = {"username": "sahin", "email": "sahin@example.com"}
```

### **Set ব্যবহার করুন যখন:**
- Duplicate remove করতে হবে
- Mathematical operations (union, intersection)
- Membership testing

```python
unique_ids = {101, 102, 103, 102}  # {101, 102, 103}
```

---

## 🎯 Practice Problems

### Problem 1: Data Type Identification
```python
# নিচের variable গুলোর type কী?
a = 42
b = 3.14
c = "Python"
d = [1, 2, 3]
e = (1, 2, 3)
f = {"key": "value"}
g = {1, 2, 3}
h = True
```

### Problem 2: Type Conversion
```python
# String "123" কে integer-এ convert করে 77 যোগ করুন
```

### Problem 3: List Operations
```python
# একটি list তৈরি করুন: [10, 20, 30]
# শেষে 40 যোগ করুন
# প্রথম element print করুন
# List এর length print করুন
```

### Problem 4: Dictionary
```python
# একটি dictionary তৈরি করুন নিজের তথ্য দিয়ে
# তারপর একটি নতুন key-value যোগ করুন
```

---

## 🔑 মূল পয়েন্ট

1. ✅ Python dynamically typed - type declare করতে হয় না
2. ✅ `type()` দিয়ে data type check করা যায়
3. ✅ Mutable (list, dict, set) vs Immutable (int, str, tuple)
4. ✅ সঠিক data type নির্বাচন করা গুরুত্বপূর্ণ
5. ✅ Type conversion করা যায় প্রয়োজন অনুযায়ী

---

**মনে রাখবেন:** সঠিক data type নির্বাচন করলে কোড efficient এবং maintainable হয়! 🚀

**Happy Coding! 💻**