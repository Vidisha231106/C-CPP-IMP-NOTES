# The Complete AI & Machine Learning Guide
### From Intermediate to Expert — Every Concept, Model, Pipeline & Trend 

---

> **How to use this guide:** This document is structured from fundamentals → advanced. Each section builds on the last. Use the Table of Contents to jump around. Code snippets are in Python unless noted.

---

## Table of Contents

**Core Foundations (Sections 1–22)**

1. [Foundations of Machine Learning](#1-foundations-of-machine-learning)
2. [Statistical Learning Theory](#2-statistical-learning-theory)
3. [Core ML Algorithms (Classical)](#3-core-ml-algorithms-classical)
4. [Feature Engineering & Data Preprocessing](#4-feature-engineering--data-preprocessing)
5. [Deep Learning Fundamentals](#5-deep-learning-fundamentals)
6. [Convolutional Neural Networks (CNNs)](#6-convolutional-neural-networks-cnns)
7. [Recurrent Networks: RNNs, LSTMs, GRUs](#7-recurrent-networks-rnns-lstms-grus)
8. [The Transformer Architecture](#8-the-transformer-architecture)
9. [Natural Language Processing (NLP)](#9-natural-language-processing-nlp)
10. [Computer Vision](#10-computer-vision)
11. [Generative Models](#11-generative-models)
12. [Reinforcement Learning](#12-reinforcement-learning)
13. [Large Language Models (LLMs)](#13-large-language-models-llms)
14. [Prompt Engineering](#14-prompt-engineering)
15. [RAG Pipelines — All Types](#15-rag-pipelines--all-types)
16. [AI Agents & Agentic Systems](#16-ai-agents--agentic-systems)
17. [Vector Databases & Embeddings](#17-vector-databases--embeddings)
18. [MLOps & Production ML](#18-mlops--production-ml)
19. [Evaluation Metrics — Complete Reference](#19-evaluation-metrics--complete-reference)
20. [AI Safety, Alignment & Ethics](#20-ai-safety-alignment--ethics)
21. [Latest AI Models & News (2025–2026)](#21-latest-ai-models--news-20252026)
22. [Hands-On Cheat Sheets](#22-hands-on-cheat-sheets)

**Advanced Topics (Sections 23–38)**

23. [Graph Neural Networks (GNNs)](#23-graph-neural-networks-gnns)
24. [Time Series Machine Learning](#24-time-series-machine-learning)
25. [Explainable AI (XAI)](#25-explainable-ai-xai)
26. [Bayesian Machine Learning](#26-bayesian-machine-learning)
27. [Causal ML](#27-causal-ml)
28. [Federated Learning](#28-federated-learning)
29. [Continual Learning](#29-continual-learning)
30. [Meta-Learning](#30-meta-learning)
31. [Knowledge Distillation](#31-knowledge-distillation)
32. [TinyML & Edge AI](#32-tinyml--edge-ai)
33. [AutoML & Hyperparameter Optimization](#33-automl--hyperparameter-optimization)
34. [Advanced ML Topics](#34-advanced-ml-topics)
35. [Advanced LLM Architectures](#35-advanced-llm-architectures)
36. [Quantum Machine Learning](#36-quantum-machine-learning)
37. [Complete LLM Additions](#37-complete-llm-additions)
38. [Representation Learning & Transfer Learning (Deep Dive)](#38-representation-learning--transfer-learning-deep-dive)

**Appendices**

- [Appendix A: Important Research Papers Timeline](#appendix-a-important-research-papers-timeline)
- [Appendix B: Glossary of Key Terms](#appendix-b-glossary-of-key-terms)
- [Appendix C: The AI/ML Engineering Stack (2026)](#appendix-c-the-aiml-engineering-stack-2026)
- [Appendix D: Interview Questions — ML & AI](#appendix-d-interview-questions--ml--ai)

---

## 1. Foundations of Machine Learning

### 1.1 What is Machine Learning?

Machine Learning (ML) is a subfield of Artificial Intelligence where systems learn patterns from data rather than being explicitly programmed with rules. Formally:

> **ML Definition:** A computer program is said to learn from experience **E** with respect to task **T** and performance measure **P**, if its performance on T, as measured by P, improves with experience E. — Tom Mitchell (1997)

### 1.2 The Three Paradigms of Learning

#### Supervised Learning
The model learns a mapping from inputs **X** to outputs **Y**, given labeled training pairs `{(x₁,y₁), ..., (xₙ,yₙ)}`.

- **Goal:** Minimize prediction error on unseen data
- **Key assumption:** Training and test data are i.i.d. (independent and identically distributed)
- **Examples:** Classification, Regression, Sequence labeling

```
Input X ──► [Model f(x)] ──► Prediction ŷ
                ↑
         Training: minimize Loss(ŷ, y)
```

#### Unsupervised Learning
No labels. The model discovers structure, patterns, or compressed representations in raw data.

- **Goal:** Find hidden structure — clusters, manifolds, generative factors
- **Examples:** Clustering (K-Means), Dimensionality Reduction (PCA, t-SNE), Generative Modeling (VAEs, GANs)

#### Reinforcement Learning (RL)
An **agent** interacts with an **environment**, takes **actions**, receives **rewards**, and learns a **policy** to maximize cumulative reward.

- **Key components:** State, Action, Reward, Policy, Value Function
- **Examples:** Game playing (AlphaGo), robotics, RLHF for LLMs

#### Semi-Supervised & Self-Supervised Learning
- **Semi-supervised:** Few labeled + many unlabeled samples
- **Self-supervised:** Labels generated automatically from raw data (e.g., masked language modeling, contrastive learning — the backbone of GPT, BERT, CLIP)

---

### 1.3 The ML Workflow (End-to-End)

```
1. Problem Definition
       ↓
2. Data Collection & Labeling
       ↓
3. Exploratory Data Analysis (EDA)
       ↓
4. Feature Engineering & Preprocessing
       ↓
5. Model Selection & Training
       ↓
6. Evaluation & Validation
       ↓
7. Hyperparameter Tuning
       ↓
8. Deployment & Monitoring
       ↓
9. Feedback Loop & Retraining
```

---

### 1.4 Inductive Bias

Every learning algorithm makes assumptions about the form of the target function. This is called **inductive bias** — it's what allows generalization beyond training data.

| Algorithm | Inductive Bias |
|---|---|
| Linear Regression | Target is a linear function |
| Decision Trees | Shorter trees are preferred (Occam's Razor) |
| K-Nearest Neighbors | Similar inputs have similar outputs |
| Neural Networks | Hierarchical compositional structure |
| SVMs | Maximum-margin separating hyperplane |

---

## 2. Statistical Learning Theory

### 2.1 Bias-Variance Tradeoff

The generalization error of a model decomposes as:

```
Expected MSE = Bias² + Variance + Irreducible Noise
```

- **Bias:** Error from wrong assumptions in the learning algorithm. High bias → underfitting
- **Variance:** Error from sensitivity to small fluctuations in training data. High variance → overfitting
- **Irreducible Noise:** Inherent noise in the data — cannot be reduced

```
           High Bias              Low Bias
           (Underfitting)        (May overfit)

Complexity: |—————————————————————————→
             Simple               Complex
             
Test Error:  \_______/ (U-shaped curve)
              Sweet spot = optimal complexity
```

**Practical implication:**
- More data → reduces variance (helps complex models)
- Regularization → increases bias, decreases variance
- Ensemble methods → reduce variance (bagging) or bias (boosting)

---

### 2.2 The Curse of Dimensionality

As the number of features **d** grows:
1. Volume of the space grows exponentially — data becomes sparse
2. Distance metrics lose meaning (all points equidistant)
3. More data needed to cover the space: O(n^d)

**Consequences:**
- KNN fails in high dimensions
- Density estimation becomes intractable
- Manifold hypothesis: Real data lies on a low-dimensional manifold

**Solutions:**
- Dimensionality reduction (PCA, UMAP, autoencoders)
- Feature selection
- Deep learning (learns the manifold implicitly)

---

### 2.3 PAC Learning Theory (Probably Approximately Correct)

A concept class **C** is **PAC-learnable** if there exists an algorithm that, for any distribution D and any ε, δ > 0, produces a hypothesis h such that:

```
P[error(h) ≤ ε] ≥ 1 - δ
```
with sample complexity polynomial in 1/ε, 1/δ, and the size of the concept.

**VC Dimension:** The largest set of points that can be shattered (classified correctly in all possible ways) by a hypothesis class.

- VC dim of linear classifiers in d dimensions = d+1
- Higher VC dim → more expressive → more data needed to generalize

---

### 2.4 Cross-Validation

Never evaluate on training data. Use held-out sets.

#### k-Fold Cross-Validation
```
Data: [Fold 1 | Fold 2 | Fold 3 | Fold 4 | Fold 5]

Round 1: Train on [2,3,4,5], Test on [1]
Round 2: Train on [1,3,4,5], Test on [2]
...
Round 5: Train on [1,2,3,4], Test on [5]

Final score = mean of all 5 test scores
```

#### Stratified k-Fold
Preserves class distribution in each fold. **Always use for imbalanced datasets.**

#### Leave-One-Out (LOOCV)
k = n. Every sample is a test set once. High variance, computationally expensive.

#### Time Series Split
For temporal data — never shuffle! Future cannot leak into past.

---

### 2.5 Regularization

Regularization reduces overfitting by adding a penalty to model complexity.

#### L1 Regularization (Lasso)
```
Loss_regularized = Loss + λ Σ|wᵢ|
```
- Induces **sparsity** — drives weights to exactly zero
- Useful for feature selection
- Non-differentiable at 0 → subgradient or coordinate descent

#### L2 Regularization (Ridge)
```
Loss_regularized = Loss + λ Σwᵢ²
```
- Shrinks weights toward zero but rarely to exactly zero
- Closed-form solution exists
- Equivalent to a Gaussian prior on weights (MAP estimation)

#### Elastic Net
```
Loss_regularized = Loss + λ₁Σ|wᵢ| + λ₂Σwᵢ²
```
Combines L1 and L2. Best for correlated features.

#### Dropout (Neural Networks)
Randomly set activations to zero during training (probability p). At test time, scale by (1-p).

```python
# PyTorch
import torch.nn as nn
self.dropout = nn.Dropout(p=0.5)
x = self.dropout(x)  # training only
```
Interpretation: Averaging over an exponential ensemble of thinned networks.

#### Batch Normalization
Normalize activations within a mini-batch. Acts as regularizer AND accelerates training.

```
x̂ = (x - μ_batch) / sqrt(σ²_batch + ε)
y = γ * x̂ + β   # learnable scale and shift
```

---

### 2.6 Information Theory Foundations

#### Entropy
Measures uncertainty (information content) of a random variable:
```
H(X) = -Σ p(x) log₂ p(x)    [bits]
```
- H(X) = 0 → perfectly predictable
- H(X) = log₂(n) → maximum uncertainty for n outcomes

#### Cross-Entropy (Most Important ML Loss!)
```
H(p, q) = -Σ p(x) log q(x)
```
The loss function for classification. Measures how well predicted distribution q matches true distribution p.

#### KL Divergence
```
KL(p || q) = Σ p(x) log(p(x)/q(x))
```
Asymmetric "distance" between distributions. KL(p||q) ≠ KL(q||p).
Used in VAEs, information bottleneck, RLHF.

#### Mutual Information
```
I(X;Y) = H(X) - H(X|Y) = H(Y) - H(Y|X)
```
Measures how much knowing Y tells you about X. Key in feature selection and contrastive learning.

---

## 3. Core ML Algorithms (Classical)

### 3.1 Linear & Logistic Regression

#### Linear Regression

Assumes: `y = wᵀx + b + ε` where ε ~ N(0, σ²)

**Ordinary Least Squares (OLS) solution:**
```
w* = (XᵀX)⁻¹ Xᵀy
```
(closed-form, O(d³) for d features)

**Assumptions (GAUSS-MARKOV):**
1. Linearity: E[y|x] = wᵀx
2. No multicollinearity (XᵀX invertible)
3. Homoscedasticity: constant variance of ε
4. No autocorrelation in residuals

#### Logistic Regression (Classification)

Uses sigmoid to squash linear output to probability:
```
P(y=1|x) = σ(wᵀx) = 1 / (1 + e^(-wᵀx))
```

**Loss:** Binary Cross-Entropy:
```
L = -[y log(ŷ) + (1-y) log(1-ŷ)]
```

**Multi-class:** Softmax regression:
```
P(y=k|x) = exp(wₖᵀx) / Σⱼ exp(wⱼᵀx)
```

---

### 3.2 Decision Trees

A tree that partitions feature space using axis-aligned splits.

**Splitting criteria:**
- **Gini Impurity:** `Gini = 1 - Σ pₖ²` (CART)
- **Information Gain:** `IG = H(parent) - Σ wᵢ H(childᵢ)` (ID3, C4.5)

**Tree construction (greedy):**
```
At each node:
  For each feature f, for each threshold t:
    Compute impurity reduction for split (f, t)
  Choose best (f*, t*)
  Recurse on left and right children
  Stop when: max_depth, min_samples, or pure node
```

**Hyperparameters:**
- `max_depth`: Controls overfitting
- `min_samples_split / min_samples_leaf`: Minimum data in nodes
- `max_features`: Random subspace (used in Random Forests)

**Pros:** Interpretable, handles mixed types, no scaling needed
**Cons:** High variance, unstable, poor extrapolation

---

### 3.3 Ensemble Methods

#### Bagging (Bootstrap Aggregating)

Train B models on bootstrapped datasets (sampling with replacement). Average predictions (regression) or majority vote (classification).

**Effect:** Reduces variance, roughly constant bias.

```
Bootstrap samples: D₁, D₂, ..., Dᴮ (each is 63.2% unique samples)
Models:           f₁, f₂, ..., fᴮ
Prediction:       ŷ = (1/B) Σ fᵢ(x)
```

#### Random Forest

Bagging of decision trees + **random feature subsets** at each split.

Key insight: Decorrelating the trees improves ensemble performance.

```python
from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier(
    n_estimators=100,
    max_features='sqrt',    # sqrt(d) features per split
    max_depth=None,         # grow full trees
    min_samples_leaf=1,
    oob_score=True          # out-of-bag error estimate (free cross-val!)
)
```

**Out-of-bag (OOB) error:** Each tree is tested on ~36.8% of samples not in its bootstrap. No separate validation set needed.

#### Boosting

Train models sequentially, each focusing on errors of the previous.

**AdaBoost:**
```
1. Initialize weights wᵢ = 1/n
2. For t = 1 to T:
   a. Train weak learner hₜ on weighted samples
   b. Compute weighted error: εₜ = Σ wᵢ I(hₜ(xᵢ) ≠ yᵢ) / Σ wᵢ
   c. Compute learner weight: αₜ = 0.5 ln((1-εₜ)/εₜ)
   d. Update sample weights: wᵢ ← wᵢ exp(-αₜ yᵢ hₜ(xᵢ))
3. Final: H(x) = sign(Σ αₜ hₜ(x))
```

**Gradient Boosting (GBM):**
Fits each new tree to the negative gradient (residuals) of the loss function.

```
F₀(x) = initial prediction (e.g., mean)
For m = 1 to M:
  rᵢ = -∂L(yᵢ, F_{m-1}(xᵢ)) / ∂F_{m-1}(xᵢ)   [pseudo-residuals]
  Fit tree hₘ to residuals rᵢ
  Fₘ(x) = F_{m-1}(x) + η * hₘ(x)               [η = learning rate]
```

#### XGBoost (eXtreme Gradient Boosting)

The most widely used tabular ML algorithm. Key innovations:
1. **Regularized objective:** L(θ) + Ω(f) where Ω = γT + (λ/2)Σwⱼ²
2. **Second-order Taylor expansion** of loss
3. **Sparsity-aware split** (handles missing values natively)
4. **Column subsampling** (like Random Forest)
5. **Tree pruning** by depth-first then prune

```python
import xgboost as xgb
model = xgb.XGBClassifier(
    n_estimators=500,
    learning_rate=0.05,
    max_depth=6,
    subsample=0.8,
    colsample_bytree=0.8,
    reg_alpha=0.1,      # L1
    reg_lambda=1.0,     # L2
    use_label_encoder=False,
    eval_metric='logloss'
)
```

#### LightGBM

Faster than XGBoost via:
- **Histogram-based** splitting (buckets continuous features)
- **Gradient-based One-Side Sampling (GOSS):** Keep large-gradient samples
- **Exclusive Feature Bundling (EFB):** Bundle sparse features

**Grows leaf-wise** (best leaf first) vs XGBoost's level-wise → faster convergence.

---

### 3.4 Support Vector Machines (SVMs)

Find the maximum-margin hyperplane separating classes.

**Hard-margin SVM (linearly separable):**
```
Minimize: (1/2)||w||²
Subject to: yᵢ(wᵀxᵢ + b) ≥ 1 for all i
```

**Soft-margin SVM (with slack variables ξᵢ):**
```
Minimize: (1/2)||w||² + C Σ ξᵢ
Subject to: yᵢ(wᵀxᵢ + b) ≥ 1 - ξᵢ, ξᵢ ≥ 0
```

**The Kernel Trick:**
Replace xᵢᵀxⱼ with K(xᵢ, xⱼ) = φ(xᵢ)ᵀφ(xⱼ) — compute inner products in high-dimensional space without explicit mapping!

| Kernel | Formula | Use Case |
|---|---|---|
| Linear | xᵀz | Text classification |
| RBF (Gaussian) | exp(-γ||x-z||²) | Most common, general purpose |
| Polynomial | (γxᵀz + r)^d | Image classification |
| Sigmoid | tanh(γxᵀz + r) | Neural net analog |

**Key insight:** Only **support vectors** (points on the margin boundary) define the decision boundary. Very efficient for small datasets.

---

### 3.5 K-Nearest Neighbors (KNN)

Non-parametric, lazy learning. Prediction = majority vote (classification) or mean (regression) of k nearest training points.

**Distance metrics:**
- Euclidean: √Σ(xᵢ - zᵢ)²
- Manhattan: Σ|xᵢ - zᵢ|
- Cosine: 1 - (xᵀz)/(||x|| ||z||) — for text/embeddings

**Choosing k:**
- Small k → high variance, complex boundary
- Large k → high bias, smooth boundary
- Use cross-validation; odd k for binary classification

**Scaling is mandatory** — KNN is distance-based.

---

### 3.6 Naive Bayes

Applies Bayes' theorem with strong independence assumption:
```
P(y|x) ∝ P(y) Π P(xᵢ|y)   [assumes features are conditionally independent]
```

**Variants:**
- **Gaussian NB:** P(xᵢ|y) ~ N(μ_ky, σ_ky²)
- **Multinomial NB:** For discrete counts (text classification with TF)
- **Bernoulli NB:** Binary features (document-word occurrence)
- **Complement NB:** Better for imbalanced text classification

Despite the "naive" assumption, works surprisingly well for text. Very fast to train and predict.

---

### 3.7 Clustering Algorithms

#### K-Means
```
1. Initialize k centroids randomly
2. E-step: Assign each point to nearest centroid
3. M-step: Recompute centroids = mean of assigned points
4. Repeat until convergence
```

**Objective:** Minimize within-cluster sum of squares (WCSS):
```
WCSS = Σₖ Σ_{x ∈ Cₖ} ||x - μₖ||²
```

**K-Means++ initialization:** Choose centroids proportional to squared distance from existing centroids. Improves convergence.

**Determining k:** Elbow method (plot WCSS vs k), Silhouette score.

#### DBSCAN (Density-Based Spatial Clustering)

Finds clusters as dense regions separated by low-density areas.

```
Parameters: ε (radius), MinPts (minimum neighbors)

For each point x:
  If |N_ε(x)| ≥ MinPts: x is a CORE POINT
  If |N_ε(x)| < MinPts but x ∈ N_ε(core): x is a BORDER POINT
  Otherwise: x is NOISE (outlier)
```

**Advantages:** Discovers arbitrary shapes, handles outliers, no k needed.

#### Hierarchical Clustering (Agglomerative)
```
1. Start: n clusters (each point is a cluster)
2. Merge the two most similar clusters
3. Repeat until 1 cluster
4. Visualize as dendrogram — cut at desired level
```

**Linkage criteria:**
- Single: min pairwise distance (chaining)
- Complete: max pairwise distance (compact clusters)
- Average: mean pairwise distance
- Ward: minimize total within-cluster variance (most used)

---

### 3.8 Dimensionality Reduction

#### Principal Component Analysis (PCA)

Finds orthogonal directions of maximum variance.

```
1. Center data: X_centered = X - μ
2. Compute covariance: Σ = (1/n) X_centered^T X_centered
3. Eigendecompose: Σ = V D V^T
4. Project: X_reduced = X_centered @ V[:, :k]
```

**Key properties:**
- Principal components are eigenvectors of covariance matrix
- Eigenvalues = variance explained
- PCA is the optimal linear encoder (MSE sense)
- Whitening: divide by √eigenvalue → unit variance in each PC

#### t-SNE (t-Distributed Stochastic Neighbor Embedding)

Visualizes high-dimensional data in 2D/3D by preserving local structure.

```
1. Compute pairwise Gaussian similarities in high-d: pᵢⱼ
2. Initialize 2D embeddings
3. Compute pairwise t-distribution similarities in 2D: qᵢⱼ
4. Minimize KL(P||Q) via gradient descent
```

**Key parameters:**
- **Perplexity** (5-50): Effective number of neighbors. Higher → more global structure
- Not convex — different runs give different results
- Not deterministic, cannot extrapolate to new points (use UMAP instead)

#### UMAP (Uniform Manifold Approximation and Projection)

Better than t-SNE for preserving global structure, faster, and can generalize.

Uses Riemannian geometry and fuzzy simplicial sets. Generally preferred over t-SNE for large datasets.

---

## 4. Feature Engineering & Data Preprocessing

### 4.1 Data Cleaning

#### Handling Missing Values

```python
import pandas as pd
from sklearn.impute import SimpleImputer, KNNImputer

# Statistical imputation
imputer = SimpleImputer(strategy='median')   # or 'mean', 'most_frequent', 'constant'
X_imputed = imputer.fit_transform(X)

# KNN imputation (uses similar rows)
knn_imputer = KNNImputer(n_neighbors=5)

# Multiple imputation (gold standard)
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer
iter_imputer = IterativeImputer(max_iter=10, random_state=42)
```

**When to remove vs impute:**
- >40% missing in a column → consider dropping
- MCAR (Missing Completely At Random) → mean/median imputation OK
- MAR (Missing At Random) → model-based imputation
- MNAR (Missing Not At Random) → investigate; flag as separate feature

#### Outlier Detection

```python
# IQR method
Q1, Q3 = df.quantile([0.25, 0.75])
IQR = Q3 - Q1
mask = ~((df < Q1 - 1.5*IQR) | (df > Q3 + 1.5*IQR))

# Z-score
from scipy import stats
z_scores = np.abs(stats.zscore(df))
mask = (z_scores < 3).all(axis=1)

# Isolation Forest
from sklearn.ensemble import IsolationForest
iso = IsolationForest(contamination=0.05, random_state=42)
outlier_labels = iso.fit_predict(X)  # -1 = outlier
```

---

### 4.2 Feature Scaling

**Why:** Distance-based algorithms (KNN, SVM, KMeans) and gradient descent are sensitive to scale.

| Scaler | Formula | Use When |
|---|---|---|
| StandardScaler | (x - μ) / σ | Normally distributed, SVM, PCA |
| MinMaxScaler | (x - min) / (max - min) | Need bounded [0,1], image pixels |
| RobustScaler | (x - median) / IQR | Outliers present |
| MaxAbsScaler | x / max(|x|) | Sparse data |
| Normalizer | x / ||x|| | Text/NLP, cosine similarity |

```python
from sklearn.preprocessing import StandardScaler, RobustScaler
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)   # NEVER fit_transform on test!
```

---

### 4.3 Encoding Categorical Variables

#### One-Hot Encoding
```python
pd.get_dummies(df['color'], drop_first=True)   # drop_first avoids multicollinearity
```
Creates d binary columns for d categories. Use for low-cardinality (<15) nominal features.

#### Target/Mean Encoding
Replace category with mean of target variable.
```python
means = df.groupby('city')['price'].mean()
df['city_encoded'] = df['city'].map(means)
```
⚠️ Risk of target leakage — use with cross-val mean encoding.

#### Label Encoding
Assigns integers 0, 1, 2... — **only use for ordinal features** (size: small < medium < large).

#### Embeddings for High Cardinality
Use learned embeddings (entity embeddings) for features with thousands of categories (zip codes, user IDs). Works in neural networks.

---

### 4.4 Feature Creation & Selection

#### Interaction Features
```python
from sklearn.preprocessing import PolynomialFeatures
poly = PolynomialFeatures(degree=2, interaction_only=False)
X_poly = poly.fit_transform(X)   # adds x₁², x₂², x₁x₂, etc.
```

#### Feature Selection Methods

**Filter methods** (independent of model):
```python
from sklearn.feature_selection import SelectKBest, f_classif, mutual_info_classif
selector = SelectKBest(mutual_info_classif, k=20)
X_selected = selector.fit_transform(X, y)
```

**Wrapper methods** (use model performance):
- Recursive Feature Elimination (RFE)
- Forward/Backward selection

**Embedded methods** (built into model):
- Lasso (L1 regularization) → zero out irrelevant features
- Random Forest feature importance
- Gradient Boosting feature importance

```python
from sklearn.inspection import permutation_importance
result = permutation_importance(model, X_val, y_val, n_repeats=10)
# Most reliable: permutation importance on validation set
```

---

### 4.5 Handling Imbalanced Datasets

#### Resampling Techniques

```python
from imblearn.over_sampling import SMOTE, ADASYN
from imblearn.under_sampling import RandomUnderSampler
from imblearn.combine import SMOTETomek

# SMOTE: Synthetic Minority Oversampling
# Creates synthetic samples by interpolating between minority class neighbors
smote = SMOTE(k_neighbors=5, random_state=42)
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)

# Combined approach (best practice)
smotetomek = SMOTETomek(random_state=42)
X_res, y_res = smotetomek.fit_resample(X_train, y_train)
```

**Algorithm-level solutions:**
- `class_weight='balanced'` in sklearn models
- `scale_pos_weight` in XGBoost
- Focal Loss in deep learning

**Evaluation metrics for imbalance:** Always use Precision, Recall, F1, PR-AUC, ROC-AUC — never raw accuracy.

---

## 5. Deep Learning Fundamentals

### 5.1 The Artificial Neuron

A single neuron computes:
```
z = Σ wᵢxᵢ + b   [linear combination]
a = φ(z)          [activation function]
```

Biologically inspired by the McCulloch-Pitts neuron (1943), formalized into the Perceptron by Rosenblatt (1958).

### 5.2 Activation Functions

| Function | Formula | Derivative | Use Case | Problem |
|---|---|---|---|---|
| Sigmoid | 1/(1+e^-x) | σ(1-σ) | Binary output, old | Vanishing gradient |
| Tanh | (e^x-e^-x)/(e^x+e^-x) | 1-tanh² | Hidden layers (older) | Vanishing gradient |
| ReLU | max(0, x) | 1 if x>0, else 0 | Default hidden | Dying ReLU |
| Leaky ReLU | max(αx, x), α=0.01 | 1 if x>0, else α | Fixes dying ReLU | — |
| ELU | x if x>0, α(e^x-1) else | Smooth | Better than ReLU | Slower |
| GELU | x·Φ(x) | Complex | Transformers, BERT | Computationally heavy |
| Swish | x·σ(x) | Complex | Modern deep nets | — |
| SiLU | x·σ(x) | Same as Swish | LLaMA, Mistral | — |
| Softmax | e^xᵢ/Σe^xⱼ | Complex | Multi-class output | Numerical instability |
| Mish | x·tanh(softplus(x)) | Complex | YOLOv4 | — |

**Why GELU dominates in modern LLMs:**
- Smooth approximation to ReLU
- Stochastic interpretation: GELU(x) = x · P(X ≤ x) where X ~ N(0,1)
- Better gradient flow than ReLU in very deep networks

---

### 5.3 Backpropagation

The algorithm that makes deep learning possible. Computes gradients of the loss w.r.t. all parameters using the **chain rule** of calculus.

**Forward pass:**
```
x → [Layer 1] → a₁ → [Layer 2] → a₂ → ... → [Output] → Loss L
```

**Backward pass (chain rule):**
```
∂L/∂w₁ = ∂L/∂a₂ · ∂a₂/∂a₁ · ∂a₁/∂z₁ · ∂z₁/∂w₁
```

**Computational graph view:**
Every operation (add, multiply, matrix multiply, ReLU) has:
- A forward function
- A backward function (computes local gradient)

Gradients accumulate via the chain rule as they flow backward.

```python
# PyTorch autograd (automatic differentiation)
import torch
x = torch.tensor([2.0], requires_grad=True)
y = x ** 3 + 2*x
y.backward()
print(x.grad)   # dy/dx = 3x² + 2 = 14.0 at x=2
```

**Gradient flow problem:**
- Sigmoid/Tanh squash gradients → vanishing gradients in deep nets
- ReLU can "die" if gradients become exactly 0

**Solutions:**
- Skip connections (ResNets)
- Layer/Batch Normalization
- Better initialization (He, Xavier/Glorot)
- Gradient clipping (for RNNs)

---

### 5.4 Weight Initialization

Poor initialization → exploding/vanishing gradients before training even begins.

**Xavier/Glorot initialization** (for Tanh/Sigmoid):
```
w ~ U[-1/√n, 1/√n]   or   w ~ N(0, 1/n)
where n = number of input units
```

**He initialization** (for ReLU):
```
w ~ N(0, 2/n)
```
Factor of 2 compensates for ReLU zeroing half the outputs.

```python
import torch.nn as nn
nn.init.kaiming_normal_(layer.weight, mode='fan_in', nonlinearity='relu')
nn.init.xavier_normal_(layer.weight)
```

---

### 5.5 Optimization Algorithms

#### Stochastic Gradient Descent (SGD)
```
w ← w - η · ∇L(w; xᵢ, yᵢ)   [one sample at a time]
```

**Mini-batch SGD** (standard practice):
```
w ← w - η · (1/B) Σᵢ∈B ∇L(w; xᵢ, yᵢ)
```
Typical batch size: 32-512. Larger batches → smoother gradients but may converge to sharp minima.

#### Momentum
Accumulates gradient direction, dampens oscillations:
```
v ← β·v - η·∇L
w ← w + v
```
β = 0.9 typical. Accelerates along consistent gradient directions.

#### Adam (Adaptive Moment Estimation)

The most widely used optimizer for deep learning:
```
m ← β₁·m + (1-β₁)·∇L         [first moment: mean of gradients]
v ← β₂·v + (1-β₂)·∇L²        [second moment: variance of gradients]
m̂ = m / (1-β₁^t)              [bias correction]
v̂ = v / (1-β₂^t)
w ← w - η · m̂ / (√v̂ + ε)
```

Default: β₁=0.9, β₂=0.999, ε=1e-8, η=1e-3

**Why Adam works:** Adaptive learning rate per parameter — large updates for rarely-seen features, small updates for common features.

#### AdamW (Adam + Weight Decay)

Decouples weight decay from gradient update (fixes L2 regularization in Adam):
```
w ← w - η · m̂/(√v̂ + ε) - η·λ·w
```
**Standard choice for LLMs and modern neural networks.**

#### Learning Rate Schedulers

```python
# Cosine annealing (most popular for LLMs)
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=100)

# Warmup + cosine (transformer standard)
from transformers import get_cosine_schedule_with_warmup
scheduler = get_cosine_schedule_with_warmup(
    optimizer, 
    num_warmup_steps=1000, 
    num_training_steps=100000
)

# One-cycle (super-convergence)
scheduler = torch.optim.lr_scheduler.OneCycleLR(optimizer, max_lr=0.01, total_steps=1000)
```

---

### 5.6 Normalization Techniques

| Technique | Normalizes Over | Best For |
|---|---|---|
| Batch Norm | Batch dimension | CNNs, large batches |
| Layer Norm | Feature dimension | Transformers, RNNs |
| Instance Norm | Spatial dimension per sample | Style transfer |
| Group Norm | Groups of channels | Small batches, detection |
| RMS Norm | Feature dimension (no mean) | LLaMA, modern LLMs |

**RMSNorm** (used in LLaMA, Mistral):
```
RMSNorm(x) = x / RMS(x) * γ
where RMS(x) = √(1/d Σ xᵢ²)
```
Faster than LayerNorm — removes mean-centering operation, empirically equivalent.

---

### 5.7 Training a Neural Network End-to-End (PyTorch)

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader, TensorDataset

# 1. Define model
class MLP(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(input_dim, hidden_dim),
            nn.BatchNorm1d(hidden_dim),
            nn.GELU(),
            nn.Dropout(0.3),
            nn.Linear(hidden_dim, hidden_dim // 2),
            nn.GELU(),
            nn.Linear(hidden_dim // 2, output_dim)
        )
    
    def forward(self, x):
        return self.net(x)

# 2. Setup
model = MLP(128, 256, 10).to('cuda')
optimizer = optim.AdamW(model.parameters(), lr=1e-3, weight_decay=0.01)
criterion = nn.CrossEntropyLoss()
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=50)

# 3. Training loop
for epoch in range(50):
    model.train()
    for X_batch, y_batch in train_loader:
        X_batch, y_batch = X_batch.cuda(), y_batch.cuda()
        optimizer.zero_grad()
        logits = model(X_batch)
        loss = criterion(logits, y_batch)
        loss.backward()
        torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)  # gradient clipping
        optimizer.step()
    scheduler.step()
    
    # 4. Validation
    model.eval()
    with torch.no_grad():
        val_loss = sum(criterion(model(xb.cuda()), yb.cuda()) for xb, yb in val_loader)
    print(f"Epoch {epoch}: Val Loss = {val_loss:.4f}")
```

---

## 6. Convolutional Neural Networks (CNNs)

### 6.1 The Convolution Operation

A convolution applies a **kernel** (filter) across an input, computing dot products at every position:

```
(f * g)[i,j] = Σₘ Σₙ f[m,n] · g[i-m, j-n]
```

**3 Key properties that make CNNs work:**
1. **Translation equivariance:** Same features detected regardless of position
2. **Parameter sharing:** One filter reused across all positions (vs fully-connected)
3. **Local connectivity:** Each neuron sees only a local receptive field

```
Input: 32×32×3
Kernel: 3×3×3×64 → 64 filters
Output feature map: 30×30×64 (with no padding)
Output size formula: (H - Fh + 2P) / S + 1
```

**Hyperparameters:**
- **Kernel size:** 3×3 (most common), 5×5, 7×7
- **Stride:** How much kernel moves per step (default 1)
- **Padding:** "same" (output = input size) or "valid" (no padding)
- **Depth:** Number of filters = number of output channels

---

### 6.2 CNN Architectural Blocks

**Pooling layers:**
- Max pooling: Takes maximum in each window — preserves dominant features
- Average pooling: Averages each window — smoother
- Global Average Pooling (GAP): Averages entire feature map → vector

**Batch Normalization → ReLU → Convolution** (modern ordering)

**Depthwise Separable Convolution** (MobileNet):
```
Standard conv: H×W×Cin × Kh×Kw×Cin × Cout  ← expensive
Depthwise separable: 
  Step 1 (Depthwise): H×W×Cin filters of Kh×Kw×1
  Step 2 (Pointwise): H×W×Cin → H×W×Cout with 1×1 convs
  
Cost reduction: ~8-9× fewer operations
```

---

### 6.3 Landmark CNN Architectures

#### AlexNet (2012) — The CNN Revolution
- First to win ImageNet decisively (15.3% top-5 error vs 26% prior)
- Key innovations: ReLU, Dropout, data augmentation, GPU training

#### VGG (2014)
- Simple: only 3×3 convolutions, max pooling, increasing depth
- VGG-16, VGG-19 — still widely used as feature extractors

#### GoogLeNet / Inception (2014)
**Inception module:** Parallel 1×1, 3×3, 5×5 convolutions + pooling in one block. 
- 1×1 convolutions reduce dimensionality before expensive 3×3, 5×5 ops

#### ResNet (2015) — Residual Networks

**Skip connections** solve vanishing gradient in very deep networks:
```
output = F(x, {Wᵢ}) + x    [residual/skip connection]
```
If the optimal function is close to identity, it's easier to learn F(x) → 0 than to learn identity directly.

Versions: ResNet-18, -34, -50, -101, -152.
ResNet-50 uses **bottleneck blocks**: 1×1 → 3×3 → 1×1 (reduces parameters)

#### DenseNet (2017)
Each layer connects to ALL subsequent layers:
```
xₗ = Hₗ([x₀, x₁, ..., x_{l-1}])   [concatenation]
```
Better gradient flow, feature reuse, compact parameters.

#### EfficientNet (2019)
**Compound scaling:** Scale width, depth, and resolution together using a coefficient φ:
```
depth: d = α^φ
width: w = β^φ  
resolution: r = γ^φ
α·β²·γ² ≈ 2   (found by NAS)
```
EfficientNet-B0 to B7 — state-of-the-art efficiency/accuracy tradeoff.

---

### 6.4 Object Detection

#### Two-Stage Detectors (Region-based)

**R-CNN (2014):** Extract ~2000 region proposals (selective search) → CNN on each → classify

**Fast R-CNN:** Share CNN computation across proposals, add ROI Pooling.

**Faster R-CNN:** Replace selective search with **Region Proposal Network (RPN)** — fully convolutional, slides over feature map.

```
Image → Backbone CNN → Feature Map
                             ↓            ↓
                           RPN          ROI Pooling
                        (proposals)    (proposal features)
                             ↓
                       Classification + Regression
```

#### One-Stage Detectors (YOLO Family)

**YOLOv1 (2016):** Divide image into S×S grid. Each cell predicts B boxes + confidence + C class probs.

**YOLO3–8 evolutionary improvements:**
- Multi-scale detection (3 scales)
- Feature Pyramid Networks (FPN) for small objects
- Anchor boxes
- Darknet-53 / CSPDarknet backbone
- Label smoothing, Mosaic augmentation
- Decoupled head, Task-Aligned Learning

**YOLOv8/v10/v11 (2024-2025):**
- Anchor-free detection
- New backbone architectures
- ~50ms inference on edge devices

**SSD (Single Shot MultiBox Detector):**
Predicts at multiple scales directly from backbone feature maps.

---

## 7. Recurrent Networks: RNNs, LSTMs, GRUs

### 7.1 Vanilla RNN

Processes sequential data by maintaining a hidden state:
```
hₜ = tanh(Wₕhₜ₋₁ + Wₓxₜ + b)
yₜ = Wᵧhₜ
```

**Problems:**
1. **Vanishing gradients:** For long sequences, ∂L/∂h₀ = Π (∂hₜ/∂hₜ₋₁) → 0 or ∞
2. **Short-term memory:** Can't capture long-range dependencies

---

### 7.2 LSTM (Long Short-Term Memory)

Introduces a **cell state** (cₜ) — a highway for gradients to flow with minimal manipulation:

```
Forget gate:  fₜ = σ(Wf·[hₜ₋₁, xₜ] + bf)       [what to forget from cell]
Input gate:   iₜ = σ(Wi·[hₜ₋₁, xₜ] + bi)        [what new info to store]
Cell update:  c̃ₜ = tanh(Wc·[hₜ₋₁, xₜ] + bc)     [candidate values]
Cell state:   cₜ = fₜ ⊙ cₜ₋₁ + iₜ ⊙ c̃ₜ           [update cell]
Output gate:  oₜ = σ(Wo·[hₜ₋₁, xₜ] + bo)        [what to output]
Hidden state: hₜ = oₜ ⊙ tanh(cₜ)                 [filtered cell state]
```

**Key insight:** Sigmoid gates control information flow. The additive cell state update prevents gradient vanishing along the "CEC" (Constant Error Carousel).

---

### 7.3 GRU (Gated Recurrent Unit)

Simplified LSTM with fewer parameters:
```
Reset gate:  rₜ = σ(Wr·[hₜ₋₁, xₜ])
Update gate: zₜ = σ(Wz·[hₜ₋₁, xₜ])
New content: h̃ₜ = tanh(W·[rₜ ⊙ hₜ₋₁, xₜ])
Hidden:      hₜ = (1-zₜ) ⊙ hₜ₋₁ + zₜ ⊙ h̃ₜ
```

GRU merges cell state and hidden state. Similar performance to LSTM in practice, 25% fewer parameters.

---

### 7.4 Bidirectional RNNs

Process sequence in both directions — capture past AND future context:
```
Forward:   →  h₁, h₂, h₃, ..., hₙ
Backward:  ←  h₁', h₂', h₃', ..., hₙ'
Combined:  [hₜ; hₜ'] at each timestep
```
Used in BERT, original seq2seq models, NER, POS tagging.

---

### 7.5 Sequence-to-Sequence (Seq2Seq)

Encoder-decoder architecture for translation, summarization, Q&A:

```
Input: "Hello world" → Encoder (LSTM) → context vector c
Output: "Bonjour monde" ← Decoder (LSTM conditioned on c)
```

**Bottleneck problem:** Fixed-length context vector c loses information for long sequences.

**Solution: Attention Mechanism (Bahdanau, 2015)**
```
eₜₛ = vᵀ tanh(Wₕhₛ + Wₛdₜ)    [alignment score]
αₜₛ = softmax(eₜₛ)               [attention weights]
cₜ  = Σₛ αₜₛhₛ                   [context vector for each decoder step]
```
The decoder looks at ALL encoder states at each step, weighted by relevance.

---

## 8. The Transformer Architecture

The most important architecture in modern AI, introduced in "Attention Is All You Need" (Vaswani et al., 2017).

### 8.1 Self-Attention (The Core Mechanism)

```
Q = X · Wq   (Queries)
K = X · Wk   (Keys)
V = X · Wv   (Values)

Attention(Q, K, V) = softmax(QKᵀ / √dₖ) · V
```

**Intuition:**
- Each token creates a **query** ("what am I looking for?")
- Each token creates a **key** ("what do I contain?")
- Dot product Q·Kᵀ scores relevance
- Scaling by √dₖ prevents softmax saturation
- Result: weighted sum of **values** V based on relevance

**Complexity:** O(n²·d) where n = sequence length, d = dimension.
For n=4096, this is expensive — motivates FlashAttention, linear attention, etc.

---

### 8.2 Multi-Head Attention

Run h attention heads in parallel, each with different projections:
```
head_i = Attention(Q·Wq_i, K·Wk_i, V·Wv_i)
MultiHead(Q,K,V) = Concat(head₁,...,headₕ) · Wₒ
```

**Why multiple heads?** Each head can attend to different aspects:
- Head 1: syntactic relationships
- Head 2: semantic similarity
- Head 3: coreference
- ...

Typical: h=8 heads, d_model=512, d_head=d_model/h=64.

---

### 8.3 Transformer Block

Each block consists of:
```
x → LayerNorm → Multi-Head Attention → (+residual) → LayerNorm → FFN → (+residual)
```

**Feed-Forward Network (FFN):**
```
FFN(x) = max(0, xW₁ + b₁)W₂ + b₂
```
Dimension: d_model → 4×d_model → d_model (fan out by 4×).

**Pre-norm vs Post-norm:**
- Original paper: post-norm (normalize AFTER residual)
- Modern LLMs (GPT-3, LLaMA): pre-norm (normalize BEFORE attention) — more stable training

---

### 8.4 Positional Encoding

Transformers have no inherent sense of order. Must inject position information.

**Sinusoidal (original, fixed):**
```
PE(pos, 2i)   = sin(pos / 10000^(2i/d))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d))
```
Can extrapolate to unseen lengths, but not as good as learned.

**Learned Positional Embeddings:**
Trainable vectors for each position. GPT-2 style.

**Relative Positional Encoding:**
Encode position as relative offsets between tokens, not absolute positions.
- T5 uses learned relative biases
- **ALiBi:** Add linear bias to attention logits (fast, good extrapolation)
- **RoPE (Rotary Position Embedding):** Used in LLaMA, Mistral, GPT-NeoX

**RoPE:** Rotates Q and K vectors in complex space by position angle:
```
f(x, m) = x · e^(imθ)   [multiply by rotation matrix at position m]
```
Relative positions preserved: (Q_m · K_n)* = (Q_0 · K_{n-m})
Excellent extrapolation beyond training length (with YaRN/LongRoPE extensions).

---

### 8.5 Encoder-Only, Decoder-Only, Encoder-Decoder

**Encoder-only (BERT family):**
- Bidirectional — every token attends to all others
- Best for: Classification, NER, sentence embeddings, understanding tasks
- Examples: BERT, RoBERTa, DeBERTa, ELECTRA

**Decoder-only (GPT family):**
- Causal (autoregressive) — each token only attends to past tokens
- Best for: Text generation, code, dialogue, completion
- Examples: GPT series, LLaMA, Mistral, Falcon, Claude, Gemini

**Encoder-Decoder (T5/BART family):**
- Encoder processes input fully; decoder generates output autoregressively
- Best for: Translation, summarization, Q&A with source document
- Examples: T5, BART, Flan-T5, mT5

---

### 8.6 FlashAttention

Standard attention: O(n²) memory, multiple HBM reads/writes.

**FlashAttention (Dao et al., 2022):**
- Fuses the attention computation into a single GPU kernel
- Tiling: computes in SRAM blocks, never materializes full n×n matrix
- IO-complexity: O(n²/B) HBM reads (B = SRAM block size)

**Result:** 2-4× speedup, 5-20× memory reduction. Now standard in all production transformers.

**FlashAttention-2/3 (2023/2024):**
- Better parallelism across sequence dimension
- Optimize for H100 Tensor Cores
- 2-3× faster than FA1

---

## 9. Natural Language Processing (NLP)

### 9.1 Text Preprocessing Pipeline

```python
import re
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer, WordNetLemmatizer

# Standard preprocessing
def preprocess(text):
    # 1. Lowercase
    text = text.lower()
    # 2. Remove HTML, URLs, special chars
    text = re.sub(r'<[^>]+>', ' ', text)
    text = re.sub(r'http\S+', ' ', text)
    text = re.sub(r'[^a-z0-9\s]', ' ', text)
    # 3. Tokenize
    tokens = word_tokenize(text)
    # 4. Remove stopwords
    stops = set(stopwords.words('english'))
    tokens = [t for t in tokens if t not in stops and len(t) > 2]
    # 5. Lemmatize (better than stemming for NLP)
    lemmatizer = WordNetLemmatizer()
    tokens = [lemmatizer.lemmatize(t) for t in tokens]
    return tokens
```

---

### 9.2 Word Representations

#### Bag of Words (BoW) & TF-IDF

```
TF-IDF(t, d) = TF(t,d) × IDF(t)
TF(t,d)  = count(t,d) / total_tokens(d)
IDF(t)   = log(N / df(t))   [N = docs, df = docs containing t]
```

Sparse, high-dimensional, no word order, no semantics.

#### Word2Vec (2013)

Neural network trained to predict context from word (Skip-gram) or word from context (CBOW).

**Skip-gram objective:**
```
Maximize: Σ Σ log P(context_word | center_word)
          sentences w ∈ context
```

**Negative sampling:** Instead of normalizing over full vocabulary, sample k "negative" words.

**Resulting embeddings:**
- Similar words cluster together: `king - man + woman ≈ queen`
- Dimension: typically 100-300
- Captures semantic and syntactic relationships

#### GloVe (Global Vectors)
```
Jₒ = Σᵢⱼ f(Xᵢⱼ) (wᵢᵀw̃ⱼ + bᵢ + b̃ⱼ - log Xᵢⱼ)²
```
Trains on co-occurrence matrix. Combines local (Word2Vec) and global (matrix factorization) statistics.

#### FastText
Represents words as bags of character n-grams. Handles OOV words (morphology-aware).

---

### 9.3 Subword Tokenization

Modern LLMs don't tokenize by word — they use subword tokenization.

**Byte Pair Encoding (BPE):**
```
1. Start with character vocabulary
2. Count all adjacent pairs
3. Merge most frequent pair into new token
4. Repeat until target vocab size (e.g., 50,000)

"unrelated" → "un" "rel" "ated"  (if those are learned merges)
```

**WordPiece (BERT):**
Similar to BPE but maximizes language model likelihood instead of frequency.
Prefix unknown subwords with "##": `playing → play ##ing`

**SentencePiece (T5, LLaMA):**
Language-agnostic, treats text as raw bytes, handles spaces, multilingual.

**Tiktoken (GPT-4, GPT-3.5):**
Fast BPE implementation with GPT-specific optimizations.

**Byte-level BPE (GPT-2):**
Operates on raw UTF-8 bytes. Never produces UNK tokens.

---

### 9.4 BERT and the Encoder Revolution

**BERT (Bidirectional Encoder Representations from Transformers, 2018)**

Pre-training tasks:
1. **Masked Language Modeling (MLM):** Mask 15% of tokens, predict them
2. **Next Sentence Prediction (NSP):** Predict if two sentences are consecutive

```
Input: [CLS] sentence_A [SEP] sentence_B [SEP]
Mask:  15% tokens randomly → [MASK]

Loss = MLM loss + NSP loss (binary CE)
```

**BERT-base:** 12 layers, 768 hidden, 12 heads → 110M params
**BERT-large:** 24 layers, 1024 hidden, 16 heads → 340M params

**Fine-tuning BERT for downstream tasks:**
```python
from transformers import BertForSequenceClassification, BertTokenizer
import torch

tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertForSequenceClassification.from_pretrained('bert-base-uncased', num_labels=2)

inputs = tokenizer("I love NLP!", return_tensors='pt', truncation=True, padding=True)
outputs = model(**inputs, labels=torch.tensor([1]))
loss = outputs.loss
logits = outputs.logits
```

**Variants:**
- **RoBERTa:** More data, dynamic masking, no NSP → better
- **DeBERTa:** Disentangled attention (position and content separate), Virtual Adversarial Training
- **ALBERT:** Parameter sharing across layers → smaller, faster
- **DistilBERT:** Knowledge distillation → 60% of params, 97% performance

---

### 9.5 Text Generation: GPT Architecture

GPT (Generative Pre-trained Transformer) uses a decoder-only transformer trained with causal language modeling:

```
P(x₁, x₂, ..., xₙ) = Π P(xₜ | x₁, ..., xₜ₋₁)
```

**Autoregressive generation:**
```python
from transformers import GPT2LMHeadModel, GPT2Tokenizer

tokenizer = GPT2Tokenizer.from_pretrained('gpt2')
model = GPT2LMHeadModel.from_pretrained('gpt2')

inputs = tokenizer("The future of AI is", return_tensors='pt')
outputs = model.generate(
    **inputs,
    max_new_tokens=100,
    temperature=0.8,        # controls randomness
    top_k=50,               # top-k sampling
    top_p=0.95,             # nucleus sampling
    do_sample=True,
    repetition_penalty=1.1  # penalize repeated tokens
)
print(tokenizer.decode(outputs[0]))
```

**Decoding strategies:**
- **Greedy:** Always pick argmax → deterministic but repetitive
- **Beam search:** Maintain k best sequences at each step → good for translation
- **Top-k sampling:** Sample from top k tokens by probability
- **Nucleus (top-p):** Sample from smallest set of tokens summing to probability p
- **Temperature:** T < 1 = more focused; T > 1 = more random

---

### 9.6 Instruction Tuning & RLHF

**Supervised Fine-Tuning (SFT):**
Fine-tune a base language model on (instruction, response) pairs.

**RLHF (Reinforcement Learning from Human Feedback):**
```
Phase 1: Supervised Fine-Tuning (SFT)
  Base model → Fine-tune on instruction-following demos

Phase 2: Reward Model (RM) Training
  Collect human preferences: prefer response A over B
  Train RM: r(x, y) predicts human preference score

Phase 3: PPO Training
  Use RM to score model outputs
  Update policy to maximize reward via PPO
  KL penalty: keep close to SFT model (prevent reward hacking)

L_PPO = E[r(x,y)] - β·KL[π_θ(y|x) || π_sft(y|x)]
```

**Direct Preference Optimization (DPO):**
Skips the separate reward model — reformulates RLHF as supervised learning:
```
L_DPO(θ) = -E[(x,yw,yl)] log σ(β log(π_θ(yw|x)/π_ref(yw|x)) - β log(π_θ(yl|x)/π_ref(yl|x)))
```
Where `yw` = preferred, `yl` = dispreferred response. Simpler, more stable.

---

## 10. Computer Vision

### 10.1 Vision Transformers (ViT)

"An Image is Worth 16×16 Words" (Dosovitskiy et al., 2020).

**Key idea:** Split image into patches, treat as token sequence.

```
Image: 224×224×3
Patches: 14×14 patches of 16×16 pixels each → 196 patch tokens
Flatten each patch: 16×16×3 = 768 dimensions
Add position embedding
[CLS] token + 196 patch tokens → Transformer encoder
```

**Advantages:**
- Global receptive field from layer 1
- Scales better with data than CNNs
- Pretrained ViTs: ViT-B/16, ViT-L/16, ViT-H/14

**Disadvantages:**
- Needs large datasets (overcomes lack of CNN inductive bias)
- Quadratic attention complexity

**DeiT, Swin Transformer, MAE:**
- **DeiT:** Efficient training of ViT with knowledge distillation
- **Swin Transformer:** Hierarchical ViT with window-based attention (O(n) complexity)
- **MAE (Masked Autoencoders):** Mask 75% of patches, reconstruct — powerful self-supervised pretraining

---

### 10.2 Semantic & Instance Segmentation

**Semantic segmentation:** Assign class label to every pixel.
- FCN (Fully Convolutional Network): Remove final FC layers, use transposed conv for upsampling
- U-Net: Encoder-decoder with skip connections (medical imaging standard)
- DeepLab series: Atrous (dilated) convolutions, ASPP module

**Instance segmentation:** Detect individual objects + segment each.
- **Mask R-CNN:** Extends Faster R-CNN with a mask prediction head
- **YOLACT:** Real-time instance segmentation

**Panoptic segmentation:** Combines semantic + instance.
- Every pixel labeled (things + stuff)

---

### 10.3 Image Generation

**Generative Adversarial Networks (GANs):** See Section 11.
**Diffusion Models:** See Section 11.
**CLIP (Contrastive Language-Image Pretraining):** See Section 11.

---

## 11. Generative Models

### 11.1 Variational Autoencoders (VAEs)

Encoder-decoder architecture with a **probabilistic bottleneck**.

```
Encoder: q_φ(z|x) = N(μ_φ(x), σ_φ(x)²)    [approximate posterior]
Decoder: p_θ(x|z)                             [generative model]
Prior:   p(z) = N(0, I)                       [latent space prior]

ELBO = E_q[log p_θ(x|z)] - KL(q_φ(z|x) || p(z))
       ──────────────────   ──────────────────────
        reconstruction         regularization
           term                    term
```

**Reparameterization trick:**
```
z = μ + σ * ε,  ε ~ N(0, I)
```
Allows backprop through the sampling operation.

**Use cases:** Image generation, data augmentation, representation learning, anomaly detection.

---

### 11.2 Generative Adversarial Networks (GANs)

Two networks in competition:
```
Generator G(z; θg):      Noise z → Fake data
Discriminator D(x; θd):  Data x → Real/Fake probability

Minimax game:
min_G max_D E[log D(x)] + E[log(1 - D(G(z)))]
```

**Training dynamics:**
1. Train D: maximize distinguishing real vs fake
2. Train G: fool D (minimize D's accuracy on fakes)
3. Nash equilibrium: D(x) = 0.5 everywhere (can't distinguish)

**GAN variants:**

| Variant | Key Idea | Best For |
|---|---|---|
| DCGAN | Conv/Deconv architecture | Basic image synthesis |
| WGAN | Wasserstein distance, gradient penalty | Training stability |
| StyleGAN/2/3 | Disentangled style control, progressive growing | High-quality faces |
| BigGAN | Large-scale, class-conditional | ImageNet synthesis |
| Pix2Pix | Paired image-to-image translation | Image editing |
| CycleGAN | Unpaired image translation (cycle consistency) | Style transfer |
| GauGAN/SPADE | Semantic image synthesis | Landscape generation |

**GAN training challenges:**
- **Mode collapse:** Generator produces limited diversity
- **Training instability:** Loss oscillation
- **Evaluation:** Use FID (Fréchet Inception Distance), IS (Inception Score)

---

### 11.3 Diffusion Models

The dominant paradigm for image generation (DALL-E 2, Stable Diffusion, Midjourney, FLUX).

**Forward process (adding noise):**
```
q(xₜ|xₜ₋₁) = N(xₜ; √(1-βₜ)xₜ₋₁, βₜI)

After T steps: xT ~ N(0, I)  [pure Gaussian noise]
```

**Reverse process (learned denoising):**
```
p_θ(xₜ₋₁|xₜ) = N(xₜ₋₁; μ_θ(xₜ,t), Σ_θ(xₜ,t))
```

**Training objective:**
```
L = E[||ε - ε_θ(√(ᾱₜ)x₀ + √(1-ᾱₜ)ε, t)||²]
```
Predict the noise ε added at timestep t. Surprisingly simple!

**Key architectures:**
- **U-Net with time embedding** — original DDPM, Stable Diffusion
- **DiT (Diffusion Transformer)** — patches + transformers, Sora, Stable Diffusion 3, Flux

**Samplers (inference acceleration):**
- DDPM: 1000 steps
- DDIM: 50-100 steps (deterministic)
- DPM-Solver: 20-30 steps
- Flow Matching (Rectified Flow): Straight paths, ~8 steps (Stable Diffusion 3)

**Classifier-Free Guidance (CFG):**
```
ε_guided = ε_uncond + w * (ε_cond - ε_uncond)
```
w = guidance scale (7-12 typical). Higher w → more adherent to prompt, less diversity.

---

### 11.4 CLIP & Multimodal Models

**CLIP (Contrastive Language-Image Pre-training, OpenAI 2021):**

Trains vision encoder + text encoder to maximize cosine similarity of matching image-text pairs:
```
L = -Σᵢ log(exp(sim(vᵢ, tᵢ)/τ) / Σⱼ exp(sim(vᵢ, tⱼ)/τ))
```
Trained on 400M image-text pairs from the internet.

**Zero-shot classification:**
```python
import clip, torch
model, preprocess = clip.load("ViT-B/32")

image = preprocess(image_pil).unsqueeze(0)
text = clip.tokenize(["a dog", "a cat", "a car"])

image_features = model.encode_image(image)
text_features = model.encode_text(text)

probs = (100.0 * image_features @ text_features.T).softmax(dim=-1)
# No training needed! Just compare embeddings.
```

**CLIP successors:**
- **SigLIP (Google):** Sigmoid loss instead of softmax — scales better
- **DINOv2 (Meta):** Self-supervised ViT, excellent dense features
- **OpenCLIP:** Open-source CLIP with better data curation

---

## 12. Reinforcement Learning

### 12.1 Markov Decision Process (MDP) Formalism

An MDP is defined by (S, A, P, R, γ):
- **S:** State space
- **A:** Action space
- **P(s'|s,a):** Transition dynamics
- **R(s,a,s'):** Reward function
- **γ:** Discount factor (0 < γ < 1)

**Goal:** Find policy π(a|s) that maximizes expected cumulative return:
```
G_t = Σ_{k=0}^{∞} γᵏ R_{t+k+1}
```

**Value functions:**
```
V^π(s) = E_π[G_t | S_t = s]    [state value]
Q^π(s,a) = E_π[G_t | S_t=s, A_t=a]    [action value]

Bellman equation: V^π(s) = Σₐ π(a|s) Σ_{s'} P(s'|s,a)[R(s,a,s') + γV^π(s')]
```

**Optimal policy:** π*(s) = argmax_a Q*(s,a)

---

### 12.2 Q-Learning

Model-free, off-policy algorithm that learns Q*(s,a) directly:

```
Q(s,a) ← Q(s,a) + α[r + γ max_{a'} Q(s',a') - Q(s,a)]
                                     ─────────────────
                                      TD target        TD error
```

**ε-greedy exploration:**
```
With prob ε: random action (explore)
With prob 1-ε: argmax_a Q(s,a) (exploit)
Decay ε over time.
```

**DQN (Deep Q-Network, DeepMind 2015):**
Replaces Q-table with neural network Q(s, a; θ).

Key innovations:
1. **Experience replay buffer:** Store (s,a,r,s') tuples, sample random batches
2. **Target network:** Separate frozen network θ⁻ for targets, updated periodically
3. **Reward clipping:** Clip to [-1, 1]

```
L(θ) = E[(r + γ max_{a'} Q(s',a'; θ⁻) - Q(s,a; θ))²]
```

**Double DQN:** Decouple action selection and evaluation to reduce overestimation:
```
Target = r + γ Q(s', argmax_{a'} Q(s',a'; θ); θ⁻)
```

**Dueling DQN:** Separate streams for V(s) and A(s,a):
```
Q(s,a; θ) = V(s; θ) + A(s,a; θ) - (1/|A|) Σ A(s,a'; θ)
```

---

### 12.3 Policy Gradient Methods

Instead of learning Q-values, directly optimize the policy π_θ:

```
J(θ) = E_π[G_t]

Policy Gradient Theorem:
∇_θ J(θ) = E_π[∇_θ log π_θ(a|s) · Q^π(s,a)]
```

**REINFORCE:**
```
θ ← θ + α Σ_t ∇_θ log π_θ(aₜ|sₜ) · Gₜ
```

**Baseline subtraction (variance reduction):**
```
∇_θ J = E[∇_θ log π_θ(a|s) · (Q(s,a) - V(s))]
              ──────────────────────────────────
                    advantage A(s,a)
```

---

### 12.4 Actor-Critic Methods

Maintain two networks:
- **Actor:** π_θ(a|s) — the policy
- **Critic:** V_w(s) — estimates state value

```
Advantage: A(s,a) = r + γV(s') - V(s)   [TD error as advantage]
Actor update:  θ ← θ + α ∇_θ log π_θ(a|s) · A(s,a)
Critic update: w ← w - α_c ∇_w (A(s,a))²
```

#### Proximal Policy Optimization (PPO)

The dominant RL algorithm for LLM fine-tuning and robotics:

```
L_clip(θ) = E[min(rₜ(θ) Aₜ, clip(rₜ(θ), 1-ε, 1+ε) Aₜ)]

rₜ(θ) = π_θ(aₜ|sₜ) / π_θ_old(aₜ|sₜ)   [probability ratio]
```

The clip prevents too-large policy updates — stable, efficient.

**PPO in RLHF (InstructGPT, ChatGPT):**
```
Reward = RM(x, y) - β·KL[π_θ(y|x) || π_ref(y|x)]
```

---

### 12.5 Model-Based RL & World Models

**Model-free:** Learn directly from interaction — sample inefficient.
**Model-based:** Learn environment model P(s'|s,a), plan with it.

**World Models approach:**
1. Learn compact latent representation of observations
2. Learn transition model in latent space
3. Plan using imagined trajectories

**DreamerV3:** State-of-the-art world model — learns from raw pixels, works across domains (Atari, continuous control, Minecraft).

---

## 13. Large Language Models (LLMs)

### 13.1 Scaling Laws

**Chinchilla scaling laws (Hoffmann et al., 2022):**

For optimal performance given compute budget C:
```
N_opt ∝ C^0.5    [model parameters]
D_opt ∝ C^0.5    [training tokens]

Rule of thumb: ~20 tokens per parameter (equal scaling of N and D)
```

**Key insight:** GPT-3 (175B params, 300B tokens) was undertrained. Chinchilla (70B, 1.4T tokens) performs comparably.

**LLaMA-2/3:** Applied Chinchilla to produce 7B, 13B, 70B models trained on 2T tokens.

**Power law:** L ∝ N^{-0.076} (loss decreases smoothly with model size — no clear plateau observed).

---

### 13.2 LLM Architecture Deep Dive (Modern)

**Modern LLM stack (LLaMA-3 style):**

```
Embedding Layer
    ↓
[Transformer Block] × N:
    ├─ RMSNorm (pre-norm)
    ├─ Grouped Query Attention (GQA)
    │      ├─ Rotary Position Embeddings (RoPE)
    │      └─ KV Cache for inference
    ├─ Residual connection
    ├─ RMSNorm
    ├─ SwiGLU FFN (d_model → 8/3 × d_model → d_model)
    └─ Residual connection
    ↓
RMSNorm
    ↓
LM Head (tied with embedding weights)
```

**Grouped Query Attention (GQA):**
```
Multi-Head: Q heads = K heads = V heads = h
Multi-Query (MQA): Q heads = h, K/V heads = 1
Grouped Query (GQA): Q heads = h, K/V heads = g (1 < g < h)
```
GQA reduces KV cache memory by h/g× — critical for inference efficiency.

**SwiGLU FFN:**
```
SwiGLU(x, W, V, W₂) = (Swish(xW) ⊙ xV) W₂
```
Gate mechanism — competitive with standard FFN at same compute.

---

### 13.3 Efficient Fine-Tuning

#### LoRA (Low-Rank Adaptation)

Instead of updating W ∈ ℝ^{d×k}, add low-rank decomposition:
```
W' = W + ΔW = W + BA

where B ∈ ℝ^{d×r}, A ∈ ℝ^{r×k}, rank r << min(d,k)
```

**Parameter reduction:** Original: d×k params. LoRA: r×(d+k) params.
For d=k=4096, r=16: 16M → 131K params (120× reduction!)

```python
from peft import LoraConfig, get_peft_model

lora_config = LoraConfig(
    r=16,                    # rank
    lora_alpha=32,           # scale factor (LoRA_alpha / r)
    target_modules=["q_proj", "v_proj"],   # which layers
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)

model = get_peft_model(base_model, lora_config)
model.print_trainable_parameters()
# trainable params: 4,194,304 || all params: 7,243,436,288 || trainable%: 0.058
```

**Variants:**
- **QLoRA:** Quantize base model to 4-bit NF4, LoRA in fp16 — fine-tune 65B on single GPU
- **AdaLoRA:** Adaptive rank allocation (more rank to important layers)
- **LoRA+:** Different learning rates for A and B matrices
- **DoRA:** Decompose weights into magnitude + direction, apply LoRA to direction

---

### 13.4 Quantization

Reduce numerical precision to save memory and accelerate inference.

**Methods:**
```
FP32 (32-bit float) → FP16/BF16 (16-bit) → INT8 → INT4 → INT2

FP32:   ████████ 4 bytes per parameter
BF16:   ████ 2 bytes (same exponent as FP32, less mantissa)
INT8:   ██ 1 byte
INT4:   █ 0.5 bytes  ← QLoRA, GPTQ, AWQ
```

**Post-Training Quantization (PTQ):**
- **GPTQ:** Quantize each layer by minimizing weight reconstruction error
- **AWQ (Activation-Aware Weight Quantization):** Identify and protect salient weights
- **GGUF (llama.cpp format):** Mixed-precision quantization, CPU-friendly

**Effects:** 4-bit quantization reduces quality ~2-5 MMLU points vs FP16.

---

### 13.5 Inference Optimization

**KV Cache:**
Store key-value computations for previously generated tokens — avoids recomputation:
```
Prefill: Process prompt in parallel → store K, V for all prompt tokens
Decode: Append new K, V for each generated token, retrieve all cached K,V
```

**Speculative Decoding:**
Use small draft model to generate k tokens, verify with large model in parallel:
- Draft model: 7B generates 5 token candidates
- Target model: 70B verifies all 5 in one forward pass
- Accept 3 (if distribution matches), regenerate from token 4
- ~2-3× speedup with no quality loss

**Continuous Batching:**
Traditional batching waits for all requests to finish. Continuous batching iterates at token level — much better GPU utilization.

**PagedAttention (vLLM):**
Manages KV cache like OS virtual memory:
- No fragmentation, no over-allocation
- Memory sharing across parallel sequences
- ~3× higher throughput than Hugging Face naive serving

---

## 14. Prompt Engineering

### 14.1 Core Principles

1. **Be specific and explicit** — ambiguity leads to misinterpretation
2. **Provide context** — role, audience, constraints
3. **Use positive instructions** — "write concisely" > "don't write too long"
4. **Iterate** — prompting is empirical

### 14.2 Key Techniques

#### Zero-Shot
```
"Classify the sentiment of this review as Positive, Negative, or Neutral:
'The product arrived on time but the quality was disappointing.'
Answer:"
```

#### Few-Shot (In-Context Learning)
```
"Classify sentiment:
Review: 'Amazing product, exactly as described!' → Positive
Review: 'Broke after one day, terrible quality.' → Negative
Review: 'It works, nothing special.' → Neutral
Review: 'I expected better for this price.' →"
```

**Why it works:** Transformers infer the task from examples via in-context learning — no weight updates!

#### Chain-of-Thought (CoT)
```
"Q: If a train travels at 60mph for 2.5 hours, then at 80mph for 1.5 hours, 
what is the total distance?

A: Let me solve step by step.
Step 1: Distance in first segment = 60 × 2.5 = 150 miles
Step 2: Distance in second segment = 80 × 1.5 = 120 miles
Step 3: Total = 150 + 120 = 270 miles"
```

Add "Let's think step by step" or "Think through this carefully" to elicit reasoning.

#### Zero-Shot CoT
"Solve this problem. Think step by step before giving the final answer."

#### Self-Consistency
Generate multiple CoT answers → take majority vote. Improves reasoning accuracy significantly.

#### Least-to-Most Prompting
Break complex problems into subproblems:
1. "What subproblems need to be solved to answer X?"
2. Solve each subproblem
3. Combine solutions

#### ReAct (Reasoning + Acting)
Interleave thought/action/observation:
```
Thought: I need to find the current population of Tokyo
Action: search("Tokyo current population")
Observation: Tokyo's population is approximately 13.96 million (2023)
Thought: Now I can answer the question
Answer: Tokyo has approximately 14 million people
```

#### Role Prompting
```
"You are a senior software engineer with 15 years of experience in distributed systems. 
Review the following code for correctness, efficiency, and scalability..."
```

#### Structured Output Prompting
```
"Extract the following fields from this job posting and respond ONLY with valid JSON:
{
  "job_title": string,
  "company": string,
  "required_skills": [string],
  "salary_range": string | null,
  "location": string
}

Job posting: [...]"
```

### 14.3 System Prompts & Meta-Prompting

```python
from anthropic import Anthropic

client = Anthropic()
response = client.messages.create(
    model="claude-opus-4-6-20250514",
    max_tokens=1024,
    system="""You are an expert ML engineer. 
    Always:
    - Provide working Python code examples
    - Explain the intuition before the math
    - Note potential pitfalls
    - Suggest when NOT to use each approach""",
    messages=[
        {"role": "user", "content": "Explain gradient descent variants"}
    ]
)
```

---

## 15. RAG Pipelines — All Types

### 15.1 What is RAG?

**Retrieval-Augmented Generation (RAG)** addresses two key LLM limitations:
1. **Hallucination:** LLMs confidently generate false information
2. **Knowledge cutoff:** LLMs can't access real-time or private data

**Basic RAG formula:**
```
P(answer | question, context) where context = retrieve(question, knowledge_base)
```

---

### 15.2 Naive RAG (Standard RAG)

The original, simplest form:

```
[Question]
    ↓
[Query Encoder] → embedding
    ↓
[Vector Database] → retrieve top-k chunks
    ↓
[Prompt Constructor] → "Context: {chunks}\n\nQuestion: {q}\n\nAnswer:"
    ↓
[LLM] → Answer
```

**Implementation:**
```python
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI

# 1. Load and split documents
splitter = RecursiveCharacterTextSplitter(chunk_size=512, chunk_overlap=50)
chunks = splitter.split_documents(docs)

# 2. Build vector store
embeddings = OpenAIEmbeddings()
vectordb = Chroma.from_documents(chunks, embeddings)

# 3. Create retriever
retriever = vectordb.as_retriever(search_kwargs={"k": 5})

# 4. Chain
qa = RetrievalQA.from_chain_type(
    llm=OpenAI(temperature=0),
    retriever=retriever,
    chain_type="stuff"  # stuff all chunks into context
)
answer = qa.run(question)
```

**Limitations:**
- One-shot retrieval — if it misses, response fails
- Coarse chunking loses context
- No query reformulation
- No re-ranking

---

### 15.3 Advanced RAG

Addresses limitations through pre-retrieval, retrieval, and post-retrieval improvements.

#### Pre-Retrieval Improvements

**Query Rewriting:**
```python
rewrite_prompt = """Rewrite this question for better document retrieval.
Original: {question}
Rewritten:"""
```

**Query Decomposition:**
```
"What are the differences between BERT and GPT?"
→ Sub-queries:
   1. "What is BERT?"
   2. "What is GPT?"
   3. "BERT vs GPT architecture differences"
   4. "BERT vs GPT use cases"
```

**HyDE (Hypothetical Document Embeddings):**
```python
# Generate a hypothetical answer → embed it → use for retrieval
hyde_prompt = f"Write a paragraph that answers: {question}"
hypothetical_doc = llm(hyde_prompt)
query_embedding = embed(hypothetical_doc)   # Better alignment with doc space
```

**Step-Back Prompting:**
```
Original: "What are the effects of X on Y in condition Z?"
Step-back: "What are the general principles governing X?"
```

#### Retrieval Improvements

**Hybrid Search (Dense + Sparse):**
```python
# BM25 (sparse, keyword-based) + Dense embeddings
from langchain.retrievers import BM25Retriever, EnsembleRetriever

bm25 = BM25Retriever.from_documents(docs, k=5)
dense = vectordb.as_retriever(search_kwargs={"k": 5})

hybrid = EnsembleRetriever(
    retrievers=[bm25, dense],
    weights=[0.3, 0.7]   # Weight toward dense
)
```

**Multi-vector retrieval:**
- Store document at multiple granularities (summary + chunks)
- Retrieve by summary, return full doc

**Parent-child chunking:**
```
Parent chunks (512 tokens): stored for retrieval context
Child chunks (128 tokens): used for embedding/search

Retrieve by child match → return parent context
```

#### Post-Retrieval Improvements

**Re-ranking:**
```python
from sentence_transformers import CrossEncoder

reranker = CrossEncoder('cross-encoder/ms-marco-MiniLM-L-6-v2')
scores = reranker.predict([(query, chunk) for chunk in retrieved_chunks])
ranked = sorted(zip(chunks, scores), key=lambda x: -x[1])
top_k = [c for c, s in ranked[:3]]
```

Cross-encoders score (query, document) jointly — much more accurate than cosine similarity alone.

**Contextual Compression:**
```python
from langchain.retrievers.document_compressors import LLMChainExtractor

compressor = LLMChainExtractor.from_llm(llm)
# Only keep relevant sentences from each chunk
```

**Lost in the Middle:** LLMs attend poorly to middle of context. Put most important chunks at beginning or end.

---

### 15.4 Modular RAG

Decomposes the pipeline into independent, reusable modules:

```
┌──────────────────────────────────────────┐
│           MODULAR RAG PIPELINE           │
├──────────┬──────────────┬────────────────┤
│  MODULE  │   FUNCTION   │   EXAMPLES     │
├──────────┼──────────────┼────────────────┤
│ Query    │ Process query│ Decompose,     │
│ Planning │              │ Rewrite, Expand│
├──────────┼──────────────┼────────────────┤
│ Routing  │ Select route │ DB routing,    │
│          │              │ Expert routing │
├──────────┼──────────────┼────────────────┤
│Retrieval │ Find chunks  │ Dense, Sparse, │
│          │              │ Hybrid, Web    │
├──────────┼──────────────┼────────────────┤
│Re-rank   │ Score chunks │ Cross-encoder, │
│          │              │ Cohere Rerank  │
├──────────┼──────────────┼────────────────┤
│Filter    │ Remove noise │ Relevance score│
│          │              │ threshold      │
├──────────┼──────────────┼────────────────┤
│Generate  │ Produce ans  │ Stuff, MapRed, │
│          │              │ Refine, Iter.  │
├──────────┼──────────────┼────────────────┤
│Verify    │ Check output │ Factuality,    │
│          │              │ Citation check │
└──────────┴──────────────┴────────────────┘
```

**Chain-of-thought context synthesis methods:**

- **Stuff:** Put all chunks in context at once (simple, limited by window)
- **MapReduce:** Summarize each chunk separately → combine summaries
- **Refine:** Process chunk 1 → refine answer with chunk 2 → refine with chunk 3 → ...
- **Map-Rerank:** Score and select best synthesis from per-chunk answers

---

### 15.5 Corrective RAG (CRAG)

Adds a **relevance grader** to evaluate retrieved documents:

```
Query → Retrieve → Grade relevance of each doc
                           ↓
                   Relevant? → Generate answer
                   Ambiguous? → Refine + re-retrieve
                   Irrelevant? → Web search for fresh docs
                                       ↓
                              Knowledge refinement
                                       ↓
                              Generate corrected answer
```

```python
from langchain.prompts import ChatPromptTemplate

grade_prompt = ChatPromptTemplate.from_template("""
You are a grader assessing relevance of a retrieved document to a user question.
Document: {document}
Question: {question}
Answer with a JSON: {{"score": "yes" | "no"}}
""")

def grade_document(question, doc):
    result = grader_llm.invoke(grade_prompt.format(document=doc, question=question))
    return result["score"] == "yes"
```

---

### 15.6 Self-RAG

Trains the LLM to make retrieval decisions and critique its own output using **special reflection tokens**:

```
Tokens: [Retrieve], [ISREL], [ISSUP], [ISUSE]

Generation flow:
1. Model generates → if [Retrieve] token emitted → retrieve
2. Retrieved docs → model rates relevance [ISREL: relevant/irrelevant]
3. Model generates with context → rates support [ISSUP: fully/partial/no]
4. Model rates overall utility [ISUSE: 1-5]
5. Best-scoring segment selected
```

Key innovation: The model learns WHEN to retrieve (not all queries need retrieval), not just HOW.

---

### 15.7 GraphRAG

Uses knowledge graphs to capture relationships between entities:

```
Text corpus
    ↓
[Entity Extraction] (LLM)
    ↓
[Relation Extraction] (LLM)  
    ↓
[Knowledge Graph] → (node: entity, edge: relation, property: description)
    ↓
[Community Detection] → hierarchical summaries of entity clusters
    ↓
For queries:
  Local search: → entity-centric → traverse graph
  Global search: → map-reduce over community summaries
```

**Microsoft GraphRAG (2024):**
```
Two query modes:
1. Local: "What specific things does Alice do?" → entity subgraph
2. Global: "What are the main themes in the dataset?" → community summaries
```

**When to use GraphRAG vs standard RAG:**

| Scenario | RAG | GraphRAG |
|---|---|---|
| Factual lookups | ✅ | ✅ |
| Multi-hop reasoning | ❌ | ✅ |
| Relationship queries | ❌ | ✅ |
| Global summaries | ❌ | ✅ |
| Single document QA | ✅ | Overkill |
| Latency sensitive | ✅ | ❌ |

---

### 15.8 Agentic RAG (2025–2026 Standard)

The current dominant paradigm. The LLM acts as an **agent** that decides when, what, and how to retrieve:

```python
# LangGraph-based Agentic RAG
from langgraph.graph import Graph, END

# State: {question, context, drafts, iterations, answer}

def plan(state):
    """Decompose query into sub-questions"""
    ...

def retrieve(state):
    """Retrieve for current sub-question"""
    ...

def generate(state):
    """Generate answer draft"""
    ...

def grade(state):
    """Grade answer quality"""
    # Check: grounded? complete? accurate?
    ...

def decide(state):
    """Route: finalize, retrieve_more, web_search, or clarify"""
    if state["grade"] == "pass": return "finalize"
    elif state["iterations"] > 3: return "finalize"  # prevent loops
    else: return "retrieve"

graph = Graph()
graph.add_node("plan", plan)
graph.add_node("retrieve", retrieve)
graph.add_node("generate", generate)
graph.add_node("grade", grade)
graph.add_conditional_edges("grade", decide, {
    "finalize": END,
    "retrieve": "retrieve"
})
```

**Key capabilities of Agentic RAG:**
- **Multi-hop retrieval:** Chain multiple retrievals for complex reasoning
- **Self-correction:** Re-retrieve when output is insufficient
- **Tool use:** Web search, SQL, API calls as retrieval sources
- **Planning:** Decompose queries before retrieving
- **Memory:** Maintain conversation context across turns

---

### 15.9 Multimodal RAG

Extends RAG to images, tables, code, audio:

```
Document types → specialized extraction:
  PDF images → vision model → image summaries → embed
  Tables → extract as text + schema → embed
  Charts → vision model → data extraction → embed
  Code → AST-based chunking → embed with code-specific model

Multi-modal retrieval:
  Text query → retrieve text + images (via CLIP embeddings)
  Image query → CLIP embed → retrieve similar images + related text
```

**ColPali (2024):** Embeds PDF pages as images — no text extraction needed. Retrieves visual pages directly.

---

### 15.10 RAG Evaluation Metrics

**RAGAS Framework:**
```
Faithfulness:      Is the answer grounded in the retrieved context?
Answer Relevance:  Does the answer address the question?
Context Precision: What fraction of retrieved context was actually useful?
Context Recall:    How much of the ground truth was covered by retrieved context?
```

```python
from ragas import evaluate
from ragas.metrics import faithfulness, answer_relevancy, context_precision

result = evaluate(
    dataset=test_ds,
    metrics=[faithfulness, answer_relevancy, context_precision]
)
```

**TruLens / DeepEval:** Alternative evaluation frameworks with LLM-as-judge.

---

## 16. AI Agents & Agentic Systems

### 16.1 What is an AI Agent?

An AI agent is an LLM-powered system that can:
1. **Perceive** the environment (tools, memory, other agents)
2. **Plan** multi-step actions to achieve a goal
3. **Act** by calling tools or generating text
4. **Reflect** on results and adapt

```
[Environment: Documents, APIs, Code, Web, DBs]
         ↕ (observations)
  [Agent: LLM + Memory + Tool Use]
         ↓ (actions)
[Tools: Search, Python, SQL, Browser, Email...]
```

### 16.2 ReAct Framework

```
Thought: I need to find the current stock price of AAPL
Action: search_web("AAPL stock price today")
Observation: AAPL is trading at $189.43 (+0.8%)
Thought: Now I have the price. Should I check the P/E ratio too?
Action: financial_data("AAPL P/E ratio")
Observation: AAPL P/E ratio is 28.4
Thought: I have enough information to answer the question
Answer: Apple (AAPL) is trading at $189.43 with a P/E ratio of 28.4
```

### 16.3 Tool Use / Function Calling

```python
import anthropic

client = anthropic.Anthropic()

tools = [
    {
        "name": "get_weather",
        "description": "Get current weather for a city",
        "input_schema": {
            "type": "object",
            "properties": {
                "city": {"type": "string"},
                "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
            },
            "required": ["city"]
        }
    }
]

response = client.messages.create(
    model="claude-opus-4-6-20250514",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "What's the weather in Tokyo?"}]
)

# Check if model wants to call a tool
if response.stop_reason == "tool_use":
    tool_call = response.content[-1]
    # Execute: result = get_weather(**tool_call.input)
    # Send back result...
```

### 16.4 Memory Systems in Agents

**4 types of memory:**

```
1. SENSORY (In-context window):
   Current conversation, retrieved chunks, tool outputs
   
2. SHORT-TERM (Working memory):
   Recent conversation history (last k turns)
   Scraatchpad for intermediate reasoning
   
3. LONG-TERM (Persistent):
   a. Semantic: Facts about the world/user (vector DB)
   b. Episodic: Records of past interactions (vector DB + timestamps)
   c. Procedural: How to do tasks (fine-tuned weights or retrieved instructions)

4. EXTERNAL (Knowledge stores):
   Vector databases, SQL, documents, APIs
```

### 16.5 Multi-Agent Systems

```
Orchestrator Agent
      ├── Researcher Agent (web search, document retrieval)
      ├── Analyst Agent (data processing, calculations)
      ├── Writer Agent (drafts, formats output)
      └── Critic Agent (reviews, fact-checks)

Communication patterns:
  - Supervisor: One orchestrator directs all
  - Peer-to-peer: Agents communicate directly
  - Blackboard: Shared working memory
  - Pipeline: Each agent processes output of previous
```

**AutoGen, CrewAI, LangGraph** are the main frameworks for multi-agent orchestration.

---

## 17. Vector Databases & Embeddings

### 17.1 Embeddings Deep Dive

**Text embedding models:**

| Model | Dim | Context | Best For |
|---|---|---|---|
| text-embedding-3-small (OpenAI) | 1536 | 8191 | General purpose, cheap |
| text-embedding-3-large (OpenAI) | 3072 | 8191 | Best quality |
| embed-english-v3 (Cohere) | 1024 | 512 | English, retrieval |
| all-MiniLM-L6-v2 (SBERT) | 384 | 256 | Fast, local |
| bge-large-en-v1.5 | 1024 | 512 | MTEB top performer |
| nomic-embed-text-v1.5 | 768 | 8192 | Long context, open |
| jina-embeddings-v3 | 1024 | 8192 | Multilingual |

**Matryoshka Representation Learning (MRL):**
Embeddings that work at multiple dimensionalities. text-embedding-3 supports 256/512/1536/3072 — truncate without reembedding!

### 17.2 Approximate Nearest Neighbor (ANN) Search

Exact NN search is O(n·d). ANN trades accuracy for speed:

#### HNSW (Hierarchical Navigable Small World)
```
Hierarchical graph where:
- Level 0: All nodes (densest graph)
- Level k: Exponentially fewer nodes (longer connections)

Search: Start at level max → greedy search → descend to level 0
Insert: Assign max_layer ∝ -ln(uniform(0,1)) → insert with bidirectional links
```
Parameters: `M` (connections per node), `ef_construction` (search width during build), `ef_search`

**Best for:** High recall, fast updates (dynamic insertion).

#### IVF-PQ (Inverted File + Product Quantization)
```
IVF: Cluster vectors into k centroids (Voronoi cells)
Search: Only compare query to vectors in nearest centroids (nprobe cells)

PQ: Compress each vector by splitting into m subvectors → quantize each
Compression: 128-dim FP32 (512B) → PQ8 (8B) = 64× compression
```
**Best for:** Billion-scale datasets, memory-constrained environments.

#### ScaNN (Google)
Combination of tree-based partitioning + scoring compression — state-of-the-art ANN at scale.

### 17.3 Vector Database Comparison

| DB | Best For | Features | Self-host? |
|---|---|---|---|
| **Chroma** | Local dev, prototyping | Simple API, embedded | ✅ Open source |
| **FAISS** | Research, in-memory | Fastest raw ANN, no server | ✅ Library only |
| **Pinecone** | Production serverless | Managed, auto-scale | ❌ Cloud only |
| **Weaviate** | Multi-modal + metadata | GraphQL, BM25 hybrid, Docker | ✅ |
| **Qdrant** | Production self-hosted | Fast, Rust-based, rich filtering | ✅ |
| **Milvus** | Billion-scale | Best scalability, Kubernetes | ✅ |
| **pgvector** | PostgreSQL extension | Postgres SQL + vectors | ✅ |
| **Redis** | Cache + vectors | Low-latency, familiar | ✅ |

---

## 18. MLOps & Production ML

### 18.1 The ML Systems Stack

```
┌────────────────────────────────────────────┐
│              SERVING LAYER                 │
│   REST/gRPC APIs, Model Servers, Caches   │
├────────────────────────────────────────────┤
│              MODEL REGISTRY               │
│   MLflow, W&B, Hugging Face Hub           │
├────────────────────────────────────────────┤
│            EXPERIMENT TRACKING            │
│   MLflow, Weights & Biases, ClearML       │
├────────────────────────────────────────────┤
│            TRAINING PIPELINE              │
│   Data loading, Training loop, Logging    │
├────────────────────────────────────────────┤
│            FEATURE STORE                  │
│   Feast, Tecton, Hopsworks                │
├────────────────────────────────────────────┤
│              DATA PIPELINE                │
│   ETL, Validation, Versioning            │
├────────────────────────────────────────────┤
│               DATA LAKE                   │
│   S3, GCS, Snowflake, Delta Lake          │
└────────────────────────────────────────────┘
```

### 18.2 Experiment Tracking with MLflow

```python
import mlflow
import mlflow.sklearn

with mlflow.start_run():
    # Log params
    mlflow.log_param("learning_rate", 0.01)
    mlflow.log_param("n_estimators", 100)
    
    # Train model
    model.fit(X_train, y_train)
    
    # Log metrics
    mlflow.log_metric("accuracy", acc)
    mlflow.log_metric("f1", f1)
    
    # Log model
    mlflow.sklearn.log_model(model, "model")
    
    # Log artifacts
    mlflow.log_artifact("feature_importance.png")
```

### 18.3 Model Serving

**BentoML (framework-agnostic serving):**
```python
import bentoml

@bentoml.service(
    resources={"cpu": "2", "memory": "2Gi"},
    traffic={"timeout": 60}
)
class SentimentService:
    model = bentoml.models.get("sentiment_model:latest")
    
    @bentoml.api
    def classify(self, text: str) -> dict:
        result = self.model.predict([text])
        return {"label": result[0], "confidence": result[1]}
```

**TorchServe, Triton Inference Server** for high-throughput deep learning.

### 18.4 Data & Model Monitoring

**Data drift detection:**
```python
from evidently.report import Report
from evidently.metrics import DataDriftPreset, DataQualityPreset

report = Report(metrics=[DataDriftPreset(), DataQualityPreset()])
report.run(reference_data=train_df, current_data=production_df)
```

**Model performance monitoring:**
- Shadow mode: Run new model alongside old, compare outputs
- Canary deployment: Route 5% of traffic to new model
- A/B testing: Random assignment + statistical testing
- Champion-challenger: Keep best performing model live

### 18.5 CI/CD for ML (GitOps)

```
Code change → Unit tests → Integration tests
                                ↓
                   Data validation (Great Expectations)
                                ↓
                      Model training (triggered)
                                ↓
                    Model evaluation vs baseline
                                ↓
                    Model registry update (if better)
                                ↓
                    Deployment (canary → full rollout)
                                ↓
                    Monitoring dashboard update
```

Tools: **DVC** (data versioning), **Prefect/Airflow** (workflow), **GitHub Actions** (CI/CD triggers).

---

## 19. Evaluation Metrics — Complete Reference

### 19.1 Classification Metrics

**Confusion Matrix:**
```
                 Predicted
                 Pos    Neg
Actual  Pos  │TP (True Pos)│FN (False Neg)│
        Neg  │FP (False Pos)│TN (True Neg)│
```

**Derived metrics:**
```
Accuracy    = (TP + TN) / (TP + TN + FP + FN)
Precision   = TP / (TP + FP)    [of positives predicted, how many are right]
Recall      = TP / (TP + FN)    [of actual positives, how many caught]
Specificity = TN / (TN + FP)    [recall for negatives]
F1          = 2 × (Prec × Rec) / (Prec + Rec)
F-beta      = (1+β²) × (Prec × Rec) / (β²×Prec + Rec)
```

**ROC Curve:** Plot TPR (recall) vs FPR (1-specificity) at all thresholds.
**AUC-ROC:** Area under ROC curve. 0.5 = random, 1.0 = perfect. Threshold-independent.

**Precision-Recall Curve:** Better for imbalanced datasets (ROC is optimistic when negatives dominate).

**Average Precision (AP):** Area under PR curve. Use for detection tasks.
**mAP:** Mean AP across classes or IoU thresholds (object detection standard).

---

### 19.2 Regression Metrics

```
MAE  = (1/n) Σ|yᵢ - ŷᵢ|                    [interpretable, robust to outliers]
MSE  = (1/n) Σ(yᵢ - ŷᵢ)²                   [penalizes outliers, differentiable]
RMSE = √MSE                                  [same units as target]
MAPE = (1/n) Σ|yᵢ-ŷᵢ|/yᵢ × 100%           [percentage error, avoid if yᵢ ≈ 0]
R²   = 1 - SS_res/SS_tot                     [fraction of variance explained]
```

---

### 19.3 NLP / LLM Evaluation Metrics

**BLEU (Bilingual Evaluation Understudy):**
```
BLEU = BP × exp(Σ wₙ log pₙ)
pₙ = n-gram precision
BP = brevity penalty (penalize short outputs)
```
Used for machine translation. Range [0,1]. Higher is better.

**ROUGE (Recall-Oriented Understudy for Gisting Evaluation):**
```
ROUGE-N: n-gram recall overlap with reference
ROUGE-L: Longest Common Subsequence F-score
```
Used for summarization.

**METEOR:** Better than BLEU — includes synonym matching, paraphrase handling.

**BERTScore:**
```
Compute cosine similarity between contextual BERT embeddings
of reference and generated text.
Precision_bert, Recall_bert, F1_bert
```
Correlates better with human judgment than n-gram metrics.

**Perplexity:**
```
PPL = exp(-1/N Σ log P_model(xₜ|x<t))
```
Measures how well a language model predicts a corpus. Lower = better. (Not comparable across different tokenizers!)

**LLM Benchmarks:**

| Benchmark | What it Tests | Score (Claude Opus 4.8, 2026) |
|---|---|---|
| MMLU | General knowledge, 57 subjects | ~90%+ |
| GPQA Diamond | Graduate-level science | 94.6% |
| HumanEval | Python coding | ~95% |
| SWE-Bench | Real GitHub issues | ~70%+ |
| MATH-500 | Competition math | ~95%+ |
| AIME 2025 | Hard competition math | Top tier |
| LMSYS Arena | Human preference | ELO-based |

---

### 19.4 Retrieval Metrics

```
Hit Rate: fraction of queries where correct doc is in top-k
MRR@k (Mean Reciprocal Rank): (1/|Q|) Σ 1/rank_of_first_relevant
NDCG@k: considers rank position + relevance grade
MAP (Mean Average Precision): averages precision at each recall level
```

---

## 20. AI Safety, Alignment & Ethics

### 20.1 The Alignment Problem

Training powerful AI systems that reliably pursue goals humans actually want.

**Key challenges:**
1. **Specification problem:** Reward functions are hard to specify fully (e.g., Goodhart's Law: "when a measure becomes a target, it ceases to be a good measure")
2. **Generalization problem:** Behavior in training ≠ behavior in deployment
3. **Scalable oversight:** Humans can't evaluate superhuman AI outputs
4. **Goal stability:** Advanced AI might modify its own objectives

---

### 20.2 Constitutional AI (Anthropic)

A technique for self-supervised alignment:
1. Initial helpful response (possibly harmful)
2. **Critique:** Model critiques its response against a constitutional principle
3. **Revision:** Model revises based on critique
4. Final response is much better aligned

Reduces reliance on human feedback labels for preference data.

---

### 20.3 Types of AI Safety Risks

**Near-term risks:**
- Bias and fairness (training data bias → discriminatory outputs)
- Hallucination and factual errors
- Prompt injection attacks
- Data poisoning
- Model stealing / extraction

**Long-term risks:**
- Deceptive alignment (model performs well in training but not deployment)
- Goal misgeneralization
- Treacherous turn (advanced AI changing behavior when not monitored)
- Emergent capabilities (unexpected abilities at scale)

### 20.4 Fairness in ML

**Group fairness metrics:**

```
Demographic Parity: P(ŷ=1 | A=0) = P(ŷ=1 | A=1)     [equal positive rates]
Equal Opportunity:  P(ŷ=1 | Y=1, A=0) = P(ŷ=1 | Y=1, A=1)  [equal TPR]
Equalized Odds:     Equal TPR AND FPR across groups
Individual Fairness: Similar individuals treated similarly
```

**Impossibility theorem:** Calibration, equal FPR, and equal FNR cannot all hold simultaneously when base rates differ across groups (Chouldechova, 2017).

---

## 21. Latest AI Models & News (2025–2026)

### 21.1 The Spring 2026 LLM Sprint

The AI model landscape underwent a major shift in early 2026. Multiple frontier labs shipped new flagships within weeks of each other:

**OpenAI GPT-5.5 (April 2026):**
- Unified intelligent routing — automatically adjusts reasoning depth
- 128K output token window (largest among hosted models)
- Top performance on coding and reasoning benchmarks

**Anthropic Claude Opus 4.8 (May 2026):**
- #1 on the Artificial Analysis Intelligence Index (61.4 score)
- Leads GPQA Diamond at 94.6% — best reasoning benchmark performance
- Best coding model for sustained complex software engineering
- Excels in agentic workflows when paired with Claude Code

**Google Gemini 3.1 Pro (February 2026):**
- Focus on video, images, spatial reasoning
- Deep integration into Google Workspace and Cloud
- ~57 on the AI Intelligence Index

**DeepSeek V4 Pro:**
- MIT-licensed open-source
- 75% API price reduction vs previous generation
- Competitive with frontier models on coding/reasoning at dramatically lower cost

**Alibaba Qwen 3.7 Max:**
- Top 10 on GPQA Diamond while being among cheapest
- Strong multilingual performance

### 21.2 Context Window Explosion

All major models now support massive context:

| Model | Context Window |
|---|---|
| Llama 4 Scout | **10M tokens** |
| GPT-5.5 | 1M tokens |
| Claude Opus 4.8 | 1M tokens |
| Gemini 3.5 Flash | 1M tokens |
| Grok 4 Fast | 2M tokens |
| DeepSeek V4 | 1M tokens |

Context windows are no longer a differentiator — the competition has shifted to reasoning quality, agentic capabilities, and cost.

### 21.3 Benchmarks That Matter in 2026

Modern evaluations have moved beyond saturated academic multiple-choice:
- **SWE-Bench:** Fix real GitHub issues (Claude Opus 4.8 ~70%+)
- **GPQA Diamond:** Graduate-level science (Claude Mythos Preview 94.6%)
- **Terminal-Bench:** Real terminal tasks
- **Agentic task completion:** Multi-step real-world task success rates
- **LMSYS Chatbot Arena:** Human preference ELO rankings

### 21.4 Key Architectural Trends (2025–2026)

**Mixture of Experts (MoE):**
Use only a subset of parameters for each forward pass:
```
For each token: Router selects top-k experts
Active params ≪ Total params → same quality, less compute

Mixtral 8×7B: 47B total, 13B active
Grok-1: 314B total, ~85B active
GPT-4: Rumored ~1.8T total MoE
```

**Multimodal as Default:**
- Text + Image: Now universal (GPT-4V, Claude, Gemini, LLaMA 4)
- Video understanding: Gemini 1.5+, GPT-4o
- Audio native: GPT-4o voice mode
- Real-time: Latency <500ms for voice

**Test-Time Compute (o1, DeepSeek-R1 style):**
Allocate more computation at inference (thinking/reasoning) rather than only at training:
```
Chain-of-thought tokens before answer
Process Reward Models (PRMs) guide search
Monte Carlo Tree Search or beam search over reasoning paths
```

**Speculative reasoning:** o3, DeepSeek-R1, Gemini 2.0 Flash Thinking demonstrate that spending more compute thinking before answering dramatically improves hard reasoning.

### 21.5 Open vs Closed Models

**Open-weight leaders (2025-2026):**
- **LLaMA 4 (Meta):** Scout (10M context), Maverick (competitive flagship)
- **Qwen 3.7 Max (Alibaba):** Top benchmarks, open-source
- **DeepSeek V4 (DeepSeek):** MIT license, near-frontier quality
- **Mistral Large 3:** Strong coding and multilingual
- **Phi-4 (Microsoft):** Efficient small models (3.8B-14B)

**The cost gap is closing:** Open models at 70B-405B scale now match or exceed what closed models offered in 2023.

---

## 22. Hands-On Cheat Sheets

### 22.1 When to Use Which Model Type

```
┌─────────────────────────────────────────────────────────────┐
│                    TASK → MODEL SELECTION                   │
├─────────────────────────┬───────────────────────────────────┤
│ Text classification     │ BERT/DeBERTa + fine-tune          │
│ Sentiment analysis      │ Same or few-shot LLM              │
│ Text generation         │ GPT/LLaMA decoder-only            │
│ Translation             │ mBART, NLLB, or GPT               │
│ Summarization           │ T5, BART, or GPT                  │
│ Q&A (closed domain)     │ RAG + GPT/Claude                  │
│ Code generation         │ Claude/GPT-5.5/Codestral          │
│ Image classification    │ EfficientNet, ViT                 │
│ Object detection        │ YOLO, Faster R-CNN                │
│ Image segmentation      │ SAM 2, Mask2Former                │
│ Image generation        │ Stable Diffusion 3, FLUX          │
│ Tabular data            │ XGBoost, LightGBM, TabNet         │
│ Time series forecast    │ N-BEATS, TFT, TimesNet            │
│ Recommendation          │ Two-tower model, NCF              │
│ Anomaly detection       │ Isolation Forest, Autoencoder     │
│ Graph learning          │ GCN, GAT, GraphSAGE               │
└─────────────────────────┴───────────────────────────────────┘
```

### 22.2 Hyperparameter Quick Reference

| Hyperparameter | Typical Range | Effect |
|---|---|---|
| Learning rate | 1e-5 to 1e-2 | Most critical; use LR finder |
| Batch size | 16-512 | Larger = stable gradients, may overfit |
| Dropout | 0.1-0.5 | Higher = more regularization |
| Weight decay | 1e-4 to 0.1 | Prevents overfitting |
| Clip grad norm | 0.5-1.0 | Prevents explosion |
| Warmup steps | 1%-5% of total | Stabilizes early training |
| LoRA rank | 4-64 | Higher = more params but more expressive |
| Beam width | 4-6 | Higher = better quality, slower |
| Temperature | 0.1-1.5 | Lower = focused, higher = creative |
| Top-p | 0.9-0.99 | Nucleus sampling cutoff |

### 22.3 Python Quick Reference

```python
# ===========================
# DATA SCIENCE ESSENTIALS
# ===========================

# Pandas - Quick operations
df.describe()                          # Stats summary
df.isnull().sum()                      # Missing values
df['col'].value_counts(normalize=True) # Class distribution
df.corr()                              # Correlation matrix
df.groupby('cat')['val'].agg(['mean', 'std', 'count'])

# Sklearn pipeline (always use this!)
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier

pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('classifier', RandomForestClassifier(n_estimators=100))
])
pipe.fit(X_train, y_train)
pipe.score(X_test, y_test)

# Cross-validation
from sklearn.model_selection import cross_val_score
scores = cross_val_score(pipe, X, y, cv=5, scoring='f1_macro')
print(f"F1: {scores.mean():.3f} ± {scores.std():.3f}")

# Hyperparameter tuning
from sklearn.model_selection import RandomizedSearchCV
param_dist = {'classifier__n_estimators': [50, 100, 200],
              'classifier__max_depth': [None, 5, 10]}
search = RandomizedSearchCV(pipe, param_dist, n_iter=10, cv=3)
search.fit(X_train, y_train)
best = search.best_estimator_

# ===========================
# PYTORCH TRAINING BOILERPLATE
# ===========================

device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

def train_epoch(model, loader, optimizer, criterion):
    model.train()
    total_loss = 0
    for X, y in loader:
        X, y = X.to(device), y.to(device)
        optimizer.zero_grad()
        pred = model(X)
        loss = criterion(pred, y)
        loss.backward()
        torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
        optimizer.step()
        total_loss += loss.item()
    return total_loss / len(loader)

@torch.no_grad()
def evaluate(model, loader, criterion):
    model.eval()
    total_loss, correct = 0, 0
    for X, y in loader:
        X, y = X.to(device), y.to(device)
        pred = model(X)
        total_loss += criterion(pred, y).item()
        correct += (pred.argmax(1) == y).sum().item()
    return total_loss / len(loader), correct / len(loader.dataset)

# ===========================
# HUGGING FACE QUICK START
# ===========================

from transformers import AutoTokenizer, AutoModelForSequenceClassification
from transformers import Trainer, TrainingArguments
from datasets import load_dataset

# Load and tokenize
dataset = load_dataset("imdb")
tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")

def tokenize(examples):
    return tokenizer(examples["text"], truncation=True, padding="max_length", max_length=256)

tokenized = dataset.map(tokenize, batched=True)

# Fine-tune
model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)

training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=3,
    per_device_train_batch_size=16,
    per_device_eval_batch_size=32,
    warmup_steps=500,
    weight_decay=0.01,
    evaluation_strategy="epoch",
    load_best_model_at_end=True,
    fp16=True,   # Mixed precision
)

trainer = Trainer(model=model, args=training_args,
                  train_dataset=tokenized["train"],
                  eval_dataset=tokenized["test"])
trainer.train()
```

### 22.4 The ML Debugging Checklist

```
FIRST: Check the basics
 □ Is loss decreasing at all? If not: check learning rate, data loading
 □ Are you accidentally evaluating on training data?
 □ Data leakage? (test data statistics used in preprocessing)
 □ Target variable correct? Not shifted by 1?
 □ NaN/Inf in loss? → exploding gradients, bad init

OVERFITTING (high val loss, low train loss):
 □ More data (augmentation counts!)
 □ Increase regularization (dropout, weight decay, L1/L2)
 □ Reduce model complexity
 □ Early stopping
 □ Cross-validation

UNDERFITTING (both train and val loss high):
 □ More complex model
 □ More epochs
 □ Reduce regularization
 □ Feature engineering
 □ Increase learning rate

CONVERGENCE ISSUES:
 □ Learning rate too high → loss spikes or diverges
 □ Learning rate too low → very slow convergence
 □ Use learning rate finder
 □ Gradient clipping for RNN/Transformer

CLASSIFICATION IMBALANCE:
 □ Using accuracy? → Switch to F1/AUC
 □ class_weight='balanced' in sklearn
 □ SMOTE or class-weighted sampling

LLM FINE-TUNING:
 □ Catastrophic forgetting? → Lower LR, LoRA instead of full FT
 □ Loss not decreasing? → Check tokenizer EOS/padding
 □ Nonsense output? → Check chat template format
```

---

## 23. Graph Neural Networks (GNNs)

### 23.1 Graph Fundamentals

A **graph** G = (V, E) consists of:
- **V:** Set of nodes (vertices) — entities
- **E ⊆ V×V:** Set of edges — relationships between entities
- **Node features:** X ∈ ℝ^{|V|×d} — feature vector per node
- **Edge features:** optional attributes on edges

**Types of graphs:**
```
Undirected: edge (u,v) = edge (v,u)       [social networks, molecules]
Directed:   edge (u,v) ≠ edge (v,u)       [citation networks, DAGs]
Weighted:   edges have scalar weights     [road networks]
Heterogeneous: multiple node/edge types  [knowledge graphs]
Dynamic:    graph changes over time       [financial transactions]
```

**Key graph representations:**

**Adjacency matrix** A ∈ {0,1}^{N×N}: Aᵢⱼ = 1 if edge (i,j) exists. Dense. O(N²) memory.

**Adjacency list:** For each node, list of neighbors. Sparse. O(N+E) memory. Preferred for large sparse graphs.

**Graph Laplacian:**
```
Degree matrix: D = diag(d₁, d₂, ..., dₙ) where dᵢ = Σⱼ Aᵢⱼ
Laplacian:     L = D - A
Normalized:    L̃ = D^{-1/2} L D^{-1/2} = I - D^{-1/2} A D^{-1/2}
```

Properties of L: Symmetric PSD, eigenvalues ∈ [0,2], zero eigenvectors encode connected components.

**Graph statistics:**
- Degree distribution (power-law in real networks)
- Clustering coefficient: fraction of closed triangles around a node
- Diameter: longest shortest path
- Betweenness centrality: how often a node lies on shortest paths

---

### 23.2 Node Embeddings (Pre-GNN Era)

The goal: Learn d-dimensional vectors for nodes such that similar nodes have similar embeddings.

**DeepWalk (2014):**
```
1. Run R random walks of length L from each node
2. Treat walk = sentence, node = word
3. Train Word2Vec (Skip-gram) on walks
→ Nodes co-occurring in walks → similar embeddings
```

**Node2Vec (2016):**
Biased random walks that interpolate between:
- **BFS (breadth-first):** Explores local neighborhood → captures community structure
- **DFS (depth-first):** Explores far away → captures structural roles

```
Transition probability from v via edge (u,v) to x:
  α_pq(t, x) = 1/p if dist(t,x)=0    [return to previous node]
              = 1   if dist(t,x)=1    [same distance]
              = 1/q if dist(t,x)=2    [move away]

p = return parameter, q = in-out parameter
p=1, q=1: uniform random walk (DeepWalk)
p>>1, q<1: BFS-like (community)
p<1, q>>1: DFS-like (structural)
```

**LINE (Large-scale Information Network Embedding):**
First-order proximity (direct edges) + second-order proximity (shared neighbors).

**Limitations of shallow embeddings:**
- No feature incorporation
- Transductive only (can't embed new nodes)
- No parameter sharing across nodes
- Linear memory in number of nodes

---

### 23.3 Message Passing Neural Networks (MPNN) — The General Framework

All GNNs can be described as **message passing** over the graph:

```
For iteration k = 1 to K:
  For each node v:
    1. AGGREGATE: m_v^(k) = AGG({ h_u^(k-1) : u ∈ N(v) })
    2. UPDATE:    h_v^(k) = UPDATE(h_v^(k-1), m_v^(k))

Initial: h_v^(0) = x_v   (node features)
Output:  z_v = h_v^(K)   (final node embedding)
```

After K iterations, each node embedding captures a K-hop neighborhood.

**Graph-level tasks:** Add READOUT:
```
h_G = READOUT({h_v^(K) : v ∈ V})
     = SUM, MEAN, MAX, or Attention-weighted sum
```

---

### 23.4 Graph Convolutional Network (GCN)

**Kipf & Welling (2017)** — the seminal GNN paper.

**Spectral motivation:** Convolution on graphs via graph Fourier transform.
Simplified to a layer-wise propagation rule:

```
H^(l+1) = σ( Ã H^(l) W^(l) )

where:
Ã = D̃^{-1/2} Ã D̃^{-1/2}   [normalized adjacency with self-loops]
Ã = A + I                    [add self-connections]
D̃ᵢᵢ = Σⱼ Ãᵢⱼ               [degree of Ã]
H^(l) = node features at layer l
W^(l) = trainable weight matrix
σ = activation function (ReLU)
```

**Intuition:** Each node aggregates (normalized) sum of its neighbors' features, then applies linear transformation + activation.

```python
import torch
import torch.nn as nn
from torch_geometric.nn import GCNConv

class GCN(nn.Module):
    def __init__(self, in_channels, hidden_channels, out_channels):
        super().__init__()
        self.conv1 = GCNConv(in_channels, hidden_channels)
        self.conv2 = GCNConv(hidden_channels, out_channels)
        self.dropout = nn.Dropout(0.5)
    
    def forward(self, x, edge_index):
        x = self.conv1(x, edge_index)
        x = torch.relu(x)
        x = self.dropout(x)
        x = self.conv2(x, edge_index)
        return x  # node embeddings

# For node classification:
model = GCN(dataset.num_features, 64, dataset.num_classes)
out = model(data.x, data.edge_index)
loss = F.cross_entropy(out[data.train_mask], data.y[data.train_mask])
```

**Limitations of GCN:**
- Fixed normalization — all neighbors weighted equally
- Aggregation by sum/mean — not adaptive
- Shallow (oversmoothing beyond 3-4 layers)
- Spectral — doesn't generalize across graphs

---

### 23.5 GraphSAGE (Graph Sample and Aggregate)

**Hamilton et al. (2017)** — inductive learning on graphs (can generalize to unseen nodes).

```
For each node v at layer k:
  1. Sample: N_k(v) = SAMPLE(N(v), S_k)   [random S_k neighbors]
  2. Aggregate: h̃_v^k = AGG_k({h_u^{k-1}: u ∈ N_k(v)})
  3. Concatenate: h_v^k = σ(W^k · CONCAT(h_v^{k-1}, h̃_v^k))
  4. Normalize: h_v^k ← h_v^k / ||h_v^k||₂
```

**Aggregator choices:**
```
Mean:     h̃ = mean({h_u: u ∈ N(v)})   [simplest, like GCN]
LSTM:     Apply LSTM to random permutation of neighbors
Pooling:  h̃ = MAX(σ(W·h_u + b): u ∈ N(v))   [element-wise max]
```

**Key advantage:** Mini-batch training via neighbor sampling — scales to billions of nodes.

**Inductive:** At test time, a new node with features x can be embedded by aggregating its neighbors (unlike DeepWalk/GCN which require retraining).

---

### 23.6 Graph Attention Network (GAT)

**Veličković et al. (2018)** — learn adaptive neighbor weights via attention.

```
Attention coefficient between node i and neighbor j:
  eᵢⱼ = a( W hᵢ || W hⱼ )   [|| = concatenation]
       = LeakyReLU( aᵀ [W hᵢ || W hⱼ] )

Normalize: αᵢⱼ = softmax_j(eᵢⱼ) = exp(eᵢⱼ) / Σ_{k∈N(i)} exp(eᵢₖ)

Aggregate: h'ᵢ = σ( Σ_{j∈N(i)} αᵢⱼ W hⱼ )
```

**Multi-head attention (GAT):**
```
h'ᵢ = ||_{k=1}^{K} σ( Σ_{j∈N(i)} α^k_{ij} W^k h_j )
```
Concatenate K attention heads. For output layer: average instead.

```python
from torch_geometric.nn import GATConv

class GAT(nn.Module):
    def __init__(self, in_c, hidden_c, out_c, heads=8):
        super().__init__()
        self.conv1 = GATConv(in_c, hidden_c, heads=heads, dropout=0.6)
        self.conv2 = GATConv(hidden_c*heads, out_c, heads=1, concat=False, dropout=0.6)
    
    def forward(self, x, edge_index):
        x = F.dropout(x, p=0.6, training=self.training)
        x = F.elu(self.conv1(x, edge_index))
        x = F.dropout(x, p=0.6, training=self.training)
        return self.conv2(x, edge_index)
```

**GAT advantages:**
- Adaptive neighbor importance (vs fixed in GCN)
- No eigendecomposition — works on any graph
- Interpretable attention weights
- GATv2 (2022): fixes "static" attention problem — attention depends on both query AND key

---

### 23.7 Graph Isomorphism Network (GIN)

**Xu et al. (2019)** — provably the most expressive GNN within the message passing framework.

Proves: GNN is at most as powerful as the **Weisfeiler-Lehman (WL) graph isomorphism test**.

GIN achieves WL-level expressiveness:
```
h_v^(k) = MLP^(k)((1 + ε^(k)) · h_v^(k-1) + Σ_{u∈N(v)} h_u^(k-1))
```

Key: Sum aggregation (not mean/max) is injective — preserves multiset information.

---

### 23.8 Graph Transformers

Extend transformer architecture to graphs:

**Key challenge:** Standard self-attention = complete graph (O(N²)). Graphs are sparse.

**Approaches:**

**Graphformer (2021):**
```
Modifies transformer attention with graph-structural biases:
h'ᵢ = Attention(Q,K,V) + b_degree + b_spatial + b_edge

b_degree: centrality encoding for each node's in/out degree
b_spatial: shortest path distance between i and j
b_edge: edge feature on path from i to j
```

**NodeFormer (2022):** All-pair message passing with kernelized softmax — O(N) complexity.

**GPS (General, Powerful, Scalable Graph Transformer, 2022):**
```
h_v = MPNNLayer(h_v) + TransformerLayer(h_v)
```
Combines local message passing + global attention.

**Exphormer (2023):** Sparse attention via virtual nodes + expander graphs.

---

### 23.9 Oversmoothing & Oversquashing

**Oversmoothing:** After K layers, all node features converge to same vector — lose discriminative power.
- Solution: Skip connections, PairNorm, DropEdge, fewer layers

**Oversquashing:** Information from exponentially many distant nodes gets compressed into fixed-size vectors — exponential information loss.
- Solution: Graph rewiring, virtual nodes, graph transformers

---

### 23.10 GNN Applications

#### Recommendation Systems
```
Bipartite graph: users ↔ items
Edges = interactions (clicks, purchases, ratings)

LightGCN (2020): Simplified GCN for recommendations
  - Remove feature transformation and nonlinearity
  - Only propagate embeddings:
    e_u^(k+1) = Σ_{i∈N(u)} e_i^(k) / √|N(u)||N(i)|
  - Final: e_u = (1/(K+1)) Σ_k e_u^(k)
  
Loss: BPR (Bayesian Personalized Ranking):
  L = -Σ log σ(ê_u·ê_i - ê_u·ê_j)   [i=positive, j=negative]
```

#### Drug Discovery & Molecular ML
```
Molecules as graphs:
  Nodes = atoms (features: atomic number, valence, charge)
  Edges = bonds (features: bond type, aromaticity)

Tasks:
  - Molecular property prediction (logP, toxicity, binding affinity)
  - Drug-target interaction prediction
  - Molecule generation

Key models:
  - MPNN (2017): Quantum chemistry properties
  - AttentiveFP: FDA drug property prediction
  - DimeNet: 3D molecular geometry
  - SE(3)-Transformer: Rotationally equivariant
  - AlphaFold2: Protein structure (uses attention on residue graph)
```

#### Fraud Detection
```
Financial transaction graph:
  Nodes = accounts, merchants, cards
  Edges = transactions (features: amount, time, location)
  
GNN detects anomalous subgraph patterns:
  - Money laundering rings (circular transactions)
  - Account takeover (unusual connection patterns)
  - Synthetic identity fraud
  
Key: Temporal + heterogeneous graphs
GraphBEHRT: BERT on transaction sequences + graph structure
```

#### Social Networks
```
Tasks:
  - Community detection (node clustering)
  - Link prediction (friend recommendation)
  - Influence maximization (viral marketing)
  - Fake account detection
  
Graph-level properties matter:
  - Homophily: connected nodes tend to be similar
  - Heterophily: connected nodes tend to differ
  
H2GCN: designed for heterophilic graphs (not assumed similar neighbors)
```

#### Knowledge Graphs & Reasoning
```
Knowledge Graph: G = {(h, r, t)} — head entity, relation, tail entity
Example: (Paris, capital_of, France), (Eiffel_Tower, located_in, Paris)

Knowledge Graph Embedding (KGE):
TransE:  h + r ≈ t   (translation model)
RotatE:  h ∘ r = t   (rotation in complex space)
ComplEx: Uses complex-valued embeddings

Tasks:
  - Link prediction: P(Paris, capital_of, ?)
  - Triple classification: Is (X,Y,Z) true?
  - Entity alignment across KGs

R-GCN: Relational GCN for multi-relational graphs
KGCN: GCN for knowledge graph + recommendation
```

---

## 24. Time Series Machine Learning

### 24.1 Time Series Fundamentals

A **time series** is a sequence of values indexed by time: {y₁, y₂, ..., yₜ}.

**Decomposition:**
```
y_t = Trend_t + Seasonality_t + Residual_t   (additive)
y_t = Trend_t × Seasonality_t × Residual_t   (multiplicative, for growing amplitude)
```

**Key properties:**
- **Stationarity:** Statistical properties (mean, variance, autocorrelation) constant over time
- **Autocorrelation:** Correlation of series with its own lagged values: ρ(k) = Corr(yₜ, yₜ₋ₖ)
- **Partial Autocorrelation (PACF):** Correlation at lag k controlling for shorter lags

**Stationarity tests:**
- **ADF (Augmented Dickey-Fuller):** H₀: unit root (non-stationary). p < 0.05 → stationary
- **KPSS:** H₀: stationary. Complementary to ADF.

**Making series stationary:**
```python
# Differencing
y_diff = y.diff(1)       # first difference: removes trend
y_diff2 = y.diff().diff() # second difference: removes quadratic trend
y_log_diff = np.log(y).diff()  # log differencing: stabilizes variance
```

---

### 24.2 Classical Statistical Models

#### AR (AutoRegressive)

```
y_t = c + Σᵢ₌₁ᵖ φᵢ y_{t-i} + ε_t

AR(1): y_t = c + φ₁ y_{t-1} + ε_t
  φ₁ < 1: stationary mean-reverting
  φ₁ = 1: random walk (non-stationary)
  φ₁ > 1: explosive
```

**Selection of p:** Use PACF — cut off after lag p.

#### MA (Moving Average)

```
y_t = μ + ε_t + Σᵢ₌₁ᵍ θᵢ ε_{t-i}

MA(1): y_t = μ + ε_t + θ₁ ε_{t-1}
```

**Selection of q:** Use ACF — cut off after lag q.

#### ARIMA(p, d, q)

```
AR(p) + Integration(d) + MA(q)

d = number of differencing steps to achieve stationarity

ARIMA(1,1,1): 
  Step 1: Difference once (d=1): y'_t = y_t - y_{t-1}
  Step 2: AR(1)+MA(1) on y'_t: y'_t = φ₁y'_{t-1} + ε_t + θ₁ε_{t-1}
```

**Box-Jenkins methodology:**
1. Identify d (ADF test, plot)
2. Identify p and q (ACF, PACF plots)
3. Estimate parameters (MLE)
4. Diagnose residuals (should be white noise)

```python
from statsmodels.tsa.arima.model import ARIMA
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf

# Model selection via AIC
results = {}
for p in range(3):
    for q in range(3):
        try:
            model = ARIMA(y_train, order=(p, 1, q))
            fit = model.fit()
            results[(p,1,q)] = fit.aic
best_order = min(results, key=results.get)

# Fit best model
model = ARIMA(y_train, order=best_order)
fit = model.fit()
forecast = fit.forecast(steps=12)
```

#### SARIMA(p, d, q)(P, D, Q, s)

Extends ARIMA for **seasonal patterns** (period s):

```
Non-seasonal: ARIMA(p,d,q)
Seasonal:     ARIMA(P,D,Q,s)

Full model: SARIMA(p,d,q)(P,D,Q,s)
Example: SARIMA(1,1,1)(1,1,1,12)  ← monthly data, yearly seasonality
```

**Seasonal differencing:** y'_t = y_t - y_{t-s}

#### ARMAX, VAR, VARIMA

- **ARMAX:** ARIMA with exogenous regressors (external predictors)
- **VAR (Vector AR):** Multivariate time series — each series depends on lags of ALL series
- **VARIMA:** VAR + MA + Integration for multivariate forecasting

---

### 24.3 Prophet (Facebook/Meta)

Designed for **business time series** with strong seasonality and holiday effects.

**Model:**
```
y(t) = g(t) + s(t) + h(t) + ε_t

g(t) = trend (linear or logistic growth with changepoints)
s(t) = seasonality (Fourier series: yearly, weekly, daily)
h(t) = holiday effects (separate indicator per holiday)
ε_t  = noise
```

**Seasonal component:**
```
s(t) = Σₙ (aₙ cos(2πnt/P) + bₙ sin(2πnt/P))
```
N=10 Fourier terms for annual, N=3 for weekly.

**Changepoint detection:** Piecewise linear trend with automatic changepoint selection via L1 sparsity on rate changes.

```python
from prophet import Prophet
import pandas as pd

# Prophet requires columns: 'ds' (date) and 'y' (value)
model = Prophet(
    changepoint_prior_scale=0.05,   # flexibility of trend
    seasonality_prior_scale=10,     # seasonality strength
    holidays_prior_scale=10,
    yearly_seasonality=True,
    weekly_seasonality=True,
    daily_seasonality=False
)

# Add country holidays
model.add_country_holidays(country_name='US')

# Add custom seasonality
model.add_seasonality(name='monthly', period=30.5, fourier_order=5)

model.fit(df_train)
future = model.make_future_dataframe(periods=365)
forecast = model.predict(future)
# forecast columns: yhat, yhat_lower, yhat_upper, trend, yearly, weekly
```

**Best for:** Business KPIs, web traffic, product sales with seasonal patterns and holidays.

---

### 24.4 Temporal Convolutional Network (TCN)

Applies 1D convolutions with **causal padding** and **dilations** for sequence modeling.

```
Causal convolution: output at time t only uses inputs up to t (no future leakage)
Dilated convolution: receptive field = 2^(layer) → exponential growth

With dilation rates [1, 2, 4, 8, 16, 32]:
Layer 1: receptive field = 2
Layer 2: receptive field = 4
Layer 3: receptive field = 8
...
Layer 6: receptive field = 64

Total receptive field after stacking = 127 timesteps
```

**TCN architecture:**
```python
class TemporalBlock(nn.Module):
    def __init__(self, n_inputs, n_outputs, kernel_size, stride, dilation, padding):
        super().__init__()
        self.conv1 = weight_norm(nn.Conv1d(n_inputs, n_outputs, kernel_size,
                                           stride=stride, padding=padding, dilation=dilation))
        self.conv2 = weight_norm(nn.Conv1d(n_outputs, n_outputs, kernel_size,
                                           stride=stride, padding=padding, dilation=dilation))
        self.net = nn.Sequential(
            self.conv1, nn.ReLU(), nn.Dropout(0.2),
            self.conv2, nn.ReLU(), nn.Dropout(0.2)
        )
        # Residual connection
        self.downsample = nn.Conv1d(n_inputs, n_outputs, 1) if n_inputs != n_outputs else None
        self.relu = nn.ReLU()
    
    def forward(self, x):
        out = self.net(x)
        res = x if self.downsample is None else self.downsample(x)
        return self.relu(out + res)
```

**Advantages over RNN:** Parallelizable, stable gradients, flexible receptive field, no vanishing gradients.

---

### 24.5 Transformer-Based Forecasting

#### Vanilla Transformer for Time Series
Problem: Quadratic attention O(L²) for long sequences is prohibitive.

#### Informer (2021)
Key innovation: **ProbSparse Attention** — O(L log L) complexity:
```
Select only top-u queries by computing attention sparsity score:
M(qᵢ, K) = max_j(qᵢ·kⱼᵀ/√d) - (1/L) Σⱼ (qᵢ·kⱼᵀ/√d)
Only keep top-u queries with highest M scores.
```

Also uses: Distilling operation (halving sequence length between layers via max-pooling).

#### Autoformer (2021)
Replaces attention with **Auto-Correlation mechanism** based on Fast Fourier Transform:
```
Auto-correlation: S(τ) = (1/L) Σ xₜ x_{t-τ}  [computed via FFT in O(L log L)]
Top-k lags selected → period-aligned aggregation
```

Also introduces: Series Decomposition blocks (trend + seasonal separation at each layer).

#### PatchTST (2023) — The Breakthrough

Treats time series as patches (like ViT for images):
```
1. Divide series into patches of size p: [y₁..yₚ, y_{p+1}..y_{2p}, ...]
2. Each patch = one token (linear projection)
3. Standard Transformer encoder on patch tokens
4. Channel-independence: each variable treated separately

Benefits:
  - O((L/p)²) attention — p=16: 256× reduction in attention cost
  - Longer lookback window (can use L=512 easily)
  - Local semantic information in each patch
  - Outperforms all prior time series transformers on benchmarks
```

```python
# PatchTST in PyTorch (simplified)
class PatchTST(nn.Module):
    def __init__(self, seq_len, pred_len, patch_size=16, d_model=128, n_heads=8, n_layers=3):
        super().__init__()
        self.patch_size = patch_size
        self.n_patches = seq_len // patch_size
        self.patch_embed = nn.Linear(patch_size, d_model)
        self.pos_embed = nn.Parameter(torch.zeros(1, self.n_patches, d_model))
        encoder_layer = nn.TransformerEncoderLayer(d_model, n_heads, dim_feedforward=512)
        self.encoder = nn.TransformerEncoder(encoder_layer, n_layers)
        self.head = nn.Linear(d_model * self.n_patches, pred_len)
    
    def forward(self, x):                         # x: [B, L, C]
        B, L, C = x.shape
        x = x.permute(0, 2, 1)                   # [B, C, L]
        x = x.unfold(-1, self.patch_size, self.patch_size)  # [B, C, n_patches, patch_size]
        x = x.reshape(B*C, self.n_patches, self.patch_size)
        x = self.patch_embed(x) + self.pos_embed
        x = self.encoder(x)
        x = x.reshape(B*C, -1)
        return self.head(x).reshape(B, C, -1).permute(0, 2, 1)  # [B, pred_len, C]
```

#### Other Modern Forecasting Models

**N-BEATS (2020, Element AI):**
```
Stack of blocks, each with:
  1. Backward forecast (predict history)
  2. Forward forecast (predict future)
  3. Residual learning: subtract backward forecast, pass residual to next block
  
Interpretable variant: uses Fourier basis (seasonality) + polynomial basis (trend)
```

**N-HiTS (2022):** Hierarchical interpolation — different blocks for different temporal scales.

**TimesNet (2023):** Transforms 1D time series into 2D space capturing periodic patterns.

**iTransformer (2024):** Inverts the transformer — applies attention across variates (features) instead of time steps. State-of-the-art on many benchmarks.

**Mamba for Time Series:** SSMs (see Section 35) applied to time series — linear complexity, competitive with transformers.

---

### 24.6 Time Series Applications

#### Stock Price Prediction
```
Features:
  Technical: OHLCV, RSI, MACD, Bollinger Bands, volume
  Fundamental: P/E ratio, earnings, book value
  Sentiment: news NLP scores, social media sentiment
  Macro: interest rates, VIX, sector indices

Task: Usually predict returns (y_{t+1} - y_t)/y_t not price directly
Loss: Often Sharpe ratio, not MSE

Key caveat: Efficient Market Hypothesis — beating market consistently is very hard
Better framing: Regime detection, volatility prediction, portfolio optimization
```

#### Weather Forecasting
```
GraphCast (DeepMind, 2023):
  - Encodes globe as icosahedral grid (40,962 nodes)
  - GNN for regional updates
  - 10-day forecast surpassing ECMWF (operational NWP)

Pangu-Weather (Huawei, 2023):
  - 3D Swin Transformer operating on pressure levels
  - Hierarchical temporal aggregation
  - Best 1-7 day forecasts ever

ClimaX:
  - Foundation model for weather
  - Pre-trained on diverse ERA5 + CMIP6 data
```

#### IoT Sensor Anomaly Detection
```
Unsupervised approaches (no labels needed):
  1. LSTM Autoencoder:
     Encode sequence → bottleneck → Decode
     Anomaly score = reconstruction error
  
  2. Isolation Forest on sliding windows:
     High anomaly score → alert
  
  3. SPOT (Statistical POT):
     Extreme Value Theory for dynamic threshold
  
  4. TranAD (2022):
     Transformer-based adversarial training
     Better on high-dimensional multivariate sensors

Evaluation: Adjust F1 (if any point in anomaly window detected = TP)
```

---

## 25. Explainable AI (XAI)

### 25.1 Why XAI?

**The interpretability problem:** High-accuracy models (deep nets, ensembles) are black boxes — we can't understand why they make predictions.

**Why it matters:**
- Regulatory compliance (GDPR Article 22: right to explanation)
- Debugging and error analysis
- Trust and adoption (doctors, lawyers won't use unexplainable models)
- Detecting bias and spurious correlations
- Scientific discovery (what features actually matter?)

**Two paradigms:**
- **Intrinsically interpretable:** Linear models, decision trees, GAMs, rule lists
- **Post-hoc explanations:** SHAP, LIME, Grad-CAM (explain any model after training)

**Two scopes:**
- **Global:** Overall model behavior (what features matter in general?)
- **Local:** Single prediction explanation (why did the model predict X for this instance?)

---

### 25.2 SHAP (SHapley Additive exPlanations)

**The theoretically grounded approach.** Based on Shapley values from cooperative game theory.

**Core idea:** Fairly distribute the prediction among features by considering all possible subsets.

```
Shapley value for feature i:
  φᵢ = Σ_{S⊆F\{i}} [|S|!(|F|-|S|-1)!/|F|!] [f(S∪{i}) - f(S)]

= average marginal contribution of feature i across all orderings
```

**Properties (only attribution method with all these):**
1. **Efficiency:** Σ φᵢ = f(x) - E[f(X)] (explanations sum to prediction)
2. **Symmetry:** Features with same contribution get same Shapley value
3. **Dummy:** Features not affecting output get 0
4. **Additivity:** For ensemble of models, Shapley values add up

**TreeSHAP:** Exact O(TLD²) algorithm for tree ensembles (vs exponential exact):
```python
import shap

# Tree-based models (exact, fast)
explainer = shap.TreeExplainer(xgb_model)
shap_values = explainer.shap_values(X_test)

# Visualizations
shap.summary_plot(shap_values, X_test)      # feature importance + direction
shap.waterfall_plot(shap_values[0])         # single prediction explanation
shap.dependence_plot("feature_name", shap_values, X_test)  # SHAP vs feature value
shap.force_plot(explainer.expected_value, shap_values[0], X_test.iloc[0])
```

**KernelSHAP:** Model-agnostic SHAP via weighted linear regression over feature subsets. Slow but works for any model.

**DeepSHAP:** Fast approximation for neural networks via DeepLIFT + Shapley.

**SHAP interaction values:** φᵢⱼ — how features i and j interact in their combined contribution.

---

### 25.3 LIME (Local Interpretable Model-agnostic Explanations)

**Key idea:** Approximate the black-box model **locally** around the instance of interest with an interpretable (linear) model.

```
Algorithm for explaining prediction f(x):
1. Sample perturbed instances z' around x (by masking/perturbing features)
2. Get predictions f(z') from black-box model
3. Weight samples by proximity to x: π_x(z) = exp(-D(x,z)²/σ²)
4. Fit weighted linear model: g = argmin_g Σ π_x(z)[f(z)-g(z')]² + Ω(g)
5. Return linear model coefficients as explanation
```

```python
import lime
from lime import lime_tabular

explainer = lime.lime_tabular.LimeTabularExplainer(
    X_train,
    feature_names=feature_names,
    class_names=['No', 'Yes'],
    mode='classification'
)

# Explain a single prediction
exp = explainer.explain_instance(
    X_test[0],
    model.predict_proba,
    num_features=10,
    num_samples=1000
)
exp.show_in_notebook()
exp.as_list()  # [(feature_name, coefficient), ...]
```

**LIME for images:**
```python
from lime import lime_image
explainer = lime_image.LimeImageExplainer()
explanation = explainer.explain_instance(
    image_array,
    model.predict,
    top_labels=5,
    hide_color=0,
    num_samples=1000
)
# Visualize superpixel importance
```

**LIME vs SHAP:**
| | LIME | SHAP |
|---|---|---|
| Theoretical basis | Heuristic | Game theory (axioms) |
| Consistency | Unstable | Consistent |
| Speed | Medium | Fast (TreeSHAP) |
| Global explanations | No (only local) | Yes |
| Works with any model | Yes | Yes (KernelSHAP) |

---

### 25.4 Partial Dependence Plots (PDP) & ICE

**Partial Dependence Plot (PDP) — Global:**
Shows the marginal effect of one or two features on the prediction:
```
PD_S(x_S) = E_{x_C}[f(x_S, x_C)] = (1/n) Σᵢ f(x_S, x_C^(i))
```
Plot PD vs feature value to see average effect.

```python
from sklearn.inspection import PartialDependenceDisplay

# For single feature
PartialDependenceDisplay.from_estimator(
    model, X_train,
    features=[0, 1, (0,1)],  # 1D and 2D PD
    feature_names=feature_names
)
```

**Individual Conditional Expectation (ICE) — Local:**
Like PDP but one line per instance (not averaged). Reveals heterogeneity in feature effects.

```python
PartialDependenceDisplay.from_estimator(
    model, X_train, features=[0],
    kind='individual',   # ICE
    subsample=100        # show 100 random samples
)
```

**d-PDP (centered ICE):** Subtract each ICE line's value at reference point — highlights interaction effects.

---

### 25.5 Permutation Feature Importance

Measure how much the model performance drops when a feature is randomly shuffled:
```python
from sklearn.inspection import permutation_importance

result = permutation_importance(
    model, X_val, y_val,
    n_repeats=10,     # shuffle 10 times, take mean
    random_state=42,
    scoring='neg_mean_squared_error'
)

# result.importances_mean: mean decrease in metric per feature
# result.importances_std: std across shuffles
```

**Advantage over model-internal importance:** Works for any model, uses held-out data (not training data), measures impact on actual performance.

---

### 25.6 Integrated Gradients (Neural Networks)

**Axiomatic attribution** for deep neural networks.

```
IG_i(x) = (x_i - x'_i) × ∫₀¹ [∂F(x' + α(x-x')) / ∂xᵢ] dα

x  = input to explain
x' = baseline (zero, black image, average)
```

Numerically: average gradients along the straight path from baseline to input.

```python
import captum
from captum.attr import IntegratedGradients

ig = IntegratedGradients(model)
attributions = ig.attribute(
    input_tensor,
    baseline=torch.zeros_like(input_tensor),
    target=target_class,
    n_steps=50
)
# attributions: same shape as input, indicates contribution of each feature
```

**Satisfies axioms:** Sensitivity (non-zero attribution for relevant features), Implementation Invariance (functionally equivalent models get same attributions).

---

### 25.7 Grad-CAM (Gradient-weighted Class Activation Mapping)

Produces **visual explanations** for CNN decisions by highlighting discriminative regions.

```
Algorithm:
1. Forward pass image through CNN to layer L (last conv layer)
2. Compute gradient of class score y^c w.r.t. feature maps A^k_L
3. Global Average Pool gradients: α^c_k = (1/Z) Σᵢⱼ ∂y^c/∂A^k_{ij}
4. Weight feature maps by importance: L_Grad-CAM = ReLU(Σ_k α^c_k A^k)
5. Upsample to input size and overlay on image

ReLU: only keep positively contributing activations
```

```python
from torchcam.methods import GradCAM

cam_extractor = GradCAM(model, target_layer='layer4')
out = model(img_tensor)
cam = cam_extractor(out.squeeze(0).argmax().item(), out)
# cam: heatmap showing where model "looked" for this class
```

**Variants:**
- **Grad-CAM++:** Weighted average of positive partial derivatives — better localization
- **Score-CAM:** Uses forward pass scores instead of gradients — no gradient needed
- **EigenCAM:** Uses eigenvectors of feature maps — fast, no class-specific
- **HiResCAM:** Higher resolution by using element-wise product

---

### 25.8 Attention Visualization

For Transformer-based models, visualize attention weights:

```python
from bertviz import model_view, head_view

# BERT attention visualization
model = BertModel.from_pretrained('bert-base-uncased', output_attentions=True)
inputs = tokenizer.encode_plus(text, return_tensors='pt')
outputs = model(**inputs)

attention = outputs.attentions  # (n_layers, n_heads, seq_len, seq_len)

# Interactive head-level view
head_view(attention, tokens)

# Layer-level view
model_view(attention, tokens)
```

**Caveat:** Attention ≠ explanation. High attention weight ≠ feature importance. Attention can be misleading.

Better alternatives for LLMs:
- **Integrated Gradients on token embeddings**
- **SHAP for transformers** (via shap.Explainer with masker)
- **Attention rollout:** Propagate attention through all layers

---

## 26. Bayesian Machine Learning

### 26.1 Bayesian Inference Framework

**The Bayesian update:**
```
P(θ|D) = P(D|θ) · P(θ) / P(D)
          ──────   ─────   ────
         Posterior Likelihood × Prior / Evidence

In words:
  "What I believe about θ after seeing data D
   = how well D explains under θ × what I believed before / normalizing constant"
```

**MLE vs MAP vs Full Bayesian:**

```
MLE:         θ_MLE = argmax P(D|θ)                    [maximize likelihood]
MAP:         θ_MAP = argmax P(D|θ)P(θ)                [maximize posterior]
Full Bayes:  P(θ|D) = full posterior distribution     [uncertainty preserved]

Prediction:
MLE/MAP: ŷ = f(x; θ*)                               [point estimate]
Bayes:   P(y|x,D) = ∫ P(y|x,θ) P(θ|D) dθ          [posterior predictive]
```

**Connection to regularization:**
- MAP with Gaussian prior on θ = Ridge regression
- MAP with Laplace prior on θ = Lasso regression
- L2 regularization λ = σ²/σ_prior² (noise variance / prior variance)

---

### 26.2 Conjugate Priors

When prior and posterior are in the same family — closed-form updates!

| Likelihood | Prior | Posterior |
|---|---|---|
| Bernoulli | Beta(α,β) | Beta(α+successes, β+failures) |
| Multinomial | Dirichlet | Dirichlet(α+counts) |
| Gaussian (known σ) | Gaussian | Gaussian |
| Poisson | Gamma | Gamma |

**Example — Bayesian A/B testing:**
```python
import numpy as np
from scipy import stats

# Prior: uniform Beta(1,1) = no information
alpha_prior, beta_prior = 1, 1

# Observed: variant A got 30 conversions out of 100
# Observed: variant B got 40 conversions out of 100
alpha_A, beta_A = alpha_prior + 30, beta_prior + 70
alpha_B, beta_B = alpha_prior + 40, beta_prior + 60

# Posterior distributions
posterior_A = stats.beta(alpha_A, beta_A)
posterior_B = stats.beta(alpha_B, beta_B)

# P(B > A) via Monte Carlo
samples = 100000
p_B_wins = np.mean(posterior_B.rvs(samples) > posterior_A.rvs(samples))
print(f"P(B > A) = {p_B_wins:.3f}")

# Credible interval (vs confidence interval — directly interpretable!)
ci_B = posterior_B.interval(0.95)
print(f"95% credible interval for B's conversion rate: {ci_B}")
```

---

### 26.3 Bayesian Neural Networks (BNNs)

Put probability distributions over neural network weights:

```
Prior:      P(W) = Π N(wᵢ; 0, σ²)
Likelihood: P(D|W) = Π P(yᵢ|f(xᵢ; W))
Posterior:  P(W|D) ∝ P(D|W) P(W)   ← intractable!
```

**Why intractable:** Integral over all network weights = high-dimensional, non-conjugate.

**Variational Inference (VI) — Practical BNN:**
Approximate posterior q_φ(W) (usually Gaussian) by minimizing KL divergence:
```
L(φ) = KL(q_φ(W) || P(W)) - E_q[log P(D|W)]
      = complexity cost + reconstruction cost
```

**Bayes by Backprop (BBB):** Reparameterize w = μ + σ·ε, ε~N(0,1) → backprop through μ, ρ (σ = log(1+exp(ρ))).

```python
import torch.nn as nn

class BayesLinear(nn.Module):
    def __init__(self, in_features, out_features):
        super().__init__()
        # Weight mean and log-std
        self.weight_mu = nn.Parameter(torch.zeros(out_features, in_features))
        self.weight_rho = nn.Parameter(torch.ones(out_features, in_features) * -3)
        self.bias_mu = nn.Parameter(torch.zeros(out_features))
    
    def forward(self, x):
        weight_sigma = torch.log1p(torch.exp(self.weight_rho))
        weight = self.weight_mu + weight_sigma * torch.randn_like(self.weight_mu)
        return nn.functional.linear(x, weight, self.bias_mu)
    
    def kl_loss(self):
        weight_sigma = torch.log1p(torch.exp(self.weight_rho))
        kl = -0.5 * (1 + 2*torch.log(weight_sigma) - self.weight_mu**2 - weight_sigma**2)
        return kl.sum()
```

---

### 26.4 Monte Carlo Dropout

**Gal & Ghahramani (2016):** Dropout at test time = approximate Bayesian inference!

```python
class MCDropoutModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(784, 256)
        self.fc2 = nn.Linear(256, 10)
        self.dropout = nn.Dropout(0.5)
    
    def forward(self, x):
        # Keep dropout active during inference!
        x = self.dropout(torch.relu(self.fc1(x)))
        return self.fc2(x)
    
    def predict_with_uncertainty(self, x, n_samples=50):
        self.train()  # Keep dropout active
        predictions = torch.stack([
            torch.softmax(self.forward(x), dim=1)
            for _ in range(n_samples)
        ])
        mean = predictions.mean(0)       # E[y|x]
        uncertainty = predictions.std(0)  # Epistemic uncertainty
        return mean, uncertainty
```

**Practical advantage:** No special training needed — just run existing Dropout model multiple times at test time.

---

### 26.5 Gaussian Processes (GP)

A **Gaussian Process** defines a distribution over functions:

```
f ~ GP(m(x), k(x, x'))

m(x) = mean function (usually 0)
k(x, x') = kernel (covariance) function — encodes smoothness assumptions
```

**Prediction (GP regression):**
```
Given training data (X, y), at new point x*:

P(f* | x*, X, y) = N(μ*, σ²*)

μ* = k(x*, X) [k(X,X) + σ²I]⁻¹ y
σ²* = k(x*, x*) - k(x*, X) [k(X,X) + σ²I]⁻¹ k(X, x*)
```

**Common kernels:**
```
RBF (SE): k(x,x') = σ² exp(-||x-x'||²/2l²)   [infinitely differentiable]
Matérn-5/2: k(x,x') = σ²(1+√5r/l+5r²/3l²)exp(-√5r/l)  [twice differentiable]
Linear: k(x,x') = x·x'   [Bayesian linear regression]
Periodic: k(x,x') = σ² exp(-2sin²(π|x-x'|/p)/l²)   [periodic functions]
```

```python
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.gaussian_process.kernels import RBF, Matern, WhiteKernel

kernel = 1.0 * Matern(length_scale=1.0, nu=2.5) + WhiteKernel(noise_level=0.1)
gp = GaussianProcessRegressor(kernel=kernel, n_restarts_optimizer=10)
gp.fit(X_train, y_train)

y_pred, y_std = gp.predict(X_test, return_std=True)
# y_std: epistemic uncertainty at each test point
```

**GP strengths:** Uncertainty quantification, works well with small data, principled hyperparameter learning.
**GP weakness:** O(n³) training, O(n²) memory — scales poorly. Sparse GPs (inducing points) extend to larger datasets.

---

## 27. Causal ML

### 27.1 Correlation vs Causation

The fundamental distinction in science:

**Observational data:** We observe what happens naturally — correlation.
**Experimental data:** We intervene and measure effect — causation.

```
Spurious example:
  Ice cream sales correlate strongly with drowning deaths.
  Cause: both driven by hot weather (confounding variable)
  Intervention: Banning ice cream would not reduce drownings!
```

**Simpson's Paradox:**
```
University admissions (Berkeley):
  Overall: men admitted at higher rate than women
  By department: women admitted at higher rate in EVERY department
  Reason: women applied to harder departments
  
Lesson: aggregation can reverse causal conclusions
```

---

### 27.2 Structural Causal Models (SCMs) and DAGs

**Directed Acyclic Graph (DAG):** Encodes causal assumptions as a graph.
- Nodes = variables
- Directed edges = direct causal influences
- **No cycles** (can't be your own cause)

```
Example DAG: X → Y ← Z → W
               ↑
               W

Reads as:
  X directly causes Y
  Z directly causes Y and W
  W also causes X (through unshown path? no — it's Z→W only)
```

**d-separation:** Graphical criterion for conditional independence.
- **Fork:** Z←X→Y: X d-separates Z and Y (conditioning on X blocks path)
- **Chain:** Z→X→Y: X d-separates Z and Y
- **Collider:** Z→X←Y: X d-connects Z and Y (conditioning OPENS path — selection bias!)

---

### 27.3 The Do-Calculus

Pearl's **do(·) operator** distinguishes observation from intervention:

```
P(Y=y | X=x): observational — P(Y|X), correlational
P(Y=y | do(X=x)): interventional — what if we FORCE X=x
```

**Why they differ:**
```
Example: Doctors give Drug X to sicker patients (confounding)
P(Recovery | Drug=yes) is LOW (sicker patients get drug)
P(Recovery | do(Drug=yes)) is HIGH (drug actually helps)
```

**Backdoor criterion:**
A set Z blocks all backdoor paths (paths from X to Y going through common causes) if:
1. No node in Z is a descendant of X
2. Z blocks every path between X and Y that contains an arrow into X

If Z satisfies backdoor criterion:
```
P(Y | do(X)) = Σ_z P(Y | X, Z=z) P(Z=z)
```
This is the **adjustment formula** — identify causal effect from observational data!

---

### 27.4 Potential Outcomes Framework (Rubin Causal Model)

**Counterfactuals:**
```
Y_i(1) = outcome for unit i if treated
Y_i(0) = outcome for unit i if control

Individual Treatment Effect (ITE): τᵢ = Y_i(1) - Y_i(0)
Average Treatment Effect (ATE): τ = E[Y(1) - Y(0)]
Average Treatment on Treated (ATT): τ_ATT = E[Y(1)-Y(0) | T=1]
```

**Fundamental problem of causal inference:** We can never observe both Y_i(1) and Y_i(0) for the same unit simultaneously.

**Assumptions for identification:**
1. **SUTVA:** No interference between units
2. **Ignorability:** (Y(0),Y(1)) ⊥ T | X (no unmeasured confounders)
3. **Overlap:** 0 < P(T=1|X) < 1 (everyone has some chance of treatment)

---

### 27.5 Causal Estimation Methods

**Propensity Score Matching:**
```python
# Estimate propensity score P(T=1|X) using logistic regression
from sklearn.linear_model import LogisticRegression

ps_model = LogisticRegression().fit(X, T)
propensity_scores = ps_model.predict_proba(X)[:, 1]

# Match treated to most similar control via propensity score
# ATE = average difference in Y between matched pairs
```

**Inverse Probability Weighting (IPW):**
```
ATE_IPW = (1/n) Σᵢ [T_i Y_i / e(X_i) - (1-T_i)Y_i / (1-e(X_i))]

where e(X) = P(T=1|X) is the propensity score
```

**Doubly Robust Estimator:** Consistent if EITHER outcome model OR propensity model is correct.

**Difference-in-Differences (DiD):**
```
Uses parallel trends assumption:
  ATT = [E[Y|T=1, Post] - E[Y|T=1, Pre]] - [E[Y|T=0, Post] - E[Y|T=0, Pre]]
```

**Instrumental Variables (IV):**
When confounding exists, use an instrument Z that:
1. Affects treatment T (relevance)
2. Doesn't directly affect outcome Y (exclusion restriction)
3. Independent of confounders (independence)

```
2SLS (Two-Stage Least Squares):
  Stage 1: T̂ = α + βZ + ε    [regress T on instrument]
  Stage 2: Y = α' + γT̂ + ε'  [regress Y on predicted T]
  γ = LATE (Local Average Treatment Effect)
```

**Regression Discontinuity Design (RDD):**
When treatment assigned by threshold rule: compare just above vs just below cutoff.

---

### 27.6 Heterogeneous Treatment Effects & Uplift Modeling

**Goal:** Identify who benefits most from treatment.

**Causal Forest (Wager & Athey, 2018):**
```python
from econml.grf import CausalForest

cf = CausalForest(
    n_estimators=1000,
    min_samples_leaf=10,
    max_depth=5,
    random_state=42
)
cf.fit(X_train, T_train, y_train)
treatment_effects = cf.predict(X_test)  # Conditional ATE per individual
```

**T-Learner:**
```
Fit μ₀(x) = E[Y|X, T=0]   on control group
Fit μ₁(x) = E[Y|X, T=1]   on treated group
CATE(x) = μ₁(x) - μ₀(x)
```

**S-Learner (Single model):**
```
Fit μ(x, t) = E[Y|X, T]   on all data
CATE(x) = μ(x, 1) - μ(x, 0)
```

**X-Learner (best for imbalanced treatment):**
Cross-fit imputed treatment effects.

**Uplift modeling applications:**
- Marketing: Who responds to a promotion? (vs who buys anyway or who never buys)
- Healthcare: Who benefits from a drug? (personalized medicine)
- Churn prevention: Who would churn AND can be retained?

---

## 28. Federated Learning

### 28.1 What is Federated Learning?

Train ML models across many **decentralized devices** without centralizing data.

```
Traditional ML:
  Device 1 data ─┐
  Device 2 data ─┼─► Central Server → Train Model
  Device N data ─┘

Federated Learning:
  Device 1: local data → local model update Δ₁ ─┐
  Device 2: local data → local model update Δ₂ ─┼─► Server: Aggregate Δs → Global Model
  Device N: local data → local model update Δₙ ─┘
```

**Privacy motivation:** Medical data can't leave hospitals. Personal messages can't leave phones (iOS/Android predictive keyboard).

**Communication efficiency motivation:** Uploading raw data is expensive. Uploading model gradients is cheaper.

---

### 28.2 FedAvg (Federated Averaging)

**McMahan et al. (2017)** — the foundational FL algorithm:

```
Global round t:
  1. Server samples K clients from N total
  2. Server broadcasts current model w_t to selected clients
  3. Each client k runs E local SGD steps:
       w_k ← w_t - η ∇L_k(w_t)    [E steps]
  4. Clients send updated w_k^{t+1} to server
  5. Server aggregates:
       w_{t+1} = Σ_k (n_k/n) w_k^{t+1}    [weighted by dataset size]
```

```python
# Simplified FedAvg implementation
def federated_round(global_model, clients, fraction=0.1, local_epochs=5):
    n_clients = len(clients)
    selected = random.sample(clients, int(n_clients * fraction))
    
    local_updates = []
    total_samples = sum(c.n_samples for c in selected)
    
    for client in selected:
        # Client receives global model, trains locally
        local_model = copy.deepcopy(global_model)
        local_model = client.train(local_model, epochs=local_epochs)
        local_updates.append((local_model.state_dict(), client.n_samples))
    
    # Server aggregates (weighted FedAvg)
    global_state = global_model.state_dict()
    for key in global_state:
        global_state[key] = sum(
            state[key] * (n / total_samples)
            for state, n in local_updates
        )
    global_model.load_state_dict(global_state)
    return global_model
```

---

### 28.3 FL Challenges & Advanced Algorithms

**Statistical heterogeneity (non-IID data):**
Each client's data distribution differs. FedAvg can diverge ("client drift").

**FedProx:** Add proximal term to keep local model close to global:
```
L_k^{FedProx}(w) = L_k(w) + μ/2 ||w - w_t||²
```

**SCAFFOLD:** Variance reduction via control variates. Corrects for client drift:
```
Δw_k = w_k^{t+E} - w_t + correction_term
```

**System heterogeneity (stragglers):**
Slow/dropped clients delay training.
- **FedAsync:** Asynchronous aggregation — don't wait for all clients
- **Deadline:** Drop clients that don't respond in time

---

### 28.4 Differential Privacy in FL

Protect individual privacy by adding calibrated noise to model updates:

```
Gaussian mechanism:
  Δw_k ← Δw_k + N(0, σ²S²)

where S = sensitivity (norm clipping threshold): ||Δw_k||₂ ≤ S
      σ = noise scale (privacy parameter: smaller σ → less private)

Privacy guarantee: (ε, δ)-differential privacy
  ε = privacy budget (smaller = more private)
  δ = failure probability
```

```python
from opacus import PrivacyEngine

privacy_engine = PrivacyEngine()
model, optimizer, data_loader = privacy_engine.make_private_with_epsilon(
    module=model,
    optimizer=optimizer,
    data_loader=train_loader,
    epochs=10,
    target_epsilon=1.0,   # strong privacy guarantee
    target_delta=1e-5,
    max_grad_norm=1.0     # gradient clipping
)
```

**Privacy-utility tradeoff:** More privacy (smaller ε) → more noise → worse accuracy.
Typical: ε=1-8 for moderate privacy, ε=0.1 for strong privacy.

---

### 28.5 Secure Aggregation

Prevent server from seeing individual client updates (only aggregate):

**Pairwise masking protocol:**
```
Clients add/subtract random masks that cancel in aggregation:
  Client 1 sends: Δw₁ + r₁₂ - r₁₃
  Client 2 sends: Δw₂ - r₁₂ + r₂₃
  Client 3 sends: Δw₃ + r₁₃ - r₂₃
  Sum = Δw₁ + Δw₂ + Δw₃   (masks cancel!)
```

Server sees only the sum — never individual updates. Uses cryptographic secret sharing.

**Homomorphic Encryption:** Compute on encrypted data. Very slow but theoretically sound.

---

## 29. Continual Learning

### 29.1 Catastrophic Forgetting

**Problem:** When a neural network learns task B after task A, it forgets task A.

**Why:** Gradient descent on task B's loss modifies weights that were critical for task A.

```
Performance on Task A:
  After training task A: 95%
  After training task B: 40%  ← catastrophic forgetting
```

**Scenario types:**
- **Task-incremental:** Task identity known at test time
- **Domain-incremental:** Same task, different data distributions over time
- **Class-incremental:** New classes added over time (hardest)

---

### 29.2 Elastic Weight Consolidation (EWC)

**Kirkpatrick et al. (2017):** Protect important weights from changing.

**Key insight:** Fisher Information Matrix F measures how important each weight is for task A.

```
L(θ) = L_B(θ) + Σᵢ (λ/2) Fᵢᵢ (θᵢ - θ*_A,i)²

Fᵢᵢ = E[(∂ log P(D_A|θ) / ∂θᵢ)²]   [Fisher diagonal — importance of weight i]
θ*_A = optimal weights after training task A
λ = regularization strength
```

Intuition: Large Fᵢᵢ → weight critical for A → penalize changing it for B.

```python
class EWC_Trainer:
    def compute_fisher(self, model, dataloader):
        fisher = {n: torch.zeros_like(p) for n, p in model.named_parameters()}
        for x, y in dataloader:
            model.zero_grad()
            loss = F.cross_entropy(model(x), y)
            loss.backward()
            for n, p in model.named_parameters():
                fisher[n] += p.grad.pow(2) / len(dataloader)
        return fisher
    
    def ewc_loss(self, model, fisher, optimal_params, lambda_=1000):
        loss = 0
        for n, p in model.named_parameters():
            loss += (fisher[n] * (p - optimal_params[n]).pow(2)).sum()
        return lambda_ / 2 * loss
```

---

### 29.3 Experience Replay

**Episodic memory:** Keep a small buffer of past examples and replay them during new task training.

```
Buffer B = {(x,y)} from previous tasks
During task T training:
  L(θ) = L_T(θ; x_T, y_T) + L_replay(θ; B)
```

**GEM (Gradient Episodic Memory):**
Constrain gradients so that performance on past tasks never decreases:
```
min ||∇L_T||²  s.t.  ∇L_T · ∇L_k ≥ 0 for all past tasks k
```

**A-GEM (Average GEM):** Simpler — only constrain average gradient over replay buffer.

**Reservoir Sampling:** Maintain buffer of size M with uniform sample from all seen data:
```
For each new sample (x, y), i-th sample seen:
  With probability M/i: add to buffer (replacing random element)
  Otherwise: discard
```

**Dark Experience Replay (DER):** Replay logits (not labels) from past tasks — preserves richer knowledge.

---

### 29.4 Progressive Networks & Architecture Growth

**Progressive Networks:** Add new "columns" (sets of layers) for each task. Old columns frozen. New column receives lateral connections from all previous columns.

```
Task A column: [Layer 1A] → [Layer 2A] → [Layer 3A]
                              ↓lateral        ↓lateral
Task B column: [Layer 1B] → [Layer 2B] → [Layer 3B]
```

**PackNet:** Prune 50% of weights after each task, assign pruned weights to next task.

**DEN (Dynamically Expandable Network):** Selectively retrain, duplicate, and expand network.

**Parameter Isolation:** Each task gets a separate parameter mask — no sharing but no forgetting.

---

## 30. Meta-Learning

### 30.1 Learning to Learn

**Meta-learning:** Train models that can quickly adapt to new tasks with few examples.

**The setup:**
```
Meta-training: Many tasks T ~ p(T), each with:
  - Support set S = {(xᵢ, yᵢ)}ᵢ₌₁ᴷ    (few labeled examples)
  - Query set Q = {(xⱼ, yⱼ)}ⱼ          (evaluate generalization)

Meta-test: New task with support set only → predict on query set

N-way K-shot: N classes, K examples per class in support set
Common: 5-way 1-shot, 5-way 5-shot
```

---

### 30.2 MAML (Model-Agnostic Meta-Learning)

**Finn et al. (2017):** Find initialization θ that can quickly adapt to any task via few gradient steps.

```
Meta-objective:
min_θ Σ_{Tᵢ} L_Tᵢ(θ - α ∇_θ L_Tᵢ(θ))
           ──────────────────────────
           performance after ONE gradient step on task i

Algorithm:
  For each meta-iteration:
    1. Sample batch of tasks {T₁, ..., Tₙ}
    2. For each task Tᵢ:
         a. Sample support set Sᵢ
         b. Compute adapted params: θ'ᵢ = θ - α ∇_θ L_{Sᵢ}(θ)    [inner loop]
    3. Meta-update: θ ← θ - β ∇_θ Σᵢ L_{Qᵢ}(θ'ᵢ)               [outer loop]
```

**Key insight:** The outer loop backprops THROUGH the inner loop (second-order gradients). Computationally expensive.

**FOMAML:** First-order MAML — drop second-order terms. Surprisingly good, much cheaper.

**Reptile (OpenAI):** Even simpler — just move toward task-specific optima:
```
θ ← θ + ε (θ'ᵢ - θ)   [move θ toward adapted parameters]
```

---

### 30.3 Metric-Based Meta-Learning

**Learn a metric space** where classification is done by proximity to support examples.

#### Prototypical Networks
```python
# Prototype = mean embedding of support examples per class
class ProtoNet(nn.Module):
    def __init__(self, encoder):
        super().__init__()
        self.encoder = encoder
    
    def forward(self, support, support_labels, query):
        # Encode all
        support_emb = self.encoder(support)   # [N*K, d]
        query_emb = self.encoder(query)       # [Q, d]
        
        # Compute prototypes (class centroids)
        classes = support_labels.unique()
        prototypes = torch.stack([
            support_emb[support_labels==c].mean(0) for c in classes
        ])   # [N, d]
        
        # Distance to prototypes (Euclidean)
        dists = torch.cdist(query_emb, prototypes)   # [Q, N]
        return -dists   # higher = more similar
```

#### Matching Networks
```
Attention-weighted sum of support labels:
P(y|x, S) = Σ_{(xᵢ,yᵢ)∈S} a(x, xᵢ) yᵢ

Attention: a(x, xᵢ) = softmax(cos_sim(f(x), g(xᵢ)))
```

#### Relation Network
```
Instead of fixed metric (cosine/Euclidean), LEARN the similarity function:
  similarity(x, xᵢ) = NN(concat(f(x), g(xᵢ)))   [trainable comparator]
```

---

### 30.4 In-Context Learning as Meta-Learning

**GPT-class models perform meta-learning during pretraining:**
- Pretraining corpus contains many implicit tasks
- Model learns to "read" examples and infer the pattern → few-shot learning
- No weight updates at inference time!

**Connection:** MAML finds weights that adapt quickly; GPT-3 finds weights that encode adaptation in-context.

---

## 31. Knowledge Distillation

### 31.1 The Core Idea

**Knowledge Distillation (KD)** transfers learned knowledge from a large, accurate **teacher** model into a smaller, faster **student** model.

**Why not just train the student directly?**
The teacher's **soft probability outputs** (soft targets) carry much richer information than hard one-hot labels.

```
Hard label for "cat" image:
  cat=1, dog=0, car=0, truck=0, ...

Teacher soft output (temperature T=4):
  cat=0.72, lynx=0.15, dog=0.09, tiger=0.04, ...
           ↑
     These inter-class similarities encode
     the teacher's learned representation
```

The probability that a cat looks like a lynx (0.15) tells the student far more than the binary label ever could.

---

### 31.2 Hinton's Original KD (2015)

**Objective:** Distill knowledge via soft targets.

```
Student Loss = α × L_hard + (1-α) × L_soft

L_hard = CrossEntropy(student_logits, hard_labels)
L_soft = T² × KL(softmax(teacher_logits/T) || softmax(student_logits/T))

T = temperature (hyperparameter, typically 3–7)
α = weighting (typically 0.1–0.5 toward soft)
```

**Temperature scaling:**
```
Softmax with T: p_i = exp(z_i/T) / Σⱼ exp(zⱼ/T)

T=1: standard softmax (sharp)
T>1: softer distribution → amplifies small probabilities → richer signal
T→∞: uniform distribution (no signal)
```

The T² factor in L_soft compensates for the magnitude reduction from dividing logits by T.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

def distillation_loss(student_logits, teacher_logits, labels,
                      temperature=4.0, alpha=0.3):
    # Soft targets loss (KL divergence)
    soft_teacher = F.softmax(teacher_logits / temperature, dim=-1)
    soft_student = F.log_softmax(student_logits / temperature, dim=-1)
    loss_soft = F.kl_div(soft_student, soft_teacher, reduction='batchmean') * (temperature ** 2)

    # Hard targets loss
    loss_hard = F.cross_entropy(student_logits, labels)

    return alpha * loss_hard + (1 - alpha) * loss_soft


# Training loop
teacher.eval()  # Teacher frozen
for X, y in dataloader:
    with torch.no_grad():
        teacher_logits = teacher(X)     # No gradient for teacher

    student_logits = student(X)
    loss = distillation_loss(student_logits, teacher_logits, y)
    loss.backward()
    optimizer.step()
```

---

### 31.3 Feature-Based Distillation

Beyond matching final outputs — match **intermediate representations**.

**FitNets (2015):** Match intermediate feature maps (hints):
```
L_hint = ||W_r · f_student(x) - f_teacher(x)||²

W_r = learned projection (student may have different dim than teacher)
f = feature maps at chosen intermediate layer
```

**AT (Attention Transfer):** Match attention maps (sum of squared feature maps):
```
Attention map: F(A) = ||A||²_F normalized per spatial position
L_AT = Σ_l ||F(A_l^teacher) - F(A_l^student)||²
```

**PKT (Probabilistic Knowledge Transfer):** Match kernel matrices of features.

**CRD (Contrastive Representation Distillation, 2020):**
Uses contrastive learning — student representation should be similar to teacher's for same sample, dissimilar for different samples.
```
L_CRD = -log(exp(s·t/τ) / (exp(s·t/τ) + Σ_neg exp(s·t_neg/τ)))
```

---

### 31.4 Relation-Based Distillation

Transfer structural relationships between data points:

**RKD (Relational Knowledge Distillation, 2019):**
```
Distance-wise: l_D = Σ_{(x,y)∈T²} l_δ(ψ_D(t_x,t_y), ψ_D(s_x,s_y))
Angle-wise:    l_A = Σ_{(x,y,z)∈T³} l_δ(ψ_A(t_x,t_y,t_z), ψ_A(s_x,s_y,s_z))

ψ_D(x,y) = ||f(x)-f(y)||/μ         [pairwise distance]
ψ_A(x,y,z) = cos∠xyz                [triplet angle]
```

---

### 31.5 Self-Distillation

No separate teacher — the model teaches itself!

**Born-Again Networks (BAN):**
Train network → train identical network to imitate first → repeat.
Ensemble of "generations" consistently outperforms single model.

**Self-Distillation within one model:**
- Deeper layers supervise shallower layers
- Final predictions supervise intermediate classifier heads
- Used in **BE (Bag of Experts)** and **BYOT (Be Your Own Teacher)**

---

### 31.6 DistilBERT, TinyBERT, MobileBERT

**DistilBERT (Hugging Face, 2019):**
- 40% fewer parameters than BERT-base (66M vs 110M)
- 60% faster, 97% of BERT performance on GLUE
- Distillation during pre-training (not just fine-tuning)
- Uses cosine embedding loss + MLM loss + soft label loss

```python
from transformers import DistilBertForSequenceClassification

# Drop-in replacement for BERT-base — same API, much faster
model = DistilBertForSequenceClassification.from_pretrained('distilbert-base-uncased', num_labels=2)
```

**TinyBERT (Huawei, 2020):**
Two-stage distillation:
1. **General distillation:** Distill BERT in pre-training (attention matrices + hidden states)
2. **Task-specific distillation:** Distill fine-tuned BERT on downstream task

4-layer TinyBERT achieves 96.8% of BERT-base on GLUE.

**MobileBERT (Google, 2020):**
Bottleneck structure (128→512→128) enables depth without width.
Fine-grained distillation: Layer-to-layer matching with inverted bottleneck teacher.

**Patient KD:** Distill from multiple teacher layers simultaneously (prevents skipping useful intermediate knowledge).

---

### 31.7 Data-Free Distillation

When training data is unavailable (privacy, proprietary):

**DFAD (Data-Free Adversarial Distillation):**
```
Generator G creates synthetic data to maximize student-teacher disagreement:
  min_S max_G || f_T(G(z)) - f_S(G(z)) ||
```

**DeGAN:** Generator conditioned on class to produce diverse per-class samples.

**Fast-DFKD:** Batch normalization statistics from teacher layers used to generate matching synthetic data.

---

## 32. TinyML & Edge AI

### 32.1 The Edge Computing Paradigm

**The problem with cloud-only AI:**
```
IoT Device → (raw data) → Cloud → (prediction) → Device
  Latency: 50–200ms, bandwidth: high, privacy: risky, offline: impossible
```

**Edge AI:**
```
IoT Device → (local inference) → instant response
  Latency: <5ms, bandwidth: minimal, privacy: preserved, offline: works
```

**Target hardware:**
```
MCUs (microcontrollers): ARM Cortex-M, ESP32       → <1MB RAM, mW power
Mobile SoCs:             Apple A-series, Snapdragon → 4–16GB RAM, ~3W
Edge TPUs:               Google Coral, Intel NCS2   → dedicated ML accelerators
Edge GPUs:               NVIDIA Jetson family        → 5–60W, laptop-class ML
```

---

### 32.2 Model Compression Pipeline

```
Full-precision model (e.g., ResNet-50, 98MB FP32)
         ↓
   Pruning (remove redundant weights/neurons)
         ↓
   Quantization (reduce numerical precision)
         ↓
   Knowledge Distillation (smaller architecture)
         ↓
   Operator Fusion (combine sequential ops)
         ↓
   Deployment format (TFLite / ONNX / TensorRT)
         ↓
Edge-optimized model (e.g., MobileNetV3, 2MB INT8)
```

---

### 32.3 Quantization-Aware Training (QAT)

Post-training quantization (PTQ) converts weights after training. QAT **simulates quantization during training** so the model learns to be robust to it.

**Fake quantization:**
```python
import torch
from torch.quantization import QuantStub, DeQuantStub, prepare_qat, convert

class QATModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.quant = QuantStub()       # marks start of quantized region
        self.conv = nn.Conv2d(3, 64, 3)
        self.relu = nn.ReLU()
        self.linear = nn.Linear(64*6*6, 10)
        self.dequant = DeQuantStub()   # marks end of quantized region
    
    def forward(self, x):
        x = self.quant(x)
        x = self.relu(self.conv(x))
        x = x.flatten(1)
        x = self.linear(x)
        return self.dequant(x)

model = QATModel()
model.qconfig = torch.quantization.get_default_qat_qconfig('qnnpack')

# Prepare: insert fake-quantization nodes
model_prepared = prepare_qat(model)

# Train with fake quantization (model sees quantization noise)
for epoch in range(fine_tune_epochs):
    train_one_epoch(model_prepared, dataloader)

# Convert to actually quantized model
model_quantized = convert(model_prepared.eval())
# model_quantized uses INT8 ops natively
```

**QAT vs PTQ:**
| | PTQ | QAT |
|---|---|---|
| Calibration data needed | Yes (small) | No (full training) |
| Training time | Minutes | Hours |
| Accuracy (INT8) | ~1-2% drop | ~0.1-0.5% drop |
| Accuracy (INT4) | ~5-10% drop | ~1-3% drop |
| Recommended for | Rapid deployment | Production INT8/INT4 |

---

### 32.4 Pruning

Remove redundant parameters to reduce model size and FLOPs.

**Magnitude-based unstructured pruning:**
```python
import torch.nn.utils.prune as prune

# Prune 30% of weights with smallest absolute values in conv layer
prune.l1_unstructured(model.conv1, name='weight', amount=0.3)

# Global pruning across all layers
parameters_to_prune = [(module, 'weight') for module in model.modules()
                        if isinstance(module, (nn.Conv2d, nn.Linear))]
prune.global_unstructured(parameters_to_prune,
                           pruning_method=prune.L1Unstructured,
                           amount=0.4)

# Make pruning permanent (remove masks)
for module, name in parameters_to_prune:
    prune.remove(module, name)
```

**Structured pruning (hardware-friendly):**
- **Filter/channel pruning:** Remove entire convolution filters
- Result: Dense smaller model (vs sparse large model from unstructured)
- Compatible with standard hardware acceleration

```python
# Identify and remove low-importance filters
importance = model.conv1.weight.data.abs().mean(dim=(1,2,3))
threshold = torch.quantile(importance, 0.3)
keep_filters = importance > threshold

# Rebuild conv with fewer filters
new_conv = nn.Conv2d(model.conv1.in_channels, keep_filters.sum(), 3, padding=1)
new_conv.weight.data = model.conv1.weight.data[keep_filters]
model.conv1 = new_conv
```

**Lottery Ticket Hypothesis (Frankle & Carlin, 2019):**
Dense networks contain sparse subnetworks ("winning tickets") that, when trained in isolation from scratch with their original initialization, match full network performance.

---

### 32.5 TensorFlow Lite (TFLite)

Google's ML framework for on-device inference:

```python
import tensorflow as tf

# Convert saved model to TFLite
converter = tf.lite.TFLiteConverter.from_saved_model('saved_model_dir')

# INT8 quantization
converter.optimizations = [tf.lite.Optimize.DEFAULT]
converter.representative_dataset = representative_data_gen  # calibration data

tflite_model = converter.convert()
with open('model.tflite', 'wb') as f:
    f.write(tflite_model)

# Inference on device
interpreter = tf.lite.Interpreter(model_path='model.tflite')
interpreter.allocate_tensors()

input_details  = interpreter.get_input_details()
output_details = interpreter.get_output_details()

interpreter.set_tensor(input_details[0]['index'], input_data)
interpreter.invoke()
output = interpreter.get_tensor(output_details[0]['index'])
```

**TFLite delegates (hardware acceleration):**
```python
# GPU delegate (Android/iOS)
gpu_delegate = tf.lite.experimental.load_delegate('libdelegate.so')
interpreter = tf.lite.Interpreter(model_path='model.tflite',
                                   experimental_delegates=[gpu_delegate])

# NNAPI delegate (Android Neural Networks API)
nnapi_delegate = tf.lite.experimental.load_delegate('libNnApiDelegate.so')

# Coral Edge TPU delegate (Google Coral USB/Dev Board)
edgetpu_delegate = tf.lite.experimental.load_delegate('libedgetpu.so.1')
```

**TFLite Micro (MCUs):**
```cpp
// Arduino / Cortex-M deployment (C++)
#include "tensorflow/lite/micro/all_ops_resolver.h"
#include "tensorflow/lite/micro/micro_interpreter.h"

const tflite::Model* model = tflite::GetModel(g_model_data);  // model as C array
tflite::AllOpsResolver resolver;
static uint8_t tensor_arena[10 * 1024];  // 10KB RAM arena

tflite::MicroInterpreter interpreter(model, resolver, tensor_arena,
                                      sizeof(tensor_arena));
interpreter.AllocateTensors();

TfLiteTensor* input  = interpreter.input(0);
TfLiteTensor* output = interpreter.output(0);
input->data.f[0] = sensor_reading;
interpreter.Invoke();
float result = output->data.f[0];
```

---

### 32.6 ONNX & ONNX Runtime

**ONNX (Open Neural Network Exchange):** Intermediate representation for ML models — train in any framework, deploy anywhere.

```python
import torch.onnx

# Export PyTorch → ONNX
dummy_input = torch.randn(1, 3, 224, 224)
torch.onnx.export(
    model, dummy_input, 'model.onnx',
    input_names=['image'],
    output_names=['logits'],
    dynamic_axes={'image': {0: 'batch_size'}},  # variable batch size
    opset_version=17
)

# Optimize ONNX graph
from onnxruntime.transformers import optimizer
opt_model = optimizer.optimize_model('model.onnx', model_type='bert')
opt_model.save_model_to_file('model_opt.onnx')

# Run with ONNX Runtime
import onnxruntime as ort

sess_options = ort.SessionOptions()
sess_options.graph_optimization_level = ort.GraphOptimizationLevel.ORT_ENABLE_ALL
sess = ort.InferenceSession('model_opt.onnx',
                             sess_options,
                             providers=['CUDAExecutionProvider',
                                        'CPUExecutionProvider'])
output = sess.run(None, {'image': input_array})
```

**ONNX Runtime providers (hardware backends):**
```
CUDAExecutionProvider          → NVIDIA GPU
TensorrtExecutionProvider      → NVIDIA TensorRT (optimized)
ROCmExecutionProvider          → AMD GPU
OpenVINOExecutionProvider      → Intel CPU/GPU/VPU
CoreMLExecutionProvider        → Apple Silicon / iOS
QNNExecutionProvider           → Qualcomm NPU
CPUExecutionProvider           → Any CPU (fallback)
```

---

### 32.7 NVIDIA TensorRT

Production-grade inference optimizer for NVIDIA GPUs:

```python
import tensorrt as trt
import numpy as np

TRT_LOGGER = trt.Logger(trt.Logger.WARNING)

# Build engine from ONNX
with trt.Builder(TRT_LOGGER) as builder, \
     builder.create_network(1 << int(trt.NetworkDefinitionCreationFlag.EXPLICIT_BATCH)) as network, \
     trt.OnnxParser(network, TRT_LOGGER) as parser:

    config = builder.create_builder_config()
    config.max_workspace_size = 1 << 30   # 1GB workspace

    # INT8 mode
    config.set_flag(trt.BuilderFlag.INT8)
    config.int8_calibrator = MyCalibrator(calibration_data)

    with open('model.onnx', 'rb') as f:
        parser.parse(f.read())

    engine = builder.build_engine(network, config)

# Serialize engine to disk
with open('model.trt', 'wb') as f:
    f.write(engine.serialize())
```

**TensorRT optimizations applied automatically:**
- Layer fusion (Conv+BN+ReLU → single kernel)
- Precision calibration (FP32→INT8 with minimal accuracy loss)
- Kernel auto-tuning (selects fastest CUDA kernel per op)
- Tensor memory reuse
- Dynamic shape support

**Typical speedup:** 2–8× over PyTorch on same GPU.

---

### 32.8 NVIDIA Jetson Family

```
Jetson Nano:      4GB RAM,  472 GFLOPS,  5–10W  → entry-level robotics
Jetson TX2:       8GB RAM,  1.3 TFLOPS,  7–15W  → drones, embedded CV
Jetson Xavier NX: 16GB RAM, 21 TOPS,     10–20W → advanced robotics
Jetson Orin NX:   16GB RAM, 100 TOPS,    10–25W → autonomous vehicles
Jetson Orin AGX:  64GB RAM, 275 TOPS,    15–60W → robotics AI workstation
```

**Typical Jetson deployment pipeline:**
```
Train on cloud/workstation GPU
         ↓
Export to ONNX or TorchScript
         ↓
Convert with TensorRT on Jetson (or cross-compile)
         ↓
Deploy with DeepStream SDK (NVIDIA video analytics) or custom Python
         ↓
Run at camera frame rate (30–60 FPS) with real-time inference
```

---

### 32.9 Google Coral Edge TPU

Specialized ASIC for running TFLite models:

```
Performance: 4 TOPS (INT8 only)
Power:       ~2W USB stick, 0.5W on-device M.2/PCIe
Models:      Must be fully INT8 quantized for TPU offload
Fallback:    Unsupported ops run on CPU

Supported architectures: MobileNet, EfficientNet, YOLO (custom TPU-friendly variants)
Not supported: Transposed convolutions, non-standard ops
```

```python
import pycoral.utils.edgetpu as edgetpu
from pycoral.adapters import common, classify

interpreter = edgetpu.make_interpreter('model_edgetpu.tflite')
interpreter.allocate_tensors()

common.set_input(interpreter, image)
interpreter.invoke()
classes = classify.get_classes(interpreter, top_k=3)
```

**Coral model compilation:**
```bash
# Compile TFLite INT8 model for Edge TPU
edgetpu_compiler model_quant.tflite
# Output: model_quant_edgetpu.tflite + profiling report
# Report shows: X% ops on TPU, Y% on CPU
```

---

### 32.10 MCU Deployment (ARM Cortex-M)

**Target:** Cortex-M4/M7/M33/M55 — 256KB–2MB RAM, no OS, battery-powered.

**Frameworks:**
- **TFLite Micro** (Google) — most mature
- **Edge Impulse** — end-to-end cloud-to-MCU platform
- **STM32Cube.AI** (ST) — direct integration with STM32 toolchain
- **microTVM** (Apache TVM) — compiler for ML on MCUs

**Key constraints:**
```
Memory:         Model + activations must fit in SRAM (often <512KB)
Compute:        No FPU on M0/M0+, limited SIMD on M4/M7
Power budget:   Often <5mW for continuous inference
Latency:        <100ms for most applications
Operators:      No dynamic allocation, no recursion
```

**Model design for MCUs:**
```python
# Use tf.lite.Optimize.OPTIMIZE_FOR_LATENCY or SIZE
converter.optimizations = [tf.lite.Optimize.DEFAULT]
converter.target_spec.supported_ops = [tf.lite.OpsSet.TFLITE_BUILTINS_INT8]
converter.inference_input_type = tf.int8
converter.inference_output_type = tf.int8

# Target architecture: DS-CNN, MobileNetV1 (very small), custom 1D-CNN
```

---

## 33. AutoML & Hyperparameter Optimization

### 33.1 The AutoML Landscape

**AutoML** automates the ML pipeline — feature engineering, model selection, and hyperparameter tuning.

```
Full AutoML pipeline:
  Raw data
     ↓ Data Cleaning & Preprocessing (AutoFE)
     ↓ Feature Engineering
     ↓ Algorithm Selection (NAS / model selection)
     ↓ Hyperparameter Optimization (HPO)
     ↓ Ensemble Construction
     ↓ Evaluation
  Final model
```

---

### 33.2 Hyperparameter Optimization (HPO)

#### Grid Search & Random Search

```python
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV
from scipy.stats import loguniform, randint

param_grid = {
    'n_estimators': [50, 100, 200, 500],
    'max_depth': [3, 5, 7, None],
    'learning_rate': [0.01, 0.05, 0.1]
}

# Grid: exhaustive — 4×4×3 = 48 combinations
grid_search = GridSearchCV(xgb_model, param_grid, cv=5, scoring='roc_auc', n_jobs=-1)

# Random: sample n_iter combinations — much faster for large spaces
param_dist = {
    'n_estimators': randint(50, 500),
    'max_depth': randint(2, 10),
    'learning_rate': loguniform(1e-3, 0.3)
}
random_search = RandomizedSearchCV(xgb_model, param_dist, n_iter=50, cv=5)
```

#### Bayesian Optimization (the right approach)

Model the **objective function** as a Gaussian Process. Use the model to decide where to evaluate next.

```
Acquisition function decides exploration/exploitation tradeoff:
  Expected Improvement (EI): E[max(0, f(x) - f_best)]
  Upper Confidence Bound (UCB): μ(x) + κσ(x)
  Probability of Improvement (PI): P(f(x) > f_best + ξ)
```

#### Optuna — State-of-the-art HPO

```python
import optuna

def objective(trial):
    # Define search space
    n_estimators = trial.suggest_int('n_estimators', 50, 500)
    max_depth = trial.suggest_int('max_depth', 2, 10)
    lr = trial.suggest_float('learning_rate', 1e-3, 0.3, log=True)
    subsample = trial.suggest_float('subsample', 0.5, 1.0)
    reg_alpha = trial.suggest_float('reg_alpha', 1e-8, 1.0, log=True)

    model = xgb.XGBClassifier(
        n_estimators=n_estimators,
        max_depth=max_depth,
        learning_rate=lr,
        subsample=subsample,
        reg_alpha=reg_alpha,
        use_label_encoder=False
    )
    # 3-fold CV
    score = cross_val_score(model, X_train, y_train, cv=3, scoring='roc_auc').mean()
    return score

# Optuna uses TPE (Tree-structured Parzen Estimator) by default
study = optuna.create_study(direction='maximize',
                             sampler=optuna.samplers.TPESampler(seed=42),
                             pruner=optuna.pruners.MedianPruner())
study.optimize(objective, n_trials=100, n_jobs=4)

best_params = study.best_params
print(f"Best AUC: {study.best_value:.4f}")

# Visualize optimization history
optuna.visualization.plot_optimization_history(study)
optuna.visualization.plot_param_importances(study)
optuna.visualization.plot_parallel_coordinate(study)
```

**Optuna Pruning:** Stop unpromising trials early (like early stopping for HPO):
```python
# In objective function, report intermediate values:
for step in range(100):
    model.partial_fit(X_train, y_train)
    val_score = model.score(X_val, y_val)
    trial.report(val_score, step)
    if trial.should_prune():
        raise optuna.TrialPruned()   # Stop this trial early!
```

#### Ray Tune — Distributed HPO

```python
from ray import tune
from ray.tune.schedulers import ASHAScheduler

def train_fn(config):
    model = build_model(config)
    for epoch in range(config['epochs']):
        train_loss = train(model)
        val_acc = validate(model)
        tune.report(loss=train_loss, accuracy=val_acc)

config = {
    "lr": tune.loguniform(1e-4, 1e-1),
    "batch_size": tune.choice([16, 32, 64, 128]),
    "hidden": tune.randint(64, 512),
    "dropout": tune.uniform(0.1, 0.5)
}

scheduler = ASHAScheduler(metric="accuracy", mode="max",
                           max_t=100, grace_period=10, reduction_factor=2)

result = tune.run(
    train_fn,
    config=config,
    num_samples=50,
    scheduler=scheduler,
    resources_per_trial={"cpu": 2, "gpu": 0.5}   # fractional GPU!
)
```

---

### 33.3 AutoML Frameworks

#### Auto-sklearn

Bayesian optimization over a pipeline of sklearn estimators + hyperparameters:

```python
import autosklearn.classification

automl = autosklearn.classification.AutoSklearnClassifier(
    time_left_for_this_task=3600,    # 1 hour total
    per_run_time_limit=300,          # 5 min per model
    n_jobs=4,
    ensemble_size=50,
    metric=autosklearn.metrics.roc_auc
)
automl.fit(X_train, y_train)

# Inspect winning pipeline
print(automl.leaderboard())
print(automl.show_models())
predictions = automl.predict_proba(X_test)
```

#### AutoGluon

Best-in-class for tabular data — no configuration needed:

```python
from autogluon.tabular import TabularDataset, TabularPredictor

train_data = TabularDataset('train.csv')

predictor = TabularPredictor(
    label='target',
    eval_metric='roc_auc',
    path='autogluon_model'
).fit(
    train_data,
    time_limit=3600,               # 1 hour
    presets='best_quality',        # or 'medium_quality', 'high_quality'
    num_bag_folds=5,               # bagging for better estimates
    num_stack_levels=2             # stacking ensembles
)

# Automatic: feature engineering, model selection, HPO, ensembling, stacking
predictor.leaderboard()
predictions = predictor.predict(test_data)
feature_importance = predictor.feature_importance(test_data)
```

AutoGluon automatically builds and stacks: LightGBM, XGBoost, CatBoost, Random Forest, Extra Trees, k-NN, Neural Networks (TabTransformer, FastAI).

#### TPOT (Tree-based Pipeline Optimization Tool)

Uses **genetic programming** to evolve ML pipelines:

```python
from tpot import TPOTClassifier

tpot = TPOTClassifier(
    generations=10,        # GA generations
    population_size=50,    # pipelines per generation
    cv=5,
    scoring='accuracy',
    random_state=42,
    verbosity=2,
    n_jobs=-1
)
tpot.fit(X_train, y_train)
print(tpot.score(X_test, y_test))

# Export best pipeline as Python code!
tpot.export('best_pipeline.py')
```

---

### 33.4 Neural Architecture Search (NAS)

Automatically design neural network architectures.

**Search strategies:**
- **Random search:** Surprisingly competitive baseline
- **Evolutionary algorithms:** Mutate and select architectures
- **Reinforcement learning:** Policy generates architecture, reward = validation accuracy (NASNet, 2017)
- **Differentiable NAS (DARTS):** Relax discrete choices to continuous — optimize jointly with weights via gradient descent

**DARTS (2019):**
```
Mixed operation: ō^(i,j) = Σ_op exp(α_op) / Σ_op' exp(α_op') × op(x)

Learn architecture parameters α alongside model weights w
Architecture = argmax_op α_op per edge after training
```

**One-shot NAS / Weight Sharing:**
Train one "supernet" that contains all candidate architectures as subgraphs.
Each subnet shares weights with supernet → evaluate subnets without training from scratch.
Examples: SinglePath-NAS, OFA (Once-for-All), BigNAS.

**EfficientNet** and **MobileNetV3** were designed using NAS.

---

### 33.5 Ensemble Methods: Stacking & Blending

#### Stacking (Stacked Generalization)

Train a **meta-learner** on the predictions of base learners:

```
Level 0 (base learners, train on folds):
  Model A (LightGBM)   → ŷ_A
  Model B (XGBoost)    → ŷ_B
  Model C (Neural Net) → ŷ_C

Level 1 (meta-learner, train on ŷ_A, ŷ_B, ŷ_C):
  Logistic Regression  → final ŷ
```

```python
from sklearn.ensemble import StackingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.svm import SVC

estimators = [
    ('rf',  RandomForestClassifier(n_estimators=200)),
    ('gbm', GradientBoostingClassifier(n_estimators=200)),
    ('svm', SVC(probability=True))
]

stack = StackingClassifier(
    estimators=estimators,
    final_estimator=LogisticRegression(),
    cv=5,               # OOF predictions for training meta-learner
    passthrough=False   # pass original features to meta-learner too? 
)
stack.fit(X_train, y_train)
```

**Kaggle insight:** Stacking typically gains 0.001–0.01 AUC over best single model. The meta-learner learns which base model to trust in which region.

#### Blending

Similar to stacking but uses a holdout set (not cross-validation) to generate meta-features. Simpler, slightly worse, faster.

```python
# Blend: simple weighted average
final_pred = 0.4 * lgbm_pred + 0.35 * xgb_pred + 0.25 * nn_pred
```

Optimal weights found via Nelder-Mead or grid search on validation set.

---

### 33.6 Calibration

A model is **calibrated** if predicted probabilities match actual frequencies:
- "I predict 70% probability" → should be correct 70% of the time
- Tree models / SVMs / neural networks are often miscalibrated

**Reliability diagram (calibration curve):**
```python
from sklearn.calibration import calibration_curve, CalibratedClassifierCV
import matplotlib.pyplot as plt

frac_pos, mean_pred_value = calibration_curve(y_test, probs, n_bins=10)
plt.plot(mean_pred_value, frac_pos, marker='o', label='Uncalibrated')
plt.plot([0,1],[0,1], 'k--', label='Perfect')
```

**Platt Scaling:**
```python
# Fit sigmoid on validation set predictions
from sklearn.calibration import CalibratedClassifierCV

calibrated = CalibratedClassifierCV(base_clf, method='sigmoid', cv=5)
calibrated.fit(X_train, y_train)
calibrated_probs = calibrated.predict_proba(X_test)
```

**Isotonic Regression (non-parametric, more flexible):**
```python
calibrated = CalibratedClassifierCV(base_clf, method='isotonic', cv=5)
```

**Temperature Scaling (for neural networks):**
```
P_calibrated(y|x) = softmax(z / T)   where T learned on validation set
```
Single scalar T — simple, very effective for neural networks.

---

## 34. Advanced ML Topics

### 34.1 Contrastive Learning

**Self-supervised learning by comparing similar and dissimilar samples** — learn representations without labels.

**Core idea:**
```
Pull together: representations of two augmented views of same image (positive pair)
Push apart:    representations of different images (negative pairs)
```

#### SimCLR (2020, Google)

```
For each image x:
  1. Generate two augmented views: x̃₁, x̃₂ (crop + color jitter + blur)
  2. Encode: z₁ = g(f(x̃₁)), z₂ = g(f(x̃₂))
     f = encoder (ResNet), g = projection head (MLP)
  3. NT-Xent (Normalized Temperature-scaled Cross-Entropy) loss:

  l(i,j) = -log [exp(sim(zᵢ,zⱼ)/τ) / Σ_{k≠i} exp(sim(zᵢ,zₖ)/τ)]

Requires large batch size (4096+) for enough negatives
```

```python
# SimCLR projection head + loss (simplified)
class SimCLR(nn.Module):
    def __init__(self, encoder, projection_dim=128):
        super().__init__()
        self.encoder = encoder
        dim = encoder.output_dim
        self.projector = nn.Sequential(
            nn.Linear(dim, dim), nn.ReLU(), nn.Linear(dim, projection_dim)
        )
    
    def forward(self, x1, x2):
        h1, h2 = self.encoder(x1), self.encoder(x2)
        z1, z2 = self.projector(h1), self.projector(h2)
        return z1, z2

def nt_xent_loss(z1, z2, temperature=0.5):
    B = z1.size(0)
    z = F.normalize(torch.cat([z1, z2], dim=0), dim=1)  # [2B, d]
    sim = torch.mm(z, z.T) / temperature                  # [2B, 2B]
    
    # Positive pairs: (i, i+B) and (i+B, i)
    labels = torch.cat([torch.arange(B, 2*B), torch.arange(B)]).to(z.device)
    mask = torch.eye(2*B, dtype=bool).to(z.device)
    sim.masked_fill_(mask, float('-inf'))   # exclude self
    
    return F.cross_entropy(sim, labels)
```

#### MoCo (Momentum Contrast, 2020, Facebook)

Uses a **momentum encoder** + **queue** of negatives — decouples batch size from number of negatives:

```
Encoder q: updated by backprop
Momentum encoder k: θ_k ← m·θ_k + (1-m)·θ_q  (m=0.999)

Queue: FIFO buffer of encoded keys from past mini-batches (65536 keys)

Loss: contrastive over queue negatives
```

**Advantage:** Consistent negative embeddings (from slowly-updated momentum encoder), large effective batch size without actual large GPU batch.

#### BYOL (Bootstrap Your Own Latent, 2020, DeepMind)

**No negatives needed!** Prevents collapse via asymmetric architecture + stop-gradient:

```
Online network:   f_θ → g_θ → q_θ   (predictor on top of projector)
Target network:   f_ξ → g_ξ         (no predictor, slow-moving average of online)

Loss: MSE between online prediction and target projection
θ: updated by gradient
ξ: θ_ξ ← τ·θ_ξ + (1-τ)·θ   (EMA, τ=0.996)
```

```python
def byol_loss(online_pred, target_proj):
    # Normalize then MSE
    online_pred = F.normalize(online_pred, dim=-1)
    target_proj = F.normalize(target_proj.detach(), dim=-1)  # stop gradient!
    return 2 - 2 * (online_pred * target_proj).sum(dim=-1).mean()
```

#### DINO (Self-DIstillation with NO labels, 2021, Meta)

Applies self-distillation to Vision Transformers using:
- Multi-crop strategy (global + local views)
- Centering + sharpening to prevent collapse
- EMA teacher

ViT features from DINO are extremely useful for downstream tasks (better than supervised).

**DINOv2 (2023):** Curated high-quality dataset, self-supervised ViT-g. State-of-the-art visual features — used widely in robotics, medical imaging.

---

### 34.2 Domain Adaptation & Generalization

**Problem:** Model trained on source domain fails on target domain (distribution shift).

```
Domain shift examples:
  Lab → Hospital (lighting, equipment differences)
  Day → Night driving
  English → other language text
  Synthetic → Real images (sim-to-real)
```

#### Domain Adaptation (access to unlabeled target data)

**DANN (Domain-Adversarial Neural Networks):**
```
Feature extractor G_f: learns domain-invariant features
Label predictor G_y:   predicts class labels (source supervised)
Domain classifier G_d: predicts domain (source vs target)

Loss: L_y(G_y(G_f(x_s)), y_s) - λ L_d(G_d(G_f(x)), d)
      ─────────────────────────  ───────────────────────
           supervised loss          ADVERSARIAL domain loss
                                    (flip gradients via GRL)

Gradient Reversal Layer (GRL): Multiplies gradient by -λ during backprop
→ G_f maximizes domain classification loss (makes features indistinguishable)
```

**MMD (Maximum Mean Discrepancy):** Align feature distributions directly:
```
MMD²(Xs, Xt) = ||μ_s - μ_t||²_H   (in RKHS)
Loss = L_task + λ MMD²(G_f(Xs), G_f(Xt))
```

**CORAL (Correlation Alignment):** Match second-order statistics (covariances) across domains.

#### Domain Generalization (no target data)

**Goal:** Generalize to unseen domains using multiple source domains.

**IRM (Invariant Risk Minimization, 2020):**
Learn features that are predictive AND invariant across all environments:
```
min_{Φ,w} Σ_e R_e(w∘Φ)   s.t.  w = argmin_w̃ R_e(w̃∘Φ) for all e
```
Find representation Φ where the same classifier w is optimal across all environments.

**Mixup-based methods:** Interpolate between domains during training.

**SWAD (Stochastic Weight Averaging Densely, 2021):** Average weights along the flat loss valley — generalizes better.

---

### 34.3 Active Learning

**Problem:** Labeling is expensive. Which samples should we label next?

**Query strategies:**

```python
from modAL.models import ActiveLearner
from modAL.uncertainty import uncertainty_sampling, entropy_sampling, margin_sampling

# Pool-based active learning
learner = ActiveLearner(
    estimator=RandomForestClassifier(n_estimators=100),
    query_strategy=uncertainty_sampling,  # pick most uncertain sample
    X_training=X_initial, y_training=y_initial
)

# Active learning loop
for i in range(n_queries):
    query_idx, query_inst = learner.query(X_pool)  # find most uncertain
    
    # Human labels the selected sample
    y_new = oracle_label(query_inst)
    
    # Update model
    learner.teach(X_pool[query_idx], y_new)
    X_pool = np.delete(X_pool, query_idx, axis=0)
```

**Query strategies:**
- **Uncertainty sampling:** Pick sample with lowest max class probability
- **Margin sampling:** Pick sample with smallest margin between top-2 probs
- **Entropy sampling:** Pick sample with highest prediction entropy
- **BALD (Bayesian Active Learning by Disagreement):** Maximum mutual information between prediction and model parameters
- **Core-set:** Pick points that form a representative core subset (geometric coverage)
- **BADGE:** Combine gradient magnitude (uncertainty) + diversity

---

### 34.4 Curriculum Learning

**Bengio et al. (2009):** Train on easier examples first, gradually increase difficulty.

**Difficulty measures:**
- Loss value (high loss = hard)
- Prediction confidence (low confidence = hard)
- Dataset-specific heuristics (sentence length, image noise level)

```python
class CurriculumSampler:
    def __init__(self, dataset, difficulty_scores):
        self.dataset = dataset
        self.scores = difficulty_scores   # lower = easier
        self.epoch = 0
    
    def get_indices(self):
        # Start with easiest 20%, expand by 10% per epoch
        percentile = min(20 + self.epoch * 10, 100)
        threshold = np.percentile(self.scores, percentile)
        indices = np.where(self.scores <= threshold)[0]
        self.epoch += 1
        return indices
```

**Self-Paced Learning:** Model determines its own curriculum via a weighting term:
```
L(w, v) = Σᵢ vᵢ L(xᵢ, yᵢ; w) - λΣᵢ vᵢ
vᵢ = 1 if L(xᵢ, yᵢ; w) < λ, else 0   [select easy samples]
λ gradually increases (more samples included over time)
```

---

### 34.5 Adversarial Examples & Robust ML

**Adversarial examples:** Imperceptible perturbations that fool ML models.

```
Original image: correctly classified as "panda" (99.3%)
Add tiny noise: ε-perturbation (invisible to humans)
Result: classified as "gibbon" (99.3%) — completely wrong!
```

**FGSM (Fast Gradient Sign Method):**
```
x_adv = x + ε · sign(∇_x L(x, y; θ))
```
One step in gradient direction — cheap attack.

**PGD (Projected Gradient Descent):**
```
x^(0) = x
x^(t+1) = Π_{x+S} [x^(t) + α · sign(∇_x L(x^(t), y; θ))]
```
K steps of FGSM + project back to allowed perturbation ball. Strongest first-order attack.

**Adversarial Training (Madry et al., 2018):**
```
min_θ E_(x,y) [ max_{δ∈S} L(x+δ, y; θ) ]
```
Train on adversarial examples. Trades off ~5-15% clean accuracy for robustness.

**Certified Defenses:**
- **Randomized Smoothing:** Smooth classifier by adding Gaussian noise — certifiably robust
- **IBP (Interval Bound Propagation):** Certify all perturbations within ε-ball
- **Crown-IBP:** Tighter bounds via linear relaxation

**Auto-Attack (2020):** Standard benchmark for adversarial robustness — ensemble of diverse attacks.

---

### 34.6 Out-of-Distribution (OOD) Detection

**Problem:** Models are overconfident on inputs far from training distribution. Need to detect when input is OOD.

**Methods:**

**Maximum Softmax Probability (MSP, baseline):**
```python
probs = softmax(model(x))
ood_score = 1 - probs.max()   # low confidence = likely OOD
```

**Energy Score:**
```
E(x; f) = -T · log Σ_y exp(f_y(x) / T)
```
Free energy is lower for in-distribution. Better than MSP.

**Mahalanobis Distance:**
```
M(x) = min_c (f(x) - μ_c)ᵀ Σ⁻¹ (f(x) - μ_c)
```
Distance from class means in feature space. Highly effective.

**KNN-based OOD:**
```python
# After extracting penultimate features:
knn = NearestNeighbors(n_neighbors=1)
knn.fit(train_features)
distances, _ = knn.kneighbors(test_features)
ood_score = distances[:, 0]   # high distance = OOD
```

**Near-OOD vs Far-OOD:** Semantically similar (near) vs completely different (far) distribution. Most methods handle far-OOD well; near-OOD is hard.

---

### 34.7 Synthetic Data Generation

**When to generate synthetic data:**
- Class imbalance
- Privacy constraints (medical, financial)
- Rare event simulation
- Data augmentation

**Tabular synthetic data:**

```python
from sdv.single_table import GaussianCopulaSynthesizer, CTGANSynthesizer

# CTGAN: Conditional Tabular GAN
synthesizer = CTGANSynthesizer(
    epochs=300,
    batch_size=500,
    generator_dim=(256, 256),
    discriminator_dim=(256, 256)
)
synthesizer.fit(real_data)
synthetic_data = synthesizer.sample(num_rows=1000)

# Evaluate fidelity
from sdv.evaluation.single_table import run_diagnostic, evaluate_quality
quality = evaluate_quality(real_data, synthetic_data, metadata)
```

**Image augmentation (modern):**
```python
import albumentations as A

transform = A.Compose([
    A.RandomCrop(224, 224),
    A.HorizontalFlip(p=0.5),
    A.ColorJitter(brightness=0.2, contrast=0.2, saturation=0.2, hue=0.1, p=0.8),
    A.GaussianBlur(p=0.5),
    A.CoarseDropout(max_holes=8, max_height=32, max_width=32, p=0.5),  # CutOut
    A.GridDistortion(p=0.3),   # geometric
    A.Normalize(mean=[0.485,0.456,0.406], std=[0.229,0.224,0.225])
])
```

**MixUp & CutMix:**
```python
def mixup(x1, x2, y1, y2, alpha=0.4):
    lam = np.random.beta(alpha, alpha)
    x_mix = lam*x1 + (1-lam)*x2
    y_mix = lam*y1 + (1-lam)*y2   # mixed soft labels
    return x_mix, y_mix

def cutmix(x1, x2, y1, y2, alpha=1.0):
    lam = np.random.beta(alpha, alpha)
    # Cut and paste a random box from x2 into x1
    bbx1, bby1, bbx2, bby2 = rand_bbox(x1.size(), lam)
    x1[:, :, bby1:bby2, bbx1:bbx2] = x2[:, :, bby1:bby2, bbx1:bbx2]
    lam_actual = 1 - (bbx2-bbx1)*(bby2-bby1)/(W*H)
    y_mix = lam_actual*y1 + (1-lam_actual)*y2
    return x1, y_mix
```

---

### 34.8 Open-Set Recognition

**Closed-set:** Test classes = train classes.
**Open-set:** Test may contain unknown classes — model must detect "unknowns."

**Threshold on confidence:**
```
If max(softmax(f(x))) < threshold: output "unknown"
```
Simple but poor — neural nets can be overconfident on OOD.

**OpenMax (2016):**
Fit Weibull distributions to activation vectors of each class. Estimate probability of "unknown" class using extreme value theory.

**Class-conditional GAN:** Train GAN to generate hard negatives → train classifier to reject them.

**ODIN:** Input preprocessing + temperature scaling → better OOD separation.

---

## 35. Advanced LLM Architectures

### 35.1 State Space Models (SSMs) — Mamba

The attention bottleneck: O(n²) sequence length for transformers. State space models offer linear-time sequence modeling.

**Classic SSMs:**
```
Continuous SSM:  ẋ(t) = Ax(t) + Bu(t)
                 y(t) = Cx(t) + Du(t)

Discretized (step size Δ):
  h_t = Āh_{t-1} + B̄u_t     [linear recurrence]
  y_t = Ch_t + Du_t

Ā = exp(ΔA)
B̄ = (ΔA)⁻¹(exp(ΔA) - I)ΔB
```

**S4 (Structured State Space Sequence Model, 2022):**
Parameterizes A as HiPPO matrix — captures long-range dependencies. Can be computed as convolution during training (parallel) and as recurrence during inference (fast autoregressive).

**Mamba (2023, Gu & Dao):**

Key innovation: **Selective State Space** — make B, C, Δ **input-dependent** (selection mechanism):

```
Given input u_t:
  Δ_t = softplus(Linear_Δ(u_t))    ← input-dependent step size
  B_t = Linear_B(u_t)               ← input-dependent B
  C_t = Linear_C(u_t)               ← input-dependent C

h_t = Ā_t h_{t-1} + B̄_t u_t      ← selective SSM
y_t = C_t h_t
```

**Mamba Block:**
```python
class MambaBlock(nn.Module):
    def __init__(self, d_model, d_state=16, d_conv=4, expand=2):
        super().__init__()
        d_inner = expand * d_model
        self.in_proj = nn.Linear(d_model, d_inner * 2)  # x and z
        self.conv1d = nn.Conv1d(d_inner, d_inner, d_conv, padding=d_conv-1, groups=d_inner)
        self.act = nn.SiLU()
        # SSM parameters
        self.x_proj = nn.Linear(d_inner, d_state*2 + 1)  # B, C, Δ
        self.dt_proj = nn.Linear(1, d_inner)
        self.out_proj = nn.Linear(d_inner, d_model)
        # Learnable A (log parameterized)
        A = repeat(torch.arange(1, d_state+1), 'n -> d n', d=d_inner)
        self.A_log = nn.Parameter(torch.log(A))
    
    def forward(self, x):
        # x: [B, L, d_model]
        xz = self.in_proj(x)
        x_part, z = xz.chunk(2, dim=-1)
        # Conv + activation
        x_part = self.act(self.conv1d(x_part.transpose(1,2)).transpose(1,2)[:, :x.size(1), :])
        # SSM computation (simplified)
        ...
        return self.out_proj(y * self.act(z))
```

**Mamba vs Transformer:**
| | Transformer | Mamba |
|---|---|---|
| Complexity (training) | O(n²d) | O(nd²) |
| Complexity (inference) | O(n) per token (KV cache) | O(d²) per token (fixed state) |
| Memory (KV cache) | Grows with n | Fixed (constant state size) |
| Long sequences | Expensive | Efficient |
| Recall / in-context | Excellent | Good but worse |

**Mamba-2 (2024):** Structured matrix operations + new SSM formulation matching Transformers on language tasks.

**Hybrid models:** Jamba (SSM + Transformer), Zamba — alternate Mamba blocks with attention blocks.

---

### 35.2 RWKV (Receptance Weighted Key Value)

Combines the parallelizable training of Transformers with the O(1) inference of RNNs.

**Core formulation:**
```
Time-mixing:
  r_t = W_r · (μ_r x_t + (1-μ_r) x_{t-1})    [receptance]
  k_t = W_k · (μ_k x_t + (1-μ_k) x_{t-1})    [key]
  v_t = W_v · (μ_v x_t + (1-μ_v) x_{t-1})    [value]
  
  wkv_t = (Σ_{i<t} exp(-(t-1-i)w + k_i) v_i + exp(u+k_t) v_t) /
          (Σ_{i<t} exp(-(t-1-i)w + k_i)   + exp(u+k_t))
  
  o_t = W_o · (σ(r_t) ⊙ wkv_t)

w = channel-wise time decay (learnable, negative)
u = "bonus" for current token (learnable)
```

**Key insight:** wkv can be reformulated as a linear recurrence — computable as RNN at inference.

Training: parallel scan algorithm (like SSMs). Inference: pure RNN → constant memory, constant compute per token regardless of context length.

**RWKV-v5/v6 (2024):** Multi-head attention-like mechanism within RWKV framework. Competitive with Mistral-7B on benchmarks.

---

### 35.3 Mixture of Experts (MoE) — Deep Dive

**Architecture:**
```
Standard FFN layer:
  y = FFN(x) = W₂ · max(0, W₁x)

MoE layer:
  y = Σ_{i=1}^{K} G(x)_i · E_i(x)

where:
  G(x) = TopK(softmax(W_g · x))   [router: selects top-k experts]
  E_i(x) = FFN_i(x)               [expert i: its own W₁, W₂]
  K = number of experts to activate (usually 2 of N total)
```

**Load balancing loss:**
Ensures all experts get used, not just a few:
```
L_aux = α · Σᵢ fᵢ · Pᵢ

fᵢ = fraction of tokens routed to expert i
Pᵢ = average router probability for expert i
```

**Capacity factor:** Each expert has a fixed buffer size. Tokens exceeding capacity are dropped (or use auxiliary expert).

**Expert parallelism:** Different experts on different devices — scales MoE to hundreds of billions of parameters without linear compute cost.

**Token choice vs Expert choice:**
- **Token choice:** Each token selects top-k experts (standard)
- **Expert choice:** Each expert selects top-c tokens → perfect load balance, no dropped tokens

**Real MoE models:**
```
Mixtral 8×7B:   8 experts, 2 active, 47B total / 13B active params
Mixtral 8×22B:  8 experts, 2 active, 141B total / 39B active
DeepSeek-V3:    256 experts, 8 active, 671B total / 37B active
Grok-1:         8 experts, 2 active, 314B total / ~86B active
GPT-4:          ~16 experts (rumored), 2 active
```

**DeepSeekMoE innovations:**
- Fine-grained experts (more, smaller)
- Shared experts (always active for common knowledge)
- Expert-level balance loss

---

### 35.4 Linear & Sparse Attention

**Standard attention:** O(n²) time and space. Prohibitive for n > 32K.

**Linear Attention:**
Kernel trick to factorize attention:
```
Standard: Att(Q,K,V)_i = Σⱼ sim(qᵢ,kⱼ)vⱼ / Σⱼ sim(qᵢ,kⱼ)

Linear (kernel φ): sim(qᵢ,kⱼ) = φ(qᵢ)ᵀφ(kⱼ)

→ Att_i = φ(qᵢ)ᵀ (Σⱼ φ(kⱼ)vⱼᵀ) / φ(qᵢ)ᵀ Σⱼ φ(kⱼ)

Compute Σⱼ φ(kⱼ)vⱼᵀ once (O(n·d²)) → reuse for all queries
Total: O(n·d²) vs O(n²·d) for standard
```

**Feature maps φ:**
- **Performer:** Random Fourier features to approximate softmax kernel
- **cosFormer:** cos-based kernel (re-weighting for locality)
- **HGRN2:** Gated linear attention

**Sparse Attention:**
Only compute attention between nearby tokens or selected important ones.

```
Sliding window: token i attends to [i-w, i+w]   [Longformer]
Strided:        token i also attends to every s-th token   [BigBird]
Global tokens:  [CLS] attends to all, all attend to [CLS]  [Longformer]
Random:         random r tokens selected per query          [BigBird]
```

**Longformer (2020):** Sliding window (local) + global attention (classification token). O(n·w) complexity.

**BigBird (2020):** Sliding + global + random. Provably Turing complete as full attention.

---

### 35.5 KV Cache Compression

KV cache grows linearly with sequence length — memory bottleneck for long contexts.

**Streaming LLM / Sink tokens:**
Keep only:
1. Initial tokens (attention sinks — model always attends to them heavily)
2. Recent W tokens (sliding window)
Total: fixed memory regardless of sequence length (but loses middle context).

**H₂O (Heavy Hitter Oracle):**
Keep top-k important keys based on cumulative attention scores.
```
score_t(j) = Σ_{i≤t} α_{i,j}   [cumulative attention received]
Evict: keys with lowest scores at each step
```

**SnapKV (2024):**
Observe attention patterns in prompt → keep only important KV pairs per head.

**MLA (Multi-head Latent Attention, DeepSeek-V2):**
Compress KV cache via low-rank projection:
```
c^{KV}_t = W^{DKV} h_t    [down-project to small latent]
K_t = W^{UK} c^{KV}_t     [up-project when needed]
V_t = W^{UV} c^{KV}_t
```
Cache the compressed c^{KV} (small) instead of full K,V (large).

**Quantized KV Cache:**
Store KV in INT8/INT4 → 2-4× memory reduction with minimal quality loss.

---

### 35.6 Reasoning & Test-Time Compute

**The paradigm shift:** Instead of always-fixed inference, allocate variable compute to hard problems.

#### Chain-of-Thought (CoT) — Foundation
```
Standard: Question → Answer
CoT:      Question → Step 1... Step 2... Step 3... → Answer
```
Improves performance on math, reasoning, coding by ~10-40%.

#### Tree of Thoughts (ToT, 2023)

Explores multiple reasoning paths simultaneously:
```
Problem
   ├── Thought A₁
   │      ├── A₁.₁ (promising? continue)
   │      └── A₁.₂ (dead end? prune)
   ├── Thought A₂
   │      └── A₂.₁ → ... → Solution!
   └── Thought A₃
          └── ... (backtrack)

Search strategy: BFS or DFS with LLM-evaluated "promise score"
```

```python
def tree_of_thought(problem, branching=3, depth=3, model=llm):
    # BFS implementation
    queue = [(problem, [], 0)]
    
    while queue:
        current, path, level = queue.pop(0)
        if level == depth:
            return evaluate_solution(current, path)
        
        # Generate branching_factor candidate next thoughts
        thoughts = model.generate_thoughts(current, n=branching)
        
        # Evaluate each thought
        scores = model.evaluate_thoughts(current, thoughts)
        
        # Keep promising thoughts (score > threshold)
        for thought, score in zip(thoughts, scores):
            if score > 0.5:
                queue.append((thought, path + [thought], level + 1))
```

#### Graph of Thoughts (GoT, 2023)

Generalizes CoT and ToT — thoughts form an arbitrary graph:
- Nodes: intermediate reasoning states
- Edges: transformations (generate, aggregate, refine)
- **Aggregation:** Merge multiple thought branches
- **Refinement loops:** Revisit and improve previous thoughts

Enables: multiple reasoning paths that merge and recombine.

#### Process Reward Models (PRMs)

Score each **step** of reasoning (not just final answer):
```
Outcome Reward (ORM): R(answer) — only cares about final answer
Process Reward (PRM): R(step₁) + R(step₂) + ... + R(stepₙ)

PRM advantages:
  - Denser reward signal (learn from partial solutions)
  - Can detect where reasoning goes wrong
  - Essential for math problem solving (o1 style)
```

**OpenAI PRM800K:** Dataset of human-labeled reasoning steps. Used to train o1's process reward model.

#### Self-Consistency (SC)

Sample K diverse CoT paths → majority vote on final answers:
```python
def self_consistent_answer(question, model, k=20, temperature=0.7):
    answers = []
    for _ in range(k):
        chain = model.generate(question + "\nThink step by step.\n", 
                               temperature=temperature)
        answer = extract_answer(chain)
        answers.append(answer)
    
    # Majority vote
    return Counter(answers).most_common(1)[0][0]
```

Improves accuracy significantly on math/reasoning benchmarks.

#### Best-of-N Sampling

Generate N candidates → pick best using reward model:
```
{y₁, y₂, ..., yₙ} = generate(x, n=N)
y_best = argmax_yᵢ RM(x, yᵢ)
```

**Scaling with N:**
```
N=1:   base model performance
N=10:  ~+5-10% improvement
N=100: ~+15-25% improvement
N=1000: ~+30-40% improvement (diminishing returns)
```

Effective but expensive. Speculative decoding + small verifier = efficient approximation.

#### Monte Carlo Tree Search (MCTS) for LLMs

Full MCTS with LLM as both rollout policy and value function:
```
Selection:   UCT = Q(s,a) + c√(ln N(s) / N(s,a))
Expansion:   LLM generates next reasoning steps
Simulation:  Roll out to completion with LLM
Backprop:    Update Q values along path
```

Used in: AlphaCode 2, rStar-Math, Sky-T1.

---

### 35.7 Tool Calling & Function Calling Internals

**How function calling works under the hood:**

```
User: "What's the weather in Tokyo?"

1. Model receives system prompt with tool schema:
   {
     "name": "get_weather",
     "description": "Get current weather for a city",
     "parameters": {
       "type": "object",
       "properties": {
         "city": {"type": "string"},
         "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
       },
       "required": ["city"]
     }
   }

2. Model generates stop_reason="tool_use" + structured call:
   {"name": "get_weather", "input": {"city": "Tokyo", "unit": "celsius"}}

3. Application executes tool, returns result:
   {"temperature": 22, "condition": "Cloudy", "humidity": 68}

4. Result appended to context as tool_result message

5. Model continues generation with tool result in context
```

**Parallel tool calling:**
Modern models (GPT-4o, Claude) can call multiple tools simultaneously when independent:
```json
[
  {"name": "get_weather", "input": {"city": "Tokyo"}},
  {"name": "get_weather", "input": {"city": "London"}},
  {"name": "get_weather", "input": {"city": "New York"}}
]
```
All three execute in parallel — significant latency reduction for multi-tool workflows.

---

### 35.8 Model Context Protocol (MCP)

**MCP (Anthropic, 2024):** Open standard for connecting LLMs to external tools and data sources.

**Architecture:**
```
MCP Client (Claude Desktop / any app)
         ↕  JSON-RPC over stdio/SSE
MCP Server (tool implementation)
         ↕
External System (database, API, filesystem, etc.)
```

**MCP primitives:**
```
Resources:  Data sources the server exposes (files, DB rows, API responses)
            URI-addressable: file:///path, postgres://query, github://repo/file
            
Tools:      Functions the LLM can call (actions with side effects)
            Example: create_file, send_email, execute_sql
            
Prompts:    Reusable prompt templates servers expose
            Example: "analyze_code" prompt template with file parameter

Sampling:   Server can request LLM completions (bidirectional!)
```

**MCP server example (Python):**
```python
from mcp.server import Server
from mcp.server.stdio import stdio_server
import mcp.types as types

app = Server("my-tool-server")

@app.list_tools()
async def list_tools() -> list[types.Tool]:
    return [types.Tool(
        name="search_database",
        description="Search the product database",
        inputSchema={
            "type": "object",
            "properties": {
                "query": {"type": "string"},
                "limit": {"type": "integer", "default": 10}
            },
            "required": ["query"]
        }
    )]

@app.call_tool()
async def call_tool(name: str, arguments: dict) -> list[types.TextContent]:
    if name == "search_database":
        results = await db.search(arguments["query"], arguments.get("limit", 10))
        return [types.TextContent(type="text", text=json.dumps(results))]

async def main():
    async with stdio_server() as (read, write):
        await app.run(read, write, app.create_initialization_options())
```

**Why MCP matters:** Standardized integration — one server works with any MCP-compatible client. Ecosystem is growing rapidly (1000+ community MCP servers by 2026).

---

### 35.9 Long-Context Architectures

**Context length evolution:**
```
GPT-2:       1,024 tokens
GPT-3:       4,096 tokens
GPT-4:      32,768 tokens (launch) → 128K tokens
Claude 2.1: 200K tokens
Claude 3:   200K tokens
LLaMA 4:   10M tokens  ← Scout model (2025)
```

**Techniques for long contexts:**

**RoPE scaling (YaRN, LongRoPE, LongContext RoPE):**
```
RoPE uses θ_i = base^(-2i/d) as angle frequencies
Position k rotates by k·θ_i

For longer contexts: scale or interpolate θ_i
  Position Interpolation: scale positions by L_train/L_test
  YaRN: ramp function — don't scale high frequencies (local), scale low (global)
  LongRoPE: optimize scaling factors per frequency dimension
```

**Retrieval augmentation for very long contexts:**
Hybrid: sliding window local attention + global attention via retrieval.

**Full attention with system tricks:**
- FlashAttention-3 for H100 hardware efficiency
- Ring Attention: distribute sequence across GPUs in a ring
- Sequence parallelism: split sequence dimension for training

**Lost-in-the-Middle effect:**
LLMs recall information better from beginning and end of context than from the middle.
Mitigated by: query-aware chunking, re-ranking chunks before insertion.

---

## 36. Quantum Machine Learning

### 36.1 Quantum Computing Basics

**Classical bit:** 0 or 1.
**Qubit:** Superposition of 0 and 1:
```
|ψ⟩ = α|0⟩ + β|1⟩

where |α|² + |β|² = 1 (normalization)
|α|² = probability of measuring 0
|β|² = probability of measuring 1

Measurement: collapses superposition to definite 0 or 1
```

**n qubits:** 2ⁿ-dimensional state space — exponentially more than n classical bits.

**Bloch sphere:** Single-qubit state visualized as point on unit sphere.

**Key quantum phenomena:**
- **Superposition:** Qubit exists in multiple states simultaneously
- **Entanglement:** Qubits correlated in ways impossible classically
- **Interference:** Amplitudes add/cancel — used to amplify correct answers

**Quantum gates (unitary operations):**
```
Hadamard (H):    |0⟩ → (|0⟩+|1⟩)/√2   [create superposition]
Pauli-X (NOT):   |0⟩ → |1⟩, |1⟩ → |0⟩
CNOT:            |11⟩ → |10⟩           [2-qubit entangling gate]
Rz(θ):           |0⟩ → |0⟩, |1⟩ → e^{iθ}|1⟩   [rotation]
```

**Quantum circuit:** Sequence of quantum gates applied to qubits. Measured at end.

---

### 36.2 Variational Quantum Circuits (VQC)

The foundation of quantum ML — parameterized circuits trained by classical optimizer.

```
|0⟩ⁿ → [Data Encoding U(x)] → [Ansatz V(θ)] → Measurement → classical output

Data encoding U(x): encode classical data into quantum state
  Angle encoding: Rz(xᵢ) per feature
  Amplitude encoding: |ψ⟩ = Σᵢ xᵢ|i⟩ / ||x||

Ansatz V(θ): parameterized unitary (like "layers" in quantum NN)
  Hardware-efficient ansatz: alternating Rz, Rx rotations + CNOT entangling layers

Measurement: expectation value ⟨Z⟩ = Tr[ρ · Z] on target qubits
```

**Parameter shift rule** (quantum gradient):
```
∂⟨O⟩/∂θᵢ = (1/2)[⟨O⟩(θᵢ + π/2) - ⟨O⟩(θᵢ - π/2)]
```
Exact gradient computable from two circuit evaluations (analogous to finite differences but exact).

---

### 36.3 Quantum Neural Networks (QNNs)

```python
# PennyLane — the PyTorch of quantum ML
import pennylane as qml
import torch

n_qubits = 4
dev = qml.device("default.qubit", wires=n_qubits)

@qml.qnode(dev, interface="torch")
def quantum_circuit(inputs, weights):
    # Encode data
    qml.AngleEmbedding(inputs, wires=range(n_qubits))
    
    # Parameterized layers
    for layer_weights in weights:
        qml.BasicEntanglerLayers(layer_weights, wires=range(n_qubits))
    
    # Measure
    return [qml.expval(qml.PauliZ(i)) for i in range(n_qubits)]

# Hybrid quantum-classical model
class HybridQNN(torch.nn.Module):
    def __init__(self, n_layers=3):
        super().__init__()
        weight_shapes = {"weights": (n_layers, n_qubits)}
        self.qlayer = qml.qnn.TorchLayer(quantum_circuit, weight_shapes)
        self.linear = torch.nn.Linear(n_qubits, 2)
    
    def forward(self, x):
        x = self.qlayer(x)           # quantum layer
        return self.linear(x)        # classical post-processing

model = HybridQNN()
optimizer = torch.optim.Adam(model.parameters(), lr=0.01)
```

---

### 36.4 Quantum Kernel Methods

**Quantum kernel:** Compute similarity in exponentially large Hilbert space:
```
K(xᵢ, xⱼ) = |⟨φ(xᵢ)|φ(xⱼ)⟩|²

where |φ(x)⟩ = U(x)|0⟩ⁿ is the quantum feature map

Estimated by: running circuit, measuring overlap = P(all zeros)
```

Train classical SVM with quantum kernel — if quantum feature map provides advantage, quantum SVM outperforms classical.

**Challenge:** Quantum kernel matrices require O(N²) circuit evaluations for N training points.

---

### 36.5 Quantum Optimization Algorithms

#### QAOA (Quantum Approximate Optimization Algorithm)

Solves combinatorial optimization problems (MaxCut, TSP, Portfolio):
```
|γ, β⟩ = U_B(β_p) U_C(γ_p) ... U_B(β_1) U_C(γ_1) |+⟩ⁿ

U_C(γ) = exp(-iγC)   [problem Hamiltonian]
U_B(β) = exp(-iβB)   [mixer Hamiltonian = Σᵢ Xᵢ]

Maximize: ⟨γ,β|C|γ,β⟩ over classical parameters γ, β
```

Approximation ratio improves with depth p.

#### VQE (Variational Quantum Eigensolver)

Finds ground state energy of molecular Hamiltonians → drug discovery, materials:
```
E_min ≈ min_θ ⟨ψ(θ)|H|ψ(θ)⟩

VQE can compute:
  - Molecular ground state energy (quantum chemistry)
  - Drug-protein binding energy
  - Materials band structure
```

---

### 36.6 Quantum Advantage & Current Reality

**Theoretical advantages:**
- **Grover's algorithm:** O(√N) search vs O(N) classical — quadratic speedup
- **Shor's algorithm:** Polynomial factoring → breaks RSA encryption (not yet practical)
- **Quantum linear algebra:** HHL algorithm — potentially exponential speedup for linear systems (with caveats)
- **Quantum simulation:** Simulate quantum systems (molecules) exponentially faster

**NISQ (Noisy Intermediate-Scale Quantum) era (2024–2026):**
```
Current hardware:
  IBM Heron:     133 qubits, error rate ~0.1%
  Google Willow: 105 qubits, below threshold error correction (Dec 2024)
  IonQ:          36 qubits, higher fidelity (trapped ions)
  
NISQ challenges:
  - Decoherence: qubits lose quantum state quickly (~100μs)
  - Gate errors: ~0.1-1% per 2-qubit gate
  - Limited connectivity: not all qubits connected to each other
  - No fault tolerance: too few qubits for error correction
```

**Google Willow (December 2024):**
Demonstrated below-threshold error correction — more qubits → lower error rate (first time). Key milestone toward fault-tolerant quantum computing.

**Honest assessment of QML in 2026:**
```
What works:
  ✅ Quantum simulation of molecules (quantum chemistry advantage likely)
  ✅ Small-scale quantum kernels (proof of concept)
  ✅ Theoretical framework for quantum advantage in certain ML tasks

What doesn't (yet):
  ❌ QML beating classical ML on real large datasets
  ❌ Fault-tolerant quantum computers (5-10+ years away)
  ❌ Practical quantum advantage for generic ML

The "barren plateau" problem:
  Gradients of QNNs vanish exponentially with qubit count
  → Very hard to train large quantum circuits
  Active research area
```

**When to care about QML:**
- If working in quantum chemistry / drug discovery
- If your data is fundamentally quantum mechanical
- Research/academia exploring the frontier
- Otherwise: classical ML remains far superior for all practical tasks

---

## 37. Complete LLM Additions

### 37.1 Speculative Decoding (Full Detail)

```
Setup:
  Draft model M_q: small, fast (7B)
  Target model M_p: large, accurate (70B)

Algorithm:
  1. Draft: M_q autoregressively generates γ=5 draft tokens
     x̃₁, x̃₂, ..., x̃_γ = M_q.generate(x, n=γ)
  
  2. Verify: M_p processes all γ tokens in ONE forward pass
     [p₁, p₂, ..., p_γ, p_{γ+1}] = M_p.forward([x, x̃₁, ..., x̃_γ])
     (parallel — one forward pass of large model for γ tokens!)
  
  3. Accept/reject (token i):
     rᵢ ~ Uniform(0,1)
     If rᵢ ≤ p_p(x̃ᵢ) / p_q(x̃ᵢ): ACCEPT x̃ᵢ
     Else: REJECT, sample corrected token from adjusted distribution
           p_adjusted ∝ max(0, p_p - p_q), continue from here
  
  4. Result: Accepted tokens + one guaranteed target token
     On average: α·γ + 1 tokens per round (α = acceptance rate ~0.7-0.9)
```

**Speedup:** ~2-3× at quality identical to target model (mathematically guaranteed same distribution).

**Medusa (2024):** Add multiple prediction heads to target model itself — no separate draft model needed. Heads predict token t+2, t+3, etc. in parallel.

**EAGLE (2024):** Draft model uses target model's features → better acceptance rate.

---

### 37.2 Extended Context: All Techniques Together

```
Long context challenge: 1M tokens = 2GB KV cache for 70B model

Solution stack:

1. Architecture changes:
   RoPE scaling (YaRN) → extend without retraining
   GQA → reduce KV cache size K/G times
   Sparse attention → O(n√n) or O(n log n)

2. Memory management:
   FlashAttention → no O(n²) memory materialization
   PagedAttention (vLLM) → virtual memory for KV cache
   KV cache quantization → 2-4× compression
   H2O / SnapKV → selective eviction

3. Hardware scaling:
   Ring Attention → sequence parallelism across GPUs
   Tensor parallelism → split attention heads
   Pipeline parallelism → split layers

4. Algorithmic:
   RAG hybrid → augment 32K window with retrieval
   Hierarchical summarization → compress past context
   Recurrent state → Mamba-style fixed-size state
```

---

### 37.3 Multimodal Architecture Internals

**How visual tokens work in modern VLMs:**

```
Image → Vision Encoder (ViT-L/14 or SigLIP) → Image Features [N_img × d_v]
                                                     ↓
                                        Visual Projector (MLP or cross-attention)
                                                     ↓
                                        Image Tokens [N_tokens × d_llm]
                                                     ↓
Text Tokens [N_text × d_llm] ──────────→ [Interleaved sequence] → LLM Decoder
```

**LLaVA (2023):** Linear projection to map image features to LLM embedding space.

**LLaVA-1.5:** MLP projector + higher resolution (336×336).

**InternVL2 / LLaVA-Next:** Dynamic high-resolution — split high-res image into tiles, encode each separately.

**Gemini 1.5 native multimodal:** Trained from scratch with interleaved text/image/audio/video tokens.

**Video understanding:**
```
Video (T frames) → Per-frame ViT → [T × N_patches × d]
                                         ↓
                         Temporal compression (sampling, pooling)
                                         ↓
                               LLM context (manageable tokens)
```

---

## 38. Representation Learning & Transfer Learning (Deep Dive)

### 38.1 What Makes Good Representations?

**Properties of useful representations:**
1. **Sufficient** — captures enough information to solve downstream tasks
2. **Disentangled** — different dimensions encode different semantic factors
3. **Smooth** — similar inputs → similar representations
4. **Compact** — low-dimensional relative to raw input
5. **Invariant** — invariant to nuisances (lighting, rotation, irrelevant variation)

### 38.2 Transfer Learning Strategies

**Feature extraction (frozen backbone):**
```python
# Freeze pretrained backbone
for param in model.backbone.parameters():
    param.requires_grad = False

# Only train new head
model.head = nn.Linear(backbone_dim, n_classes)
optimizer = torch.optim.Adam(model.head.parameters(), lr=1e-3)
```
Best when: small downstream dataset + large domain difference.

**Fine-tuning (all layers):**
```python
# Unfreeze all
for param in model.parameters():
    param.requires_grad = True

# Lower LR for pretrained layers, higher for new head
optimizer = torch.optim.Adam([
    {'params': model.backbone.parameters(), 'lr': 1e-5},
    {'params': model.head.parameters(), 'lr': 1e-3}
])
```
Best when: enough data + similar domain to pretraining.

**Gradual unfreezing:**
```
Epoch 1-3:  Train head only
Epoch 4-6:  Unfreeze last 2 blocks + head
Epoch 7-9:  Unfreeze all → very low LR
```

**ULMFiT strategy for NLP:**
Discriminative fine-tuning + slanted triangular learning rates — works excellently for BERT-style models.

### 38.3 Foundation Models as Representations

```
Task              Foundation Model          Adapter
─────────────────────────────────────────────────────
Image classif.    DINOv2 / CLIP ViT-L       Linear probe
Detection         DINOv2 backbone           DETR head
Segmentation      SAM 2                     Point/box prompts
Text classif.     BERT / DeBERTa            [CLS] → Linear
Generation        GPT / LLaMA               LoRA fine-tune
Code              CodeLLaMA / DeepSeekCoder LoRA + system prompt
Speech            Whisper                   Fine-tune decoder
Audio             AudioCraft / Encodec      Fine-tune
Video             VideoMAE / InternVideo    Temporal head
Molecules         ChemBERTa / ESM2          Task-specific head
```

---

## Appendix A: Important Research Papers Timeline

| Year | Paper | Impact |
|---|---|---|
| 2012 | AlexNet | CNNs + GPU training revolution |
| 2013 | Word2Vec | Neural word embeddings |
| 2014 | GANs (Goodfellow) | Generative modeling revolution |
| 2015 | ResNet | Skip connections, deep networks |
| 2015 | Attention (Bahdanau) | Sequence-to-sequence with attention |
| 2015 | Hinton et al. — Knowledge Distillation | Teacher-student model compression |
| 2016 | GCN (Kipf & Welling) | Spectral graph convolutions on node features |
| 2016 | LIME (Ribeiro) | Local interpretable model-agnostic explanations |
| 2016 | Grad-CAM (Selvaraju) | Gradient-based visual explanation for CNNs |
| 2017 | Transformer (Vaswani) | "Attention Is All You Need" |
| 2017 | MPNN (Gilmer) | Message passing for quantum chemistry |
| 2017 | MAML (Finn) | Model-Agnostic Meta-Learning |
| 2017 | EWC (Kirkpatrick) | Elastic Weight Consolidation for continual learning |
| 2017 | Prophet (Taylor & Letham) | Scalable decomposable time series forecasting |
| 2018 | BERT (Devlin) | Bidirectional pre-training |
| 2018 | GPT-1 (OpenAI) | Generative pre-training |
| 2018 | GraphSAGE (Hamilton) | Inductive node embeddings via neighborhood sampling |
| 2018 | GAT (Veličković) | Graph Attention Networks |
| 2018 | SHAP (Lundberg & Lee) | Unified Shapley-value model explanations |
| 2019 | XLNet, RoBERTa | BERT improvements |
| 2019 | GIN (Xu) | Graph Isomorphism Network — WL-test expressiveness |
| 2019 | DistilBERT (Sanh) | BERT distillation — 40% smaller, 97% performance |
| 2019 | NAS (Zoph / DARTS) | Differentiable neural architecture search |
| 2020 | GPT-3 | Few-shot in-context learning |
| 2020 | DDPM | Denoising diffusion models |
| 2020 | LightGCN | Simplified GCN for collaborative filtering |
| 2020 | Informer (Zhou) | Efficient transformer for long-sequence forecasting |
| 2021 | CLIP | Vision-language alignment |
| 2021 | LoRA | Efficient fine-tuning |
| 2021 | DINOv2 (Caron) | Self-supervised vision foundation model |
| 2021 | FedAvg / FedProx | Federated averaging with non-IID robustness |
| 2022 | InstructGPT | RLHF alignment |
| 2022 | Chinchilla | Scaling laws revision |
| 2022 | FlashAttention | IO-efficient attention |
| 2022 | ChatGPT | Conversational AI breakthrough |
| 2022 | PatchTST (Nie) | Patch-based transformer for time series |
| 2023 | LLaMA-2 | Open LLMs |
| 2023 | GPT-4 | Multimodal frontier |
| 2023 | Mixtral | Mixture of Experts |
| 2023 | Mamba (Gu & Dao) | Selective state space model — linear-time sequence modelling |
| 2023 | RWKV (Peng) | RNN-Transformer hybrid — linear attention alternative |
| 2023 | Speculative Decoding (Leviathan) | Draft-verify decoding for LLM speedup |
| 2024 | LLaMA-3 | 405B open model |
| 2024 | Gemini Ultra | Frontier multimodal |
| 2024 | o1 (OpenAI) | Test-time compute reasoning |
| 2024 | GraphRAG (MS) | Knowledge graph + RAG |
| 2024 | FLUX | Rectified flow image gen |
| 2024 | MCP (Anthropic) | Model Context Protocol — standardised tool/agent interface |
| 2025 | DeepSeek-R1 | Open reasoning model |
| 2025 | LLaMA 4 Scout | 10M context window |
| 2026 | Spring Sprint | GPT-5.5, Claude 4.8, Gemini 3.x |

---

## Appendix B: Glossary of Key Terms

| Term | Definition |
|---|---|
| Active Learning | Iterative strategy where the model queries the most informative unlabeled samples for annotation |
| Adversarial examples | Inputs crafted with imperceptible perturbations that cause model misclassification |
| Attention | Mechanism weighting relevance of different inputs |
| AutoML | Automated machine learning — automated selection and tuning of models and pipelines |
| Autoregressive | Generating tokens one-by-one conditioned on all previous |
| Backpropagation | Algorithm to compute gradients via chain rule |
| Batching | Processing multiple examples simultaneously |
| Bayesian Optimization | Sequential, surrogate-model-guided search for optimal hyperparameters |
| Catastrophic Forgetting | Abrupt loss of old task knowledge when a neural network is trained on a new task |
| Causal inference | Methods for estimating cause-effect relationships from observational data |
| Chain-of-thought | Generating intermediate reasoning steps |
| Conformal Prediction | Distribution-free framework producing prediction sets with guaranteed coverage probability |
| Context window | Maximum tokens a model can process at once |
| Continual Learning | Training paradigm where a model learns a sequence of tasks without forgetting prior ones |
| Contrastive Learning | Self-supervised objective that pulls similar samples together and pushes dissimilar ones apart |
| Curriculum Learning | Training strategy that presents easier examples first, gradually increasing difficulty |
| Differential Privacy | Mathematical privacy guarantee bounding the influence of any single training example |
| Disentangled representation | Representation where individual dimensions independently encode distinct semantic factors |
| Domain Adaptation | Transferring a model trained on a source domain to perform well on a different target domain |
| DPO | Direct Preference Optimization — alignment without RL |
| Embedding | Dense vector representation of data |
| EWC | Elastic Weight Consolidation — continual learning via a Fisher-information penalty on important weights |
| Federated Learning | Distributed training paradigm where data never leaves client devices; only model updates are shared |
| Few-shot learning | Learning to solve new tasks from only a handful of labelled examples |
| Fine-tuning | Further training a pre-trained model on specific data |
| Flash Attention | Memory-efficient attention via tiling |
| Foundation model | Large pre-trained model for general use |
| GAT | Graph Attention Network — GNN variant using learned attention weights over neighbours |
| GCN | Graph Convolutional Network — spectral/spatial convolution over graph-structured data |
| GIN | Graph Isomorphism Network — maximally expressive GNN matching the Weisfeiler-Leman test |
| GNN | Graph Neural Network — general class of models learning on graph-structured data via message passing |
| GQA | Grouped Query Attention — efficient multi-head attention |
| Hallucination | Confidently generating factually incorrect content |
| HPO | Hyperparameter Optimization — systematic search over model configuration space |
| Integrated Gradients | Attribution method assigning feature importance by integrating gradients along a path from baseline to input |
| Knowledge Distillation | Compressing a large teacher model into a smaller student by training on soft probability outputs |
| Latent space | Compressed representation space in neural networks |
| LIME | Local Interpretable Model-agnostic Explanations — perturb input locally to fit an interpretable surrogate |
| LoRA | Low-Rank Adaptation — efficient parameter fine-tuning |
| MAML | Model-Agnostic Meta-Learning — meta-learning via fast gradient adaptation to new tasks |
| Mamba / SSM | State Space Model — linear-complexity sequence model with selective state updates |
| MCP | Model Context Protocol — open standard for connecting LLMs to external tools and data sources |
| Message Passing | Core GNN operation where nodes aggregate feature information from their graph neighbours |
| Meta-Learning | Learning-to-learn — acquiring inductive biases that enable rapid adaptation to new tasks |
| MoE | Mixture of Experts — sparse conditional computation |
| NAS | Neural Architecture Search — automated discovery of optimal neural network architectures |
| OOD Detection | Out-of-Distribution Detection — identifying inputs that fall outside the training distribution |
| Oversmoothing | Phenomenon in deep GNNs where node representations converge to indistinguishable vectors |
| Pruning | Removing redundant weights or neurons to produce a smaller, faster model |
| Quantization | Reducing numerical precision of model weights/activations (e.g., FP32 → INT8) to save memory and speed inference |
| RAG | Retrieval-Augmented Generation |
| RLHF | Reinforcement Learning from Human Feedback |
| RoPE | Rotary Position Embedding used in modern LLMs |
| RWKV | RNN-Transformer hybrid architecture with linear attention, enabling constant-time inference |
| SHAP | SHapley Additive exPlanations — game-theory-based unified feature attribution framework |
| Softmax | Function converting logits to probability distribution |
| Speculative Decoding | LLM inference speedup: a fast draft model proposes tokens, which a larger model verifies in parallel |
| TinyML | Deploying trained ML models on microcontrollers and other severely resource-constrained edge devices |
| Tokenization | Process of splitting text into model-digestible units |
| Transfer learning | Apply knowledge learned on one task to another |
| Transformer | Attention-based architecture dominating modern AI |
| Uplift Modelling | Estimating the causal effect (treatment uplift) of an action on individual-level outcomes |
| Vector database | DB optimized for storing and searching embeddings |
| VQC | Variational Quantum Circuit — parameterised quantum circuit trained with classical optimisers |
| XAI | Explainable AI — methods and techniques that make ML model decisions interpretable to humans |
| Zero-shot | Task performance without task-specific training examples |

---

## Appendix C: The AI/ML Engineering Stack (2026)

```
COMPUTE
  Cloud:      AWS (SageMaker), GCP (Vertex AI), Azure (ML), Lambda Labs
  On-prem:    NVIDIA H100/H200, AMD MI300X, Google TPU v5
  Edge:       NVIDIA Jetson, Google Coral, Apple Neural Engine

TRAINING FRAMEWORKS
  Core:       PyTorch 2.x (dominant), TensorFlow 2.x, JAX+Flax
  LLM:        Megatron-LM, DeepSpeed, FSDP (PyTorch native)
  Efficient:  torchao, bitsandbytes, vLLM, TGI (HuggingFace)

EXPERIMENT TRACKING
  Weights & Biases (most popular), MLflow, ClearML, Neptune

DATA
  Versioning: DVC, LakeFS
  Quality:    Great Expectations, Deepchecks
  Synthesis:  SDV, Gretel.ai, NVIDIA NeMo Curator

MODELS & HUBS
  HuggingFace Hub (central model repository)
  Ollama (local LLM serving)
  OpenRouter (multi-provider API routing)

VECTOR & SEARCH
  Databases: Qdrant, Weaviate, Milvus, Pinecone
  Embedding: text-embedding-3, bge-large, nomic-embed

RAG ORCHESTRATION
  LangChain, LlamaIndex, Haystack, DSPy

AGENT FRAMEWORKS
  LangGraph, CrewAI, AutoGen, Semantic Kernel

SERVING & INFERENCE
  vLLM, TGI, Triton, BentoML, Ray Serve, Ollama

EVALUATION
  RAGAS (RAG), DeepEval, LangSmith, PromptFoo, Evals (OpenAI)

MONITORING
  Evidently AI, Arize, Fiddler, WhyLabs, Arthur

SECURITY
  Guardrails AI, NeMo Guardrails, LlamaGuard
  Prompt injection detection, PII redaction
```

---

## Appendix D: Interview Questions — ML & AI

### Fundamentals
1. Why does L1 regularization produce sparse solutions but L2 doesn't?
2. What is the difference between generative and discriminative models? Give examples.
3. Explain the kernel trick in SVMs. Why does it work?
4. When would you use Random Forest over Gradient Boosting and vice versa?
5. What causes vanishing/exploding gradients? How do you fix each?

### Deep Learning
6. Why does Batch Normalization speed up training?
7. Explain why ResNet's skip connections solve the degradation problem.
8. What is the difference between Layer Norm and Batch Norm? When to use each?
9. Why is Adam preferred over SGD for most deep learning? When is SGD better?
10. What is weight tying in language models and why is it used?

### Transformers & LLMs
11. Walk through self-attention step by step — why do we scale by √dₖ?
12. What is the difference between encoder-only, decoder-only, and encoder-decoder transformers? Give one use case each.
13. How does RoPE differ from sinusoidal positional encoding? What problem does it solve?
14. Explain how RLHF works. What is the role of the KL penalty?
15. What is LoRA? Explain the math and why it reduces parameters.
16. How does KV caching work? What is its memory complexity?
17. Explain speculative decoding. Under what conditions does it provide speedup?
18. What is Grouped Query Attention (GQA) and how does it help?

### RAG & Agents
19. What is the difference between naive RAG and self-RAG?
20. Why would you use GraphRAG over standard RAG?
21. What is the ReAct framework? How does it differ from simple tool calling?
22. What is HyDE and why can it improve retrieval?

### Practical ML
23. Your model has 99% training accuracy but 70% validation accuracy. What do you do?
24. You have a 1% positive class. Walk through your entire approach.
25. How do you prevent data leakage in cross-validation for time series?
26. Your deployed model's performance degrades over time. Diagnose the issue.
27. How would you choose between SHAP and LIME for explaining a prediction?

---

*Complete AI/ML Mastery Guide — Updated through Q3 2026*
*Sections 1-22: Core ML/AI Foundations*
*Sections 23-38: Advanced Topics, Architectures, Edge, Quantum*
