# NumPy - Identity Matrix এবং Random Module

## Identity Matrix কি?

Identity Matrix হলো Matrix এর জগতে "১" এর মতো। মনে করো তুমি যেকোনো সংখ্যাকে ১ দিয়ে গুণ করলে সেই সংখ্যাই পাও, তাই না? Identity Matrix ও তেমনি - যেকোনো matrix এর সাথে identity matrix গুণ করলে সেই matrix ই পাবে।

**Identity Matrix এর বৈশিষ্ট্য:**
- Square matrix হতে হবে (row আর column সমান)
- কোনাকুনি বরাবর (diagonal) সব ১
- বাকি সব জায়গায় ০

```python
import numpy as np

# 5x5 Identity Matrix
identity = np.eye(5)
print(identity)
```

**Output:**
```
[[1. 0. 0. 0. 0.]
 [0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0.]
 [0. 0. 0. 1. 0.]
 [0. 0. 0. 0. 1.]]
```

দেখো, শুধু কোনাকুনি বরাবর ১, বাকি সব ০।

```python
# 3x3 Identity Matrix
small_identity = np.eye(3)
print(small_identity)

# 10x10 Identity Matrix
big_identity = np.eye(10)
print(big_identity)
```

**কেন দরকার?**
- Matrix operations এ অনেক দরকার পড়ে
- Linear algebra তে খুব important
- Machine learning এ inverse matrix, linear transformations এসব জায়গায় লাগে

---

## Random Module - Random Number Generation

NumPy এর একটা powerful module হলো `random`। এটা দিয়ে নানা রকম random number আর array বানানো যায়।

### Import করার দুই উপায়

```python
# উপায় ১: np.random ব্যবহার
import numpy as np
arr = np.random.rand(5)

# উপায় ২: সরাসরি import
from numpy import random
arr = random.rand(5)
```

আমি `np.random` ব্যবহার করব - এটা সবাই করে।

---

## Random Number Generation Methods

### 1. rand() - Uniform Distribution

০ থেকে ১ এর মধ্যে random number।

```python
# একটা single random number
num = np.random.rand()
print(num)  # যেমন: 0.547

# 1D array
arr = np.random.rand(5)
print(arr)  # [0.234 0.876 0.123 0.998 0.445]

# 2D array
arr_2d = np.random.rand(3, 4)
print(arr_2d)
# [[0.234 0.456 0.789 0.123]
#  [0.567 0.890 0.234 0.678]
#  [0.345 0.678 0.901 0.234]]

# 3D array
arr_3d = np.random.rand(2, 3, 4)
print(arr_3d)
```

**মনে রাখো:** `rand()` সবসময় ০ থেকে ১ এর মধ্যে দেয়।

---

### 2. randn() - Normal/Gaussian Distribution

Normal distribution থেকে random number। Negative ও আসতে পারে।

```python
# Single number
num = np.random.randn()
print(num)  # যেমন: -0.234 বা 1.567

# 1D array
arr = np.random.randn(10)
print(arr)

# 2D array
arr_2d = np.random.randn(5, 2)
print(arr_2d)

# 3D array
arr_3d = np.random.randn(3, 4, 2)
print(arr_3d)
```

**পার্থক্য:**
- `rand()` → Uniform distribution (সমান probability)
- `randn()` → Normal/Gaussian distribution (bell curve)

**Note:** Normal distribution আর Gaussian distribution একই জিনিস। Statistical distribution নিয়ে YouTube এ ভিডিও দেখো - data science এ খুব লাগে।

---

### 3. randint() - নির্দিষ্ট Range এর Integer

নির্দিষ্ট range এর মধ্যে random integer চাই? `randint()` ব্যবহার করো।

```python
# 10 থেকে 20 এর মধ্যে একটা number
num = np.random.randint(10, 21)  # 21 exclusive
print(num)

# 10টা random integer (10 থেকে 20)
arr = np.random.randint(10, 21, size=10)
print(arr)

# 2D array (5 row, 2 column)
arr_2d = np.random.randint(10, 21, size=(5, 2))
print(arr_2d)

# 3D array
arr_3d = np.random.randint(0, 100, size=(3, 4, 5))
print(arr_3d)
```

**গুরুত্বপূর্ণ:** End value সবসময় exclusive! মানে `randint(10, 21)` দিলে ১০ থেকে ২০ পাবে, ২১ না।

```python
# 0 থেকে 100
arr = np.random.randint(0, 101, size=20)

# 1 থেকে 10
arr = np.random.randint(1, 11, size=15)
```

---

## Shape - Array এর গঠন জানা

তোমার কাছে একটা array আছে কিন্তু জানো না কয়টা row, কয়টা column - `shape` দেখো!

```python
# একটা random array বানাই
arr = np.random.randint(10, 21, size=(5, 2))

# Shape দেখো
print(arr.shape)  # (5, 2)
```

`(5, 2)` মানে ৫টা row, ২টা column।

```python
# 3D array
arr_3d = np.random.rand(6, 3, 4)
print(arr_3d.shape)  # (6, 3, 4)
```

`(6, 3, 4)` মানে:
- ৬টা "layers" বা blocks
- প্রতি block এ ৩টা row
- প্রতি row এ ৪টা element

**Shape কেন দরকার?**
- Array এর size জানতে
- Debugging করতে
- Reshape করার আগে check করতে

---

## Reshape - Array এর Shape পরিবর্তন

Array এর shape change করতে হলে `reshape()` ব্যবহার করো।

```python
# 1D array
arr = np.arange(12)
print(arr)  # [0 1 2 3 4 5 6 7 8 9 10 11]
print(arr.shape)  # (12,)

# 3x4 matrix বানাই
arr_2d = arr.reshape(3, 4)
print(arr_2d)
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]

print(arr_2d.shape)  # (3, 4)
```

### Reshape এর নিয়ম

**একমাত্র নিয়ম:** Total elements same থাকতে হবে!

```python
arr = np.arange(12)  # 12টা element

# কাজ করবে
arr.reshape(3, 4)    # 3 × 4 = 12 ✅
arr.reshape(2, 6)    # 2 × 6 = 12 ✅
arr.reshape(4, 3)    # 4 × 3 = 12 ✅
arr.reshape(1, 12)   # 1 × 12 = 12 ✅

# কাজ করবে না
# arr.reshape(3, 5)  # 3 × 5 = 15 ❌ Error!
# arr.reshape(2, 5)  # 2 × 5 = 10 ❌ Error!
```

### 3D থেকে 2D

```python
# 3D array
arr_3d = np.random.randint(10, 21, size=(6, 3, 4))
print(arr_3d.shape)  # (6, 3, 4)
# Total elements = 6 × 3 × 4 = 72

# 2D তে convert করি
arr_2d = arr_3d.reshape(12, 6)  # 12 × 6 = 72 ✅
print(arr_2d.shape)  # (12, 6)

# অথবা
arr_2d = arr_3d.reshape(18, 4)  # 18 × 4 = 72 ✅
```

### 1D তে Flatten

```python
# 2D array
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr_2d.shape)  # (2, 3)

# 1D বানাই
arr_1d = arr_2d.reshape(6)  # অথবা flatten()
print(arr_1d)  # [1 2 3 4 5 6]
print(arr_1d.shape)  # (6,)
```

**Tip:** `-1` ব্যবহার করতে পারো automatic calculation এর জন্য:

```python
arr = np.arange(24)

# NumPy নিজেই calculate করবে
arr.reshape(4, -1)   # (4, 6) হবে
arr.reshape(-1, 8)   # (3, 8) হবে
arr.reshape(2, 3, -1)  # (2, 3, 4) হবে
```

---

## Useful Methods

### max() এবং min()

```python
arr = np.array([10, 45, 23, 67, 12, 89, 34])

# সবচেয়ে বড়
print(arr.max())  # 89

# সবচেয়ে ছোট
print(arr.min())  # 10
```

### argmax() এবং argmin()

Maximum/minimum এর **index** জানতে চাও?

```python
arr = np.array([10, 45, 23, 67, 12, 89, 34])

# সবচেয়ে বড় value এর index
print(arr.argmax())  # 5 (কারণ 89 index 5 এ আছে)

# সবচেয়ে ছোট value এর index
print(arr.argmin())  # 0 (কারণ 10 index 0 এ আছে)
```

**2D array তে:**

```python
arr_2d = np.random.randint(10, 21, size=(5, 4))
print(arr_2d)

# পুরো array তে max
print(arr_2d.max())

# প্রতি row এর max
print(arr_2d.max(axis=1))

# প্রতি column এর max
print(arr_2d.max(axis=0))
```

**গুরুত্বপূর্ণ Note:** 
- 1D array তে `argmax()`/`argmin()` সহজ
- Multi-dimensional array তে এটা flatten করে তারপর index দেয়
- তাই 2D/3D array কে 1D বানিয়ে তারপর use করা ভালো

```python
# 2D array
arr_2d = np.random.randint(10, 21, size=(5, 3))
print(arr_2d)

# 1D বানাই
arr_1d = arr_2d.reshape(15)  # 5 × 3 = 15
print(arr_1d)

# এখন argmax use করো
max_index = arr_1d.argmax()
print(f"Max value: {arr_1d[max_index]}")
print(f"Max index: {max_index}")
```

---

## Real Example - সব একসাথে

```python
import numpy as np

# Random 3D array বানাই
print("=== 3D Array ===")
arr_3d = np.random.randint(10, 21, size=(6, 3, 4))
print(f"Shape: {arr_3d.shape}")
print(f"Total elements: {arr_3d.size}")
print(arr_3d)

# 2D তে convert করি
print("\n=== Reshape to 2D ===")
arr_2d = arr_3d.reshape(12, 6)
print(f"New shape: {arr_2d.shape}")
print(arr_2d)

# 1D তে convert করি
print("\n=== Flatten to 1D ===")
arr_1d = arr_2d.reshape(72)
print(f"Shape: {arr_1d.shape}")
print(arr_1d)

# Statistics
print("\n=== Statistics ===")
print(f"Max: {arr_1d.max()}")
print(f"Min: {arr_1d.min()}")
print(f"Max index: {arr_1d.argmax()}")
print(f"Min index: {arr_1d.argmin()}")

# Value check
print(f"\nValue at max index: {arr_1d[arr_1d.argmax()]}")
print(f"Value at min index: {arr_1d[arr_1d.argmin()]}")

# Identity matrix
print("\n=== Identity Matrix ===")
identity = np.eye(5)
print(identity)
```

---

## গুরুত্বপূর্ণ পয়েন্ট

### 1. Distribution বুঝো

- **Uniform Distribution:** সবার probability সমান
- **Normal/Gaussian Distribution:** Bell curve, মাঝে বেশি, দুই পাশে কম

YouTube এ search করো - data science এ অনেক লাগবে।

### 2. Reshape Rules

- Total elements same থাকতে হবে
- মাথায় মাথায় গুণ করে check করো
- `-1` use করলে NumPy calculate করে নেয়

### 3. Random Module বড়

Random module অনেক বড়। আরও অনেক methods আছে:
- `choice()` - random selection
- `shuffle()` - array shuffle করা
- `permutation()` - random arrangement
- `seed()` - reproducible results

এগুলো নিয়ে experiment করো।

### 4. Matrix জ্ঞান দরকার

Higher Math এর Matrix chapter পড়ো। যোগ, বিয়োগ, গুণ - সব জানতে হবে। Machine learning এ অনেক লাগবে।

---

## Practice করো

১. **একটা 10x10 identity matrix বানাও**

২. **100টা random number (0-100) বানিয়ে তার max, min বের করো**

৩. **একটা 3D array (4x5x6) বানাও, তারপর 2D (20x6) তে convert করো**

৪. **Random array বানিয়ে তার shape change করো বিভিন্নভাবে**

৫. **Normal distribution আর Uniform distribution এর পার্থক্য নিজে experiment করো**

---

## Matrix এর গুরুত্ব

Higher Math এর Matrix chapter ভালো করে পড়ো। এটা না জানলে Machine Learning এ সমস্যা হবে। Matrix operations (যোগ, বিয়োগ, গুণ, transpose, inverse) সব clear থাকতে হবে।

আর হ্যাঁ, practice করতে থাকো। যত experiment করবে তত clear হবে। PC hang খাবে হয়তো বড় array দিলে, কিন্তু নষ্ট হবে না!

পরের topic হবে NumPy Indexing আর Slicing। সেটাও মজার।

Happy Coding! 🚀