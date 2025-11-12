# NumPy 2D Array (Matrix) Indexing এবং Slicing

## 2D Array তৈরি করা

প্রথমে একটা simple 2D array বানাই যেটা নিয়ে কাজ করব।

```python
import numpy as np

matrix = np.array([
    [1, 2, 3, 4],
    [10, 20, 30, 40],
    [100, 200, 300, 400],
    [1000, 2000, 3000, 4000]
])

print(matrix)
```

**Output:**
```
[[   1    2    3    4]
 [  10   20   30   40]
 [ 100  200  300  400]
 [1000 2000 3000 4000]]
```

এটা একটা 4x4 matrix। 4টা row আর 4টা column।

---

## Python Way - Multiple Brackets

Python এ normally 2D array তে indexing করতে হলে multiple brackets লাগে।

```python
# তৃতীয় row নিতে চাই (index 2)
third_row = matrix[2]
print(third_row)
# [100 200 300 400]

# এখন third_row থেকে index 2 নিতে চাই (300)
value = third_row[2]
print(value)
# 300
```

এটা দুইভাবে করা যায়:

```python
# Step by step
third_row = matrix[2]
value = third_row[2]

# অথবা directly
value = matrix[2][2]
print(value)
# 300
```

Multiple brackets ব্যবহার করে indexing করা যায়। কিন্তু NumPy আরও সহজ উপায় দেয়।

---

## NumPy Way - Comma দিয়ে

NumPy তে comma ব্যবহার করে আরও সহজে indexing করা যায়।

```python
# Row 2, Column 2
value = matrix[2, 2]
print(value)
# 300

# Row 3, Column 1
value = matrix[3, 1]
print(value)
# 2000

# Row 0, Column 3
value = matrix[0, 3]
print(value)
# 4
```

Syntax: `matrix[row_index, column_index]`

এটা অনেক clean আর readable।

---

## 2D Array Slicing - Row Slicing

Row slicing করা যায় 1D array এর মতোই।

```python
# Row 1 থেকে 2 পর্যন্ত (index 1 এবং 2)
rows = matrix[1:3]
print(rows)
```

**Output:**
```
[[ 10  20  30  40]
 [100 200 300 400]]
```

মনে রাখো ending index exclusive। `[1:3]` মানে index 1 আর 2।

---

## Column Slicing - গুরুত্বপূর্ণ!

এখন আসল মজা। Column slicing করতে হলে comma ব্যবহার করতে হয়।

### একটা সমস্যা

ধরো তুমি চাও:
- Row 1 এবং 2 (index 1 এবং 2)
- Column 1 এবং 2 (index 1 এবং 2)

মানে চাও: `20, 30, 200, 300`

Python এ এটা করতে হলে loop চালাতে হতো:

```python
# Python way (complicated)
rows = matrix[1:3]
new_arr = []

for row in rows:
    new_arr.append(row[1:3])

print(new_arr)
# [[20, 30], [200, 300]]
```

কিন্তু NumPy তে খুব সহজ!

### NumPy Column Slicing

```python
# Row 1-2, Column 1-2
result = matrix[1:3, 1:3]
print(result)
```

**Output:**
```
[[ 20  30]
 [200 300]]
```

Syntax: `matrix[row_slice, column_slice]`

একলাইনে! কোন loop লাগে না।

---

## বিভিন্ন Slicing Examples

### Example 1: সব row, specific columns

```python
# সব row, কিন্তু column 1 এবং 2
result = matrix[:, 1:3]
print(result)
```

**Output:**
```
[[   2    3]
 [  20   30]
 [ 200  300]
 [2000 3000]]
```

`:` মানে সব row নাও।

### Example 2: Specific rows, সব columns

```python
# Row 1-2, সব columns
result = matrix[1:3, :]
print(result)
```

**Output:**
```
[[ 10  20  30  40]
 [100 200 300 400]]
```

### Example 3: শুরু থেকে কিছু পর্যন্ত

```python
# Row 0-1, Column 0-2
result = matrix[:2, :3]
print(result)
```

**Output:**
```
[[ 1  2  3]
 [10 20 30]]
```

### Example 4: নির্দিষ্ট জায়গা থেকে শেষ পর্যন্ত

```python
# Row 1 থেকে শেষ, Column 1 থেকে শেষ
result = matrix[1:, 1:]
print(result)
```

**Output:**
```
[[  20   30   40]
 [ 200  300  400]
 [2000 3000 4000]]
```

### Example 5: একটা single row

```python
# শুধু row 2
result = matrix[2, :]
print(result)
# [100 200 300 400]

# অথবা আরও সহজ
result = matrix[2]
print(result)
# [100 200 300 400]
```

### Example 6: একটা single column

```python
# শুধু column 2
result = matrix[:, 2]
print(result)
# [3 30 300 3000]
```

এটা খুব useful! একটা column পুরো বের করে নেওয়া যায়।

---

## Slicing Rules মনে রাখো

1. **Comma দিয়ে আলাদা করো:** `[row_slice, column_slice]`

2. **Ending exclusive:** `[1:3]` মানে index 1 এবং 2, 3 না

3. **Default values:**
   - শুরু ফাঁকা = 0 থেকে
   - শেষ ফাঁকা = end পর্যন্ত
   - `[:]` = সব

4. **Row-first:** প্রথমে row, তারপর column

---

## Practical Examples

### Example: একটা নির্দিষ্ট area নাও

```python
# 5x5 matrix বানাই
big_matrix = np.arange(25).reshape(5, 5)
print(big_matrix)
```

**Output:**
```
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]
 [15 16 17 18 19]
 [20 21 22 23 24]]
```

এখন মাঝের 3x3 অংশ চাই:

```python
# Row 1-3, Column 1-3
center = big_matrix[1:4, 1:4]
print(center)
```

**Output:**
```
[[ 6  7  8]
 [11 12 13]
 [16 17 18]]
```

### Example: প্রতি alternate row এবং column

```python
# প্রতি দ্বিতীয় row এবং column
result = big_matrix[::2, ::2]
print(result)
```

**Output:**
```
[[ 0  2  4]
 [10 12 14]
 [20 22 24]]
```

### Example: Corner values

```python
# উপরের left 2x2
top_left = big_matrix[:2, :2]
print(top_left)
# [[0 1]
#  [5 6]]

# নিচের right 2x2
bottom_right = big_matrix[3:, 3:]
print(bottom_right)
# [[18 19]
#  [23 24]]
```

---

## Broadcasting এখানেও কাজ করে

মনে আছে? Slice করলে original array এর reference পাও।

```python
matrix = np.arange(16).reshape(4, 4)
print(matrix)

# একটা slice নিলাম
sub_matrix = matrix[1:3, 1:3]
print(sub_matrix)
# [[5 6]
#  [9 10]]

# Change করলাম
sub_matrix[:] = 99
print(sub_matrix)
# [[99 99]
#  [99 99]]

# Original matrix দেখি
print(matrix)
# [[ 0  1  2  3]
#  [ 4 99 99  7]
#  [ 8 99 99 11]
#  [12 13 14 15]]
```

Original matrix change হয়ে গেছে!

### Solution - Copy করো

```python
matrix = np.arange(16).reshape(4, 4)

# Copy করে নিলাম
sub_matrix = matrix[1:3, 1:3].copy()
sub_matrix[:] = 99

# Original unchanged
print(matrix)
```

---

## Important Tips

### Tip 1: সবসময় comma ব্যবহার করো

```python
# ভালো - clear
result = matrix[1:3, 2:4]

# কাজ করবে কিন্তু confusing
result = matrix[1:3][2:4]  # এটা row 1-2 নিয়ে তারপর আবার row 2-3 নিবে!
```

### Tip 2: Colon ভুলে যেও না

```python
# সব row, column 2
result = matrix[:, 2]  # ✅ সঠিক

# Error!
# result = matrix[, 2]  # ❌ ভুল, colon লাগবে
```

### Tip 3: Shape check করো

```python
matrix = np.arange(20).reshape(4, 5)
print(matrix.shape)  # (4, 5)

# Row index 0-3 পর্যন্ত valid
# Column index 0-4 পর্যন্ত valid

# Error দিবে
# matrix[5, 2]  # Row 5 নেই!
# matrix[2, 6]  # Column 6 নেই!
```

---

## Practice Problems

### Problem 1
একটা 5x5 matrix বানাও (0-24 পর্যন্ত), তারপর মাঝের 3x3 অংশ বের করো।

### Problem 2
একটা 6x6 matrix থেকে শুধু even rows (0, 2, 4) এবং even columns নাও।

### Problem 3
একটা matrix এর সব corner values (4টা কোণা) বের করো।

### Problem 4
একটা 4x6 matrix বানাও, তারপর:
- প্রথম 2 row
- শেষ 3 columns
এই অংশটুকু নাও।

---

## Real World Example

ধরো একটা image আছে যেটা 100x100 pixels। তুমি শুধু মাঝের 50x50 অংশ চাও:

```python
# Image হিসেবে একটা random matrix
image = np.random.randint(0, 255, size=(100, 100))

# মাঝের 50x50
center_start = 25
center_end = 75

cropped = image[center_start:center_end, center_start:center_end]
print(cropped.shape)  # (50, 50)
```

এভাবে image cropping করা যায়!

---

## Summary

| কাজ | Syntax | Example |
|-----|--------|---------|
| Single value | `[r, c]` | `matrix[2, 3]` |
| Row slice | `[r1:r2, :]` | `matrix[1:3, :]` |
| Column slice | `[:, c1:c2]` | `matrix[:, 1:3]` |
| Sub-matrix | `[r1:r2, c1:c2]` | `matrix[1:3, 2:4]` |
| Single row | `[r]` or `[r, :]` | `matrix[2]` |
| Single column | `[:, c]` | `matrix[:, 2]` |

মনে রাখো:
- Comma দিয়ে row আর column আলাদা করো
- Ending index exclusive
- Broadcasting থেকে বাঁচতে `.copy()` ব্যবহার করো
- Practice করতে থাকো!

2D array slicing শুরুতে একটু complicated লাগতে পারে, কিন্তু practice করলে সহজ হয়ে যাবে। Data analysis এ এটা প্রচুর লাগে।

Happy Coding! 🚀