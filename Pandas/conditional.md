# Pandas DataFrame - Conditional Selection

## Conditional Selection কি?

Conditional Selection হলো DataFrame থেকে নির্দিষ্ট শর্ত অনুযায়ী data filter করা। এটা Pandas এর অন্যতম গুরুত্বপূর্ণ feature। 

**Real-world use case:**
- কোন product এর price 1000 টাকার বেশি?
- কোন student এর marks 80 এর উপরে?
- Temperature কোথায় 30 degree এর নিচে?

এই ধরনের প্রশ্নের উত্তর পেতে আমরা conditional selection ব্যবহার করি। Database এ এগুলোকে **query** বলা হয়।

---

## DataFrame তৈরি করা

প্রথমে একটা sample DataFrame বানাই:

```python
import numpy as np
import pandas as pd

# 8x5 random array (negative values সহ)
data = np.random.randn(8, 5)

# DataFrame তৈরি
df = pd.DataFrame(
    data=data,
    index=['Row1', 'Row2', 'Row3', 'Row4', 'Row5', 'Row6', 'Row7', 'Row8'],
    columns=['Col1', 'Col2', 'Col3', 'Col4', 'Col5']
)

print(df)
```

**Output:**
```
         Col1      Col2      Col3      Col4      Col5
Row1  0.234  -0.567   0.123   0.789  -0.456
Row2 -0.891   0.234   0.678  -0.345   0.901
Row3  0.456  -0.789   0.234   0.678  -0.345
Row4 -0.123   0.456  -0.789   0.234   0.567
Row5  0.678  -0.345   0.901  -0.567   0.234
Row6 -0.234   0.678  -0.345   0.789  -0.456
Row7  0.345  -0.123   0.567  -0.234   0.678
Row8 -0.456   0.789  -0.123   0.456  -0.234
```

এখানে positive এবং negative দুই ধরনের value আছে।

---

## Basic Conditional Check

### পুরো DataFrame এ Condition Apply করা

```python
# জিরো থেকে বড় কিনা check করি
result = df > 0
print(result)
```

**Output:**
```
        Col1   Col2   Col3   Col4   Col5
Row1   True  False   True   True  False
Row2  False   True   True  False   True
Row3   True  False   True   True  False
Row4  False   True  False   True   True
Row5   True  False   True  False   True
Row6  False   True  False   True  False
Row7   True  False   True  False   True
Row8  False   True  False   True  False
```

দেখো - একটা **Boolean DataFrame** পেয়েছি! যেখানে condition সত্য সেখানে `True`, মিথ্যা হলে `False`।

### এটা কিভাবে কাজ করে?

NumPy এর **broadcasting** এর মাধ্যমে। পুরো DataFrame এর প্রতিটা element এর সাথে 0 compare হচ্ছে।

---

## Boolean DataFrame দিয়ে Filtering

এখন এই Boolean DataFrame use করে actual filtering করি:

```python
# Boolean DataFrame তৈরি
bool_df = df > 0

# Filtering করি
filtered = df[bool_df]
print(filtered)
```

**Output:**
```
         Col1      Col2      Col3      Col4      Col5
Row1  0.234       NaN   0.123   0.789       NaN
Row2    NaN   0.234   0.678       NaN   0.901
Row3  0.456       NaN   0.234   0.678       NaN
Row4    NaN   0.456       NaN   0.234   0.567
Row5  0.678       NaN   0.901       NaN   0.234
Row6    NaN   0.678       NaN   0.789       NaN
Row7  0.345       NaN   0.567       NaN   0.678
Row8    NaN   0.789       NaN   0.456       NaN
```

**যা হয়েছে:**
- শুধু positive values দেখাচ্ছে
- যেখানে condition false, সেখানে `NaN` (null value)

### এক লাইনে করা

```python
# Direct filtering
result = df[df > 0]
print(result)
```

একই output! কিন্তু **ভাগ ভাগ করে শেখা ভালো** - তাহলে ভেতরে কি হচ্ছে বুঝবে।

---

## Column-wise Conditional Selection

### সমস্যা: NaN Values

উপরের পদ্ধতিতে অনেক `NaN` values আসে। পুরো row remove করলে data নষ্ট হবে। 

**সমাধান:** শুধু একটা column এ condition apply করো!

### Single Column Condition

```python
# শুধু Col3 check করি
condition = df['Col3'] > 0
print(condition)
```

**Output:**
```
Row1     True
Row2     True
Row3     True
Row4    False
Row5     True
Row6    False
Row7     True
Row8    False
Name: Col3, dtype: bool
```

একটা **Boolean Series** পেলাম!

### এই Condition দিয়ে Filtering

```python
# Boolean Series তৈরি
bool_series = df['Col3'] > 0

# Full DataFrame filter করি
result = df[bool_series]
print(result)
```

**Output:**
```
         Col1      Col2      Col3      Col4      Col5
Row1  0.234  -0.567   0.123   0.789  -0.456
Row2 -0.891   0.234   0.678  -0.345   0.901
Row3  0.456  -0.789   0.234   0.678  -0.345
Row5  0.678  -0.345   0.901  -0.567   0.234
Row7  0.345  -0.123   0.567  -0.234   0.678
```

**Perfect!** 
- Col3 এ যেসব row এ positive value আছে, শুধু সেগুলো এসেছে
- **কোন NaN নেই!**
- পুরো row intact আছে

### এক লাইনে

```python
# Direct column condition
result = df[df['Col3'] > 0]
print(result)
```

---

## Multiple Conditions

### ❌ Logical Operators (and, or) কাজ করে না

```python
# ❌ এটা error দিবে
result = df[(df['Col3'] > 0) or (df['Col5'] < 0)]
# ValueError: The truth value of a Series is ambiguous
```

**কেন error?**
- `and`/`or` operators শুধু single boolean value নিয়ে কাজ করে
- কিন্তু আমাদের কাছে **Series of booleans** আছে!

### ✅ Bitwise Operators ব্যবহার করতে হবে

| Logical | Bitwise | Meaning |
|---------|---------|---------|
| `and`   | `&`     | এবং (AND) |
| `or`    | `\|`    | অথবা (OR) |
| `not`   | `~`     | নয় (NOT) |

### OR Condition Example

```python
# ✅ সঠিক পদ্ধতি - bitwise OR (|)
condition = (df['Col3'] > 0) | (df['Col5'] < 0)
print(condition)
```

**Output:**
```
Row1     True
Row2     True
Row3     True
Row4     True
Row5     True
Row6     True
Row7     True
Row8     True
Name: bool, dtype: bool
```

### ⚠️ Important: Brackets লাগবে!

```python
# ❌ Brackets ছাড়া error
result = df[df['Col3'] > 0 | df['Col5'] < 0]
# TypeError: Cannot perform 'or' with float64 array

# ✅ প্রতিটা condition brackets এ রাখো
result = df[(df['Col3'] > 0) | (df['Col5'] < 0)]
```

**কেন brackets দরকার?**
- Bitwise operators এর precedence বেশি
- Brackets না দিলে wrong order এ execute হয়

### Filtering with Multiple Conditions

```python
# Boolean Series তৈরি
bool_series = (df['Col3'] > 0) | (df['Col5'] < 0)

# Filter করি
result = df[bool_series]
print(result)
```

**Output:**
```
         Col1      Col2      Col3      Col4      Col5
Row1  0.234  -0.567   0.123   0.789  -0.456
Row2 -0.891   0.234   0.678  -0.345   0.901
Row3  0.456  -0.789   0.234   0.678  -0.345
Row4 -0.123   0.456  -0.789   0.234   0.567
Row5  0.678  -0.345   0.901  -0.567   0.234
...
```

Col3 > 0 **অথবা** Col5 < 0 - যেকোনো একটা সত্য হলেই row আসবে!

---

## AND Condition Example

```python
# Col3 > 0 এবং Col5 < 0 (দুটোই সত্য হতে হবে)
condition = (df['Col3'] > 0) & (df['Col5'] < 0)
result = df[condition]
print(result)
```

**Output:**
```
         Col1      Col2      Col3      Col4      Col5
Row1  0.234  -0.567   0.123   0.789  -0.456
Row3  0.456  -0.789   0.234   0.678  -0.345
```

শুধু যেসব row এ **দুটো condition-ই true** সেগুলো এসেছে!

---

## Filtered DataFrame থেকে Specific Columns

### Step by Step

```python
# 1. Condition apply করে filter করি
result = df[(df['Col3'] > 0) | (df['Col5'] < 0)]

# 2. শুধু Col1 এবং Col2 চাই
final = result[['Col1', 'Col2']]
print(final)
```

**Output:**
```
         Col1      Col2
Row1  0.234  -0.567
Row2 -0.891   0.234
Row3  0.456  -0.789
Row5  0.678  -0.345
Row7  0.345  -0.123
```

### এক লাইনে Complete Query

```python
# সবকিছু একসাথে
result = df[(df['Col3'] > 0) | (df['Col5'] < 0)][['Col1', 'Col2']]
print(result)
```

**কিভাবে পড়বে:**
1. `df['Col3'] > 0` → Boolean Series
2. `df['Col5'] < 0` → আরেকটা Boolean Series
3. `(...) | (...)` → OR operation
4. `df[condition]` → Filtered DataFrame
5. `[['Col1', 'Col2']]` → Specific columns select

---

## Comparison Operators

সব standard comparison operators কাজ করে:

| Operator | Meaning | Example |
|----------|---------|---------|
| `>`      | বড়      | `df['Age'] > 25` |
| `<`      | ছোট     | `df['Price'] < 100` |
| `>=`     | বড় বা সমান | `df['Score'] >= 80` |
| `<=`     | ছোট বা সমান | `df['Temp'] <= 30` |
| `==`     | সমান    | `df['City'] == 'Dhaka'` |
| `!=`     | সমান নয় | `df['Status'] != 'Failed'` |

---

## Practical Examples

### Example 1: Student Data

```python
students = pd.DataFrame({
    'Name': ['Rahim', 'Karim', 'Salma', 'Nadia', 'Habib'],
    'Math': [85, 92, 65, 95, 72],
    'English': [78, 88, 92, 85, 68],
    'Science': [90, 85, 70, 92, 75]
})

# Math এ 80+ যারা পেয়েছে
high_math = students[students['Math'] >= 80]
print(high_math)
```

**Output:**
```
    Name  Math  English  Science
0  Rahim    85       78       90
1  Karim    92       88       85
3  Nadia    95       85       92
```

### Example 2: Multiple Subject Condition

```python
# Math এ 80+ এবং Science এ 85+
top_students = students[
    (students['Math'] >= 80) & (students['Science'] >= 85)
]
print(top_students)
```

**Output:**
```
    Name  Math  English  Science
0  Rahim    85       78       90
1  Karim    92       88       85
3  Nadia    95       85       92
```

### Example 3: OR with Column Selection

```python
# Math বা English এ 90+ যারা পেয়েছে, তাদের Name এবং Math
result = students[
    (students['Math'] >= 90) | (students['English'] >= 90)
][['Name', 'Math', 'English']]

print(result)
```

**Output:**
```
    Name  Math  English
1  Karim    92       88
2  Salma    65       92
3  Nadia    95       85
```

---

## Common Patterns

### Pattern 1: Range Check

```python
# 70 থেকে 90 এর মধ্যে
mid_range = students[
    (students['Math'] >= 70) & (students['Math'] <= 90)
]
```

### Pattern 2: Negative Condition

```python
# Math এ 80 এর নিচে
below_80 = students[students['Math'] < 80]

# অথবা NOT operator দিয়ে
not_high = students[~(students['Math'] >= 80)]
```

### Pattern 3: Multiple OR

```python
# তিনটা subject এর যেকোনো একটায় 90+
any_high = students[
    (students['Math'] >= 90) |
    (students['English'] >= 90) |
    (students['Science'] >= 90)
]
```

---

## Step-by-Step vs One-liner

### Step-by-Step (Learning এর জন্য ভালো)

```python
# 1. Condition তৈরি
cond1 = df['Col3'] > 0
cond2 = df['Col5'] < 0

# 2. Combine করি
combined = cond1 | cond2

# 3. Filter করি
filtered = df[combined]

# 4. Columns select করি
result = filtered[['Col1', 'Col2']]
```

### One-liner (Quick কাজের জন্য)

```python
result = df[(df['Col3'] > 0) | (df['Col5'] < 0)][['Col1', 'Col2']]
```

**কোনটা use করবে?**
- **শেখার সময়:** Step-by-step → বুঝতে সহজ
- **কাজের সময়:** One-liner → দ্রুত, efficient
- **Complex queries:** Step-by-step → debug করতে সহজ

---

## Key Points

1. **Comparison operators** সব DataFrame/Series এ কাজ করে
2. Result সবসময় **Boolean DataFrame/Series**
3. Boolean indexing দিয়ে **filtering** হয়
4. **Column-wise condition** ভালো - NaN এড়াতে
5. Multiple conditions এর জন্য **bitwise operators** (`&`, `|`, `~`)
6. **Logical operators** (`and`, `or`, `not`) কাজ করে না!
7. প্রতিটা condition **brackets** এ রাখতে হবে
8. এক লাইনে করা possible কিন্তু **ভাগ ভাগ করে শেখো**

---

## Common Mistakes এবং Solutions

### ❌ Mistake 1: Logical Operators

```python
# Wrong
df[(df['A'] > 0) and (df['B'] < 0)]  # Error!
```

**✅ Solution:**
```python
# Correct
df[(df['A'] > 0) & (df['B'] < 0)]
```

### ❌ Mistake 2: Missing Brackets

```python
# Wrong - precedence issue
df[df['A'] > 0 | df['B'] < 0]  # Error!
```

**✅ Solution:**
```python
# Correct
df[(df['A'] > 0) | (df['B'] < 0)]
```

### ❌ Mistake 3: NaN Problems

```python
# পুরো DataFrame এ condition - অনেক NaN
result = df[df > 0]  # Not recommended
```

**✅ Solution:**
```python
# Specific column এ condition
result = df[df['ColName'] > 0]  # Better!
```

---

## Why Conditional Selection Important?

1. **Data Analysis** - Specific data খুঁজে বের করা
2. **Filtering** - Unwanted data বাদ দেওয়া
3. **Queries** - Database-style questions এর উত্তর
4. **Preprocessing** - ML এর জন্য data prepare করা
5. **Business Logic** - Real-world conditions apply করা

**Example Real Use Cases:**
- Sales > 10000 এর customers
- Temperature > 35°C এর দিনগুলো
- Stock price যেদিন 5% বেড়েছে
- Failed students (marks < 40)

---

## Practice Tips

1. ✅ **ভাগ ভাগ করে** condition তৈরি করো
2. ✅ প্রতিটা step এ `print()` করে **verify** করো
3. ✅ Different columns এ **experiment** করো
4. ✅ `&` এবং `|` দুটোই **practice** করো
5. ✅ Complex queries এ **intermediate variables** use করো
6. ✅ One-liner এ যাওয়ার **আগে** step-by-step শেখো

---

## Next Steps

এখন যা জানো:
- Basic conditional selection
- Boolean indexing
- Multiple conditions (`&`, `|`)
- Column selection with filtering
- One-liner queries

পরবর্তীতে শিখবো:
- `.isin()` method (multiple values check)
- `.between()` method (range checking)
- `.query()` method (SQL-style)
- String conditions (`str.contains()`, etc.)
- Complex filtering with functions

Happy Learning! 🚀