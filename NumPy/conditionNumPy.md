# NumPy Conditional Selection এবং Universal Functions

## Conditional Selection কি?

Conditional selection মানে হলো নির্দিষ্ট শর্ত মেনে array থেকে elements বের করা। যেমন - একটা array থেকে শুধু 5 এর বেশি সংখ্যাগুলো চাই।

### Python এ Normal Way

প্রথমে দেখি Python এ normally কিভাবে করতাম:

```python
arr = [3, 1, 4, 5, 2, 567, 5678, 23, 4]

# খালি list বানাই
answer = []

# Loop চালিয়ে check করি
for i in arr:
    if i > 5:
        answer.append(i)

print(answer)
# [567, 5678, 23]
```

এটা কাজ করে কিন্তু অনেক লাইন লাগে। Loop লাগে, condition check করতে হয়।

---

## NumPy Way - Broadcasting দিয়ে

NumPy তে এটা অনেক সহজ! Broadcasting এর কারণে একলাইনে করা যায়।

### Step 1: Boolean Array তৈরি

```python
import numpy as np

# NumPy array বানাই
np_arr = np.array([3, 1, 4, 5, 2, 567, 5678, 23, 4])

# Condition check করি
bool_arr = np_arr > 5
print(bool_arr)
```

**Output:**
```
[False False False False False  True  True  True False]
```

দেখো কি হলো! প্রতিটা element এর জন্য check হয়েছে:
- 3 > 5? False
- 1 > 5? False
- 4 > 5? False
- 5 > 5? False (5 সমান, বড় না)
- 2 > 5? False
- 567 > 5? True
- 5678 > 5? True
- 23 > 5? True
- 4 > 5? False

### Step 2: Boolean Array দিয়ে Indexing

এখন এই boolean array টা ব্যবহার করে original array থেকে শুধু True জায়গার values নিতে পারি:

```python
# Boolean indexing
result = np_arr[bool_arr]
print(result)
# [567 5678  23]
```

Magic! শুধু True জায়গার values এসেছে।

### একলাইনে সব

দুই step আলাদা না করে একসাথে করতে পারো:

```python
result = np_arr[np_arr > 5]
print(result)
# [567 5678  23]
```

এক লাইন! Loop নেই, কোন manual check নেই। এটাই NumPy এর power।

---

## কিভাবে কাজ করে?

### Broadcasting Concept

যখন `np_arr > 5` লিখো, NumPy প্রতিটা element এ এই condition apply করে:

```
Array:     [3,    1,    4,    5,    2,    567,  5678, 23,   4]
           ↓     ↓     ↓     ↓     ↓     ↓     ↓     ↓     ↓
Check > 5: False False False False False True  True  True  False
```

এটাকে বলে **broadcasting** - একটা operation সব elements এ automatically apply হয়।

### Boolean Indexing

Boolean array দিয়ে indexing করলে:
- True জায়গার values নেয়
- False জায়গার values বাদ দেয়

```python
arr = np.array([10, 20, 30, 40])
mask = np.array([True, False, True, False])

result = arr[mask]
print(result)
# [10 30]
```

---

## বিভিন্ন Comparison Operators

সব comparison operators ব্যবহার করা যায়:

### Greater Than (>)

```python
arr = np.array([3, 1, 4, 5, 2, 567, 5678, 23, 4])

result = arr[arr > 5]
print(result)
# [567 5678  23]
```

### Less Than (<)

```python
result = arr[arr < 5]
print(result)
# [3 1 4 2 4]
```

### Greater Than or Equal (>=)

```python
result = arr[arr >= 5]
print(result)
# [5 567 5678  23]
```

এবার 5 ও আছে কারণ >= দিয়েছি।

### Less Than or Equal (<=)

```python
result = arr[arr <= 5]
print(result)
# [3 1 4 5 2 4]
```

### Equal (==)

```python
arr = np.array([1, 2, 3, 2, 4, 2, 5])

result = arr[arr == 2]
print(result)
# [2 2 2]
```

সব 2 গুলো পেয়ে গেছি!

### Not Equal (!=)

```python
result = arr[arr != 2]
print(result)
# [1 3 4 5]
```

2 ছাড়া সব।

---

## Arithmetic Operations - Broadcasting

শুধু comparison না, arithmetic operations এও broadcasting কাজ করে!

### Addition

```python
arr = np.array([1, 2, 3, 4, 5])

# প্রতিটা element এ 10 যোগ
result = arr + 10
print(result)
# [11 12 13 14 15]
```

### Multiplication

```python
# প্রতিটা element 2 দিয়ে গুণ
result = arr * 2
print(result)
# [2 4 6 8 10]
```

### Division

```python
# প্রতিটা element 2 দিয়ে ভাগ
result = arr / 2
print(result)
# [0.5 1.  1.5 2.  2.5]
```

### Array + Array

```python
arr1 = np.array([1, 2, 3])
arr2 = np.array([10, 20, 30])

result = arr1 + arr2
print(result)
# [11 22 33]
```

Element-wise addition: 1+10, 2+20, 3+30

---

## Division by Zero - সাবধান!

Zero দিয়ে ভাগ করলে কি হয়?

```python
arr = np.arange(5)
print(arr)
# [0 1 2 3 4]

# Zero দিয়ে ভাগ করলাম
result = arr / 0
```

**Warning দেখাবে:**
```
RuntimeWarning: divide by zero encountered
RuntimeWarning: invalid value encountered
```

**Output:**
```
[nan inf inf inf inf]
```

- `0 / 0 = nan` (Not a Number - অসংজ্ঞায়িত)
- `অন্য / 0 = inf` (Infinity - অসীম)

Python এ error দিত, কিন্তু NumPy warning দেয় এবং চলতে থাকে।

**সাবধানতা:**
- Zero দিয়ে ভাগ এড়িয়ে চলো
- Check করো divisor zero কিনা
- nan আর inf values পেলে সমস্যা হতে পারে

---

## Multiple Conditions (Advanced)

একাধিক শর্ত একসাথে দিতে হলে `&` (and) এবং `|` (or) ব্যবহার করতে হয়।

**Note:** Python এর `and`/`or` না, `&`/`|` ব্যবহার করতে হবে এবং brackets লাগবে!

### AND Condition

```python
arr = np.arange(20)

# 5 থেকে বড় AND 15 থেকে ছোট
result = arr[(arr > 5) & (arr < 15)]
print(result)
# [6 7 8 9 10 11 12 13 14]
```

### OR Condition

```python
# 5 থেকে ছোট OR 15 থেকে বড়
result = arr[(arr < 5) | (arr > 15)]
print(result)
# [0 1 2 3 4 16 17 18 19]
```

**গুরুত্বপূর্ণ:** Brackets অবশ্যই লাগবে!

```python
# ❌ ভুল
result = arr[arr > 5 & arr < 15]  # Error!

# ✅ সঠিক
result = arr[(arr > 5) & (arr < 15)]
```

---

## Universal Functions (ufuncs)

NumPy তে অনেক built-in mathematical functions আছে যেগুলো broadcasting support করে। এগুলোকে বলে Universal Functions বা ufuncs।

### Mathematical Operations

```python
arr = np.array([1, 4, 9, 16, 25])

# Square root
print(np.sqrt(arr))
# [1. 2. 3. 4. 5.]

# Square (power of 2)
print(np.square(arr))
# [  1  16  81 256 625]

# Power
print(np.power(arr, 3))  # প্রতিটার cube
# [    1    64   729  4096 15625]

# Absolute value
arr2 = np.array([-1, -2, 3, -4])
print(np.abs(arr2))
# [1 2 3 4]
```

### Exponential & Logarithm

```python
arr = np.array([1, 2, 3])

# Exponential (e^x)
print(np.exp(arr))
# [ 2.71828183  7.3890561  20.08553692]

# Natural log
print(np.log(arr))
# [0.         0.69314718 1.09861229]

# Log base 10
print(np.log10(arr))
# [0.         0.30103    0.47712125]
```

### Trigonometric Functions

```python
angles = np.array([0, 30, 45, 60, 90])

# Degrees to radians
rad = np.deg2rad(angles)

# Sin, Cos, Tan
print(np.sin(rad))
print(np.cos(rad))
print(np.tan(rad))
```

### Rounding Functions

```python
arr = np.array([1.2, 2.7, 3.5, 4.1])

# Round
print(np.round(arr))
# [1. 3. 4. 4.]

# Floor (নিচের দিকে)
print(np.floor(arr))
# [1. 2. 3. 4.]

# Ceiling (উপরের দিকে)
print(np.ceil(arr))
# [2. 3. 4. 5.]
```

### Statistical Functions

```python
arr = np.array([1, 2, 3, 4, 5])

# Sum
print(np.sum(arr))  # 15

# Mean (average)
print(np.mean(arr))  # 3.0

# Standard deviation
print(np.std(arr))  # 1.41...

# Min, Max
print(np.min(arr))  # 1
print(np.max(arr))  # 5
```

### Matrix Multiplication

```python
# দুইটা 2x2 matrix
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

# Matrix multiplication
result = np.matmul(a, b)
print(result)
# [[19 22]
#  [43 50]]
```

---

## Practical Examples

### Example 1: Grade Filter

```python
marks = np.array([45, 67, 89, 23, 78, 90, 56, 34, 91])

# 60 এর বেশি marks
passed = marks[marks >= 60]
print("Passed:", passed)
# [67 89 78 90 91]

# 60 এর কম marks
failed = marks[marks < 60]
print("Failed:", failed)
# [45 23 56 34]
```

### Example 2: Temperature Range

```python
temps = np.array([25, 30, 35, 40, 45, 22, 28])

# 25-35 degrees এর মধ্যে
comfortable = temps[(temps >= 25) & (temps <= 35)]
print("Comfortable:", comfortable)
# [25 30 35 28]
```

### Example 3: Outlier Detection

```python
data = np.array([10, 12, 15, 13, 200, 14, 11, 16, 300])

# Mean বের করি
mean = np.mean(data)
std = np.std(data)

# Mean থেকে 2*std এর বেশি দূরে (outlier)
outliers = data[np.abs(data - mean) > 2*std]
print("Outliers:", outliers)
# [200 300]
```

---

## Important Universal Functions List

### Arithmetic
- `np.add()`, `np.subtract()`, `np.multiply()`, `np.divide()`
- `np.power()`, `np.sqrt()`, `np.square()`
- `np.mod()`, `np.remainder()`

### Trigonometric
- `np.sin()`, `np.cos()`, `np.tan()`
- `np.arcsin()`, `np.arccos()`, `np.arctan()`
- `np.deg2rad()`, `np.rad2deg()`

### Exponential & Log
- `np.exp()`, `np.log()`, `np.log10()`, `np.log2()`

### Rounding
- `np.round()`, `np.floor()`, `np.ceil()`, `np.trunc()`

### Statistical
- `np.sum()`, `np.mean()`, `np.median()`
- `np.std()`, `np.var()`
- `np.min()`, `np.max()`, `np.argmin()`, `np.argmax()`

### Comparison
- `np.maximum()`, `np.minimum()` (element-wise)
- `np.greater()`, `np.less()`, `np.equal()`

আরও দেখতে চাইলে NumPy documentation দেখো: https://numpy.org/doc/stable/reference/ufuncs.html

---

## Key Takeaways

1. **Conditional Selection** - শর্ত দিয়ে elements filter করা একলাইনে
2. **Broadcasting** - Operation সব elements এ automatically apply হয়
3. **Boolean Indexing** - True/False array দিয়ে indexing করা যায়
4. **Universal Functions** - Built-in mathematical functions যা broadcasting support করে
5. **Zero Division** - Warning দেয় কিন্তু error না, সাবধান থাকো

NumPy শেখার জন্য:
- Practice করো বিভিন্ন conditions দিয়ে
- Universal functions try করো
- Documentation পড়ো
- Exercise solve করো

এই ছিল NumPy এর শেষ basics! এখন Pandas শেখার সময়। Pandas এ এই সব concept আরও powerful ভাবে কাজে লাগবে।

Happy Coding! 🚀