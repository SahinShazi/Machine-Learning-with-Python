# Python Functions

## Function কি জিনিস?

Function হলো কোডের একটা ব্লক যেটা একটা নির্দিষ্ট কাজ করে। তুমি একবার function লিখবে, তারপর যতবার খুশি ততবার সেটা call করতে পারবে। 

মনে করো তোমার একটা calculator আছে। সেটাতে যোগ, বিয়োগ, গুণ সব আলাদা আলাদা বাটন আছে। Function ও সেরকম - প্রতিটা function একটা নির্দিষ্ট কাজ করে।

**কেন ব্যবহার করবে?**
- একই কোড বারবার লিখতে হয় না
- কোড organized থাকে
- debugging সহজ হয়
- অন্যরা বুঝতে পারে কোনটা কি করছে

## Function এর Structure

```python
def function_name(parameters):
    # যা করতে চাও
    return result  # optional
```

দেখো কয়েকটা জিনিস আছে:
- `def` - এটা দিয়ে বুঝানো হয় function শুরু হচ্ছে
- `function_name` - তুমি যা খুশি নাম দিতে পারো
- `parameters` - যা input নিবে (না থাকলে খালি রাখো)
- `return` - কি ফেরত দিবে (সব সময় লাগে না)

---

## Problem 01: সবচেয়ে সহজ Function

এটা একটা function যেটা কোনো input নেয় না, শুধু একটা message প্রিন্ট করে।

```python
def greet():
    print("Hello from Python!")

greet()
```

**কি হচ্ছে:**
- `def greet():` দিয়ে function বানালাম
- ভিতরে শুধু একটা print আছে
- `greet()` দিয়ে function কে call করলাম

খেয়াল করো - function define করলেই চলে না, call করতে হয়। নইলে কিচ্ছু হবে না!

```python
def greet():
    print("Hello")
# এখানে কিছু হবে না, কারণ call করিনি

greet()  # এখন হবে!
```

---

## Problem 02: Parameter সহ Function

এবার আমরা function কে কিছু দিবো, সে সেটা নিয়ে কাজ করবে।

```python
username = "Sahin"

def greet_user(name):
    print(name)

greet_user("Hello, " + username + "! How are you today?")
```

**একটু বুঝি:**
- `greet_user(name)` - এখানে `name` হলো parameter
- যখন call করছি `greet_user("Hello, " + username + "!")` - পুরো string টা name এ যাবে
- তারপর সেটা print হবে

আরেকটু ভালো করে লিখতে পারতাম:

```python
def greet_user(name):
    print(f"Hello, {name}! How are you today?")

username = "Sahin"
greet_user(username)
```

এটা একটু cleaner লাগে না? f-string ব্যবহার করলে পড়তে সহজ হয়।

---

## Problem 03: কিছু Return করা

এখন মজার জিনিস। Function কিছু করবে আর ফেরত দিবে result।

```python
def add_numbers(a, b):
    return a + b

print(add_numbers(2, 5))
```

**ব্যাপারটা কি:**
- Function দুইটা সংখ্যা নিচ্ছে: a আর b
- সেগুলো যোগ করছে
- `return` দিয়ে ফেরত দিচ্ছে
- আমরা সরাসরি print করে দেখছি

Return এর মজা হলো তুমি result টা রেখে দিতে পারো:

```python
result = add_numbers(10, 20)
print(f"যোগফল হলো: {result}")

# আবার ব্যবহার করতে পারো
double_result = result * 2
print(double_result)
```

**Print vs Return এর পার্থক্য:**

```python
# শুধু print
def add_print(a, b):
    print(a + b)

# Return করছে
def add_return(a, b):
    return a + b

# দেখো পার্থক্য
x = add_print(2, 3)   # 5 print হবে কিন্তু x = None
y = add_return(2, 3)  # 5 return হবে, y = 5

print(x)  # None
print(y)  # 5
```

---

## Problem 04: List নিয়ে কাজ করা

এটা একটু বড় example। একটা list থেকে শুধু জোড় সংখ্যা বের করতে হবে।

```python
array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

def even_num(arr):
    new_arr = []
    
    for num in arr:
        if num % 2 == 0:
            new_arr.append(num)
    
    return new_arr

results = even_num(array)
print(results)
```

**Step by step:**

১. একটা খালি list বানালাম `new_arr = []`
২. পুরো array তে loop চালালাম
৩. প্রতিটা সংখ্যা check করলাম জোড় কিনা (`num % 2 == 0`)
৪. জোড় হলে `new_arr` তে append করলাম
৫. শেষে পুরো list return করলাম

আউটপুট দেখবে: `[2, 4, 6, 8, 10]`

**আরেকটু সহজ উপায় (List Comprehension):**

```python
def even_num(arr):
    return [num for num in arr if num % 2 == 0]

results = even_num(array)
print(results)
```

এটা একই কাজ করছে কিন্তু একলাইনে! শুরুতে একটু কঠিন লাগতে পারে, কিন্তু অভ্যাস হলে সহজ।

---

## আরও কিছু Useful Examples

### একাধিক Parameter

```python
def calculate(a, b, operation):
    if operation == "add":
        return a + b
    elif operation == "subtract":
        return a - b
    elif operation == "multiply":
        return a * b
    else:
        return "Invalid operation"

print(calculate(10, 5, "add"))       # 15
print(calculate(10, 5, "subtract"))  # 5
print(calculate(10, 5, "multiply"))  # 50
```

### Default Parameter

```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Sahin")              # Hello, Sahin!
greet("Rahim", "Hi")        # Hi, Rahim!
greet("Karim", "Assalamu Alaikum")  # Assalamu Alaikum, Karim!
```

Default value দিলে parameter optional হয়ে যায়।

### একাধিক Value Return

```python
def get_user_info():
    name = "Sahin"
    age = 25
    city = "Dhaka"
    return name, age, city

user_name, user_age, user_city = get_user_info()
print(f"{user_name}, {user_age} years old, from {user_city}")
```

### Variable Length Arguments

কখনো জানো না কতগুলো argument আসবে? `*args` ব্যবহার করো:

```python
def add_all(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

print(add_all(1, 2, 3))           # 6
print(add_all(1, 2, 3, 4, 5))     # 15
print(add_all(10, 20))            # 30
```

---

## একটা Real Example - Calculator

চলো একটা simple calculator বানাই:

```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b == 0:
        return "Error: Cannot divide by zero"
    return a / b

# ব্যবহার করি
num1 = 10
num2 = 5

print(f"যোগ: {add(num1, num2)}")
print(f"বিয়োগ: {subtract(num1, num2)}")
print(f"গুণ: {multiply(num1, num2)}")
print(f"ভাগ: {divide(num1, num2)}")
print(f"ভাগ: {divide(num1, 0)}")  # Error দেখাবে
```

---

## Common Mistakes

### ভুল ১: Function Call না করা

```python
def greet():
    print("Hello")

greet  # ❌ কিছু হবে না!
greet()  # ✅ এভাবে call করতে হবে
```

### ভুল ২: Return ভুলে যাওয়া

```python
def add(a, b):
    a + b  # ❌ return নেই!

result = add(2, 3)
print(result)  # None আসবে
```

সঠিক:
```python
def add(a, b):
    return a + b  # ✅
```

### ভুল ৩: Indentation

```python
def greet():
print("Hello")  # ❌ IndentationError!

def greet():
    print("Hello")  # ✅
```

### ভুল ৪: Parameter সংখ্যা না মিলানো

```python
def add(a, b):
    return a + b

add(5)     # ❌ Error - দুইটা চাই
add(5, 3)  # ✅
```

---

## কিছু Tips

১. **Function এর নাম meaningful দাও:**
```python
# ❌ খারাপ
def f(x):
    return x * 2

# ✅ ভালো
def double_number(num):
    return num * 2
```

২. **একটা function একটা কাজ করবে:**
```python
# ❌ এক function এ অনেক কাজ
def do_everything(name, age):
    print(f"Hello {name}")
    print(f"Age: {age}")
    return age * 2

# ✅ আলাদা আলাদা function
def greet(name):
    print(f"Hello {name}")

def double_age(age):
    return age * 2
```

৩. **Comment লিখো (বড় function এর জন্য):**
```python
def calculate_discount(price, discount_percent):
    """
    দাম থেকে discount বাদ দিয়ে final price বের করে
    
    Parameters:
    price - মূল দাম
    discount_percent - কত শতাংশ discount
    
    Returns:
    final_price - discount এর পর যা দাম থাকে
    """
    discount = price * (discount_percent / 100)
    return price - discount
```

---

## Practice করো

১. **তাপমাত্রা Converter**: Celsius কে Fahrenheit এ convert করার function
২. **Grade Calculator**: নম্বর দিলে grade (A, B, C...) return করবে
৩. **List Maximum**: একটা list নিয়ে সবচেয়ে বড় সংখ্যা return করবে
৪. **Palindrome Checker**: একটা word palindrome কিনা check করবে (যেমন "madam")

---

মনে রাখো, function হলো programming এর building block। যত বেশি practice করবে তত clear হবে। শুরুতে একটু confusing লাগতে পারে কিন্তু অভ্যাস হলে দেখবে সব সহজ!

Happy Coding! 🚀