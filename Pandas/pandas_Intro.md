# Pandas Series - পরিচিতি

## Pandas কি?

Pandas হলো NumPy এর উপর তৈরি একটা powerful library যা data analysis এর জন্য ব্যবহার হয়। NumPy যেখানে কঠিন আর complex, Pandas সেখানে সহজ আর user-friendly।

একে অনেকে বলে "Programmer's Excel" - কারণ এটা দিয়ে data নিয়ে সব রকম কাজ করা যায়।

### Installation

```bash
# Conda দিয়ে
conda install pandas

# অথবা pip দিয়ে
pip install pandas
```

### Import করা

```python
import numpy as np
import pandas as pd
```

`pd` নাম দিয়ে import করা একটা standard practice।

---

## Pandas এর দুইটা মূল জিনিস

১. **Series** - 1D data (একটা column)
২. **DataFrame** - 2D data (পুরো table)

আজকে Series নিয়ে কথা বলব। DataFrame পরে।

---

## Pandas Series কি?

Series হলো NumPy array এর Pandas version। এটা একটা 1D array যেখানে custom index থাকতে পারে।

### Simple Series তৈরি

```python
import pandas as pd
import numpy as np

# NumPy array তৈরি করি
data = np.arange(5, 10)
print(data)
# [5 6 7 8 9]

# Series এ convert করি
pd_series = pd.Series(data=data)
print(pd_series)
```

**Output:**
```
0    5
1    6
2    7
3    8
4    9
dtype: int64
```

দেখো পার্থক্য - বাম দিকে index আছে (0, 1, 2, 3, 4)। এটাই Series এর বিশেষত্ব!

---

## Series এর গঠন

একটা Series এ দুইটা জিনিস থাকে:

১. **Index** - বাম পাশে (label)
২. **Values** - ডান পাশে (actual data)

```python
pd_series = pd.Series(data=[5, 6, 7, 8, 9])
print(pd_series)

# 0    5   ← Index 0, Value 5
# 1    6   ← Index 1, Value 6
# 2    7   ← Index 2, Value 7
# 3    8   ← Index 3, Value 8
# 4    9   ← Index 4, Value 9
```

---

## Indexing - NumPy এর মতো

Series এ indexing একদম NumPy/Python list এর মতো:

```python
pd_series = pd.Series(data=[5, 6, 7, 8, 9])

# Index 0
print(pd_series[0])  # 5

# Index 2
print(pd_series[2])  # 7

# Negative indexing
print(pd_series[-1])  # 9 (শেষ element)
```

Output type হবে `numpy.int64` - কারণ ভিতরে NumPy array আছে।

---

## Custom Index - এইটা নতুন!

এখন মজা শুরু। আমরা নিজেদের মতো index দিতে পারি!

### Index দিয়ে Series তৈরি

```python
# Custom index
indices = ['a', 'b', 'c', 'd', 'e']
data = [5, 6, 7, 8, 9]

pd_series_2 = pd.Series(data=data, index=indices)
print(pd_series_2)
```

**Output:**
```
a    5
b    6
c    7
d    8
e    9
dtype: int64
```

এখন index হয়ে গেছে a, b, c, d, e!

### Custom Index দিয়ে Access

```python
# এখন এভাবে access করতে পারো
print(pd_series_2['a'])  # 5
print(pd_series_2['c'])  # 7
print(pd_series_2['e'])  # 9

# Numeric index ও কাজ করবে কিন্তু warning আসবে
print(pd_series_2[0])  # 5 (warning সহ)
```

Custom index থাকলে সেটা use করাই ভালো।

---

## Index দেখা এবং Change করা

### Index দেখা

```python
pd_series = pd.Series(data=[5, 6, 7, 8, 9])

# Index দেখো
print(pd_series.index)
```

**Output:**
```
RangeIndex(start=0, stop=5, step=1)
```

মনে পড়ছে? `range()` বা `np.arange()` এর মতো!

### Index Change করা

Existing Series এর index change করতে পারো:

```python
# আগে default index ছিল (0, 1, 2, 3, 4)
pd_series = pd.Series(data=[5, 6, 7, 8, 9])

# Custom index set করি
indices = ['a', 'b', 'c', 'd', 'e']
pd_series.index = indices

print(pd_series)
```

**Output:**
```
a    5
b    6
c    7
d    8
e    9
dtype: int64
```

Index overwrite হয়ে গেছে!

---

## Dictionary থেকে Series

Python dictionary মনে আছে? Key-value pairs?

Dictionary থেকে সরাসরি Series বানানো যায়!

```python
# Dictionary তৈরি
data_dict = {
    'a': 2,
    'b': 456,
    'c': 78
}

# Series বানাই
pd_series_3 = pd.Series(data=data_dict)
print(pd_series_3)
```

**Output:**
```
a      2
b    456
c     78
dtype: int64
```

দেখো - dictionary এর keys হয়ে গেছে index, আর values হয়ে গেছে data!

---

## Series vs Dictionary - Similarity

Custom index এর সাথে Series আর Dictionary অনেকটা same:

### Dictionary
```python
my_dict = {'a': 5, 'b': 6, 'c': 7}
print(my_dict['a'])  # 5
```

### Series
```python
my_series = pd.Series({'a': 5, 'b': 6, 'c': 7})
print(my_series['a'])  # 5
```

কিন্তু Series এ আরও অনেক powerful features আছে যা Dictionary তে নেই।

---

## Important Attributes

Series এর কিছু useful attributes:

```python
series = pd.Series([10, 20, 30, 40, 50])

# Values দেখো (NumPy array হিসেবে)
print(series.values)
# [10 20 30 40 50]

# Index দেখো
print(series.index)
# RangeIndex(start=0, stop=5, step=1)

# Data type
print(series.dtype)
# int64

# Size (কতগুলো element)
print(series.size)
# 5

# Shape
print(series.shape)
# (5,)
```

---

## Complete Examples

### Example 1: Student Marks

```python
# Dictionary দিয়ে
marks = {
    'Rahim': 85,
    'Karim': 92,
    'Salma': 78,
    'Nadia': 95
}

marks_series = pd.Series(marks)
print(marks_series)

# Output:
# Rahim    85
# Karim    92
# Salma    78
# Nadia    95
# dtype: int64

# Access করি
print(marks_series['Rahim'])  # 85
print(marks_series['Nadia'])  # 95
```

### Example 2: Temperature Data

```python
# NumPy array দিয়ে
temps = np.array([25, 28, 30, 27, 29])
days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri']

temp_series = pd.Series(data=temps, index=days)
print(temp_series)

# Output:
# Mon    25
# Tue    28
# Wed    30
# Thu    27
# Fri    29
# dtype: int32

# Wednesday এর temperature
print(temp_series['Wed'])  # 30
```

### Example 3: Custom Index Update

```python
# Default index দিয়ে শুরু
series = pd.Series([100, 200, 300, 400])
print("Before:")
print(series)

# Index change করি
series.index = ['A', 'B', 'C', 'D']
print("\nAfter:")
print(series)
```

**Output:**
```
Before:
0    100
1    200
2    300
3    400
dtype: int64

After:
A    100
B    200
C    300
D    400
dtype: int64
```

---

## NumPy Array vs Pandas Series

| Feature | NumPy Array | Pandas Series |
|---------|-------------|---------------|
| Index | শুধু numeric | যেকোনো type হতে পারে |
| Structure | শুধু values | Index + Values |
| Similarity | Python List | Python Dictionary |
| Use case | Mathematical operations | Data analysis |

---

## কেন Series ব্যবহার করবো?

১. **Custom Index** - meaningful labels দিতে পারো
২. **Dictionary like** - key দিয়ে access করা যায়
৩. **Powerful methods** - data analysis এর জন্য অনেক built-in functions
৪. **DataFrame এর base** - DataFrame বুঝতে হলে Series জানতে হবে

---

## Key Points মনে রাখো

1. Series = NumPy array + Custom Index
2. Two ways to create:
   - `pd.Series(data=array, index=labels)`
   - `pd.Series(dictionary)`
3. Index access করতে: `series.index`
4. Index change করতে: `series.index = new_indices`
5. Dictionary keys → Index, Dictionary values → Data

---

## Best Practices

### ১. Parameter Names Use করো

```python
# ❌ এড়িয়ে চলো
series = pd.Series([1, 2, 3], ['a', 'b', 'c'])

# ✅ এটা better
series = pd.Series(data=[1, 2, 3], index=['a', 'b', 'c'])
```

Clear হয় কোনটা data আর কোনটা index।

### ২. Index সংখ্যা Match করাও

```python
data = [1, 2, 3, 4]
index = ['a', 'b', 'c']  # ❌ 3টা index কিন্তু 4টা data

# Error দিবে!
# series = pd.Series(data=data, index=index)
```

Data আর index এর size same হতে হবে।

### ৩. Meaningful Index ব্যবহার করো

```python
# ❌ খারাপ
series = pd.Series([85, 90, 78], index=['a', 'b', 'c'])

# ✅ ভালো
series = pd.Series([85, 90, 78], index=['Math', 'English', 'Physics'])
```

---

## পরবর্তী ধাপ: DataFrame

Series শিখলে এখন DataFrame ready। DataFrame হলো multiple Series একসাথে - একটা complete table!

Series জানা থাকলে DataFrame বুঝতে সহজ হবে, কারণ DataFrame আসলে Series দের collection।

---

## Summary

- **Pandas** → Data analysis library
- **Series** → 1D labeled array
- **Custom Index** → Dictionary এর মতো
- **Creation** → NumPy array বা Dictionary থেকে
- **Next** → DataFrame শিখবো

Series হলো Pandas এর building block। এটা ভালো করে বুঝলে DataFrame অনেক সহজ লাগবে।

Happy Coding! 🚀