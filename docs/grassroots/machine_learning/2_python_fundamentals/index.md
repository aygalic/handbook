# Machine Learning with Python

## Data Processing and Analysis

### Loading and Initial Data Exploration
```python
import pandas as pd
import numpy as np
from scipy import stats

# Load data
df = pd.read_csv('data.csv')

# Basic exploration
print(df.info())  # Data types and missing values
print(df.describe())  # Statistical summary

# Missing values analysis
missing_values = df.isnull().sum() / len(df) * 100
print(missing_values[missing_values > 0])

# Correlation analysis
correlation_matrix = df.corr()
```

### Data Cleaning and Preprocessing

#### Handling Missing Values
```python
# Strategy 1: Remove rows with missing values
df_cleaned = df.dropna()

# Strategy 2: Imputation
from sklearn.impute import SimpleImputer, KNNImputer

# Mean imputation
imputer = SimpleImputer(strategy='mean')
df['column'] = imputer.fit_transform(df[['column']])

# KNN imputation for more complex relationships
imputer = KNNImputer(n_neighbors=5)
df_imputed = pd.DataFrame(
    imputer.fit_transform(df),
    columns=df.columns
)

# Strategy 3: Advanced imputation
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer
# Uses the relationships between features to impute values
imputer = IterativeImputer(random_state=42)
```

#### Feature Scaling
```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler, RobustScaler

# Standardization (z-score)
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Min-Max scaling
scaler = MinMaxScaler()
X_scaled = scaler.fit_transform(X)

# Robust scaling (handles outliers better)
scaler = RobustScaler()
X_scaled = scaler.fit_transform(X)
```

#### Handling Categorical Variables
```python
# One-hot encoding
X_encoded = pd.get_dummies(df, columns=['categorical_column'])

# Label encoding
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
df['encoded_column'] = le.fit_transform(df['categorical_column'])

# Ordinal encoding
from sklearn.preprocessing import OrdinalEncoder
encoder = OrdinalEncoder()
```

### Feature Engineering

#### Creating New Features
```python
# Polynomial features
from sklearn.preprocessing import PolynomialFeatures
poly = PolynomialFeatures(degree=2)
X_poly = poly.fit_transform(X)

# Interaction terms
df['interaction'] = df['feature1'] * df['feature2']

# Date features
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day_of_week'] = df['date'].dt.dayofweek
```

#### Feature Selection
```python
from sklearn.feature_selection import (
    SelectKBest, f_classif, RFE, SelectFromModel
)
from sklearn.ensemble import RandomForestClassifier

# Statistical selection
selector = SelectKBest(score_func=f_classif, k=10)
X_selected = selector.fit_transform(X, y)

# Recursive feature elimination
rfe = RFE(estimator=RandomForestClassifier(), n_features_to_select=10)
X_selected = rfe.fit_transform(X, y)

# L1-based selection
from sklearn.linear_model import Lasso
selector = SelectFromModel(Lasso())
X_selected = selector.fit_transform(X, y)
```

## Model Training and Evaluation

### Train-Test Split
```python
from sklearn.model_selection import train_test_split

# Simple split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Stratified split for classification
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
```

### Cross-Validation
```python
from sklearn.model_selection import (
    cross_val_score, KFold, StratifiedKFold,
    TimeSeriesSplit
)

# K-Fold CV
cv = KFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=cv)

# Stratified K-Fold for classification
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=cv)

# Time series CV
cv = TimeSeriesSplit(n_splits=5)
scores = cross_val_score(model, X, y, cv=cv)
```

### Model Training and Hyperparameter Tuning
```python
from sklearn.model_selection import (
    GridSearchCV, RandomizedSearchCV
)

# Grid search
param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10]
}

grid_search = GridSearchCV(
    estimator=model,
    param_grid=param_grid,
    cv=5,
    scoring='accuracy',
    n_jobs=-1
)
grid_search.fit(X_train, y_train)

# Random search
from scipy.stats import randint
param_dist = {
    'n_estimators': randint(100, 500),
    'max_depth': randint(10, 50),
    'min_samples_split': randint(2, 20)
}

random_search = RandomizedSearchCV(
    estimator=model,
    param_distributions=param_dist,
    n_iter=100,
    cv=5,
    scoring='accuracy',
    n_jobs=-1
)
random_search.fit(X_train, y_train)
```

### Model Evaluation

#### Classification Metrics
```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score,
    f1_score, roc_auc_score, confusion_matrix,
    classification_report
)

# Basic metrics
y_pred = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, y_pred):.3f}")
print(f"Precision: {precision_score(y_test, y_pred, average='weighted'):.3f}")
print(f"Recall: {recall_score(y_test, y_pred, average='weighted'):.3f}")
print(f"F1 Score: {f1_score(y_test, y_pred, average='weighted'):.3f}")

# ROC-AUC for binary classification
y_prob = model.predict_proba(X_test)[:, 1]
print(f"ROC-AUC: {roc_auc_score(y_test, y_prob):.3f}")

# Detailed classification report
print(classification_report(y_test, y_pred))
```

#### Regression Metrics
```python
from sklearn.metrics import (
    mean_squared_error, mean_absolute_error,
    r2_score, mean_absolute_percentage_error
)

# Calculate metrics
y_pred = model.predict(X_test)
print(f"MSE: {mean_squared_error(y_test, y_pred):.3f}")
print(f"RMSE: {np.sqrt(mean_squared_error(y_test, y_pred)):.3f}")
print(f"MAE: {mean_absolute_error(y_test, y_pred):.3f}")
print(f"R2 Score: {r2_score(y_test, y_pred):.3f}")
print(f"MAPE: {mean_absolute_percentage_error(y_test, y_pred):.3f}")
```

### Model Interpretability
```python
import shap
from sklearn.inspection import permutation_importance

# SHAP values
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X_test)

# Feature importance
feature_imp = pd.DataFrame(
    {'feature': X.columns, 'importance': model.feature_importances_}
).sort_values('importance', ascending=False)

# Permutation importance
perm_importance = permutation_importance(model, X_test, y_test)
```

## Best Practices

### Pipeline Creation
```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer

# Define preprocessing for numerical and categorical columns
numeric_features = ['feature1', 'feature2']
categorical_features = ['feature3', 'feature4']

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())
])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(drop='first', handle_unknown='ignore'))
])

# Combine preprocessing steps
preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)
    ])

# Create full pipeline
pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('classifier', RandomForestClassifier())
])

# Use pipeline
pipeline.fit(X_train, y_train)
```

### Model Persistence
```python
import joblib

# Save model
joblib.dump(model, 'model.joblib')

# Load model
loaded_model = joblib.load('model.joblib')
```

### Cross-Validation Best Practices
1. **Data Leakage Prevention**
```python
# Wrong: Scaling before splitting
X_scaled = scaler.fit_transform(X)
X_train, X_test = train_test_split(X_scaled, y)

# Correct: Scaling after splitting
X_train, X_test = train_test_split(X, y)
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

2. **Nested Cross-Validation**
```python
from sklearn.model_selection import cross_validate, KFold

def nested_cv(X, y, inner_cv, outer_cv, model, param_grid):
    outer_scores = []
    
    for train_idx, test_idx in outer_cv.split(X, y):
        X_train, X_test = X[train_idx], X[test_idx]
        y_train, y_test = y[train_idx], y[test_idx]
        
        # Inner CV for parameter tuning
        grid_search = GridSearchCV(
            estimator=model,
            param_grid=param_grid,
            cv=inner_cv
        )
        grid_search.fit(X_train, y_train)
        
        # Evaluate on test set
        score = grid_search.score(X_test, y_test)
        outer_scores.append(score)
    
    return outer_scores
```

### Common Pitfalls and Solutions
1. **Class Imbalance**
```python
from imblearn.over_sampling import SMOTE
from imblearn.pipeline import Pipeline as ImbPipeline

# Handling imbalanced data
smote = SMOTE(random_state=42)
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)

# Or use pipeline with SMOTE
pipeline = ImbPipeline([
    ('preprocessor', preprocessor),
    ('smote', SMOTE()),
    ('classifier', RandomForestClassifier())
])
```

2. **Feature Scaling for Tree-Based Models**
```python
# Tree-based models don't require scaling
tree_pipeline = Pipeline([
    ('preprocessor', categorical_transformer),  # Only handle categorical
    ('classifier', RandomForestClassifier())
])

# Non-tree models need scaling
linear_pipeline = Pipeline([
    ('preprocessor', preprocessor),  # Handle both numeric and categorical
    ('classifier', LogisticRegression())
])
```

Remember:
1. Always split data before preprocessing
2. Use pipelines to prevent data leakage
3. Choose appropriate cross-validation strategy
4. Consider domain-specific requirements
5. Monitor for overfitting/underfitting
6. Document preprocessing steps and parameters