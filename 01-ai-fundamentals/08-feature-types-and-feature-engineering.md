# Phase 1 — AI Fundamentals

## Lesson 8: Feature Types and Feature Engineering

**New lesson** · **AI + MLOps foundations**

Let's get into it! 🚀

In the previous lessons, you learned what features are, how models train, and how neural networks learn. Now we're going one level deeper:

**How do we convert raw data into useful information that a machine learning model can actually learn from?**

This is called *feature engineering*, and it's one of the most important skills in practical machine learning.

We'll use examples from infrastructure and Kubernetes so you can connect the AI concepts to systems you already understand.

---

## 1. Why feature types matter

Imagine we're building an ML model that predicts whether a Kubernetes pod will experience a resource-related failure in the next 10 minutes.

Our raw dataset might look like this:

| CPU usage | Restarts | Region | Severity | Pod type |
|---:|---:|---|---|---|
| 82.5% | 3 | ap-south-1 | High | frontend |
| 35.2% | 0 | us-east-1 | Low | backend |
| 91.0% | 5 | ap-south-1 | Critical | worker |
| 48.7% | 1 | eu-west-1 | Medium | frontend |

Look closely. These columns don't all represent the same kind of information.

- CPU usage is a measurement.
- Restarts are a count.
- Region is a category.
- Severity is an ordered category.
- Pod type is a category.

A model needs these values represented in a form it can process. Understanding their types helps us choose the right transformations.

## 2. The four major feature types

### A. Numerical features

Values represented as numbers.

| Type | Example | Description |
|---|---:|---|
| **Continuous** | 82.5 | CPU utilization (%) — can take many values within a range |
| **Discrete** | 3 | Restart count — usually countable whole-number values |

Examples:
- Continuous: latency, temperature, memory usage, CPU utilization.
- Discrete: pod count, HTTP request count, number of restarts.

### B. Categorical features

Values representing groups or labels rather than measurements.

| Type | Example | Description |
|---|---|---|
| **Nominal** | Region | ap-south-1, us-east-1 — no natural order |
| **Ordinal** | Severity | Low, Medium, High — has a meaningful order |

Examples:
- Nominal: cloud region, instance family, namespace, operating system.
- Ordinal: low/medium/high, bronze/silver/gold, severity levels.

There's also a useful special case:

**Boolean features:** values such as `true/false`, `0/1`, or `healthy/unhealthy`.

For example, `is_node_ready` could be represented as `1` for true and `0` for false.

### One important distinction

A number isn't automatically a numerical feature.

For example:

- `region_id = 1` means ap-south-1.
- `region_id = 2` means us-east-1.
- `region_id = 3` means eu-west-1.

These are identifiers, not measurements. The model shouldn't assume region 3 is greater than region 1.

That brings us to encoding.

---

## 3. Encoding: converting categories into numbers

Most traditional ML algorithms expect numerical input. We therefore need to convert categorical values into numerical representations.

Two important techniques are **one-hot encoding** and **ordinal encoding**.

### A. One-hot encoding

Suppose we have a feature called `region`:

| Original region |
|---|
| ap-south-1 |
| us-east-1 |
| eu-west-1 |
| ap-south-1 |

We create a separate column for each category:

| ap-south-1 | us-east-1 | eu-west-1 |
|---:|---:|---:|
| 1 | 0 | 0 |
| 0 | 1 | 0 |
| 0 | 0 | 1 |
| 1 | 0 | 0 |

**How one-hot encoding works**

`region = us-east-1`

| ap-south-1 | us-east-1 | eu-west-1 |
|---:|---:|---:|
| 0 | **1** | 0 |

Why is this useful?

Because the model can distinguish categories without creating a fake numerical ranking between them.

**The downside:** if a feature has thousands of unique categories, one-hot encoding can create thousands of columns. This is called high cardinality.

For example, encoding every unique pod name could produce an enormous feature space.

### B. Ordinal encoding

Now suppose we have a severity feature:

| Severity |
|---|
| Low |
| Medium |
| High |
| Critical |

We can map it like this:

| Severity | Encoded value |
|---|---:|
| Low | 0 |
| Medium | 1 |
| High | 2 |
| Critical | 3 |

Here, numerical ordering makes sense because severity has a meaningful order.

But be careful: encoding does not necessarily mean the difference between Low and Medium is mathematically equal to the difference between High and Critical.

Use ordinal encoding when the order is meaningful and the model can sensibly use that representation.

### One practical rule

- Nominal category → usually one-hot encoding.
- Ordinal category → ordinal encoding when appropriate.
- Numerical feature → usually retain its numerical meaning, with transformations where useful.

---

## 4. Feature engineering: creating useful signals

Now we get to the interesting part.

Feature engineering means creating, transforming, or combining existing data into more useful features for a model.

Imagine Prometheus gives us these raw metrics:

```text
cpu_usage = 850 millicores
cpu_request = 1000 millicores

memory_usage = 900 MiB
memory_limit = 1024 MiB

restarts_in_last_hour = 6
total_requests = 10000
failed_requests = 1200
```

These raw values are useful, but we can create more meaningful signals.

### Example 1: CPU saturation ratio

Formula:

\[
CPU\ Ratio = \frac{CPU\ Usage}{CPU\ Request}
\]

\[
= \frac{850}{1000}=0.85
\]

This tells us CPU usage is 85% of the requested CPU.

Note: usage can exceed the request, so this ratio can be greater than 1.

### Example 2: Memory headroom

\[
Memory\ Headroom = Memory\ Limit - Memory\ Usage
\]

\[
=1024-900=124\ MiB
\]

This gives us an indication of how much memory capacity remains before the configured limit.

### Example 3: HTTP error rate

\[
Error\ Rate = \frac{Failed\ Requests}{Total\ Requests}
\]

\[
=\frac{1200}{10000}=0.12=12\%
\]

A 12% error rate can be more informative than simply knowing that 1,200 requests failed, because it accounts for total traffic volume.

### Example 4: Restart rate

Instead of using only a restart count, calculate restarts per hour:

\[
Restart\ Rate = \frac{6}{1}=6
\]

This gives us six restarts per hour.

### Example 5: Rolling average

Imagine CPU usage over the previous five minutes:

```text
Minute 1: 60%
Minute 2: 70%
Minute 3: 80%
Minute 4: 90%
Minute 5: 100%
```

The rolling average is:

\[
\frac{60+70+80+90+100}{5}=80\%
\]

A rolling average helps smooth short-lived fluctuations and capture trends.

### Raw data → engineered features → ML model

```text
Raw infrastructure metrics
CPU, memory, restarts, requests
          |
          v
Feature engineering
CPU ratio, memory headroom, error rate, rolling average
          |
          v
ML model
Predict resource-related failure risk
```

### A critical engineering constraint: prediction time

Suppose the model predicts whether a pod will fail in the next 10 minutes.

You can use CPU and memory measurements from the previous 15 minutes. You cannot use metrics recorded five minutes after the prediction was made.

For time-series features, every rolling window must end at the prediction timestamp.

This is how good feature engineering avoids accidentally giving the model information from the future.

---

## 5. Feature selection: choosing what the model should learn from

Feature engineering creates potentially useful features. Feature selection decides which ones are worth keeping.

Imagine our dataset has these columns:

| Feature | Keep? | Reason |
|---|---|---|
| CPU utilization | **Yes** | Can indicate resource pressure |
| Memory headroom | **Yes** | Can indicate memory exhaustion risk |
| Restart count | **Yes** | Can indicate instability |
| Random pod UUID | **Usually no** | Usually no predictive meaning |
| Duplicate CPU metric | **Review** | May add redundant information |
| Future failure timestamp | **No** | Unavailable at prediction time |

Why does feature selection matter?

- Irrelevant features can add noise.
- Redundant features can make models harder to interpret.
- Too many features can increase computation and storage.
- Unstable features can cause poor performance when production data changes.

### Three common approaches

**1. Domain knowledge**

Use your understanding of the system.

For example, if predicting Kubernetes memory-related failures, memory usage, memory headroom, and recent OOM kills may be relevant.

**2. Statistical filtering**

Look at properties such as:
- Variance: does the feature actually change?
- Correlation: is it strongly related to another feature?
- Distribution: does it contain useful variation?

**3. Model-based selection**

Train a model and investigate which features influence its predictions.

Two useful techniques:

- Feature importance: estimates how much a model relies on a feature.
- Permutation importance: shuffle one feature and measure how much model performance deteriorates.

One caution: feature importance is not proof of causation. A model relying heavily on CPU utilization does not prove that CPU utilization caused the failure.

---

## 6. Feature pipelines: making transformations repeatable

This is where things become especially important for MLOps.

Suppose you trained a model with the following transformations:

1. Convert region into one-hot encoded columns.
2. Convert severity into ordinal values.
3. Feed the resulting features into a classifier.

Your production inference service must apply compatible transformations.

Imagine training used:

```text
ap-south-1 → [1, 0, 0]
us-east-1  → [0, 1, 0]
eu-west-1  → [0, 0, 1]
```

But production uses a different encoding order:

```text
ap-south-1 → [0, 0, 1]
us-east-1  → [1, 0, 0]
eu-west-1  → [0, 1, 0]
```

The model receives different meanings for the same input positions. Predictions can become incorrect.

This is why we use a feature preprocessing pipeline.

### Scikit-learn example

Let's implement a simple pipeline that handles numerical and categorical columns.

```python
import pandas as pd

from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression

data = pd.DataFrame({
    "cpu_usage": [82.5, 35.2, 91.0, 48.7],
    "restart_count": [3, 0, 5, 1],
    "region": [
        "ap-south-1",
        "us-east-1",
        "ap-south-1",
        "eu-west-1"
    ],
    "pod_type": [
        "frontend",
        "backend",
        "worker",
        "frontend"
    ]
})

labels = [1, 0, 1, 0]

numerical_features = [
    "cpu_usage",
    "restart_count"
]

categorical_features = [
    "region",
    "pod_type"
]

preprocessor = ColumnTransformer(
    transformers=[
        (
            "categorical",
            OneHotEncoder(handle_unknown="ignore"),
            categorical_features
        ),
        (
            "numerical",
            "passthrough",
            numerical_features
        )
    ]
)

model_pipeline = Pipeline([
    ("preprocessor", preprocessor),
    ("classifier", LogisticRegression())
])

model_pipeline.fit(data, labels)

prediction = model_pipeline.predict(
    pd.DataFrame([{
        "cpu_usage": 88.0,
        "restart_count": 4,
        "region": "ap-south-1",
        "pod_type": "worker"
    }])
)

print(prediction)
```

**Important concept**

The `Pipeline` packages preprocessing and model training together.

The `OneHotEncoder` learns its category mapping during fitting. At inference, the same fitted encoder is reused.

`handle_unknown="ignore"` allows an unseen category to be handled without crashing. It does not mean the model has learned how to predict well for that new category.

In real projects, fit the pipeline only on training data, then use the fitted pipeline to transform validation, test, and production inputs. We'll explore preprocessing and scaling in Lesson 9.

### Why this matters in infrastructure

Think of the feature pipeline as part of your deployable model artifact.

```text
TRAINING ENVIRONMENT

Dataset → Feature pipeline → Model training
                         |
                         | Version and package together
                         v
                  Model artifact
           Fitted preprocessing + trained model
                         |
                         v
PRODUCTION ENVIRONMENT

Incoming metrics → Same feature pipeline → Prediction
```

This connects to several concepts you'll encounter later:

- **Feature store:** a system for managing and serving reusable features.
- **Feature schema:** defines expected feature names, types, and constraints.
- **Feature drift:** production feature distributions change over time.
- **Training-serving skew:** training and production calculate features differently.
- **Feature versioning:** tracks changes to feature definitions and transformations.

These are major concerns in production ML platforms.

---

## 7. Practical coding exercise

Let's create a tiny feature engineering pipeline using Kubernetes-style monitoring data.

Start with this dataset:

```python
import pandas as pd

df = pd.DataFrame({
    "cpu_usage": [850, 400, 950, 600],
    "cpu_request": [1000, 1000, 1000, 800],
    "memory_usage": [900, 500, 980, 600],
    "memory_limit": [1024, 1024, 1024, 800],
    "failed_requests": [120, 20, 250, 30],
    "total_requests": [1000, 1000, 1000, 1000],
    "region": [
        "ap-south-1",
        "us-east-1",
        "ap-south-1",
        "eu-west-1"
    ],
    "severity": [
        "High",
        "Low",
        "Critical",
        "Medium"
    ]
})
```

Your tasks:

1. Create `cpu_ratio` using CPU usage divided by CPU request.
2. Create `memory_headroom` using memory limit minus memory usage.
3. Create `error_rate` using failed requests divided by total requests.
4. Encode `severity` using the mapping Low=0, Medium=1, High=2, Critical=3.
5. Explain why `region` should not simply be encoded as 1, 2, and 3.
6. Identify which features you'd use to predict resource-related failures and explain why.

**Expected output:** a transformed DataFrame containing your original columns plus the engineered features.

Try implementing it yourself before looking anything up. I'll review your Python code and explain any mistakes.

---

## 8. Knowledge check

Let's see whether the concepts have clicked. Answer these eight questions; I'll review your answers before we move on.

### 1. CPU utilization (e.g. 82.5%) is what type of feature?

- A. Numerical continuous
- B. Numerical discrete
- C. Nominal categorical
- D. Ordinal categorical

### 2. A cloud region such as ap-south-1 is an example of:

- A. Ordinal categorical
- B. Nominal categorical
- C. Numerical continuous
- D. Boolean

### 3. Which encoding is generally appropriate for unordered categories?

- A. Ordinal encoding
- B. One-hot encoding
- C. Standardization
- D. Gradient descent

### 4. When is ordinal encoding appropriate?

- A. For every string column
- B. When categories have a meaningful order
- C. For random IDs
- D. Only for continuous measurements

### 5. You want to predict failure 10 minutes from now. Which feature is valid?

- A. CPU usage from 5 minutes ago
- B. CPU usage from 5 minutes in the future
- C. The actual future failure timestamp
- D. A metric collected after the failure

### 6. What does feature engineering do?

- A. Only deletes columns
- B. Creates or transforms useful model inputs
- C. Deploys a model to Kubernetes
- D. Replaces model training

### 7. Why package preprocessing with the trained model?

- A. To make the model file smaller
- B. To guarantee compatible transformations during inference
- C. To eliminate the need for datasets
- D. To avoid model evaluation

### 8. What is training-serving skew?

- A. Training takes longer than inference
- B. Training and production calculate or represent features differently
- C. A model has too many layers
- D. A GPU runs out of memory

---

## Lesson 8 completion checklist

- [ ] Understand numerical and categorical feature types.
- [ ] Understand one-hot and ordinal encoding.
- [ ] Complete the feature engineering Python exercise.
- [ ] Complete the knowledge check.

**One thing I want you to remember:** A machine learning model is only as useful as the information we give it. Better features can sometimes improve a model more than choosing a more complicated algorithm.

Once you've submitted the quiz and practical exercise, I'll review both and we can mark Lesson 8 complete. 💪
