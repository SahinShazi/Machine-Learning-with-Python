# Pandas DataFrame - Merge & Join

## Merge এবং Join কি?

যখন দুইটা আলাদা DataFrame কে একসাথে করতে হয়, তখন **Merge** বা **Join** ব্যবহার করা হয়।

**Simple Example:**
- একটা DataFrame এ customer names
- আরেকটা DataFrame এ তাদের orders
- দুটোকে একসাথে করে full data পাও

**SQL জানো?** তাহলে `JOIN` operations মনে করো - একই concept!

---

## Merge vs Join

| Feature | Merge | Join |
|---------|-------|------|
| **Key Column** | নিজে specify করতে হয় | Default এ index use হয় |
| **Method** | `pd.merge()` (global) | `df.join()` (DataFrame method) |
| **Flexibility** | বেশি flexible | Simple cases এর জন্য |

**একমাত্র পার্থক্য:** Key column কিভাবে specify করা হয়!

---

## Types of Joins

### Visual Understanding

```
Left DataFrame:     Right DataFrame:
  K0, K1, K2, K3      K0, K2, K4, K5
  
Inner Join:   K0, K2        (শুধু common)
Left Join:    K0, K1, K2, K3  (left সব + common)
Right Join:   K0, K2, K4, K5  (right সব + common)
Outer Join:   K0, K1, K2, K3, K4, K5  (সব)
```

---

## Sample DataFrames

### Merge এর জন্য

```python
import pandas as pd

# Left DataFrame
left = pd.DataFrame({
    'key1': ['K0', 'K1', 'K2', 'K3'],
    'A': ['A0', 'A1', 'A2', 'A3'],
    'B': ['B0', 'B1', 'B2', 'B3']
})

# Right DataFrame
right = pd.DataFrame({
    'key1': ['K0', 'K2', 'K4', 'K5'],
    'C': ['C0', 'C2', 'C4', 'C5'],
    'D': ['D0', 'D2', 'D4', 'D5']
})

print("Left DataFrame:")
print(left)
print("\nRight DataFrame:")
print(right)
```

**Output:**
```
Left DataFrame:
  key1   A   B
0   K0  A0  B0
1   K1  A1  B1
2   K2  A2  B2
3   K3  A3  B3

Right DataFrame:
  key1   C   D
0   K0  C0  D0
1   K2  C2  D2
2   K4  C4  D4
3   K5  C5  D5
```

**Common keys:** K0, K2

---

## Inner Merge (Default)

### শুধু common keys

```python
# Inner merge
result = pd.merge(left, right, on='key1')
print(result)
```

**Output:**
```
  key1   A   B   C   D
0   K0  A0  B0  C0  D0
1   K2  A2  B2  C2  D2
```

**যা হলো:**
- শুধু K0 এবং K2 (common keys)
- সব columns একসাথে (A, B, C, D)

### Explicit Inner Join

```python
# 'how' parameter দিয়ে specify করা
result = pd.merge(left, right, on='key1', how='inner')
print(result)
```

Same result - কারণ `inner` হলো default!

---

## Outer Merge

### সব keys (missing = NaN)

```python
# Outer merge
result = pd.merge(left, right, on='key1', how='outer')
print(result)
```

**Output:**
```
  key1    A    B    C    D
0   K0   A0   B0   C0   D0
1   K1   A1   B1  NaN  NaN
2   K2   A2   B2   C2   D2
3   K3   A3   B3  NaN  NaN
4   K4  NaN  NaN   C4   D4
5   K5  NaN  NaN   C5   D5
```

**যা হলো:**
- সব keys আছে (K0-K5)
- যেখানে match নেই সেখানে `NaN`
- K1, K3 → right এ নেই → C, D = NaN
- K4, K5 → left এ নেই → A, B = NaN

---

## Left Merge

### Left DataFrame এর সব + common

```python
# Left merge
result = pd.merge(left, right, on='key1', how='left')
print(result)
```

**Output:**
```
  key1   A   B    C    D
0   K0  A0  B0   C0   D0
1   K1  A1  B1  NaN  NaN
2   K2  A2  B2   C2   D2
3   K3  A3  B3  NaN  NaN
```

**যা হলো:**
- Left এর সব keys (K0, K1, K2, K3)
- Right থেকে matching data
- K1, K3 → right এ নেই → NaN

---

## Right Merge

### Right DataFrame এর সব + common

```python
# Right merge
result = pd.merge(left, right, on='key1', how='right')
print(result)
```

**Output:**
```
  key1    A    B   C   D
0   K0   A0   B0  C0  D0
1   K2   A2   B2  C2  D2
2   K4  NaN  NaN  C4  D4
3   K5  NaN  NaN  C5  D5
```

**যা হলো:**
- Right এর সব keys (K0, K2, K4, K5)
- Left থেকে matching data
- K4, K5 → left এ নেই → NaN

---

## Join Method

### Join DataFrames তৈরি

```python
# Left DataFrame (custom index)
left_join = pd.DataFrame({
    'A': ['A0', 'A2', 'A3', 'A4', 'A6'],
    'B': ['B0', 'B2', 'B3', 'B4', 'B6']
}, index=[0, 2, 3, 4, 6])

# Right DataFrame (custom index)
right_join = pd.DataFrame({
    'C': ['C0', 'C1', 'C2', 'C3'],
    'D': ['D0', 'D1', 'D2', 'D3']
}, index=[0, 1, 2, 3])

print("Left:")
print(left_join)
print("\nRight:")
print(right_join)
```

**Output:**
```
Left:
    A   B
0  A0  B0
2  A2  B2
3  A3  B3
4  A4  B4
6  A6  B6

Right:
    C   D
0  C0  D0
1  C1  D1
2  C2  D2
3  C3  D3
```

**Common indices:** 0, 2, 3

---

## Basic Join

### Default = Left Join (on index)

```python
# Join (default left join)
result = left_join.join(right_join)
print(result)
```

**Output:**
```
    A   B    C    D
0  A0  B0   C0   D0
2  A2  B2   C2   D2
3  A3  B3   C3   D3
4  A4  B4  NaN  NaN
6  A6  B6  NaN  NaN
```

**যা হলো:**
- Left এর সব indices (0, 2, 3, 4, 6)
- Right থেকে matching values
- 4, 6 → right এ নেই → NaN

---

## Inner Join

### শুধু common indices

```python
# Inner join
result = left_join.join(right_join, how='inner')
print(result)
```

**Output:**
```
    A   B   C   D
0  A0  B0  C0  D0
2  A2  B2  C2  D2
3  A3  B3  C3  D3
```

শুধু 0, 2, 3 - যেগুলো দুই DataFrame এই আছে!

---

## Merge vs Join Summary

### Merge Example

```python
# Key column explicitly বলতে হবে
pd.merge(left, right, on='key1', how='inner')
```

### Join Example

```python
# Index automatically key হিসেবে use হয়
left_join.join(right_join, how='inner')
```

### Key Difference

```python
# Merge: Column নাম দিতে হয়
pd.merge(df1, df2, on='column_name')

# Join: Index use হয় (optional column specify করা যায়)
df1.join(df2)
```

---

## Practical Example

### Student Marks এবং Attendance

```python
# Marks DataFrame
marks = pd.DataFrame({
    'Student_ID': ['S1', 'S2', 'S3', 'S4'],
    'Math': [85, 92, 78, 88],
    'Science': [90, 85, 88, 92]
})

# Attendance DataFrame
attendance = pd.DataFrame({
    'Student_ID': ['S1', 'S2', 'S5', 'S6'],
    'Days_Present': [45, 42, 48, 40],
    'Total_Days': [50, 50, 50, 50]
})

print("Marks:")
print(marks)
print("\nAttendance:")
print(attendance)
```

**Output:**
```
Marks:
  Student_ID  Math  Science
0         S1    85       90
1         S2    92       85
2         S3    78       88
3         S4    88       92

Attendance:
  Student_ID  Days_Present  Total_Days
0         S1            45          50
1         S2            42          50
2         S5            48          50
3         S6            40          50
```

### Inner Merge - শুধু common students

```python
# S1, S2 দুই DataFrame এই আছে
result = pd.merge(marks, attendance, on='Student_ID', how='inner')
print(result)
```

**Output:**
```
  Student_ID  Math  Science  Days_Present  Total_Days
0         S1    85       90            45          50
1         S2    92       85            42          50
```

### Outer Merge - সব students

```python
# সব students, missing data = NaN
result = pd.merge(marks, attendance, on='Student_ID', how='outer')
print(result)
```

**Output:**
```
  Student_ID  Math  Science  Days_Present  Total_Days
0         S1  85.0     90.0          45.0        50.0
1         S2  92.0     85.0          42.0        50.0
2         S3  78.0     88.0           NaN         NaN
3         S4  88.0     92.0           NaN         NaN
4         S5   NaN      NaN          48.0        50.0
5         S6   NaN      NaN          40.0        50.0
```

Perfect! সবার data একসাথে, missing = NaN।

---

## Join Types Comparison

| Join Type | Result | Use Case |
|-----------|--------|----------|
| **Inner** | শুধু common | দুই DataFrame এ যারা আছে |
| **Outer** | সব (+ NaN) | Complete picture চাই |
| **Left** | Left সব + match | Main data preserve |
| **Right** | Right সব + match | Reference data preserve |

---

## Multiple Key Columns

একাধিক column দিয়ে merge:

```python
# Multiple keys
result = pd.merge(
    df1, 
    df2, 
    on=['key1', 'key2'],  # দুইটা column match করবে
    how='inner'
)
```

---

## Different Column Names

যদি key column এর নাম আলাদা হয়:

```python
# Left এ 'student_id', Right এ 'id'
result = pd.merge(
    df1, 
    df2, 
    left_on='student_id',
    right_on='id',
    how='inner'
)
```

---

## Key Points

1. **Merge** = দুই DataFrame একসাথে (column-wise)
2. **Key column** লাগে matching এর জন্য
3. **Inner** (default) = শুধু common
4. **Outer** = সব (missing = NaN)
5. **Left** = left সব + matching
6. **Right** = right সব + matching
7. **Join** = merge এর মতো কিন্তু index use করে
8. Missing values automatically `NaN` হয়

---

## Common Mistakes

### ❌ Mistake 1: Key column ভুলে যাওয়া

```python
pd.merge(left, right)  # Error! 'on' specify করো
```

### ✅ Solution:

```python
pd.merge(left, right, on='key1')
```

### ❌ Mistake 2: Join type না বুঝা

```python
# Inner join করলে data loss হতে পারে
result = pd.merge(df1, df2, how='inner')  # কিছু rows missing!
```

### ✅ Solution:

```python
# প্রথমে বুঝো কোনটা দরকার
# All data চাই? → outer
# Common only? → inner
```

---

## When to Use What?

### Use Merge যখন:
- Key column আলাদাভাবে আছে
- Multiple key columns
- Different column names

### Use Join যখন:
- Index based joining
- Quick operations
- Simple cases

---

## Quick Reference

```python
# Merge
pd.merge(left, right, on='key', how='inner')   # Common only
pd.merge(left, right, on='key', how='outer')   # All
pd.merge(left, right, on='key', how='left')    # Left + match
pd.merge(left, right, on='key', how='right')   # Right + match

# Join (index based)
df1.join(df2)                    # Default left
df1.join(df2, how='inner')       # Common indices
df1.join(df2, how='outer')       # All indices

# Different column names
pd.merge(df1, df2, left_on='col1', right_on='col2')

# Multiple keys
pd.merge(df1, df2, on=['key1', 'key2'])
```

---

## Practice Task

তোমার জন্য task:

1. দুইটা DataFrame বানাও (products এবং prices)
2. Inner merge করো
3. Outer merge করো
4. দেখো কি difference হয়
5. Left এবং Right merge try করো

```python
# তোমার code এখানে
products = pd.DataFrame({...})
prices = pd.DataFrame({...})
```

Happy Learning! 🚀