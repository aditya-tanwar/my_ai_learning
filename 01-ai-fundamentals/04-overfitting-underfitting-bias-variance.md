# 04 — Overfitting, Underfitting, Bias, and Variance

> **Visual companion:** [Open the HTML lesson](./04-overfitting-underfitting-bias-variance.html)

> **Learning path:** AI Infrastructure / MLOps  
> **Phase:** 1 — Artificial Intelligence Fundamentals  
> **Lesson:** 04  
> **Status:** Taught — knowledge check 6/8; practical exercise pending  
> **Prerequisites:** Lessons 01–03

## Learning objectives

- Explain generalization and why unseen data matters.
- Distinguish underfitting from overfitting.
- Understand high bias and high variance intuitively.
- Read training and validation loss trends.
- Explain regularization and early stopping.
- Connect model quality to MLOps and infrastructure workflows.

## 1. Generalization

**Generalization** is a model's ability to make good predictions on data it has never seen during training.

A model that memorizes training examples but fails on new data has not generalized well.

The goal of machine learning is not merely to perform well on training data. It is to learn patterns that remain useful on representative, unseen data.

## 2. Underfitting

Underfitting occurs when a model has not learned the underlying patterns adequately. It may be too simple, poorly configured, trained insufficiently, or supplied with inadequate features.

Typical symptom:

- Poor performance on training data.
- Poor performance on validation data.

Example: a pod-failure model assumes every pod above 80% CPU will fail, even though workload type, memory pressure, traffic, and application behavior also matter.

Possible causes include:
- Model too simple.
- Insufficient or unsuitable features.
- Insufficient training.
- Poor data representation.

## 3. Overfitting

Overfitting occurs when a model learns training-specific details, noise, or accidental correlations instead of patterns that generalize.

Typical symptom:

- Very good training performance.
- Significantly worse validation performance.

Example: a model learns a correlation specific to one Kubernetes cluster and performs poorly on other clusters with different workloads.

Possible causes include:
- Excessive model complexity for the amount of data.
- Too little representative training data.
- Irrelevant or misleading features.
- Noisy training data.
- Training for too long in some learning setups.

## 4. Compare underfitting, good fit, and overfitting

| Situation | Training performance | Validation performance |
|---|---|---|
| Underfitting | Poor | Poor |
| Good fit | Good | Good |
| Overfitting | Excellent | Poor |

A large gap between training and validation performance is a warning sign of poor generalization. The gap alone is not a complete diagnosis; also inspect data quality, split design, and metric choice.

## 5. Bias and variance

**Bias** is error associated with overly simplistic assumptions. High bias commonly leads to underfitting.

Example: assuming CPU utilization alone determines pod failure, regardless of workload or other telemetry.

**Variance** describes how sensitive a learned model is to the particular training sample. High variance commonly leads to overfitting.

Example: a model learns cluster-specific noise and fails to perform consistently on other clusters.

| Concept | Intuition | Common association |
|---|---|---|
| High bias | Model is too simplistic | Underfitting |
| High variance | Model is too sensitive to training data | Overfitting |
| Balanced | Learns useful patterns that generalize | Better generalization |

A simple memory aid:
- High bias: the model hasn't captured enough of the real pattern.
- High variance: the model has captured too much of the quirks in its training examples.

## 6. Detecting overfitting with training and validation loss

Loss is a numerical measure of prediction error for a chosen objective. Lower loss is generally better when comparing the same loss function on comparable datasets.

Illustrative example:

| Epoch | Training loss | Validation loss |
|---:|---:|---:|
| 1 | 0.80 | 0.85 |
| 2 | 0.55 | 0.60 |
| 3 | 0.35 | 0.42 |
| 4 | 0.20 | 0.38 |
| 5 | 0.10 | 0.55 |
| 6 | 0.05 | 0.82 |

Training loss keeps falling, but validation loss begins rising after epoch 4. This suggests the model is fitting the training data increasingly well while generalization is getting worse.

The best checkpoint may be around the epoch with the best validation performance, subject to the selected metric and validation procedure.

## 7. Ways to reduce overfitting

### Regularization

Regularization discourages unnecessarily complex solutions by adding a penalty to the training objective. L1 and L2 are common forms. The detailed mathematics will be covered later where useful.

### Early stopping

Monitor validation performance and stop training when it stops improving. Save and use the best checkpoint rather than automatically using the final epoch.

### More representative data

Collect examples across different clusters, workloads, node types, traffic patterns, and operating conditions. More data helps only when it is relevant and representative.

### Feature selection

Remove irrelevant, unstable, or leakage-prone features. A random generated pod name is unlikely to be a useful general signal.

### Reduce model complexity

Choose a model whose complexity is appropriate for the dataset and task. More complexity is not automatically better.

These are techniques to evaluate, not guarantees. Recheck performance on held-out data after changes.

## 8. Infrastructure and MLOps perspective

An example training workflow:

1. Read a versioned training dataset from object storage.
2. Run a training job in Kubernetes using CPU or GPU resources.
3. Record training and validation metrics.
4. Save model checkpoints.
5. Select a checkpoint based on validation performance.
6. Record the dataset version, code version, configuration, and artifacts.
7. Register the candidate model.
8. Require evaluation gates before production promotion.

MLOps concerns:

- **Compute:** unnecessary training consumes CPU/GPU time and money.
- **Memory:** model and dataset size affect RAM and VRAM requirements.
- **Storage:** preserve checkpoints, datasets, and experiment artifacts.
- **Observability:** track loss, task metrics, training duration, and resource use.
- **Reproducibility:** record data versions, code, configuration, and model artifacts.
- **Release quality:** do not promote a model based only on training accuracy.

MLflow and model registries will be covered in the MLOps phase.

## 9. Practical exercise

Three models predict Kubernetes pod failures:

| Model | Training accuracy | Validation accuracy |
|---|---:|---:|
| Model A | 62% | 60% |
| Model B | 94% | 91% |
| Model C | 99% | 68% |

Answer:

1. Which model is underfitting?
2. Which model is overfitting?
3. Which model currently generalizes best?
4. Why shouldn't Model C be selected just because it has the highest training accuracy?
5. What techniques would you try to improve Model C?

## 10. Knowledge check

**Result: 6/8.** Two answers need review:

- Question 1: Underfitting is the correct diagnosis when both training and validation accuracy are poor.
- Question 8: Model B is the best choice from the supplied results because it has strong training and validation performance with a relatively small gap.

The remaining six answers were correct.

## 11. Interview prompts

1. What is the difference between overfitting and underfitting?
2. How would you detect overfitting during training?
3. What is early stopping?
4. Why monitor validation loss?
5. How can regularization help a model generalize?
6. How would you prevent an overfitted model from being promoted through an ML CI/CD pipeline?

## 12. Next lesson

**Phase 1 — Lesson 05: Model Training Fundamentals — Parameters, Weights, Biases, Loss, and Optimization**

Planned topics:
- What a model parameter is.
- Weights and biases.
- Parameters versus hyperparameters.
- Prediction and loss.
- How optimization updates parameters.
- Gradient descent intuition.
- How these ideas connect to neural networks and training compute.

## Progress tracker

| Phase | Lesson | Status |
|---|---|---|
| 1 — AI Fundamentals | 01 — What Is Artificial Intelligence? | Complete — 6/6 |
| 1 — AI Fundamentals | 02 — Traditional Machine Learning | Complete |
| 1 — AI Fundamentals | 03 — Classification, Regression, and Model Evaluation | Complete — 7/8; exercise reviewed |
| 1 — AI Fundamentals | 04 — Overfitting, Underfitting, Bias, and Variance | Taught — 6/8; practical exercise pending |
| 1 — AI Fundamentals | 05 — Model Training Fundamentals | Next |
