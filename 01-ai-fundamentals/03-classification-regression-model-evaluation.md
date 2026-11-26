# 03 — Classification, Regression, and Model Evaluation

> **Learning path:** AI Infrastructure / MLOps  
> **Phase:** 1 — Artificial Intelligence Fundamentals  
> **Lesson:** 03  
> **Status:** Complete — knowledge check 7/8; practical calculations reviewed  
> **Prerequisites:** Lessons 01 and 02 — completed

## Learning objectives

By the end of this lesson, you should be able to:

- Distinguish classification from regression.
- Explain binary and multiclass classification.
- Read a confusion matrix.
- Explain true positives, false positives, true negatives, and false negatives.
- Calculate accuracy, precision, recall, and F1 score.
- Explain why accuracy can be misleading for imbalanced datasets.
- Understand classification probabilities and decision thresholds.
- Choose evaluation metrics for infrastructure failure prediction.

## 1. Classification

Classification predicts a category or class.

Example: predict whether a Kubernetes pod will fail within the next 10 minutes.

- 0 — No failure
- 1 — Failure

This is **binary classification** because there are two possible classes.

Other examples include spam/not spam and fraudulent/legitimate transactions.

### Multiclass classification

Multiclass classification chooses among more than two classes.

Example: predict a pod state:

- Running
- Pending
- CrashLoopBackOff
- ImagePullBackOff

The output is a category, not an arbitrary continuous measurement.

## 2. Regression

Regression predicts a numerical value, often a continuous quantity.

Example: predict CPU utilization 10 minutes from now:

- Input: current CPU, memory, request rate, and recent utilization history.
- Output: predicted CPU utilization of 91.5%.

Other infrastructure examples:

- Forecasting memory consumption.
- Predicting application response time.
- Forecasting cloud infrastructure costs.
- Estimating GPU memory requirements.

### Classification vs regression

| Aspect | Classification | Regression |
|---|---|---|
| Output | Category/class | Numerical value |
| Example | Pod failure: Yes/No | CPU utilization: 91.5% |
| Common algorithms | Logistic regression, decision trees | Linear regression, regression trees |
| Evaluation examples | Precision, recall, F1 | MAE, MSE, RMSE |

**Important:** Despite its name, logistic regression is commonly used for classification.

## 3. Confusion matrix

A confusion matrix compares predicted classes with actual outcomes.

For a binary pod-failure classifier:

| | Actual failure | Actual healthy |
|---|---:|---:|
| Predicted failure | 18 — TP | 7 — FP |
| Predicted healthy | 2 — FN | 73 — TN |

The four outcomes:

- **True Positive (TP):** Predicts failure, and failure occurs.
- **False Positive (FP):** Predicts failure, but the pod stays healthy. This is a false alarm.
- **False Negative (FN):** Predicts healthy, but the pod fails. This is a missed failure.
- **True Negative (TN):** Predicts healthy, and the pod stays healthy.

For infrastructure monitoring, false negatives can be costly because real failures are missed. False positives also matter because excessive alerts create operational noise.

## 4. Evaluation metrics

Using TP = 18, FP = 7, FN = 2, and TN = 73:

### Accuracy

The proportion of all predictions that were correct.

```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
         = (18 + 73) / 100
         = 91%
```

### Precision

Among all predictions of failure, how many were actually failures?

```
Precision = TP / (TP + FP)
          = 18 / (18 + 7)
          = 72%
```

Higher precision generally means fewer false alarms.

### Recall

Among all actual failures, how many did the model detect?

```
Recall = TP / (TP + FN)
       = 18 / (18 + 2)
       = 90%
```

Higher recall means fewer missed failures.

### F1 score

The harmonic mean of precision and recall.

```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
   = 2 × (0.72 × 0.90) / (0.72 + 0.90)
   = 0.80, or 80%
```

F1 is useful when you want a balance between precision and recall.

## 5. Why accuracy can be misleading

Imagine monitoring 1,000 pods:

- 990 remain healthy.
- 10 fail.

A model that predicts every pod as healthy achieves 99% accuracy, but detects none of the failures.

| Outcome | Count |
|---|---:|
| TP | 0 |
| FP | 0 |
| FN | 10 |
| TN | 990 |

- Accuracy = 99%
- Recall = 0%
- Precision is undefined because the model made no positive predictions.

This is a class-imbalance problem: healthy pods greatly outnumber failed pods.

Therefore, accuracy alone is insufficient for evaluating a failure-detection model.

## 6. Choosing a metric

| Operational goal | Metric to consider |
|---|---|
| Reduce false failure alerts | Precision |
| Catch as many real failures as possible | Recall |
| Balance false alarms and missed failures | F1 |
| Overall proportion of correct predictions | Accuracy |

There is no universally best metric. Choose based on the cost of false positives and false negatives. An early-warning system may prioritize recall, but too many false alarms can overwhelm an operations team.

## 7. Classification probabilities and thresholds

A classifier may output a probability or score before assigning a class.

Example:

```
Estimated failure probability = 0.82
```

With a threshold of 0.5:

- Score >= 0.5: predict failure.
- Score < 0.5: predict healthy.

Lowering the threshold generally increases the number of positive predictions. This can increase recall, but may also increase false positives.

Raising the threshold generally makes positive predictions less frequent. This can reduce false alarms, but may increase missed failures.

A score is not automatically a perfectly calibrated real-world probability; calibration must be evaluated separately.

## 8. Practical exercise — reviewed

Given:

- TP = 40
- FP = 10
- FN = 20
- TN = 130
- Total = 200

| Metric | Calculation | Result |
|---|---|---:|
| Accuracy | (40 + 130) / 200 | 85% |
| Precision | 40 / (40 + 10) | 80% |
| Recall | 40 / (40 + 20) | 66.67% |
| F1 | 2 × (0.80 × 0.6667) / (0.80 + 0.6667) | 72.73% |

## 9. Knowledge check

**Result: 7/8 (87.5%).**

The one correction was the recall question:

Recall measures the proportion of actual positive cases that the model correctly detects.

```
Recall = TP / (TP + FN)
```

The other concepts were answered correctly.

## 10. Interview prompts

1. What is the difference between classification and regression?
2. What is binary classification? How does it differ from multiclass classification?
3. Explain TP, FP, FN, and TN using a pod-failure prediction example.
4. What is the difference between precision and recall?
5. Why can a model with 99% accuracy still be poor?
6. What is the F1 score, and when is it useful?
7. How does changing a classification threshold affect precision and recall?
8. Which metric would you prioritize for an infrastructure early-warning system, and why?

## 11. Inference: where it fits

This lesson used classification scores and thresholds, but it did not cover the mechanics of inference in depth.

Inference is introduced in Phase 1 as the act of using a trained model to produce predictions. It will receive dedicated, deeper coverage later:

- **Phase 6 — LLM Inference and Serving:** inference engines, request lifecycle, prefill/decode, batching, KV cache, CPU/GPU execution, latency and throughput.
- **Phase 8 — AI Application Engineering:** APIs, asynchronous inference, queues, workers, and streaming.
- **Phase 11 — Production AI on Kubernetes:** deploying, scaling, monitoring, and troubleshooting inference services.

## 12. Next lesson

**Phase 1 — Lesson 04: Overfitting, Underfitting, Bias, and Variance (taught; assessment 6/8, exercise pending)**

Planned topics:

- Generalization and unseen data.
- Underfitting and overfitting.
- Training vs validation performance.
- Bias and variance intuition.
- Regularization and early stopping.
- How to recognize overfitting in model metrics.

## Progress tracker

| Phase | Lesson | Status |
|---|---|---|
| 1 — AI Fundamentals | 01 — What Is Artificial Intelligence? | Complete — 6/6 |
| 1 — AI Fundamentals | 02 — Traditional Machine Learning | Complete — knowledge check and exercise reviewed |
| 1 — AI Fundamentals | 03 — Classification, Regression, and Model Evaluation | Complete — 7/8; exercise reviewed |
| 1 — AI Fundamentals | 04 — Overfitting, Underfitting, Bias, and Variance | Taught — 6/8; practical exercise pending |
