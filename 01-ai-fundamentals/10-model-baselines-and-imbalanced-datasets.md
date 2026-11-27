# Phase 1 — Lesson 10: Model Baselines and Imbalanced Datasets

Welcome back! 🚀 This lesson is particularly useful for real-world ML systems because production datasets are often **imbalanced**.

Imagine you're building a model to predict Kubernetes pod failures. You collect 100,000 pod observations:

- 99,500 observations are healthy.
- 500 observations contain a failure.

A model that always predicts *healthy* gets 99.5% accuracy.

Sounds brilliant, right? Not at all. It has failed to identify a single failing pod.

This is exactly why we need model baselines, class-imbalance techniques, and the right evaluation metrics.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain what a baseline model is and why we need one.
2. Recognize class imbalance and understand why accuracy can be misleading.
3. Explain class weights, oversampling, and undersampling.
4. Understand ROC-AUC and PR-AUC conceptually.
5. Choose evaluation metrics based on the problem.
6. Build an evaluation workflow that avoids data leakage.

We'll keep the mathematics practical and focus on engineering decisions.

---

## 1. What is a baseline model?

Before building a sophisticated ML model, we need a simple model to compare against.

That simple reference is called a **baseline model**.

Suppose we're predicting whether a Kubernetes pod will fail.

Our dataset has these features:

| CPU usage | Memory usage | Restarts | Failure |
|---:|---:|---:|---|
| 45% | 500 MB | 0 | No |
| 92% | 950 MB | 5 | Yes |
| 60% | 700 MB | 1 | No |
| 88% | 900 MB | 4 | Yes |
| 35% | 400 MB | 0 | No |

We could immediately build a neural network. But how would we know whether it is actually learning anything useful?

We start with a baseline.

### Types of baselines

**A. Majority-class baseline**

Always predicts the most common class.

For example:

```text
Healthy: 99,500
Failure:    500

Baseline prediction: Always Healthy
```

It is extremely simple, but it tells us how misleading accuracy could be.

**B. Rule-based baseline**

Use a basic operational rule:

```python
if cpu_usage > 90 and restarts > 3:
    prediction = "Failure"
else:
    prediction = "Healthy"
```

This is not necessarily ML. It gives us a practical benchmark based on existing operational knowledge.

**C. Simple ML baseline**

Train a basic model, such as Logistic Regression or a Decision Tree.

This gives us a benchmark before experimenting with more complex models.

**D. Advanced model**

Only after measuring the simpler approaches do we try more complex models, such as Random Forest, XGBoost, or neural networks.

<box border radius="lg" padding={3} gap={2}>
  **A sensible model development sequence**

  <list connector="solid" marker="number" gap={2}>
    <list-item>Majority-class baseline</list-item>
    <list-item>Rule-based baseline</list-item>
    <list-item>Simple ML model</list-item>
    <list-item>More advanced model</list-item>
    <list-item>Compare results and justify complexity</list-item>
  </list>
</box>

**Infrastructure connection:** Think of a baseline as a performance benchmark in infrastructure engineering. Before optimizing a Kubernetes workload, you measure its current CPU, memory, latency, and throughput. In ML, you need a comparable starting point before claiming that a model is better.

## 2. What is an imbalanced dataset?

A dataset is imbalanced when one class has significantly more observations than another.

Examples:

| Problem | Common class | Rare class |
|---|---|---|
| Fraud detection | Legitimate transactions | Fraudulent transactions |
| Pod failure prediction | Healthy pods | Failed pods |
| Security detection | Normal activity | Security incidents |
| Manufacturing | Normal products | Defective products |
| Medical diagnosis | No disease | Disease present |

Let's use our pod failure dataset.

<box border radius="lg" padding={3} gap={3}>
  **Dataset distribution**

  <row justify="between">
    **Healthy pods**
    <title color="success" size="xl" tabularNums>99.5%</title>
  </row>
  <box background="surface-tertiary" radius="full" height="14px" clip>
    <box background="#16A34A" width="99.5%" height="14px" radius="full"/>
  </box>
  <row justify="between">
    **Failed pods**
    <title color="danger" size="xl" tabularNums>0.5%</title>
  </row>
  <box background="surface-tertiary" radius="full" height="14px" clip>
    <box background="#DC6B6B" width="0.5%" height="14px" radius="full"/>
  </box>
  <caption>Illustrative distribution: 100,000 observations, 500 failures.</caption>
</box>

The problem is that a model can achieve excellent accuracy by mostly predicting the majority class.

### Why accuracy fails here

Remember the confusion matrix from Lesson 3:

| | Actually failed | Actually healthy |
|---|---:|---:|
| Predicted failed | TP | FP |
| Predicted healthy | FN | TN |

- **TP:** Correctly identified a failure.
- **FP:** Raised a failure alert when the pod was healthy.
- **FN:** Missed an actual failure.
- **TN:** Correctly identified a healthy pod.

Now imagine this model:

| Metric | Result |
|---|---:|
| True positives | 0 |
| False positives | 0 |
| False negatives | 500 |
| True negatives | 99,500 |
| Accuracy | 99.5% |

It achieved 99.5% accuracy, but detected zero failures.

That is why, for imbalanced datasets, we must look beyond accuracy.

## 3. Techniques for handling class imbalance

There are several ways to help a model learn from a minority class.

### A. Class weights

Class weighting tells the model that mistakes involving the minority class should carry a higher penalty during training.

For example:

```text
Healthy observations: 99,500
Failure observations:    500
```

Without class weighting, the model may focus heavily on predicting healthy because that class dominates the dataset.

With class weighting, we give greater importance to failure examples.

Conceptually:

```text
Misclassifying a healthy pod: lower penalty
Missing a failing pod:        higher penalty
```

The exact penalties depend on the weighting strategy and model.

In Scikit-learn, Logistic Regression supports:

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    class_weight="balanced",
    max_iter=1000
)
```

`class_weight="balanced"` automatically calculates weights based on class frequencies.

**When is this useful?**

- Fraud detection
- Failure prediction
- Security anomaly classification
- Rare-event detection

Class weighting is often a good first technique to try because it doesn't require creating or deleting training examples.

### B. Oversampling

Oversampling increases the representation of the minority class in the training data.

Suppose we have:

```text
Healthy: 99,500
Failure:    500
```

We could increase the representation of failure examples by duplicating existing minority-class observations.

For example:

```text
Before:
Healthy: 99,500
Failure:    500

After oversampling:
Healthy: 99,500
Failure: 20,000
```

<text color="secondary" size="xs">Illustrative counts, not a recommended universal ratio.</text>

There are two common approaches:

- Random oversampling: duplicate existing minority-class samples.
- Synthetic oversampling: generate artificial minority-class examples.

One popular synthetic technique is <text weight="semibold"><Entity value="SMOTE" category="scientific_concept" disambig="Synthetic Minority Over-sampling Technique for machine learning"/></text> (Synthetic Minority Over-sampling Technique).

SMOTE generates synthetic examples using relationships between minority-class samples.

![SMOTE: before and after oversampling](https://metricgate.com/blogs/assets/blogs/imbalanced-classification-smote-oversampling.png)

**Conceptual illustration of adding synthetic minority-class samples.**

**When is this useful?**

- When the minority class has too few examples.
- When class weighting alone does not provide sufficient results.
- When the synthetic samples are meaningful for the data.

**Important:** Synthetic examples must make sense for the data. SMOTE can generate unrealistic combinations, especially with categorical or operational data that has constraints.

### C. Undersampling

Undersampling reduces the number of majority-class examples.

For example:

```text
Before:
Healthy: 99,500
Failure:    500

After undersampling:
Healthy:  2,000
Failure:    500
```

We remove some healthy examples to make the dataset less imbalanced.

**Advantages:**
- Smaller training dataset.
- Faster training.
- Can reduce majority-class dominance.

**Disadvantages:**
- We discard potentially useful information.
- The model may not learn enough about normal behavior.

### Which technique should you choose?

| Technique | What it does | Main trade-off |
|---|---|---|
| Class weights | Increases minority-class penalty | May need weight tuning |
| Oversampling | Adds minority examples | Can overfit or create unrealistic samples |
| SMOTE | Creates synthetic minority examples | Synthetic samples may be unsuitable |
| Undersampling | Removes majority examples | Loses information |

My recommended starting sequence:

1. Establish a baseline.
2. Try class weighting.
3. Evaluate oversampling.
4. Evaluate undersampling if the dataset is very large.
5. Compare results on the same untouched validation/test data.

### Critical MLOps rule: Never resample before splitting!

This is an important data-leakage risk.

Incorrect:

```text
Entire dataset
     |
     v
Oversampling / SMOTE
     |
     v
Train / Test split
```

Why is this wrong?

If synthetic or duplicated examples are created before splitting, related examples can appear in both training and test sets. The evaluation can become overly optimistic.

Correct:

```text
             Entire dataset
                   |
                   v
             Train / Test split
                /       \
               /         \
              v           v
       Training data    Test data
              |
              v
       Resampling or
       class weighting
              |
              v
          Train model
              |
              v
       Evaluate on the
       untouched test data
```

Resampling belongs inside the training pipeline, and only the training data should be resampled.

---

## 4. ROC-AUC: Understanding how well a model separates classes

Let's understand another important evaluation metric.

A classification model often produces a probability rather than directly producing a Yes/No answer.

For example:

```text
Pod A: 0.95 probability of failure
Pod B: 0.80 probability of failure
Pod C: 0.25 probability of failure
Pod D: 0.05 probability of failure
```

We need a threshold to convert these probabilities into predictions.

For example, with a threshold of 0.5:

```text
Probability >= 0.5  -> Failure
Probability <  0.5  -> Healthy
```

But what happens if we change the threshold?

| Pod | Failure probability | Threshold 0.5 | Threshold 0.8 |
|---|---:|---|---|
| A | 0.95 | Failure | Failure |
| B | 0.80 | Failure | Failure |
| C | 0.25 | Healthy | Healthy |
| D | 0.05 | Healthy | Healthy |

A lower threshold generally identifies more positive cases, but it may also generate more false alarms.

A higher threshold generally reduces false alarms, but may miss more failures.

### What is ROC?

ROC stands for **Receiver Operating Characteristic**.

A ROC curve plots:

- True Positive Rate (TPR) — also called recall or sensitivity.
- False Positive Rate (FPR) — the proportion of actual negative cases incorrectly predicted as positive.

The curve shows how these two rates change as we vary the prediction threshold.

### What is ROC-AUC?

AUC means Area Under the Curve.

ROC-AUC measures how well a model ranks positive examples above negative examples across different thresholds.

Its usual range is 0 to 1:

| ROC-AUC | General interpretation |
|---:|---|
| 0.5 | No better than random ranking |
| 0.6–0.7 | Weak discrimination |
| 0.7–0.8 | Reasonable discrimination |
| 0.8–0.9 | Good discrimination |
| 0.9–1.0 | Excellent discrimination |

These are rough guidelines, not universal quality standards.

![ROC curve showing true positive rate against false positive rate](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-1-0716-3195-9_20/MediaObjects/515045_1_En_20_Fig3_HTML.png)

**ROC curve comparison of classifier performance.**

**Infrastructure example:** Suppose you are building a model to detect suspicious pod behavior. ROC-AUC helps you understand whether the model generally ranks failing pods above healthy pods.

But there's a catch.

With highly imbalanced datasets, ROC-AUC can look impressive even when the model's failure alerts are not very useful.

That brings us to PR-AUC.

---

## 5. PR-AUC: Why it matters for rare events

PR stands for **Precision-Recall**.

The Precision-Recall curve plots:

- Precision — of all positive predictions, how many were actually positive?
- Recall — of all actual positive examples, how many did we identify?

Recall:

```text
Recall = TP / (TP + FN)
```

Precision:

```text
Precision = TP / (TP + FP)
```

Imagine a model raises 1,000 failure alerts:

- 100 are actual failures.
- 900 are false alarms.

Then:

```text
Precision = 100 / 1000 = 10%
```

Suppose there were 500 actual failures in total:

```text
Recall = 100 / 500 = 20%
```

This model catches only 20% of failures, and 90% of its alerts are false alarms.

Not a great operational experience!

![Precision-recall curve and baseline](https://sinyi-chou.github.io/images/prauc/PR_auc_area_plot.png)

**Precision-recall curve overview.**

### ROC-AUC vs PR-AUC

| Metric | What it focuses on | Useful when |
|---|---|---|
| ROC-AUC | Ranking positive vs negative cases across thresholds | You want an overall discrimination measure |
| PR-AUC | Precision and recall for the positive class | Positive cases are rare and false alarms matter |
| Accuracy | Overall fraction of correct predictions | Classes are reasonably balanced and error costs are similar |
| F1 | Balance between precision and recall | You want a single score combining both |

For our pod failure prediction example, PR-AUC is particularly useful because failures are rare.

One important detail: the baseline PR-AUC for a random ranking is approximately the positive-class prevalence. If only 0.5% of observations are failures, that baseline is approximately 0.005.

That makes PR-AUC especially useful when comparing models on the same dataset.

## 6. How do we choose the right metric?

This is an engineering decision, not just a mathematical one.

Think about what happens when the model makes a mistake.

| Scenario | More important concern | Metrics to consider |
|---|---|---|
| Fraud detection | Missing fraudulent transactions | Recall, PR-AUC |
| Security threat detection | Missing attacks, while controlling alert fatigue | Recall, Precision, PR-AUC |
| Pod failure prediction | Missing failures vs too many alerts | Recall, Precision, PR-AUC |
| Spam detection | Avoiding legitimate email being marked as spam | Precision, Recall, F1 |
| Balanced classification task | Overall prediction quality | Accuracy, F1, ROC-AUC |

There is no universally best metric.

For example, consider two different operational requirements:

**Scenario A — High recall priority**

You want to identify as many failing pods as possible, even if the system generates some extra alerts.

You might choose a lower threshold.

**Scenario B — High precision priority**

Your on-call team is overwhelmed with alerts. You want alerts to be highly reliable, even if some failures are missed.

You might choose a higher threshold.

This is the precision-recall trade-off.

### A production-minded way to select a threshold

1. Define the cost of false positives and false negatives.
2. Evaluate model predictions at different thresholds.
3. Choose a threshold that meets the operational requirement.
4. Validate it on data that wasn't used to train or tune the model.
5. Monitor alert volume and missed failures after deployment.

Don't automatically use 0.5. It is just a default decision threshold, not a universal optimum.

## 7. Putting it together with Scikit-learn

Let's create a small classification workflow using class weighting.

We'll use:

- CPU usage
- Memory usage
- Restart count

The target is whether a pod fails.

```python
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

from sklearn.metrics import (
    classification_report,
    confusion_matrix,
    roc_auc_score,
    average_precision_score
)

# Example dataset
df = pd.DataFrame({
    "cpu": [45, 92, 60, 88, 35, 95, 70, 85, 40, 99],
    "memory": [500, 950, 700, 900, 400, 980, 750, 920, 450, 990],
    "restarts": [0, 5, 1, 4, 0, 7, 2, 5, 0, 8],
    "failure": [0, 1, 0, 1, 0, 1, 0, 1, 0, 1]
})

X = df[["cpu", "memory", "restarts"]]
y = df["failure"]

# Split before fitting preprocessing or resampling
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.3,
    random_state=42,
    stratify=y
)

# Build model pipeline
pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("model", LogisticRegression(
        class_weight="balanced",
        max_iter=1000
    ))
])

# Train
pipeline.fit(X_train, y_train)

# Predictions
predictions = pipeline.predict(X_test)
probabilities = pipeline.predict_proba(X_test)[:, 1]

# Evaluate
print(confusion_matrix(y_test, predictions))
print(classification_report(y_test, predictions))

print("ROC-AUC:", roc_auc_score(y_test, probabilities))
print("PR-AUC:", average_precision_score(y_test, probabilities))
```

A few important observations:

1. `stratify=y` attempts to preserve class proportions in the train/test split.
2. The imputer and scaler are fitted using training data only.
3. `class_weight="balanced"` adjusts the importance of classes during training.
4. `predict_proba()` gives probabilities, allowing us to evaluate different thresholds.
5. `average_precision_score()` gives Average Precision (AP), a commonly used summary of the precision-recall curve. It is often reported as a PR-AUC-style metric, though it is not mathematically identical to every PR-curve integration convention.

**Important:** This is a small teaching dataset, not a meaningful performance benchmark. With only ten observations, the results will be unstable. Real evaluation requires a sufficiently large dataset, especially enough minority-class examples.

## 8. How this fits into an MLOps pipeline

This is where your infrastructure experience becomes particularly valuable.

<box border radius="lg" padding={3} gap={2}>
  **Production classification workflow**

  <box background="rgba(59,130,246,0.08)" radius="md" padding={3} align="center">
    **1. Data ingestion**

    <text color="secondary" size="xs">Collect Kubernetes metrics and failure labels</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg"/>
  </box>
  <box background="rgba(59,130,246,0.08)" radius="md" padding={3} align="center">
    **2. Data validation**

    <text color="secondary" size="xs">Check missing values, duplicates, schema and class distribution</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg"/>
  </box>
  <box background="rgba(59,130,246,0.08)" radius="md" padding={3} align="center">
    **3. Train / test split**

    <text color="secondary" size="xs">Keep evaluation data separate</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg"/>
  </box>
  <box background="rgba(59,130,246,0.08)" radius="md" padding={3} align="center">
    **4. Training pipeline**

    <text color="secondary" size="xs">Preprocessing → class weighting / sampling → model training</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg"/>
  </box>
  <box background="rgba(59,130,246,0.08)" radius="md" padding={3} align="center">
    **5. Model evaluation**

    <text color="secondary" size="xs">Precision, recall, F1, ROC-AUC, PR-AUC and threshold selection</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg"/>
  </box>
  <box background="rgba(34,197,94,0.09)" radius="md" padding={3} align="center">
    **6. Model registry and deployment**

    <text color="secondary" size="xs">Version, approve, package and deploy the model</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg"/>
  </box>
  <box background="rgba(34,197,94,0.09)" radius="md" padding={3} align="center">
    **7. Production monitoring**

    <text color="secondary" size="xs">Monitor prediction distribution, alert volume, data drift and actual model quality</text>
  </box>
</box>

Some monitoring considerations:

- Has the percentage of failures changed?
- Is the model generating too many false alerts?
- Are actual failures being missed?
- Has the distribution of CPU or memory inputs changed?
- Is the model's precision or recall degrading?

One important distinction: you need actual ground-truth labels to measure production precision and recall. Without knowing what really happened, you can monitor input and prediction distributions, but you cannot reliably calculate those quality metrics.

---

## 9. Lesson 10 — Knowledge check

Let's test your understanding. These questions are designed around practical ML engineering scenarios.

1. **Why do we create a baseline model before building a complex model?**
   - A. To make the dataset larger
   - B. To establish a simple reference for comparison
   - C. To eliminate the need for testing
   - D. To increase the number of parameters

2. **A dataset has 99.5% healthy pods and 0.5% failures. A model predicts every pod as healthy. What is true?**
   - A. It has perfect recall
   - B. It has 99.5% accuracy but fails to detect failures
   - C. It has perfect precision and recall
   - D. It is necessarily a good model

3. **What does class_weight='balanced' do?**
   - A. Duplicates minority-class records
   - B. Removes majority-class records
   - C. Adjusts the training penalty based on class frequency
   - D. Changes the test dataset

4. **When should you apply oversampling or SMOTE?**
   - A. Before splitting the entire dataset
   - B. Only to the training portion
   - C. To both training and test data
   - D. After deploying the model

5. **What does ROC-AUC measure?**
   - A. The total number of correct predictions
   - B. How well the model ranks positive examples above negative examples across thresholds
   - C. The model's training time
   - D. The percentage of missing values

6. **Why is PR-AUC particularly useful for rare positive events?**
   - A. It ignores false positives
   - B. It focuses on the precision-recall trade-off for the positive class
   - C. It always gives a higher score than ROC-AUC
   - D. It measures GPU utilization

7. **A failure-detection system must catch as many actual failures as possible, even if it raises extra alerts. Which metric should you prioritize?**
   - A. Recall
   - B. Precision
   - C. Accuracy
   - D. Training loss only

8. **Your model produces too many false failure alerts. What is one sensible action?**
   - A. Always lower the threshold
   - B. Investigate precision and evaluate a higher threshold
   - C. Resample the test data
   - D. Ignore false positives

### Knowledge check answer explanations

1. **Correct answer: B.** A baseline gives us a reference point. A complex model should demonstrate measurable improvement over it.
2. **Correct answer: B.** It achieves 99.5% accuracy but detects zero failures. This is the classic imbalanced-data problem.
3. **Correct answer: C.** Class weighting gives different importance to errors involving different classes.
4. **Correct answer: B.** Resampling must happen only on training data to avoid leakage into evaluation.
5. **Correct answer: B.** ROC-AUC measures ranking/discrimination ability across thresholds.
6. **Correct answer: B.** Precision and recall expose the usefulness of positive predictions when positives are rare.
7. **Correct answer: A.** Recall measures the proportion of actual failures that the model catches.
8. **Correct answer: B.** A higher threshold may reduce false positives, but you must evaluate the resulting recall trade-off.

---

## 10. Practical exercise — Kubernetes failure prediction

Now let's apply the lesson.

You have a dataset with 100,000 pod observations:

| Category | Count |
|---|---:|
| Healthy pods | 98,000 |
| Failed pods | 2,000 |

Your model currently achieves:

| Metric | Score |
|---|---:|
| Accuracy | 98% |
| Precision | 20% |
| Recall | 15% |
| ROC-AUC | 0.82 |
| PR-AUC | 0.12 |

Answer these questions:

1. Why is 98% accuracy not enough to conclude that the model is good?
2. What does 15% recall tell you about missed pod failures?
3. What does 20% precision tell you about the alerts?
4. Why might PR-AUC be more informative than ROC-AUC for this dataset?
5. Which technique would you try first: class weights, oversampling, or undersampling? Explain why.
6. What would happen if you applied SMOTE before splitting your dataset?
7. If your on-call team wants fewer false alerts, which metric should you pay particular attention to?
8. How would you select and validate a prediction threshold before deploying the model?

You don't need to write Python code for this exercise. I want to see your engineering reasoning.

---

## Lesson 10 summary

| Concept | Remember this |
|---|---|
| Baseline | Simple reference model used for comparison |
| Class imbalance | One class has significantly more observations than another |
| Class weights | Give minority-class errors greater importance |
| Oversampling | Increase minority-class representation |
| Undersampling | Reduce majority-class representation |
| SMOTE | Generate synthetic minority-class examples |
| ROC-AUC | Measures ranking discrimination across thresholds |
| PR-AUC | Summarizes the precision-recall trade-off |
| Threshold | Controls the conversion from probability to predicted class |

**The most important takeaway:** A model with high accuracy is not necessarily a useful model. In production, the cost of missing an event and the cost of generating a false alert matter much more than a single headline score.

Once you've answered the knowledge check and practical exercise, send me your answers. I'll review them, explain any gaps, and then we'll move to **Lesson 11: Responsible AI and ML Reproducibility**.
