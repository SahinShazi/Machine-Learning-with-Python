# Pandas DataFrame - Missing Values (dropna & fillna)

## Missing Values কি?

Real-world data সবসময় perfect হয় না। অনেক সময় কিছু values **missing** থাকে বা **empty** থাকে। এগুলোকে বলা হয় **Missing Values** বা **Null Values**।

**Example:**
- Survey data তে কেউ কিছু প্রশ্নের উত্তর দেয়নি
- Sensor failure এ data record হয়নি
- Manual entry তে ভুলে data skip হয়েছে
- Database থেকে কিছু data lost হয়েছে

Pandas এ missing values দেখায় `NaN` (Not a Number) দিয়ে।

---

## Sample DataFrame তৈরি করা

```python
import numpy as np
import pandas as pd

# Missing values সহ DataFrame
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, 7, 8],
    'C': [9, 10, 11, 12]
})

print(df)
```

**Output:**
```
     A    B   C
0  1.0  5.0   9
1  2.0  NaN  10
2  NaN  7.0  11
3  4.0  8.0  12
```

দেখো - কিছু জায়গায় `NaN` আছে। এগুলোই missing values।

---

## .dropna() - Missing Values Remove করা

### Basic Usage

`.dropna()` method missing values বিশিষ্ট rows বা columns remove করে দেয়।

```python
# Missing values বিশিষ্ট rows remove
result = df.dropna()
print(result)
```

**Output:**
```
     A    B   C
0  1.0  5.0   9
3  4.0  8.0  12
```

**যা হয়েছে:**
- Row 0 এবং Row 3 রয়ে গেছে (কোন NaN নেই)
- Row 1 এবং Row 2 remove হয়েছে (NaN ছিল)

### Default Behavior

By default, `.dropna()` **rows** remove করে (axis=0)।

---

## axis Parameter

### axis=0 (Rows remove - Default)

```python
# Row remove (default)
result = df.dropna(axis=0)
print(result)
```

**Output:**
```
     A    B   C
0  1.0  5.0   9
3  4.0  8.0  12
```

### axis=1 (Columns remove)

```python
# Column remove
result = df.dropna(axis=1)
print(result)
```

**Output:**
```
    C
0   9
1  10
2  11
3  12
```

**যা হয়েছে:**
- Column C রয়ে গেছে (কোন NaN নেই)
- Column A এবং B remove হয়েছে (NaN ছিল)

⚠️ **সতর্কতা:** Column remove করা বিপজ্জনক! পুরো feature চলে যায়। **এড়িয়ে চলো!**

---

## inplace Parameter

### Without inplace (Default)

```python
# Original DataFrame unchanged
result = df.dropna()
print(df)  # Still has NaN values
```

Default এ original DataFrame এ কোন পরিবর্তন হয় না।

### With inplace=True

```python
# Permanent change
df.dropna(inplace=True)
print(df)  # NaN rows removed permanently
```

`inplace=True` দিলে original DataFrame-ই change হয়ে যায়।

---

## thresh Parameter - Threshold

### thresh কি?

`thresh` (threshold) বলে দেয় কমপক্ষে কতগুলো **non-null values** থাকলে row রাখবে।

### Example: thresh=3

```python
# DataFrame আবার দেখি
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, 7, 8],
    'C': [9, 10, 11, 12]
})

print(df)
```

**Output:**
```
     A    B   C
0  1.0  5.0   9   → 3টা non-null
1  2.0  NaN  10   → 2টা non-null
2  NaN  7.0  11   → 2টা non-null
3  4.0  8.0  12   → 3টা non-null
```

```python
# কমপক্ষে 3টা non-null value লাগবে
result = df.dropna(thresh=3)
print(result)
```

**Output:**
```
     A    B   C
0  1.0  5.0   9
3  4.0  8.0  12
```

**Logic:**
- Row 0: 3টা non-null → ✅ Keep
- Row 1: 2টা non-null → ❌ Remove
- Row 2: 2টা non-null → ❌ Remove  
- Row 3: 3টা non-null → ✅ Keep

### Example: thresh=2

```python
# কমপক্ষে 2টা non-null value লাগবে
result = df.dropna(thresh=2)
print(result)
```

**Output:**
```
     A    B   C
0  1.0  5.0   9
1  2.0  NaN  10
2  NaN  7.0  11
3  4.0  8.0  12
```

শুধু Row 1 এবং Row 2 ও রয়ে গেছে কারণ তাদের 2টা করে non-null আছে!

### Example: thresh=5 (সব remove!)

```python
# কমপক্ষে 5টা non-null লাগবে (অসম্ভব!)
result = df.dropna(thresh=5)
print(result)
```

**Output:**
```
Empty DataFrame
Columns: [A, B, C]
Index: []
```

কেন? কারণ মাত্র 3টা column আছে, কখনো 5টা non-null হতে পারে না!

---

## .fillna() - Missing Values Fill করা

### কেন Fill করবো?

Row remove করলে data loss হয়। Better approach হলো missing values কে কোন value দিয়ে **fill** করা।

### Basic Usage

```python
# সব NaN কে 'MISSING' দিয়ে fill করি
result = df.fillna('MISSING')
print(result)
```

**Output:**
```
        A        B   C
0     1.0      5.0   9
1     2.0  MISSING  10
2  MISSING      7.0  11
3     4.0      8.0  12
```

### Numeric Value দিয়ে Fill

```python
# সব NaN কে 0 দিয়ে fill করি
result = df.fillna(0)
print(result)
```

**Output:**
```
     A    B   C
0  1.0  5.0   9
1  2.0  0.0  10
2  0.0  7.0  11
3  4.0  8.0  12
```

---

## Column-wise Fill করা (Mean/Median)

### সমস্যা: সব column একসাথে fill না করে আলাদা করতে চাই

Best practice হলো প্রতিটা column এর missing values সেই column এর **mean** বা **median** দিয়ে fill করা।

### Old Method (Deprecated in Pandas 3.0)

```python
# ❌ এটা Pandas 3.0 তে কাজ করবে না
df['A'].fillna(df['A'].mean(), inplace=True)  # Warning!
```

⚠️ **Warning:** Series এ `.fillna()` with `inplace=True` deprecated হয়ে যাচ্ছে!

### ✅ New Method (Recommended)

Dictionary দিয়ে specific columns fill করতে হবে:

```python
# Column A এর mean দিয়ে fill করি
df.fillna({'A': df['A'].mean()}, inplace=True)
print(df)
```

**Output:**
```
          A    B   C
0       1.0  5.0   9
1       2.0  NaN  10
2  2.333333  7.0  11   ← A এর mean = (1+2+4)/3
3       4.0  8.0  12
```

### Multiple Columns Fill করা

```python
# Reset DataFrame
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, 7, 8],
    'C': [9, 10, 11, 12]
})

# A এর mean, B এর 0 দিয়ে fill
df.fillna({
    'A': df['A'].mean(),
    'B': 0
}, inplace=True)

print(df)
```

**Output:**
```
          A    B   C
0       1.0  5.0   9
1       2.0  0.0  10
2  2.333333  7.0  11
3       4.0  8.0  12
```

---

## Practical Examples

### Example 1: Student Marks

```python
students = pd.DataFrame({
    'Name': ['Rahim', 'Karim', 'Salma', 'Nadia'],
    'Math': [85, np.nan, 78, 92],
    'English': [88, 92, np.nan, 85],
    'Science': [90, 85, 88, 92]
})

print("Original:")
print(students)
```

**Output:**
```
    Name  Math  English  Science
0  Rahim  85.0     88.0       90
1  Karim   NaN     92.0       85
2  Salma  78.0      NaN       88
3  Nadia  92.0     85.0       92
```

#### Solution 1: Drop rows (Not recommended - data loss!)

```python
clean = students.dropna()
print(clean)
```

**Output:**
```
    Name  Math  English  Science
0  Rahim  85.0     88.0       90
3  Nadia  92.0     85.0       92
```

Karim আর Salma চলে গেছে! 😢

#### Solution 2: Fill with mean (Better!)

```python
students.fillna({
    'Math': students['Math'].mean(),
    'English': students['English'].mean()
}, inplace=True)

print(students)
```

**Output:**
```
    Name  Math  English  Science
0  Rahim  85.0     88.0       90
1  Karim  85.0     92.0       85   ← Math mean = 85
2  Salma  78.0     88.33      88   ← English mean = 88.33
3  Nadia  92.0     85.0       92
```

Perfect! সবাই আছে এবং reasonable values দিয়ে fill হয়েছে।

---

## Comparison: dropna() vs fillna()

| Aspect | dropna() | fillna() |
|--------|----------|----------|
| **Data Loss** | ❌ হ্যাঁ (rows/columns remove) | ✅ না (values fill করে) |
| **Use Case** | Very few missing values | Many missing values |
| **Best For** | Large datasets | Small datasets |
| **Risk** | Information loss | Wrong assumptions |
| **When to Use** | <5% missing data | >5% missing data |

### Decision Guide

```python
# Missing data চেক করি
missing_percent = (df.isnull().sum() / len(df)) * 100
print(missing_percent)

# Output:
# A    25.0   ← 25% missing
# B    25.0
# C     0.0
# dtype: float64

# Decision:
if missing_percent['A'] < 5:
    df.dropna(subset=['A'], inplace=True)  # Drop if <5%
else:
    df.fillna({'A': df['A'].mean()}, inplace=True)  # Fill if >5%
```

---

## Different Fill Strategies

### 1. Fill with Mean (গড়)

```python
df.fillna({'A': df['A'].mean()})
```

**Best for:** Normally distributed numerical data

### 2. Fill with Median (মধ্যমা)

```python
df.fillna({'A': df['A'].median()})
```

**Best for:** Skewed data (outliers থাকলে)

### 3. Fill with Mode (সর্বোচ্চ সংখ্যক)

```python
df.fillna({'A': df['A'].mode()[0]})
```

**Best for:** Categorical data

### 4. Forward Fill (আগের value)

```python
df.fillna(method='ffill')
```

**Best for:** Time series data

### 5. Backward Fill (পরের value)

```python
df.fillna(method='bfill')
```

**Best for:** Time series data

---

## Common Patterns

### Pattern 1: Drop rows with ANY null

```python
# যেকোনো column এ NaN থাকলে drop
df.dropna()  # or df.dropna(how='any')
```

### Pattern 2: Drop rows with ALL null

```python
# সব column এ NaN থাকলে drop
df.dropna(how='all')
```

### Pattern 3: Drop specific column এর NaN

```python
# শুধু column A এর NaN rows drop
df.dropna(subset=['A'])
```

### Pattern 4: Fill সব columns

```python
# সব numerical columns এর mean দিয়ে fill
numeric_columns = df.select_dtypes(include=[np.number]).columns

fill_values = {col: df[col].mean() for col in numeric_columns}
df.fillna(fill_values, inplace=True)
```

---

## Pandas 3.0 Update ⚠️

### What's Changing?

Pandas 3.0 থেকে Series এ `.fillna()` with `inplace=True` deprecated হয়ে যাচ্ছে।

### ❌ Old Way (Deprecated)

```python
# This will show warning and won't work in Pandas 3.0
df['A'].fillna(df['A'].mean(), inplace=True)
```

### ✅ New Way (Recommended)

```python
# Use dictionary in DataFrame.fillna()
df.fillna({'A': df['A'].mean()}, inplace=True)
```

### Check Your Pandas Version

```python
import pandas as pd
print(pd.__version__)

# If version < 3.0: Old way still works (with warning)
# If version >= 3.0: Must use new way
```

---

## Best Practices

### ✅ DO

1. **Analyze first** - কতটুকু missing data আছে check করো
2. **Use mean/median** - Numerical data তে
3. **Domain knowledge** - যা logical সেটা use করো
4. **Document** - কিভাবে fill করলে note করো
5. **Test impact** - Fill করার পর result check করো

### ❌ DON'T

1. **Don't drop columns** - Feature loss হয়
2. **Don't fill blindly** - কি দিয়ে fill করছো চিন্তা করো
3. **Don't ignore** - Missing data analysis করো
4. **Don't use wrong strategy** - Text data তে mean দিও না!
5. **Don't forget inplace** - Permanent change চাইলে `inplace=True`

---

## Key Points

1. **Missing Values** = `NaN` in Pandas
2. **`.dropna()`** removes rows/columns with NaN
3. **Default axis=0** (rows remove)
4. **axis=1** columns remove করে (avoid!)
5. **thresh** parameter minimum non-null values specify করে
6. **`.fillna()`** missing values fill করে
7. **Column-wise fill** better (mean/median use করো)
8. **Dictionary syntax** recommended for specific columns
9. **inplace=True** permanent changes করে
10. **Pandas 3.0** এ Series.fillna() deprecated হচ্ছে

---

## Workflow Suggestion

```python
# Step 1: Check missing data
print(df.isnull().sum())

# Step 2: Visualize (if possible)
# import matplotlib.pyplot as plt
# df.isnull().sum().plot(kind='bar')

# Step 3: Decide strategy
# - <5% missing → dropna()
# - >5% missing → fillna()

# Step 4: Implement
if missing_percentage < 5:
    df.dropna(subset=['important_column'], inplace=True)
else:
    df.fillna({
        'numeric_col': df['numeric_col'].mean(),
        'category_col': df['category_col'].mode()[0]
    }, inplace=True)

# Step 5: Verify
print(df.isnull().sum())  # Should be 0
```

---

## Quick Reference

```python
# Check missing values
df.isnull().sum()
df.isna().sum()  # Same as isnull()

# Drop methods
df.dropna()                    # Drop rows with any NaN
df.dropna(axis=1)             # Drop columns with any NaN
df.dropna(how='all')          # Drop only if all values are NaN
df.dropna(subset=['A', 'B'])  # Drop based on specific columns
df.dropna(thresh=2)           # Keep rows with ≥2 non-null values

# Fill methods
df.fillna(0)                  # Fill all with 0
df.fillna({'A': 0, 'B': 1})  # Fill specific columns
df.fillna(method='ffill')     # Forward fill
df.fillna(method='bfill')     # Backward fill

# Statistical fills
df.fillna({'A': df['A'].mean()})     # Mean
df.fillna({'A': df['A'].median()})   # Median
df.fillna({'A': df['A'].mode()[0]})  # Mode

# Permanent changes
df.dropna(inplace=True)
df.fillna(0, inplace=True)
```

---

## Real-World Example

```python
# E-commerce sales data
sales = pd.DataFrame({
    'Product': ['A', 'B', 'C', 'D', 'E'],
    'Price': [100, np.nan, 150, 200, np.nan],
    'Quantity': [5, 10, np.nan, 8, 12],
    'Discount': [10, 15, 20, np.nan, 25]
})

print("Original:")
print(sales)

# Strategy:
# - Price: Fill with mean (numerical)
# - Quantity: Fill with median (can have outliers)
# - Discount: Fill with 0 (no discount assumed)

sales.fillna({
    'Price': sales['Price'].mean(),
    'Quantity': sales['Quantity'].median(),
    'Discount': 0
}, inplace=True)

print("\nCleaned:")
print(sales)
```

---

## Practice Exercise

তোমার data এ missing values handle করো:

```python
# Sample data
df = pd.DataFrame({
    'Name': ['Rahim', 'Karim', 'Salma', 'Nadia', 'Habib'],
    'Age': [25, np.nan, 30, 28, np.nan],
    'Salary': [50000, 60000, np.nan, 70000, 55000],
    'City': ['Dhaka', 'Chittagong', np.nan, 'Sylhet', 'Dhaka']
})

# Tasks:
# 1. কতটুকু missing data আছে?
# 2. Age fill করো mean দিয়ে
# 3. Salary fill করো median দিয়ে
# 4. City এর NaN row drop করো
# 5. Final result দেখাও
```

---

Happy Learning! 🚀