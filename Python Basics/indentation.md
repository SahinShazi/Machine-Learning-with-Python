# Python Indentation - ইন্ডেন্টেশন

## 📚 Indentation কী?

**Indentation** হলো কোডের লাইনের শুরুতে দেওয়া ফাঁকা স্থান (spaces বা tabs)। Python-এ indentation শুধু সুন্দর দেখানোর জন্য নয়, এটি কোডের **অংশ** এবং **বাধ্যতামূলক**।

### অন্যান্য ভাষার সাথে পার্থক্য:

**C/C++/Java:**
```c
if (x > 5) {
    printf("Greater");
}
```
এখানে `{ }` দিয়ে code block বোঝানো হয়

**Python:**
```python
if x > 5:
    print("Greater")
```
এখানে **indentation** দিয়ে code block বোঝানো হয়

---

## 🎯 কেন Indentation গুরুত্বপূর্ণ?

Python-এ indentation ছাড়া কোড চলবে না! এটি প্রোগ্রামের **structure** এবং **logic** নির্ধারণ করে।

### সুবিধা:
1. ✅ কোড পড়তে সহজ এবং পরিষ্কার
2. ✅ কোডের hierarchy বোঝা যায়
3. ✅ ভুল খুঁজে বের করা সহজ
4. ✅ Professional কোড লেখার অভ্যাস তৈরি হয়

---

## 📏 Indentation নিয়ম

### ১. স্পেস বনাম ট্যাব (Spaces vs Tabs)

**সুপারিশ:** **4 spaces** ব্যবহার করুন (PEP 8 standard)

```python
# ✅ সঠিক (4 spaces)
if True:
    print("Hello")
    
# ❌ ভুল (tabs এবং spaces মিশ্রণ)
if True:
	print("Hello")  # tab
    print("World")  # spaces
```

### ২. একই লেভেলে সমান Indentation

```python
# ✅ সঠিক
if True:
    print("Line 1")
    print("Line 2")
    print("Line 3")

# ❌ ভুল
if True:
    print("Line 1")
  print("Line 2")    # কম space
      print("Line 3")  # বেশি space
```

### ৩. Nested Blocks এ বাড়তি Indentation

```python
# ✅ সঠিক
if True:
    print("Outer")
    if True:
        print("Inner")
        
# প্রতিটি লেভেলে 4 spaces যোগ হয়
```

---

## 💻 উদাহরণসহ ব্যবহার

### Example 1: If-Else Statement

```python
age = 18

if age >= 18:
    print("You are an adult")
    print("You can vote")
else:
    print("You are a minor")
    print("You cannot vote")
```

**ব্যাখ্যা:**
- `if` এর পরে colon (`:`) এবং নতুন লাইনে indentation
- if block-এর সব statement একই লেভেলে indent
- `else` if-এর সাথে একই লেভেলে, তার ভিতরের কোড indent

---

### Example 2: Nested If Statement

```python
num = 15

if num > 0:
    print("Positive number")
    
    if num % 2 == 0:
        print("Even number")
    else:
        print("Odd number")
else:
    print("Negative or zero")
```

**ব্যাখ্যা:**
- প্রথম `if` block → 4 spaces
- ভিতরের `if` block → 8 spaces (4+4)
- প্রতিটি নতুন লেভেলে 4 spaces যোগ

**Output:**
```
Positive number
Odd number
```

---

### Example 3: For Loop

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(f"I like {fruit}")
    print(f"{fruit} is delicious")
    
print("Loop finished")
```

**ব্যাখ্যা:**
- loop এর ভিতরের সব কোড indent করা
- loop শেষ হলে আবার normal indent-এ ফিরে আসা

**Output:**
```
I like apple
apple is delicious
I like banana
banana is delicious
I like cherry
cherry is delicious
Loop finished
```

---

### Example 4: While Loop

```python
count = 1

while count <= 3:
    print(f"Count is {count}")
    count += 1
    
print("Done counting")
```

**ব্যাখ্যা:**
- while loop এর body indent করা হয়েছে
- loop এর বাইরের statement normal indent-এ

**Output:**
```
Count is 1
Count is 2
Count is 3
Done counting
```

---

### Example 5: Function Definition

```python
def greet(name):
    print(f"Hello, {name}!")
    print("Welcome to Python")
    return "Greeting complete"

result = greet("Sahin")
print(result)
```

**ব্যাখ্যা:**
- function এর সব body indent করতে হয়
- function call normal indent-এ

**Output:**
```
Hello, Sahin!
Welcome to Python
Greeting complete
```

---

### Example 6: Complex Nested Structure

```python
students = [
    {"name": "Rahim", "marks": 85},
    {"name": "Karim", "marks": 92},
    {"name": "Salma", "marks": 78}
]

for student in students:
    print(f"Student: {student['name']}")
    
    if student['marks'] >= 90:
        print("  Grade: A+")
        print("  Excellent!")
    elif student['marks'] >= 80:
        print("  Grade: A")
        print("  Very Good!")
    else:
        print("  Grade: B")
        print("  Good!")
    
    print("---")
```

**ব্যাখ্যা:**
- for loop → 4 spaces
- if/elif/else → 8 spaces (loop এর ভিতরে)
- print statements → 12 spaces (if এর ভিতরে)

**Output:**
```
Student: Rahim
  Grade: A
  Very Good!
---
Student: Karim
  Grade: A+
  Excellent!
---
Student: Salma
  Grade: B
  Good!
---
```

---

## ❌ সাধারণ ভুল এবং সমাধান

### ভুল 1: IndentationError

```python
# ❌ ভুল
if True:
print("Hello")

# Error: IndentationError: expected an indented block
```

**সমাধান:**
```python
# ✅ সঠিক
if True:
    print("Hello")
```

---

### ভুল 2: অসমান Indentation

```python
# ❌ ভুল
def example():
    print("Line 1")
      print("Line 2")  # বেশি space

# Error: IndentationError: unexpected indent
```

**সমাধান:**
```python
# ✅ সঠিক
def example():
    print("Line 1")
    print("Line 2")
```

---

### ভুল 3: Tab এবং Space মিশ্রণ

```python
# ❌ ভুল (দেখতে ঠিক মনে হলেও error দিবে)
def test():
	print("Tab")     # একটি tab
    print("Space")   # spaces

# Error: TabError: inconsistent use of tabs and spaces
```

**সমাধান:**
```python
# ✅ সঠিক (সব জায়গায় spaces)
def test():
    print("Tab")
    print("Space")
```

---

### ভুল 4: Unnecessary Indentation

```python
# ❌ ভুল
name = "Sahin"
    print(name)  # কোন কারণ ছাড়াই indent

# Error: IndentationError: unexpected indent
```

**সমাধান:**
```python
# ✅ সঠিক
name = "Sahin"
print(name)
```

---

## 🛠️ Indentation চেক করার উপায়

### 1. Text Editor Settings

**VS Code:**
- Settings → Tab Size: 4
- Insert Spaces: ✅ (checked)

**PyCharm:**
- Settings → Editor → Code Style → Python
- Tab size: 4, Indent: 4

### 2. Python Built-in Tool

```bash
python -m tabnanny your_file.py
```
এটি indentation সমস্যা খুঁজে বের করবে

---

## 📊 Indentation Hierarchy উদাহরণ

```python
# Level 0 (No indentation)
print("Level 0")

if True:                          # Level 0
    print("Level 1")              # Level 1 (4 spaces)
    
    if True:                      # Level 1
        print("Level 2")          # Level 2 (8 spaces)
        
        if True:                  # Level 2
            print("Level 3")      # Level 3 (12 spaces)
            
            for i in range(2):    # Level 3
                print("Level 4")  # Level 4 (16 spaces)
```

---

## 🎯 Best Practices

### ✅ করুন:
1. সবসময় **4 spaces** ব্যবহার করুন
2. একই প্রজেক্টে একই indentation style ব্যবহার করুন
3. Text editor configure করে নিন
4. PEP 8 guidelines follow করুন

### ❌ করবেন না:
1. Tabs এবং spaces একসাথে ব্যবহার করবেন না
2. Random indentation দিবেন না
3. Copy-paste করে indentation নষ্ট করবেন না

---

## 📝 অনুশীলন সমস্যা

### Problem 1: ভুল খুঁজে বের করুন

```python
# এই কোডে কী ভুল আছে?
def calculate():
print("Calculating")
    result = 10 + 20
  return result
```

**উত্তর:**
- প্রথম print statement indent নেই
- return statement এর indent ভুল

---

### Problem 2: সঠিকভাবে Indent করুন

```python
# এই কোড সঠিকভাবে indent করুন
for i in range(3):
print(f"Number {i}")
if i % 2 == 0:
print("Even")
else:
print("Odd")
```

**সঠিক উত্তর:**
```python
for i in range(3):
    print(f"Number {i}")
    if i % 2 == 0:
        print("Even")
    else:
        print("Odd")
```

---

### Problem 3: Nested Structure তৈরি করুন

একটি প্রোগ্রাম লিখুন যা:
- 1 থেকে 5 পর্যন্ত loop চালাবে
- প্রতিটি সংখ্যা check করবে জোড় নাকি বিজোড়
- জোড় হলে তার square প্রিন্ট করবে
- বিজোড় হলে তার cube প্রিন্ট করবে

---

## 🔑 মূল পয়েন্ট

| বিষয় | বিবরণ |
|------|-------|
| **Standard** | 4 spaces per level |
| **সাথে ব্যবহার** | `:` (colon) এর পরে |
| **প্রয়োজন** | if, for, while, def, class |
| **ভুল** | IndentationError, TabError |
| **Tool** | tabnanny, linters |

---

## 🚀 পরবর্তী পদক্ষেপ

Indentation আয়ত্তে এলে:
1. Functions লিখুন
2. Classes তৈরি করুন
3. Complex nested structures নিয়ে কাজ করুন
4. Real-world projects শুরু করুন

---

**মনে রাখবেন:** Python-এ indentation শুধু style নয়, এটি syntax! সঠিক indentation = সঠিক কোড। 💪

**Happy Coding! 🎉**