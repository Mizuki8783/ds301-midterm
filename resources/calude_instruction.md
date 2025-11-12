# Data Preprocessing and Feature Engineering
## Step-by-Step Process

**Legend:**
- ✅ **CONFIRMED**: Explicitly stated in the research paper
- 🔄 **INFERRED**: Standard practice, likely done but not explicitly detailed in paper
- ❓ **ASSUMED**: Best practice assumption based on typical data science workflows

---

## Step 1: Load and Inspect Data 🔄
- Load the dataset (30,000 records, 25 variables) ✅
- Check data types and basic statistics 🔄
- Verify target variable distribution (should be ~22.1% default rate) ✅

**Paper states**: Dataset contains 30,000 records with 25 variables; overall default rate is 22.1%

---

## Step 2: Handle Missing Values 🔄
- Identify missing values in all columns 🔄
- For numerical columns: fill with median ❓
- For categorical columns: fill with mode ❓
- Verify no missing values remain 🔄

**Paper states**: "dealing with missing values" - method not specified

---

## Step 3: Handle Outliers 🔄
- Identify outliers in numerical features using IQR or percentile methods ❓
- Cap outliers at 1st and 99th percentile (especially for bill and payment amounts) ❓
- Remove only clearly erroneous values (e.g., invalid ages) ❓

**Paper states**: "dealing with...outliers" - method not specified

---

## Step 4: Clean Categorical Variables 🔄

### EDUCATION ❓
- Recode categories 0, 5, 6 as category 4 (others)
- Final categories: 1 (graduate school), 2 (university), 3 (high school), 4 (others)

### MARRIAGE ❓
- Recode category 0 as category 3 (others)
- Final categories: 1 (married), 2 (single), 3 (others)

### SEX ❓
- Verify only values 1 (male) and 2 (female) exist

### Payment Status (PAY_0 to PAY_6) ❓
- Validate values are in expected range (-2 to 9)

**Paper states**: Nothing specific about categorical variable cleaning - this is standard practice for this dataset

---

## Step 5: Feature Engineering

### Create Gender-Marriage Combined Feature ✅
**New variable: GENDER_MARRIAGE**

Categories:
- 1: Married man ✅
- 2: Single man ✅
- 3: Divorced man ✅
- 4: Married woman ✅
- 5: Single woman ✅
- 6: Divorced woman ✅

**Paper states**: "a category for married men, a combined category for married women and single men, a category for 'divorced' men, a category for single women, and a category for 'divorced' women"

### Exclude Divorced Women ✅
- Remove all records where GENDER_MARRIAGE = 6
- Reason: Insufficient data may negatively affect model

**Paper states**: "Since there is too little data for divorced women, which may affect the results of the model, we choose to exclude divorced women"

### Optional Additional Features ❓
- Average bill amount over 6 months
- Average payment amount over 6 months
- Payment to bill ratio
- Total number of months with delayed payment
- Maximum payment delay across all months

**Paper states**: Nothing about these additional features - these are NOT in the paper

---

## Step 6: Prepare Features and Target 🔄

### Remove Non-Predictive Columns ❓
- Drop ID column

### Define Target Variable ✅
- Target: Y (default_payment) with values 0 (no default) and 1 (default)

**Paper states**: "Where default payment is the labeled column, 0 is default and 1 is not defaulted"

### Define Feature Set 🔄
- All remaining variables except the target

---

## Step 7: Train-Test Split

### Split Data ✅ (ratio unclear)
- Split ratio: 70% training, 30% testing ❓
- Use stratified split to maintain class distribution ❓
- Set random seed for reproducibility ❓
- Verify default rates are similar in both sets (~22%) ❓

**Paper states**: "divide the dataset into subsets at an appropriate ratio" - exact ratio NOT specified

---

## Step 8: Handle Class Imbalance ✅

### Calculate Class Weights ✅
- Calculate class weights using "balanced" strategy 🔄
- Minority class (defaults) receives higher weight ✅
- Majority class (non-defaults) receives lower weight ✅
- These weights will be used during model training ✅

**Paper states**: "To address the issue of class imbalance, set class weights"

---

## Step 9: Final Verification ❓
- Confirm no missing values in features or target ❓
- Verify shapes are consistent between X and y ❓
- Check that features are numerical ❓
- Ensure target is binary (0, 1) ❓
- Confirm no data leakage (target not in features) ❓
- Validate train and test sets have similar default rates ❓

**Paper states**: Nothing about verification steps - these are best practices

---

## What the Paper ACTUALLY Says About Preprocessing:

> "Subsequently, initiate data cleaning, dealing with missing values and outliers. After encoding the features, divide the dataset into subsets at an appropriate ratio."

That's it. The entire preprocessing description in the paper.

---

## Summary

### CONFIRMED from Paper:
- ✅ Dataset: 30,000 records, 25 variables, 22.1% default rate
- ✅ Create Gender-Marriage combined feature (6 categories)
- ✅ Exclude divorced women category
- ✅ Train-test split (ratio not specified)
- ✅ Set class weights to handle imbalance
- ✅ Target variable: Y (0 = default, 1 = no default)

### INFERRED (Standard Practice):
- 🔄 Missing value handling (method not specified)
- 🔄 Outlier treatment (method not specified)
- 🔄 Categorical variable cleaning
- 🔄 Feature encoding
- 🔄 Data validation steps

### NOT IN PAPER:
- ❌ Additional engineered features (averages, ratios, etc.)
- ❌ Specific train-test split ratio
- ❌ Specific outlier treatment method
- ❌ Specific missing value imputation strategy
- ❌ Categorical variable recoding details

**Ready for**: Decision tree model training with grid search and class weights