# README.md




## 3_finance.ipynb

This code processes a **credit risk dataset** (predicting whether a person will default on a loan within 2 years). Let me break it down from the basics, building on your pandas knowledge.

---

### **Step 1: The New Libraries**

You know `pandas`. Here are the newcomers:

```python
import matplotlib.pyplot as plt  # Python's fundamental plotting library (like Excel charts)
import seaborn as sns            # Built on top of matplotlib; makes statistical plots prettier
from sklearn.preprocessing import MinMaxScaler  # Scikit-learn: the standard ML library in Python
from sklearn.model_selection import train_test_split
```

**Analogy**: If `pandas` is like Excel sheets, `matplotlib` is like Excel's chart tool, and `sklearn` (scikit-learn) is like Excel's Data Analysis ToolPak—pre-built statistical functions.

---

### **Step 2: Visualizing Outliers with Boxplots**

```python
numeric_cols = data.select_dtypes(include=['float64', 'int64']).columns

plt.figure(figsize=(12, 8))  # Create a canvas 12x8 inches
for i, col in enumerate(numeric_cols, 1):
    plt.subplot(3, 4, i)     # Create a grid: 3 rows, 4 columns, position i
    sns.boxplot(x=data[col]) # Draw a boxplot for this column
    plt.title(col)

plt.tight_layout()  # Auto-adjust spacing so titles don't overlap
plt.show()
```

**What is a boxplot?**
- A visual representation of **quartiles** (Q1, Q2/median, Q3).
- The "whiskers" typically extend to 1.5×IQR.
- **Dots outside whiskers** = potential outliers (extreme values).

**The loop explained**:
- `enumerate(numeric_cols, 1)` → gives you `(1, 'column1'), (2, 'column2')...` 
- `plt.subplot(3,4,i)` → organizes 12 charts (3×4 grid) in one figure.
- This lets you see outliers across all numeric columns at once.

---

### **Step 3: Outlier Detection (The IQR Method)**

This is the mathematical core of the snippet:

```python
Q1 = data[numeric_cols].quantile(0.25)  # First quartile (25th percentile)
Q3 = data[numeric_cols].quantile(0.75)  # Third quartile (75th percentile)
IQR = Q3 - Q1                           # Interquartile Range (the middle 50% of data)
```

**The Logic**:
- **Normal range**: `[Q1 - 1.5×IQR,  Q3 + 1.5×IQR]`
- Anything outside this range is considered an outlier (statistical standard).

```python
data_cleaned = data[~((data[numeric_cols] < (Q1 - 1.5 * IQR)) | 
                       (data[numeric_cols] > (Q3 + 1.5 * IQR))).any(axis=1)]
```

**Breaking down this intimidating line**:

1. **Inner comparison**: `data[numeric_cols] < (Q1 - 1.5 * IQR)` creates a boolean mask (True/False) for values below the lower bound.
2. **`|`** (OR operator): Combines lower bound violations with upper bound violations (`>`).
3. **`.any(axis=1)`**: Checks if *any* column in a row has an outlier. Returns one True/False per row.
4. **`~`**: The **NOT** operator (bitwise inverse). Keeps rows where NO outliers exist (`~True` becomes `False`, filtering out the outlier rows).

**Visual intuition**:
```
Row 1: [Normal, Normal, OUTLIER]  → any() = True   → ~True  = False (deleted)
Row 2: [Normal, Normal, Normal]   → any() = False  → ~False = True  (kept)
```

---

### **Step 4: Duplicate Removal**

```python
duplicates = data_cleaned.duplicated()     # Returns boolean Series (True where row is duplicate)
num_duplicates = duplicates.sum()          # True counts as 1, False as 0
data_cleaned = data_cleaned[~duplicates]   # Keep only non-duplicates (~ inverts booleans)
```

---

### **Step 5: Min-Max Scaling (Normalization)**

Previously you saw `StandardScaler` (z-score). This uses `MinMaxScaler`:

```python
scaler = MinMaxScaler()
data_cleaned[numeric_cols] = scaler.fit_transform(data_cleaned[numeric_cols])
```

**What it does**:
- Transforms each feature to range **[0, 1]** using formula:  
  $$x_{new} = \frac{x - x_{min}}{x_{max} - x_{min}}$$

**When to use which**:
- **StandardScaler** (z-score): When data has outliers (you just removed them, so this is less critical here).
- **MinMaxScaler**: When you need bounded values (0-1) or when using algorithms sensitive to magnitude (neural networks, K-NN, PCA).

---

### **Step 6: Preparing for Machine Learning**

```python
target_variable = 'SeriousDlqin2yrs'  # 1 = will default, 0 = won't default (binary classification)

X = data_cleaned.drop(columns=[target_variable])  # Features (inputs)
y = data_cleaned[target_variable]                 # Target (label to predict)

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

**The variables**:
- `X` (capital): Feature matrix (all columns except target). Shape: `(rows, features)`
- `y` (lowercase): Target vector. Shape: `(rows,)` 
- `test_size=0.2`: 80% data for training, 20% for testing model performance.

---

### **⚠️ Critical Issue in This Code (Data Leakage)**

Just like your previous example, this code has the **same sequencing error**:

1. It removes outliers from the **entire** dataset first
2. Then scales the **entire** dataset
3. Then splits

**The Problem**: Information from the test set (e.g., global min/max for scaling, global IQR for outlier removal) leaks into the training process. This causes overly optimistic accuracy when you eventually evaluate the model.

**Correct Order**:
```python
# 1. Split first
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# 2. Fit scaler ONLY on train, transform both
scaler.fit(X_train[numeric_cols])
X_train[numeric_cols] = scaler.transform(X_train[numeric_cols])
X_test[numeric_cols] = scaler.transform(X_test[numeric_cols])

# 3. (Optional) Outlier removal should ideally happen within training CV folds, 
#    not globally, to avoid test-set information influencing train set boundaries
```

---

### **Summary Cheat Sheet**

| Concept            | Purpose                                  |
| ------------------ | ---------------------------------------- |
| `sns.boxplot`      | Visualize distribution and spot outliers |
| `quantile(0.25)`   | Calculate Q1 (25th percentile)           |
| `IQR`              | Robust measure of statistical dispersion |
| `~df.any(axis=1)`  | Filter rows where no condition is True   |
| `MinMaxScaler`     | Compress features to [0,1] range         |
| `train_test_split` | Separate data for unbiased evaluation    |

This pipeline prepares financial data for a classification model (likely Logistic Regression or Random Forest) to predict loan defaults.