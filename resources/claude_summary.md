# Credit Risk Management Research Methodology
## Based on Decision Tree Model

---

## 1. Data Acquisition & Understanding

### Dataset Source
- **Origin**: UCI Machine Learning Repository (via Kaggle)
- **Name**: "Default of Credit Card Clients Dataset"
- **Time Period**: April-September 2005, Taiwan credit card customers
- **Size**: 30,000 records with 25 variables

### Variables Structure

#### Target Variable
- **Y (default payment next month)**: Default payment (0=no default, 1=default)

#### Feature Variables (24 predictors)

**Demographics:**
- **X2 (SEX)**: Gender (1=male, 2=female)
- **X3 (EDUCATION)**: Education level (1=graduate school, 2=university, 3=high school, 4=others, 5=unknown, 6=unknown)
- **X4 (MARRIAGE)**: Marital status (1=married, 2=single, 3=others)
- **X5 (AGE)**: Age in years

**Credit Information:**
- **X1 (LIMIT_BAL)**: Amount of given credit in NT dollars

**Payment History (Repayment Status):**
- **X6 (PAY_0)**: Repayment status in September 2005 (-1=pay duly, 1=payment delay for one month, 2=payment delay for two months, ..., 9=payment delay for nine months and above)
- **X7 (PAY_2)**: Repayment status in August 2005
- **X8 (PAY_3)**: Repayment status in July 2005
- **X9 (PAY_4)**: Repayment status in June 2005
- **X10 (PAY_5)**: Repayment status in May 2005
- **X11 (PAY_6)**: Repayment status in April 2005

**Bill Amounts:**
- **X12 (BILL_AMT1)**: Amount of bill statement in September 2005 (NT dollar)
- **X13 (BILL_AMT2)**: Amount of bill statement in August 2005
- **X14 (BILL_AMT3)**: Amount of bill statement in July 2005
- **X15 (BILL_AMT4)**: Amount of bill statement in June 2005
- **X16 (BILL_AMT5)**: Amount of bill statement in May 2005
- **X17 (BILL_AMT6)**: Amount of bill statement in April 2005

**Payment Amounts:**
- **X18 (PAY_AMT1)**: Amount of previous payment in September 2005
- **X19 (PAY_AMT2)**: Amount of previous payment in August 2005
- **X20 (PAY_AMT3)**: Amount of previous payment in July 2005
- **X21 (PAY_AMT4)**: Amount of previous payment in June 2005
- **X22 (PAY_AMT5)**: Amount of previous payment in May 2005
- **X23 (PAY_AMT6)**: Amount of previous payment in April 2005

---

## 2. Exploratory Data Analysis (EDA)

### Overall Statistics

#### Default Rate Analysis
- **Overall default rate**: 22.1%
- This is considered a substantially elevated proportion in the credit card industry
- Indicates heightened risk exposure for financial institutions

### Demographic Analysis

#### Gender Analysis
- **Finding**: No significant difference in default rates between genders
- Female cardholders are more numerous in the dataset
- When other variables are controlled, gender is not a robust predictor of credit default

#### Education Analysis
- **Key Finding**: Higher educational attainment correlates with lower default rates
- Majority of clients have educational levels exceeding high school

**Reasons for Lower Default Rates Among Highly Educated:**
1. Higher levels of education are typically associated with greater earning potential
2. Individuals with advanced degrees may possess better financial knowledge and planning abilities
3. More stable employment prospects

#### Age Analysis - U-Shaped Distribution

**High-Risk Age Groups:**
- **20-24 years**: 27.2% default rate
  - Early career stage
  - Unstable incomes
  - Weaker financial buffers
  - Lower financial literacy
  - Prone to excessive overdrafts
  
- **60-64 years**: 29.7% default rate
  - Post-retirement income decline
  - Higher medical expenses
  - High target for financial fraud
  - Imbalanced existing debt structure
  
- **70-74 years**: 28.6% default rate

**Low-Risk Age Groups (Core Customer Segment):**
- **25-29 years**: 21.2% default rate (6,933 customers)
- **30-34 years**: 19.3% default rate (6,078 customers)
- These groups represent the main concentration of bank customers
- Already in stable employment
- Have financial management experience
- Good balance between income and debt
- Should be focus of cultivation as "low-risk core customer group"

**Risk Management Implication:**
- Despite smaller numbers, 60+ age group requires key monitoring
- 714 individuals in 60-64 age group with 29.7% default rate
- High risk exposure per account
- Potential impact on bank's asset quality in concentrated default scenarios

### Correlation Analysis

#### Pearson Correlation Heatmap (Figure 1)
**Variables with Strong Correlation to Default:**
- PAY_1 (Repayment status September)
- PAY_2 (Repayment status August)
- PAY_3 (Repayment status July)
- PAY_4 (Repayment status June)
- PAY_5 (Repayment status May)
- PAY_6 (Repayment status April)
- AGE

### Repayment Status Analysis - Critical Finding

#### Relationship Between Payment Delays and Default Probability

**Pattern Observed**: Significant positive trend between repayment status and default probability
- The more months the repayment is delayed, the higher the probability of default

#### Detailed Analysis by Payment Status

**PAY_1 (Most Recent Month) Analysis:**
- Normal repayment (value ≤ 0): Default rate <20%
- PAY_1 = 2: Rapid increase in default rate
- PAY_1 = 3: Default rate reaches 75.8%
- PAY_1 = 7: Default rate peaks at 77.8%

**PAY_5 (5 Months Prior) Analysis:**
- PAY_5 = 2: Default rate 54.2%
- PAY_5 ≥ 6: Default rate rapidly rises to >75%
- PAY_5 = 8: Default rate reaches 100%

**Behavioral Finance Explanation:**

1. **Cash Flow Constraints**: Delayed repayment reflects borrower's short-term cash flow issues
2. **Interest Snowball Effect**: Late fees and penalties create additional financial burden
3. **Credit Score Deterioration**: Consecutive late payments significantly lower credit rating
4. **Vicious Credit Cycle**: Lower credit rating limits ability to obtain new financing

**Structural Leap Characteristic:**
- Significant increase in default rates when transitioning from "non-delayed" (0 or -1) to "minor delinquency" (1 or 2)
- Even slight delays serve as early signals of deteriorating creditworthiness
- **Risk Control Strategy**: Monitor not only high-delay customers but also those with incipient delinquency signs

**Conclusion**: Monthly repayment status indicators effectively reflect changes in borrower's credit status and have strong discriminatory ability

---

## 3. Data Preprocessing & Feature Engineering

### Feature Engineering

#### Combined Gender-Marital Status Categories

**Rationale:**
- Males had relatively higher default rates
- Married individuals had relatively higher default rates
- Combined features improve model efficiency and data processing

**New Categories Created:**
1. **Category 1**: Married man
2. **Category 2**: Single man
3. **Category 3**: Divorced man
4. **Category 4**: Married woman
5. **Category 5**: Single woman
6. **Category 6**: Divorced woman (EXCLUDED - insufficient data)

**Data Exclusion Decision:**
- Divorced women category excluded due to too little data
- Insufficient data may negatively affect model results

### Data Cleaning Steps

1. **Missing Values Handling**: Addressed missing values in the dataset
2. **Outlier Treatment**: Dealt with outliers appropriately
3. **Feature Encoding**: Encoded categorical variables for model input
4. **Data Exclusion**: Removed divorced women category due to insufficient samples

### Train-Test Split

- Dataset divided into training and testing subsets
- Split performed at "appropriate ratio" (specific ratio not detailed in paper)
- Common practice: 70-30 or 80-20 split

---

## 4. Model Development Process

### Models Evaluated

Four different machine learning models were tested:

1. **Logistic Regression**
2. **Decision Tree** (Primary focus of research)
3. **Random Forest**
4. **Support Vector Machine (SVM)**

---

## 5. Decision Tree Model Training

### Phase A: Baseline Decision Trees with Different Depths

#### Depth = 2
- **Accuracy**: 0.820
- **F1-score**: 0.425

#### Depth = 3
- **Accuracy**: 0.821
- **F1-score**: 0.470
- **Best baseline performance**

#### Depth = 5
- **Accuracy**: 0.817
- **F1-score**: 0.453
- **Observation**: Decreased performance compared to depth 3, indicating potential overfitting

**Key Insight**: Within a certain range, accuracy and F1-score fluctuate as tree depth increases. An overly deep decision tree may suffer from overfitting, leading to worse performance on test set.

### Phase B: Addressing Class Imbalance

#### Problem Identification
- **Class Imbalance**: Only 22.1% of samples are defaults (minority class)
- Traditional models tend to be overly biased towards majority-class samples
- Risk of underestimating default risks in credit prediction

#### Solution: Class Weights
- Set class weights to give more importance to minority class (defaults)
- Helps model pay more attention to default cases
- Improves recall and F1-score for minority class

### Phase C: Hyperparameter Optimization via Grid Search

#### Hyperparameters Tuned

1. **max_depth**: Maximum depth of the tree
   - Controls how deep the tree can grow
   - Affects model complexity and overfitting risk

2. **min_samples_split**: Minimum number of samples required to split an internal node
   - Controls when a node should be split
   - Higher values prevent overfitting

3. **min_samples_leaf**: Minimum number of samples required to be at a leaf node
   - Controls minimum size of leaf nodes
   - Helps avoid creating leaves with very few samples

#### Grid Search Process

1. **Define Parameter Grid**: Specify ranges for each hyperparameter
2. **Cross-Validation**: Use k-fold cross-validation to evaluate each combination
3. **Iterate**: Test all possible combinations of parameters
4. **Select Optimal**: Choose hyperparameters with best cross-validation performance
5. **Train Final Model**: Use optimal hyperparameters on full training set

**Goal**: Systematically optimize model performance rather than relying on experience-based parameter selection

### Phase D: Model Variants and Results

#### Variant 1: Grid Search Tuned (Default)
- **Accuracy**: 0.761
- **F1-score**: 0.508
- **Analysis**: Default parameters did not adapt well to the data, resulting in poor model performance
- **Note**: High F1-score suggests balanced recall and precision, but overall accuracy is significantly lower

#### Variant 2: Grid Search Tuned (No Class Weight)
- **Accuracy**: 0.821
- **F1-score**: 0.470
- **Analysis**: Performance similar to baseline decision tree with depth 3
- **Observation**: Grid search without class weights provides limited improvement

#### Variant 3: Grid Search Tuned (With Class Weight) ⭐ **BEST MODEL**
- **Accuracy**: 0.851
- **F1-score**: 0.585
- **Performance**: Highest accuracy and F1-score among all decision tree variants
- **Conclusion**: Setting class weights effectively improves model performance when dealing with imbalanced class data
- **Benefits**: 
  - More accurate credit risk predictions
  - Improved ability to identify positive cases (default customers)
  - Maintains high overall accuracy

---

## 6. Model Evaluation

### Evaluation Metrics

The following metrics were used to comprehensively evaluate model performance:

1. **Accuracy**: Overall proportion of correct predictions (both default and non-default)
2. **Precision**: Of all predicted defaults, what proportion were actual defaults
3. **Recall**: Of all actual defaults, what proportion were correctly identified
4. **F1-score**: Harmonic mean of precision and recall (balances both metrics)
5. **ROC-AUC**: Area under the Receiver Operating Characteristic curve (model's ability to distinguish between classes)

### Comparative Model Performance

#### Performance Summary Table

| Model | Accuracy | Recall | F1-Score | Computational Complexity | Overfitting Risk | Ability to Handle Imbalanced Data |
|-------|----------|--------|----------|-------------------------|------------------|-----------------------------------|
| **Logistic Regression** | 0.75 | 0.50 | 0.55 | Low | High | Medium (Requires Parameter Tuning) |
| **Decision Tree** | 0.92 | 0.58 | 0.65 | Low | High (Requires Pruning) | Medium (Requires Pruning) |
| **Random Forest** | 0.72 | 0.52 | 0.50 | Medium | Medium | Strong (Built-in Balancing) |
| **Support Vector Machine (SVM)** | 0.70 | 0.44 | 0.40 | High | Low | Weak (Depends on Sampling) |

### Detailed Model Analysis

#### Logistic Regression
- **Accuracy**: 0.75
- **F1-score**: 0.55
- **Characteristics**:
  - Low complexity with relatively simple structure
  - Highly sensitive to data imbalance
  - Potential for performance improvement through parameter tuning
- **Limitation**: May not capture complex non-linear relationships in data

#### Decision Tree (Primary Model)
- **Accuracy**: 0.92
- **F1-score**: 0.65
- **Characteristics**:
  - **Remarkable advantages**: Highest accuracy and F1-score
  - Clear and intuitive tree-like structure
  - Strong ability to capture complex data patterns
  - Low model complexity but high efficiency
- **Advantages**:
  - Handles data in interpretable tree structure
  - Efficiently explores relationships among data features
  - Completes classification tasks with high precision
- **Limitations**:
  - Sensitive to data imbalance (addressed through class weights)
  - Risk of overfitting (can be reduced through pruning)
- **Optimization**: Pruning operations can further optimize performance

#### Random Forest
- **Accuracy**: 0.72
- **F1-score**: 0.50
- **Characteristics**:
  - Moderate model complexity
  - Moderate sensitivity to data imbalance
  - Built-in balancing mechanism
  - Significant potential for performance enhancement
- **Limitation**: Lower performance compared to optimized decision tree

#### Support Vector Machine (SVM)
- **Accuracy**: 0.70
- **F1-score**: 0.40
- **Characteristics**:
  - High level of model complexity
  - Low sensitivity to data imbalance
  - Improving performance heavily relies on sampling
  - Performance enhancement is rather challenging
- **Limitation**: Lowest F1-score among all models tested

### Model Selection Conclusion

**Decision Tree emerges as the optimal model** due to:
1. Highest accuracy (0.92) and F1-score (0.65)
2. Strong interpretability critical for financial applications
3. Low computational complexity
4. Effective handling of imbalanced data when combined with class weights
5. Ability to capture complex, non-linear relationships in credit data

---

## 7. Model Interpretation

### Feature Importance Analysis

The decision tree model's interpretability allows for clear understanding of which features contribute most to predictions.

#### Key Predictors Identified

**Primary Predictors (Strongest Correlation with Default):**
1. **Payment History Variables**: PAY_1, PAY_2, PAY_3, PAY_4, PAY_5, PAY_6
   - Most significant predictors of credit default
   - Recent payment behavior (PAY_1) has strongest impact
   - Historical payment patterns provide consistent signal
   - Should be considered as core variables in model

2. **Age**: Demonstrates U-shaped relationship with default risk
   - Young adults (20-24) and elderly (60+) show elevated risk
   - Middle-aged customers (25-34) represent lowest risk segment

3. **Combined Gender-Marital Status Features**: Created features provide additional predictive power

#### Decision-Making Process

The decision tree model makes predictions by:
1. Starting at the root node with all samples
2. Splitting based on most informative features (e.g., PAY_1 value)
3. Creating branches for different feature value ranges
4. Continuing to split until reaching leaf nodes with class predictions
5. Each path from root to leaf represents a decision rule

### Business Insights and Recommendations

#### High-Risk Customer Segments

1. **Young Adults (20-24 years)**
   - Default rate: 27.2%
   - **Characteristics**: 
     - Early career stage with unstable income
     - Weaker financial buffers
     - Lower financial literacy
   - **Action**: Enhanced monitoring, lower credit limits, financial education programs

2. **Elderly Customers (60+ years)**
   - Default rate: 60-64 age group at 29.7%
   - **Characteristics**:
     - Post-retirement income decline
     - Higher medical expenses
     - Potential fraud targets
   - **Action**: Key monitoring despite smaller group size, specialized support services

3. **Customers with Payment Delays**
   - Even minor delays (1-2 months) signal increased risk
   - Consecutive delays dramatically increase default probability
   - **Action**: 
     - Implement early warning system for first-time delays
     - Intensify monitoring of incipient delinquency
     - Proactive customer outreach for at-risk accounts

#### Low-Risk Customer Segments (Core Banking Demographic)

**Age 25-34 years**
- Default rates: 19.3% - 21.2%
- Largest customer group (13,011 customers)
- **Characteristics**:
  - Stable employment
  - Financial management experience
  - Good income-to-debt balance
- **Strategy**: Focus cultivation efforts, premium service offerings, retention programs

#### Risk Management Strategies

1. **Real-time Monitoring**: 
   - Track monthly repayment status continuously
   - Flag accounts with first-time delays
   - Implement automated early warning alerts

2. **Dynamic Risk Assessment**:
   - Update risk scores monthly based on payment behavior
   - Adjust credit limits based on changing risk profiles
   - Use model predictions for proactive risk management

3. **Segmented Approach**:
   - Differentiated risk strategies by age group
   - Tailored credit products for different risk segments
   - Customized communication and collection strategies

4. **Preventive Measures**:
   - Financial literacy programs for young adults
   - Fraud protection for elderly customers
   - Early intervention for customers showing warning signs

---

## 8. Conclusion and Future Research Directions

### Research Findings Summary

The study successfully demonstrates that:
1. **Decision tree model exhibits comprehensive performance** in credit default prediction
2. **Hyperparameter optimization through grid search** significantly improves model performance
3. **Class weight adjustment** effectively addresses imbalanced data challenges
4. **Repayment history is the strongest predictor** of credit default
5. **Model achieves balance** between accuracy, interpretability, and computational efficiency

### Critical Success Factors

1. **Handling Class Imbalance**: Setting class weights provided the biggest performance gain
2. **Systematic Optimization**: Grid search with cross-validation superior to default parameters
3. **Feature Engineering**: Combined demographic features improved model efficiency
4. **Validation Strategy**: Cross-validation essential for preventing overfitting

### Future Research Directions

#### 1. Incorporate Diverse Data Types
- **Behavioral Data**: Customer consumption patterns, shopping habits
- **Social Data**: Social media activity information (with privacy considerations)
- **Alternative Data**: Mobile phone usage, utility payments, rental history
- **Goal**: More comprehensive capture of credit risk characteristics

#### 2. Develop Visualization Tools
- **Purpose**: Illustrate decision-making processes of complex models
- **Benefits**:
  - Help financial institutions understand model logic
  - Strengthen trust in model outcomes
  - Facilitate regulatory compliance and explanation
  - Enable non-technical stakeholders to interpret results

#### 3. Establish Real-Time Monitoring Systems
- **Capabilities**:
  - Continuously track borrowers' credit status
  - Promptly adjust risk evaluation results
  - Adapt to market fluctuations in real-time
  - Provide immediate decision-making support
- **Implementation**: Dynamic updating systems that respond to new data

#### 4. Cross-Industry Applications
- **Supply Chain Finance**: Adapt model for vendor/supplier credit assessment
- **Consumer Finance**: Apply to personal loans, auto financing
- **Industry-Specific Tailoring**: Customize models based on sector characteristics
- **Goal**: Promote widespread adoption of machine learning in financial risk management

#### 5. Model Enhancement
- **Ensemble Methods**: Combine decision trees with other algorithms
- **Deep Learning**: Explore neural networks for capturing complex patterns
- **Explainable AI**: Further improve interpretability of complex models
- **Automated Feature Engineering**: Develop systems to automatically discover relevant features

### Practical Implications

**For Financial Institutions:**
- More accurate and efficient credit risk assessment tool
- Enhanced ability to mitigate risks
- Improved decision-making processes
- Better resource allocation for risk management

**For Borrowers:**
- More fair and objective credit evaluation
- Potential for better credit terms with good payment history
- Transparency in credit decision-making

**For Regulators:**
- Evidence-based approach to credit risk management
- Framework for assessing institutional risk management practices
- Support for financial stability objectives

---

## References

1. Wang, Z. Q. (2025). Does Digital Transformation Amplify Credit Risks in Commercial Banks? China Knowledge Network.

2. Ken Brown, Peter Moles. (2014). Credit Risk Management: Methods, Models, and Tools. EBS Online.

3. Sun G., Wang Y., Li Q. (2022). The Impact of Green Credit on Commercial Bank Credit Risk. China Knowledge Network.

4. Gross, J. P. K., Cekic, O., Hossler, D., & Hillman, N. (2021). What matters in student loan default: A review of the literature. Journal of Student Financial Aid, 39(1), 19–29.

5. Zhao, Y. (2021). Research on Credit Default Prediction Based on Fusion Model and Feature Importance Analysis. Data, 202103: 62-64.

6. Breiman, L. (2001). Random forests. Machine Learning, 45(1), 5-32.

7. Hastie, T., Tibshirani, R., & Friedman, J. H. (2009). The Elements of Statistical Learning. Springer, 2, 44-47.

8. Breiman, L., Friedman, J. H., Olshen, R. A., & Stone, C. J. (1984). Classification and Regression Trees. Wadsworth & Brooks/Cole.

9. Chawla, N. V., Bowyer, K. W., Hall, L. O., & Kegelmeyer, W. P. (2022). SMOTE: Synthetic minority over-sampling technique. Journal of Artificial Intelligence Research, 16, 321-357.

10. Vapnik, V. N. (1995). The Nature of Statistical Learning Theory. Springer, 5, 105-108.

---

*Research Paper*: Credit Risk Management Based on Decision Tree Model  
*Authors*: Yutian Gan, Han Luo, Wei Wei  
*Conference*: ICEMGD 2025 Symposium: Innovating in Management and Economic Development  
*DOI*: 10.54254/2754-1169/2025.LH23948