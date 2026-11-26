# 02 — Traditional Machine Learning

> **Visual companion:** [Open the HTML lesson](./02-traditional-machine-learning.html)

> **Learning path:** AI Infrastructure / MLOps  
> **Phase:** 1 — Artificial Intelligence Fundamentals  
> **Lesson:** 02  
> **Status:** Complete — knowledge check and practical exercise reviewed  
> **Prerequisite:** Lesson 01 — What Is Artificial Intelligence? (completed, 6/6)

## Learning objectives

By the end of this lesson, you should be able to:

- Explain what a dataset, sample, feature, label, and feature vector are.
- Distinguish features (inputs) from labels (targets).
- Explain the purpose of training, validation, and test datasets.
- Describe a typical traditional ML workflow.
- Explain the basic training loop and distinguish training from inference.
- Recognize data leakage and why it invalidates evaluation.

## 1. What is traditional Machine Learning?

Traditional ML refers to machine-learning approaches that learn patterns from data, often using algorithms such as:

- Linear regression
- Logistic regression
- Decision trees
- Random forests
- Gradient-boosted trees
- Support vector machines (SVM)

These methods are used for prediction, classification, anomaly detection, and forecasting. The algorithms themselves will be covered in later lessons.

### Running example: Kubernetes pod failure prediction

Imagine collecting historical data from Kubernetes clusters. The goal is to predict whether a pod is likely to fail based on resource utilization and behavior.

## 2. What is a dataset?

A **dataset** is an organized collection of data used to train, validate, or test a machine-learning model.

Example:

| CPU usage | Memory usage | Restart count | Error rate | Pod failed? |
|---:|---:|---:|---:|---|
| 35% | 40% | 0 | 0.1% | No |
| 92% | 95% | 5 | 18% | Yes |
| 55% | 60% | 0 | 0.5% | No |
| 88% | 90% | 3 | 12% | Yes |
| 45% | 50% | 0 | 0.2% | No |

Each row is a **record**, **observation**, or **sample**. Each column represents a variable.

A dataset can come from spreadsheets, databases, CSV, JSON, logs, telemetry platforms, images, audio, or other sources.

### Important terminology

| Term | Meaning |
|---|---|
| Dataset | The collection of observations |
| Sample / observation | One individual record or row |
| Feature | An input variable used by the model |
| Label / target | The answer the model is trained to predict |
| Feature vector | The collection of feature values for one sample |

## 3. Features vs labels

### Features — inputs

Features are the information provided to a model to help it make a prediction.

For this example:

- CPU usage
- Memory usage
- Restart count
- Error rate

We can represent the inputs as:

```text
X = [CPU, Memory, Restarts, ErrorRate]
```

Here, X represents the input features.

### Label — expected answer

The label is the value we want the model to predict:

```text
y = PodFailed
```

For example, the label can be 1 when the pod failed and 0 when it did not.

**Remember:**
- Features = what the model knows before making its prediction.
- Label = the answer we want the model to learn to predict.

During supervised learning, the model uses many examples of X paired with y to learn a relationship between them.

Features must be available at prediction time. If information that only becomes available after a pod fails is included, the model may receive an unfair advantage. This is one form of data leakage.

## 4. Training, validation, and test datasets

A model should not be evaluated only on the data it learned from. We separate the available labelled data into portions.

Illustrative split for 10,000 observations:

| Split | Percentage | Observations | Purpose |
|---|---:|---:|---|
| Training | 70% | 7,000 | Learn model parameters |
| Validation | 15% | 1,500 | Compare models and tune hyperparameters |
| Test | 15% | 1,500 | Final evaluation on held-out data |

These proportions are examples, not universal rules. The appropriate split depends on the problem, dataset size, class balance, and whether observations have a time or group structure.

### 4.1 Training dataset

This is the data the algorithm learns from. It uses the feature values and known labels to adjust its model parameters.

Think of training as **learning from practice examples**.

### 4.2 Validation dataset

This data is used during model development to evaluate and compare model versions or configurations.

It helps tune **hyperparameters** — settings chosen by the engineer rather than learned directly as model parameters.

Examples:
- Decision-tree maximum depth.
- Learning rate.
- Number of trees in a random forest.
- Regularization strength.

Think of validation as **checking progress and adjusting the learning strategy**.

### 4.3 Test dataset

The test dataset is held back until final evaluation. It should not be used to fit the model, select configurations, or repeatedly tune it.

After development, evaluate the chosen model on the test dataset to estimate how it performs on previously unseen data.

Think of testing as **the final examination**.

| Dataset | Main purpose | Used to update learned model parameters? |
|---|---|---|
| Training | Learn patterns | Yes |
| Validation | Compare and tune models | Not directly |
| Test | Final evaluation | No |

Validation can influence model selection, so it is not a completely untouched final evaluation set. The test set should remain held out until the final evaluation.

## 5. The traditional ML workflow

1. **Data collection:** Gather historical Kubernetes metrics, logs, and pod outcomes.
2. **Data preparation:** Clean missing values, fix inconsistent formats, remove duplicates, and prepare the data.
3. **Feature and label selection:** Select useful inputs and define the target outcome.
4. **Dataset splitting:** Separate observations into training, validation, and test sets.
5. **Model training:** Give the training data to an ML algorithm so it can learn patterns.
6. **Model validation and tuning:** Compare model performance and adjust hyperparameters using validation data.
7. **Final evaluation:** Evaluate the selected model using the held-out test dataset.
8. **Model deployment:** Package the model and expose it through an API or inference service.
9. **Monitoring:** Monitor prediction quality, errors, latency, resource consumption, and changing data patterns.

### Map this to DevOps

| ML workflow | Familiar tools / concepts |
|---|---|
| Data collection | Prometheus, logs, databases, telemetry pipelines |
| Data preparation | Python scripts, Pandas, ETL jobs |
| Model training | Python ML code running as a job/container |
| Experiment tracking | MLflow |
| Model artifact storage | Object storage or model registry |
| Model deployment | Docker, Kubernetes, FastAPI |
| Monitoring | Prometheus, Grafana, Datadog plus model-quality metrics |

Much of the infrastructure lifecycle is familiar to DevOps. New concerns include data quality, training, model evaluation, and model-specific behavior.

## 6. What happens during model training?

Suppose the training dataset contains 7,000 examples. The algorithm receives:
- Feature values: CPU, memory, restarts, and error rate.
- Expected labels: whether each pod failed.

A simplified training loop:

1. The model receives input features.
2. It produces a prediction.
3. The prediction is compared with the known label.
4. A mathematical function measures prediction error. This is called the **loss function**.
5. The learning algorithm adjusts model parameters to reduce that loss.
6. The process repeats across many examples.

Example: the expected label is failure (1), but the model estimates a failure probability of 0.65. The learning algorithm uses the error signal to adjust its parameters.

The details vary by algorithm. Later, when studying neural networks, we will examine weights, loss functions, gradients, and optimization in more detail.

## 7. Training vs inference

| Training | Inference |
|---|---|
| Learns from historical data | Makes predictions using learned patterns |
| Updates model parameters | Uses existing model parameters |
| Often computationally heavier | Often optimized for latency and throughput |
| Commonly runs as a batch or scheduled job | Commonly runs as an API or online service |
| Produces a trained model artifact | Produces predictions |

For the Kubernetes example:
- **Training:** Process historical pod data and train a failure prediction model.
- **Inference:** Receive current pod metrics and return a failure probability.

The model may be trained periodically and serve predictions continuously.

## 8. Data leakage

**Data leakage** occurs when information that would not legitimately be available for a prediction influences training or evaluation.

Example: to predict whether a pod will fail, a dataset includes a feature indicating that the pod has already entered `CrashLoopBackOff`. The model has effectively been given the answer.

Other examples:
- Using future measurements to predict a past event.
- Allowing test-set information to influence training or model selection.
- Preprocessing the full dataset before splitting when that causes information from validation or test data to leak into training.

For a goal such as predicting pod failure 10 minutes in advance, features must come from data available before that prediction point.

Dataset design is therefore not just a data-science detail. It determines whether reported model performance is meaningful.

## 9. From a trained model to a prediction

After training, the model is saved as an artifact. Depending on the model implementation, this contains learned parameters and other state needed to make predictions.

A serving application loads the artifact and accepts new feature values:

```text
New pod metrics
      |
      v
Feature preparation
      |
      v
Trained model
      |
      v
Prediction: 0.87
      |
      v
Application decision
```

A score of 0.87 could represent an estimated 87% probability of failure, assuming the model is configured to output calibrated probabilities.

The application might use that score to trigger an alert. The model does not automatically restart a pod unless an action system is built around it.

## 10. Key takeaways

1. A dataset is a collection of observations; each row is a sample.
2. Features are model inputs; labels are the targets to predict.
3. Training data fits model parameters.
4. Validation data helps compare models and tune hyperparameters.
5. Test data is held back for final evaluation.
6. Training learns patterns; inference uses the trained model to make predictions.
7. Data leakage can make model evaluation misleading.
8. A production ML workflow extends beyond training to deployment and monitoring.

## 11. Practical exercise

You are building a model to predict whether a Kubernetes pod will enter `CrashLoopBackOff` within the next 10 minutes.

Available telemetry:
- CPU usage: 85%
- Memory usage: 91%
- Restart count: 2
- Container error rate: 12%
- Pod failed within next 10 minutes?: Yes

Answer:
1. Which columns should be features?
2. Which column should be the label?
3. Which dataset should the algorithm learn from?
4. Which dataset should be used to tune the model?
5. Which dataset should be reserved for final evaluation?
6. Should the pod's current `CrashLoopBackOff` status be a feature if predicting whether it will enter that state in the next 10 minutes? Explain why.

## 12. Knowledge check

**Status: Completed — knowledge check and practical exercise reviewed.**

1. What is a dataset?
   - A. A trained model
   - B. A collection of observations or records
   - C. A GPU memory allocation
   - D. A model-serving API

2. In the pod-failure example, CPU usage, memory usage, and restart count are:
   - A. Labels
   - B. Predictions
   - C. Features
   - D. Loss functions

3. What is the label in a supervised pod-failure model?
   - A. CPU usage
   - B. Memory usage
   - C. Pod failure outcome
   - D. Restart count

4. Which dataset is used to fit or learn the model's parameters?
   - A. Training dataset
   - B. Validation dataset
   - C. Test dataset
   - D. Production dataset only

5. Why do we need a validation dataset?
   - A. To store model backups
   - B. To tune and compare model configurations
   - C. To replace training data
   - D. To expose the inference API

6. When should you use the test dataset?
   - A. During every training update
   - B. To tune every hyperparameter
   - C. For final evaluation after model selection
   - D. To replace missing training labels

7. What is data leakage?
   - A. Losing a dataset from object storage
   - B. Using information in training/evaluation that would not legitimately be available
   - C. Compressing a CSV file
   - D. Splitting a dataset into three portions

8. What is inference?
   - A. Collecting raw data
   - B. Training a model from scratch
   - C. Using a trained model to make predictions
   - D. Selecting validation metrics

**Answer format:** `1-B, 2-C, 3-C, 4-A, 5-B, 6-C, 7-B, 8-C`

## 13. Interview prompts

1. What is the difference between a feature and a label?
2. Why do we split data into training, validation, and test datasets?
3. What is the difference between a model parameter and a hyperparameter?
4. Explain the end-to-end workflow for building a traditional ML model.
5. What is data leakage? Give an infrastructure example.
6. What is the difference between training and inference?
7. How would you deploy a trained model as a service on Kubernetes?

## 14. Next lesson

**Phase 1 — Lesson 03: Classification, Regression, and Model Evaluation (completed)**

Planned topics:
- Classification vs regression.
- Binary and multi-class classification.
- Continuous numeric predictions.
- Confusion matrix.
- Accuracy, precision, recall, and F1 score.
- Why accuracy can be misleading for imbalanced datasets.
- Choosing metrics for infrastructure failure prediction.

## Progress tracker

| Phase | Lesson | Status |
|---|---|---|
| 1 — AI Fundamentals | 01 — What Is Artificial Intelligence? | Complete — 6/6 |
| 1 — AI Fundamentals | 02 — Traditional Machine Learning | Complete — knowledge check and exercise reviewed |
| 1 — AI Fundamentals | 03 — Classification, Regression, and Model Evaluation | Complete — 7/8; exercise reviewed |
