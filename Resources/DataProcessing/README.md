# 📚 Python+AI Core Knowledge Learning Guide (Based on 5 Real-World Files)

Tailored for your current coding level, I've designed a **"Imitate → Understand → Modify → Create"** learning path to transform these code snippets into memorable knowledge cards.

---

## **I. Quick Reference Table of Core Knowledge (Memorize the Skeleton First)**

### **🎯 "Universal Opening Sequence" Used in All Files**
```python
import pandas as pd
import numpy as np
data = pd.read_csv('xxx.csv')  # Read data
# ... various processing ...
data.to_csv('xxx_cleaned.csv', index=False)  # Save data
```
**Memory Rhyme**: "Read→Modify→Save" three steps, don't forget `index=False`

---

### **📊 Data Cleaning Four Musketeers (Must Memorize)**

| Operation           | Code                               | Purpose                                 | Common Mistakes                                              |
| ------------------- | ---------------------------------- | --------------------------------------- | ------------------------------------------------------------ |
| **Drop Nulls**      | `data.dropna()`                    | Delete entire rows with any empty cells | Deletes too much? Use `subset=['col_name']` to target specific columns |
| **Drop Duplicates** | `data.drop_duplicates()`           | Delete completely identical rows        | Accidentally deleting valid data? Add `keep='last'` to keep the newest |
| **Convert Type**    | `data['col'].astype(int)`          | Change strings to numbers               | Letters can't convert? Use `pd.to_numeric(errors='coerce')` first |
| **Remove Outliers** | `data[data['Age'].between(18,70)]` | Keep only rows where age is 18-70       | Too many conditions? Connect with `&`, wrap each condition in parentheses |

**Memory Trick**: Imagine you're cleaning an Excel sheet—these four steps are the **standard process for filtering out bad data**.

---

### **🔍 Data Validation "Stamp Method" (Core Logic in Files 1 & 5)**

```python
# Stamp each row with four stamps: Age Stamp, Income Stamp, Loan Stamp, Credit Stamp
data['is_age_valid'] = data['Age'].between(18,70)          # Valid=True
data['is_income_valid'] = data['Income'] > 2000            # Valid=True
data['is_loan_valid'] = data['LoanAmount'] < data['Income']*5
data['is_credit_valid'] = data['CreditScore'].between(300,850)

# Master Stamp: Only valid if ALL stamps are True
data['is_valid'] = data[['is_age_valid','is_income_valid','is_loan_valid','is_credit_valid']].all(axis=1)

# Keep only rows with valid master stamp
cleaned_data = data[data['is_valid'] == True]
```
**Memory Rhyme**: **"Single Column Stamping → Master Stamping → Rip Out Bad Rows"**  
**axis=1**: Check horizontally across a row for all stamps  
**all()**: Must be ALL True to count as True

---

### **📈 Group Statistics "Cake-Cutting Method" (Used in Files 2, 3, 4, 5)**

```python
# Group by SensorType, count rows in each group, calculate each group's mean
data.groupby('SensorType')['Value'].agg(['count','mean'])

# Group by Location and SensorType, calculate temperature/humidity mean for each location
data.groupby(['Location','SensorType'])['Value'].mean()

# Group by Gender, calculate average speed for males and females
data.groupby('Gender')['Speed'].mean()
```
**Memory Trick**: `groupby` is like **cutting a cake**—first slice by a standard (gender/location), then **count people** or **calculate averages** for each slice.

---

## **II. Memory Strengthening Training (15-Minute Daily Practice)**

### **🎵 Memory Rhyme (Sing 3 Times to Remember)**
```
pandas pandas read data,
dropna dropna delete blanks,
astype astype convert format,
groupby groupby cut the cake,
to_csv to_csv save it well,
index False don't forget!
```

### **🧩 Code Fill-in-the-Blank Cards (Print and Write by Hand)**
```python
import ______ as pd  # Import data processing library
data = pd.______('file.csv')  # Read CSV file

# Delete null values
data = data.______()

# Convert to integer type
data['Age'] = data['Age'].______(int)

# Keep only 18-70 year olds
data = data[data['Age'].______(18, 70)]

# Group by gender and calculate average
result = data.______('Gender')['Income'].______()

# Save file
data.______('cleaned.csv', index=False)
```
**Answer Key**: `pandas`, `read_csv`, `dropna()`, `astype`, `between`, `groupby`, `mean()`, `to_csv`

---

### **🔁 "Change One Number" Practice Method (Critical!)**

Take **File 3's sensor_data.ipynb** for practice—change only one thing each time and observe the result:

1. **Change Temperature Range**: Modify `-10` to `0`, see how the abnormal count changes from 1011
2. **Change Grouping Method**: Change `groupby('SensorType')` to `groupby('Location')`, see how output changes
3. **Change Save Name**: Change `'sensor_data_cleaned.csv'` to `'my_data.csv'`, see if the file is generated

**Purpose**: Through **tiny modifications**, feel the cause-and-effect relationship in code and build muscle memory.

---

## **III. Common Errors First Aid Kit (Refer When Spotted)**

### **Error 1: `SettingWithCopyWarning`**
```python
# Wrong way
data['Age'][0] = 25  # Will warn

# Correct way
data.loc[0, 'Age'] = 25  # Use loc to locate
```
**Memory Aid**: Want to modify **a specific cell** in the table? Must use `loc[row, column]`

### **Error 2: `KeyError: 'column_name'`**
```python
# Error reason: Column name typo, or column doesn't exist
data['Age']  # Will error if CSV has 'AGE' or 'age'

# Check method
print(data.columns)  # Print all column names to see
```
**Memory Aid**: Before operating, always `print(data.columns)` to confirm case sensitivity

### **Error 3: `TypeError: unsupported operand`**
```python
# Error reason: Mixing strings and numbers
data['Age'] + 10  # Will error if Age column contains '25' strings

# Fix
data['Age'] = data['Age'].astype(int)  # Convert to integer first
```
**Memory Aid**: Before any calculation, always use `.astype()` to confirm format

---

## **IV. Ultimate Advice (From a Level 3 Trainer)**

1. **Don't Memorize Code, Memorize Patterns**: These 5 files are essentially the **"Read→Clean→Validate→Aggregate→Save"** 5 patterns—everything else is variation.

2. **Build Your Own "Code LEGO Library"**: Save commonly used code blocks (like `groupby count`, `between filter`) as snippets, copy and modify parameters when needed.

3. **Comments Are More Important Than Code**: Add comments after every line NOW explaining "what this line does"—you won't be lost when reviewing in 3 days.

4. **Data Is AI's Food**: What you're learning isn't "writing code," but **"preparing clean food to feed AI models"**—this mindset makes you value every step's significance.

5. **Make It Run First, Then Optimize**: Don't obsess over "which line is more elegant"—first make the program run and produce results, then consider improvements. Elegant code that doesn't run is useless.

## Homework

**📌 Comprehensive Practice Task List**

**Phase 1: Data Cleaning (using 6_main_orders.csv)**

- **Drop Nulls**: Delete rows containing any null values (missing quantity, customer_rating, etc.)
- **Drop Duplicates**: Remove duplicate orders based on `order_id` (keep the first occurrence)
- **Convert Type**: Convert `order_date` to datetime format
- **Remove Outliers**: Delete rows where `quantity` ≤ 0 or > 10, `unit_price` ≤ 0, or `customer_rating` < 1 or > 5
- **Rename Columns**: Rename `total_amount` to `revenue`
- **Reorder Columns**: Move `order_id`, `customer_id`, `order_date` to the front
- **Filter Rows**: Keep only orders where `product_category` is "Electronics" AND `revenue` > 500
- **Sort Data**: Sort by `revenue` in descending order
- **Add New Columns**: Add `is_high_value` column (True if revenue > 1000)
- **Validation**: Count rows before/after cleaning

**Phase 2: Data Merging (with 6_customer_info.csv)**

- **Merge**: Merge `main_orders.csv` and `customer_info.csv` on `customer_id`
- **Filter**: Keep only orders where `customer_tier` is "Gold" AND `age` is between 25-45
- **Add Column**: Add `profit_margin` column (assume cost is 70% of revenue)
- **Group & Aggregate**: Group by `customer_tier`, calculate average revenue, average age, and order count
- **Sort**: Sort by `lifetime_value` in descending order
- **Select Columns**: Keep only `customer_name`, `age`, `customer_tier`, `revenue`, `profit_margin`
- **Save**: Save result as `final_analysis.csv`

**All exercises must count rows: original/cleaned/final**  
**Suggest using assert to verify data integrity**  
**Use print() after each step to view results**

## This line is the **atomic operation** of pandas data analysis, executing "split-apply-combine" in a single expression.

### Execution Flow Visualization

Given `target_customers` DataFrame:

```python
# Source Data
   customer_id customer_tier  revenue  age  order_id
0        C001          Gold  1799.98   28    ORD001
1        C004          Gold  1299.99   31    ORD004
2        C002        Silver   179.97   35    ORD002
3        C005        Silver   159.96   29    ORD005
4        C003        Bronze    64.95   42    ORD003
```

---

### Step 1: Split
```python
.groupby('customer_tier', observed=False)
```

**Effect**: Partitions DataFrame by distinct values in `customer_tier`  
**observed=False**: Includes **all** possible tier categories (even if empty in data)  
**Result**:
```
Gold Group:
├─ C001: revenue=1799.98, age=28, order_id=ORD001
└─ C004: revenue=1299.99, age=31, order_id=ORD004

Silver Group:
├─ C002: revenue=179.97, age=35, order_id=ORD002
└─ C005: revenue=159.96, age=29, order_id=ORD005

Bronze Group:
└─ C003: revenue=64.95, age=42, order_id=ORD003
```

---

### Step 2: Apply
```python
.agg(
    avg_revenue=('revenue', 'mean'),    # Mean of revenue column
    avg_age=('age', 'mean'),            # Mean of age column
    total_orders=('order_id', 'count')  # Count of non-null order_id
)
```

**Effect**: Applies specified functions **to each group independently**  
**Dictionary syntax**: `new_column_name = ('source_column', 'aggregation_function')`  
**Key functions**:
- `mean`: Arithmetic average
- `count`: Non-null value count
- Others: `sum`, `max`, `min`, `nunique` (distinct count)

**Computation**:
```
Gold Group:
├─ avg_revenue = (1799.98 + 1299.99) / 2 = 1549.99
├─ avg_age     = (28 + 31) / 2 = 29.5
└─ total_orders = 2

Silver Group:
├─ avg_revenue = (179.97 + 159.96) / 2 = 169.97
├─ avg_age     = (35 + 29) / 2 = 32.0
└─ total_orders = 2

Bronze Group:
├─ avg_revenue = 64.95
├─ avg_age     = 42.0
└─ total_orders = 1
```

---

### Step 3: Combine (Reset Index)
```python
.reset_index()
```

**Effect**: Converts grouping key from **index** to **regular column**  
**Comparison**:

```python
# Before reset_index()
               avg_revenue  avg_age  total_orders
customer_tier                                    
Gold              1549.99     29.5             2
Silver             169.97     32.0             2
Bronze              64.95     42.0             1
# customer_tier is ROW INDEX

# After reset_index()
  customer_tier  avg_revenue  avg_age  total_orders
0          Gold      1549.99     29.5             2
1        Silver       169.97     32.0             2
2        Bronze        64.95     42.0             1
# customer_tier is REGULAR COLUMN
```

---

### Best Practices & Design Rationale

#### ✅ Why Dictionary Syntax?
```python
# Recommended: Explicit naming and operations
agg(
    avg_revenue=('revenue', 'mean'),
    total_revenue=('revenue', 'sum')
)

# Not recommended: Ambiguous column names
agg(['mean', 'sum'])  # Generates 'revenue_mean', 'revenue_sum'
```

#### ✅ Multi-Column Grouping
```python
# Group by two dimensions
.groupby(['customer_tier', 'product_category'], observed=False)
```

#### ✅ Conditional Aggregation
```python
# Average revenue for high-value orders only
agg(
    avg_revenue=('revenue', lambda x: x[x>1000].mean())
)
```

#### ⚠️ Common Pitfall
**Forgetting `reset_index()` causes downstream errors**:
```python
# ❌ WRONG
tier_analysis = df.groupby('tier').agg(...)
tier_analysis['tier']  # KeyError! 'tier' is index, not column

# ✅ CORRECT
tier_analysis = df.groupby('tier').agg(...).reset_index()
tier_analysis['tier']  # Accessible
```

---

### Extension Capabilities

Quickly add more metrics:
```python
agg(
    avg_revenue=('revenue', 'mean'),
    total_revenue=('revenue', 'sum'),
    max_age=('age', 'max'),
    unique_customers=('customer_id', 'nunique'),
    std_rating=('customer_rating', 'std')
).reset_index()
```

**Core advantage**: Single-pass computation—faster than multiple `groupby` calls.

---

### Summary

This one-liner = **Data splitting + Multi-metric calculation + Structure adjustment**, the **atomic operation** of data science pipelines. Master it, and you master 80% of pandas analysis logic.

## **Memorization Framework: The "GAR" Pattern**

Think **GAR** (like "garage" where you store organized tools):
- **G** = **Group** (split data into buckets)
- **A** = **Apply** (run calculations on each bucket)
- **R** = **Reset** (make the result clean and usable)

---

### **1. The 3-Step Formula (Never Forget)**

```python
df.groupby('key').agg(metric=('col', 'func')).reset_index()
```

**Memorize this template like a phone number**: **G-A-R is mandatory**

- **Skip `reset_index()`** → your code breaks later (can't access grouping key)
- **Skip `agg()`** → you just get a GroupBy object, not results
- **Skip `groupby()`** → there's nothing to aggregate

---

### **2. The "Dictionary Trick" Inside `agg()`**

**Pattern**: `new_name=('old_column', 'operation')`

**Memory device**: Think **"Name Tag on a Column"**

```
avg_revenue=('revenue', 'mean')
^^^^^^^^^^^ ^^^^^^^^^^ ^^^^^^
   |           |         |
   |           |         └─ What to DO? (mean/sum/count)
   |           └─────────── On what COLUMN? (revenue/age)
   └─────────────────────── What to CALL the result? (avg_revenue)
```

**Visualize slapping a sticky note** on each column saying "Compute this and name it that."

---

### **3. The "Why Reset?" Rule of Thumb**

**Mental model**: `groupby()` **eats** your column and turns it into an **index**

- **Before**: `customer_tier` is a **column** (easy to access)
- **After**: `customer_tier` becomes an **index** (harder to access)

**Memory phrase**: **"ResetIndex or Regret It"**

```python
# No reset → Index ( inaccessible ❌ )
tier_analysis['customer_tier']  # KeyError!

# With reset → Column ( accessible ✅ )
tier_analysis['customer_tier']  # Works!
```

---

### **4. Common Functions: The "MSC" Trio**

Memorize the **three 80/20 functions** you'll use 80% of the time:

- **M**ean (`'mean'`) → average value
- **S**um (`'sum'`) → total value
- **C**ount (`'count'`) → number of rows

**Bonus**: `nunique` (distinct count) for unique items

---

### **5. The "One-Liner" Mantra**

**Repeat this 3 times when you code**:

> "Group it, tag it, clean it"

- **Group it**: `groupby()` splits your data
- **Tag it**: `agg()` calculates and names results
- **Clean it**: `reset_index()` makes it pretty

---

### **6. Flashcard Version (Save This)**

| Component | What it does | What you forget | Consequence |
|-----------|--------------|-----------------|-------------|
| `groupby()` | Splits into buckets | Nothing | Can't group |
| `agg()` | Calculates metrics | Syntax: `name=('col', 'func')` | Ambiguous columns |
| `reset_index()` | Makes index a column | **THIS ONE!** | KeyError later |

---

### **7. Practice Drill (30 seconds)**

Close your eyes and recite:

> "I have a DataFrame. I want metrics by category. I write: **df.groupby('category').agg(metric=('value', 'mean')).reset_index()**"

Do this before bed for 3 nights → **muscle memory locked in**.

---

### **Final Memory Hook**

**Real-world analogy**: It's like **organizing a warehouse**:
- `groupby()` → Sort items into bins (Gold/Silver/Bronze bins)
- `agg()` → Put a label on each bin: "Avg Value: $X, Count: Y"
- `reset_index()` → Make the bin labels part of the main inventory list, not just sticky notes on the bins

**GAR**: Group → Apply → Reset. Done.
