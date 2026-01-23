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
