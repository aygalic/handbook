# Machine Learning Theory: Classical Models and Methods

## Linear Models

### Linear Regression
* **Concept**: Models relationship between dependent variable y and independent variables X using a linear equation
* **Mathematical Form**: y = Xβ + ε
* **Loss Function**: Mean Squared Error (MSE)
    - L(β) = 1/n Σ(yi - xiᵀβ)²

* **Key Properties**:
    - Easy to interpret
    - Fast to train
    - Assumes linear relationship
    - Sensitive to outliers
    - Assumes independence of features

* **Regularization Variants**:
    1. **Ridge Regression (L2)**
        - Adds penalty term: λ||β||₂²
        - Shrinks coefficients toward zero
        - Good for handling multicollinearity
    
    2. **Lasso Regression (L1)**
        - Adds penalty term: λ||β||₁
        - Can zero out coefficients
        - Performs feature selection
    
    3. **Elastic Net**
        - Combines L1 and L2 penalties
        - Penalty term: λ₁||β||₁ + λ₂||β||₂²

### Logistic Regression
* **Concept**: Models probability of binary outcome using logistic function
* **Mathematical Form**: P(y=1|X) = 1/(1 + e^(-Xβ))
* **Loss Function**: Binary Cross-Entropy
    - L(β) = -1/n Σ(yi log(pi) + (1-yi)log(1-pi))

* **Key Properties**:
    - Outputs probabilities between 0 and 1
    - Can be extended to multiclass (one-vs-rest or multinomial)
    - Assumes linear decision boundary
    - Less sensitive to outliers than linear regression

## Support Vector Machines (SVM)

### Linear SVM
* **Concept**: Finds optimal hyperplane maximizing margin between classes
* **Mathematical Form**: 
    - Primal: min 1/2||w||² subject to yi(wᵀxi + b) ≥ 1
    - Dual: max Σαi - 1/2Σ(αiαjyiyj<xi,xj>)

* **Key Properties**:
    - Maximizes margin between classes
    - Less prone to overfitting
    - Sensitive to feature scaling
    - Memory efficient (only stores support vectors)

### Kernel SVM
* **Concept**: Projects data to higher dimensional space using kernel trick
* **Common Kernels**:
    1. **RBF (Gaussian)**:
        - K(x,y) = exp(-γ||x-y||²)
        - γ controls flexibility
    
    2. **Polynomial**:
        - K(x,y) = (γ<x,y> + r)^d
        - d is polynomial degree
    
    3. **Sigmoid**:
        - K(x,y) = tanh(γ<x,y> + r)

* **Key Properties**:
    - Can learn non-linear decision boundaries
    - Computationally intensive for large datasets
    - Requires careful kernel selection

## Decision Trees

### Basic Decision Tree
* **Concept**: Recursively splits data based on feature values
* **Splitting Criteria**:
    1. **Classification**:
        - Gini Impurity: 1 - Σpi²
        - Entropy: -Σpi log(pi)
    
    2. **Regression**:
        - MSE: 1/n Σ(yi - ȳ)²
        - MAE: 1/n Σ|yi - ȳ|

* **Key Properties**:
    - Easy to interpret
    - Can handle non-linear relationships
    - No feature scaling needed
    - Prone to overfitting
    - Can capture feature interactions

* **Hyperparameters**:
    - Maximum depth
    - Minimum samples per leaf
    - Maximum features
    - Minimum impurity decrease

## Ensemble Methods

### Bagging (Bootstrap Aggregating)

#### Random Forest
* **Concept**: Averages predictions from multiple decorrelated trees
* **Key Components**:
    1. Bootstrap sampling of data
    2. Random feature subset at each split
    3. Averaging (regression) or voting (classification)

* **Key Properties**:
    - Reduces overfitting
    - Lower variance than single trees
    - Can estimate feature importance
    - Parallelizable

* **Mathematical Basis**:
    - Variance reduction through averaging
    - Error rate bound related to tree correlation

### Boosting

#### AdaBoost (Adaptive Boosting)
* **Concept**: Sequentially builds weak learners focusing on misclassified samples
* **Algorithm**:
    1. Initialize sample weights: wi = 1/n
    2. For each iteration t:
        - Train weak learner ht
        - Calculate error: εt = Σwi[yi ≠ ht(xi)]
        - Calculate weight: αt = 1/2 ln((1-εt)/εt)
        - Update sample weights
    3. Final prediction: H(x) = sign(Σαtht(x))

#### Gradient Boosting
* **Concept**: Sequentially builds models to minimize loss function gradient
* **Algorithm**:
    1. Initialize prediction: F₀(x) = argmin(γ) Σ L(yi,γ)
    2. For each iteration m:
        - Calculate negative gradients: rim
        - Fit base learner hm to rim
        - Find optimal step size: γm
        - Update model: Fm(x) = Fm-₁(x) + γm hm(x)

* **Variants**:
    1. **XGBoost**:
        - Adds regularization term
        - Second-order approximation
        - Efficient implementation
    
    2. **LightGBM**:
        - Gradient-based one-side sampling
        - Exclusive feature bundling
        - Leaf-wise tree growth
    
    3. **CatBoost**:
        - Ordered boosting
        - Efficient categorical feature handling
        - Less prone to overfitting

## Model Selection and Complexity

### Bias-Variance Tradeoff
* **Bias**: Model's ability to capture underlying patterns
* **Variance**: Model's sensitivity to training data variation
* **Total Error**: Error = Bias² + Variance + Irreducible Error

### Model Complexity Control
* **Regularization Methods**:
    1. Parameter penalties (L1, L2)
    2. Early stopping
    3. Pruning (for trees)
    4. Dropout (for neural networks)

* **Validation Strategies**:
    1. Hold-out validation
    2. K-fold cross-validation
    3. Leave-one-out cross-validation
    4. Stratified cross-validation

## Feature Importance and Model Interpretation

### Global Interpretation
* **Feature Importance Methods**:
    1. Coefficient magnitude (linear models)
    2. Gini importance (trees)
    3. Permutation importance
    4. SHAP values

### Local Interpretation
* **Methods**:
    1. LIME (Local Interpretable Model-agnostic Explanations)
    2. Individual SHAP values
    3. Local feature importance

## Assumptions and Limitations

### Linear Models
* Linearity
* Independence
* Homoscedasticity
* Normal distribution (for inference)

### SVM
* Feature scaling importance
* Kernel selection
* Computational complexity

### Trees and Ensembles
* Prone to overfitting (single trees)
* Memory requirements
* Training time (especially boosting)
* Black box nature (ensembles)

## Practical Considerations

### Model Selection Guidelines
1. **Dataset Size**:
    - Small: Linear models, SVM
    - Medium: Random Forest, Boosting
    - Large: Gradient Boosting, Deep Learning

2. **Feature Types**:
    - Numerical: Any model
    - Categorical: Trees, CatBoost
    - Mixed: Trees, Ensemble methods

3. **Interpretability Requirements**:
    - High: Linear models, Single trees
    - Medium: Random Forest
    - Low: Complex ensembles

4. **Training Time Constraints**:
    - Fast: Linear models, Single trees
    - Medium: Random Forest
    - Slow: Boosting, Kernel SVM

Remember:
1. No free lunch theorem - no universally best model
2. Start simple, increase complexity as needed
3. Domain knowledge is crucial
4. Consider computational resources
5. Balance accuracy vs. interpretability