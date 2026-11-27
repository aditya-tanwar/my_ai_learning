Absolutely. That's an important distinction, and I'll use it to shape the rest of the curriculum.

**We're training you to become an AI Infrastructure / MLOps / LLMOps engineer — not a data scientist or ML researcher.**

You need enough ML knowledge to understand what models do, how they are trained and evaluated, and how to operate them reliably. You do not need to spend months deriving statistical algorithms or mastering research-level mathematics.

From Lesson 11 onward, I'll prioritize:

- **Understand the system:** model lifecycle, datasets, model artifacts, training and inference.
- **Build and operate:** Python, Docker, Kubernetes, cloud GPUs, model serving and pipelines.
- **Automate:** Jenkins, CI/CD, GitOps, Terraform, MLflow and workflow orchestration.
- **Production engineering:** monitoring, reliability, security, reproducibility, scaling and FinOps.
- **Know the ML concepts:** enough to make good infrastructure decisions and collaborate with ML engineers.

We'll still cover responsible AI and reproducibility, but from an engineering and platform-operations perspective.

# **Phase 1 — Lesson 11: Responsible AI and ML Reproducibility**

Final lesson of Phase 1

## **Learning objectives**

By the end of this lesson, you'll understand:

1. What responsible AI means in production.
2. How bias and fairness can affect model behavior.
3. Explainability: why a model made a prediction.
4. Data privacy and security responsibilities.
5. What makes an ML experiment reproducible.
6. How to version datasets, model artifacts, configurations and dependencies.
7. How to design a reliable, auditable ML deployment pipeline.

We'll use a familiar example throughout: a Kubernetes pod failure prediction service.

## **1. What is responsible AI?**

Imagine your organization builds an ML model to predict Kubernetes pod failures.

The model receives:

- CPU utilization
- Memory utilization
- Container restart count
- Namespace
- Node type
- Deployment region

It predicts whether a pod is likely to fail.

Now imagine the model consistently predicts that workloads running in one particular region are more likely to fail, even though the region itself isn't the cause.

Or imagine the model was trained using outdated data and keeps recommending unnecessary pod restarts.

These are not merely model accuracy problems. They are production risks.

**Responsible AI** means developing and operating AI systems in ways that are safe, fair, transparent, privacy-conscious and accountable.

For an infrastructure engineer, the key question is:

*How do we make sure the AI platform doesn't silently produce harmful, unreliable or untrustworthy outcomes?*

## **2. Bias and fairness**

### **What is bias in ML?**

Bias occurs when a model's predictions are systematically skewed because of problems in the data, the model, or the way the system is used.

Consider this dataset:

| Region   | Pod observations | Recorded failures |
| -------- | ---------------- | ----------------- |
| Region A | 50,000           | 500               |
| Region B | 10,000           | 800               |
| Region C | 40,000           | 400               |

A model might learn that Region B is associated with failures.

But why?

Possibilities include:

- Region B genuinely has infrastructure problems.
- Region B runs more demanding workloads.
- Monitoring is more detailed in Region B.
- The dataset contains inconsistent failure labels.
- The model has learned a correlation that doesn't represent the actual cause.

The important lesson: **correlation is not necessarily causation.**

### **Types of bias worth knowing**

| Type             | Meaning                                                        | Example                                                   |
| ---------------- | -------------------------------------------------------------- | --------------------------------------------------------- |
| Sampling bias    | Training data doesn't represent the real population            | Only production workloads from one region                 |
| Historical bias  | Past data contains existing inequalities or problems           | Old operational decisions influence future predictions    |
| Measurement bias | Measurements are inconsistent                                  | Different monitoring systems calculate CPU differently    |
| Label bias       | Labels are inaccurate or inconsistent                          | A pod restart is incorrectly labelled as a failure        |
| Deployment bias  | Model is used in a context different from its intended purpose | A model trained for one workload type is used for another |

### **What should an AI platform engineer do?**

You are not expected to independently conduct advanced fairness research.

Your responsibility is to help make the data and model lifecycle auditable and reliable:

- Track where datasets came from.
- Record data collection timestamps.
- Preserve feature definitions and schemas.
- Detect changes in data distributions.
- Track model versions and evaluation results.
- Ensure deployment teams know the model's intended use.
- Make it possible to roll back a problematic model.

For more sensitive AI applications, fairness evaluation may require domain experts and dedicated assessment methods.

## **3. Explainability: Why did the model make this prediction?**

Imagine your model generates this result:

```
Pod: payment-service-7d8f9
Failure probability: 94%
Prediction: High risk
```

An engineer asks:

**Why does the model think this pod is going to fail?**

A production system should provide enough information to investigate the prediction.

This is where explainability comes in.

### **What is explainability?**

Explainability means understanding which inputs or factors influenced a model's prediction.

For example:

```
Prediction: High failure risk

Contributing signals:
- High CPU utilization
- Increasing memory consumption
- Repeated container restarts
```

This is useful because the operations team can investigate the underlying infrastructure.

### **Two approaches you should know**

**A. Global explainability**

Understand the model's overall behavior across a dataset.

Example:

```
Most influential features:
1. Restart count
2. Memory utilization
3. CPU utilization
```

This helps engineers and model owners understand what the model generally relies on.

**B. Local explainability**

Understand an individual prediction.

Example:

```
Pod payment-service-7d8f9

Why was it flagged?

- Memory utilization contributed strongly.
- Restart count increased the predicted risk.
- CPU utilization had a smaller contribution.
```

### **Tools worth knowing**

!![SHAP Values for Model Interpretability | MetricGate](https://images.openai.com/static-rsc-4/rYCO420dBoL0Ui46AQzvBQaNa4-tkW39jztGzIfMP3L1UpQgBUTJle--zWg7oH4nYGRedrRuk_Dv6EGE-ktjKxd_7pm6yFcha0tv08cpdckF1yya6Kno1SFOuZve-uKl_sdVPYsMMpwl8YUUSRYek35HgrWvEgZVjdGOrfjWI5c?purpose=inline)

**SHAP**

Explains feature contributions to individual predictions and overall model behavior.

Useful for understanding model decisions.

!![Which Explanation Can You Trust? SHAP vs LIME on Heart Disease Prediction | by Myakalarajkumar | Medium](https://images.openai.com/static-rsc-4/Gr56CPVKitXl1SVTSAUV0bJrs5ViKFLygGfdpDvF8bThFWyjLIWItDpn-xhlAaeElzE_ayWkwEjFmy8bpBdkHy7jSoY3N4Kqh4F-DHaZp5sF8KEFVOGNcdBTf1HQJ_IkxNBacnIKGkE-FSlXk6U715j3EOMyfQMfiSBXpFjgTKQ?purpose=inline)

**LIME**

Creates a simpler local approximation to explain an individual prediction.

Useful for local prediction explanations.

You don't need to become an expert in SHAP or LIME right now. Understand what they provide and how their outputs can be integrated into an ML workflow.

### **Infrastructure engineering perspective**

Your platform should make it possible to associate a prediction with:

- Model name and version.
- Model artifact identifier.
- Input feature values or an appropriately protected record of them.
- Prediction timestamp.
- Prediction score.
- Explanation metadata, when available.

This is particularly useful for debugging, auditing and incident investigation.

**Important:** Explainability tools provide explanations or approximations of model behavior. They do not automatically prove that a feature caused the prediction.

## **4. Data privacy and security**

This is where responsible AI connects directly with the security and platform engineering work you already know.

Imagine a company trains an internal support assistant using:

- Customer support conversations.
- Application logs.
- Internal documentation.
- Infrastructure configuration.
- Incident reports.

Some of this data could contain:

- API keys.
- Passwords.
- Access tokens.
- Customer information.
- Internal hostnames.
- Personal information.

If this data enters a training pipeline without controls, sensitive information may be exposed through storage, logs, model artifacts or downstream applications.

### **What can go wrong?**

| Risk                                | Example                                                           |
| ----------------------------------- | ----------------------------------------------------------------- |
| Sensitive data in training datasets | Customer details included in training records                     |
| Secrets leakage                     | API tokens included in log files                                  |
| Unauthorized dataset access         | An engineer accesses another team's training data                 |
| Model artifact exposure             | Private model artifacts are publicly accessible                   |
| Excessive logging                   | Prompts contain sensitive information and are stored indefinitely |
| Insecure model downloads            | An untrusted model artifact enters the platform                   |

### **How should an AI platform protect data?**

Think about the security controls you already use in cloud infrastructure.

| Existing infrastructure concept | AI platform application                           |
| ------------------------------- | ------------------------------------------------- |
| IAM / RBAC                      | Control who can access datasets and models        |
| KMS                             | Encrypt datasets and model artifacts              |
| Secrets Manager                 | Store API keys and credentials                    |
| S3 bucket policies              | Restrict model and dataset access                 |
| Kubernetes NetworkPolicies      | Restrict communication between workloads          |
| Audit logs                      | Track access to sensitive resources               |
| Image scanning                  | Scan AI workload container images                 |
| Artifact signing                | Verify the integrity and origin of model packages |

### **Example: Secure AI artifact storage**

**Secure artifact flow**

**Training pipeline**

Produces model and evaluation artifacts

**Private object storage**

Encryption at rest using KMS

**Access control**

IAM policies, role separation and audit logging

**Deployment pipeline**

Verifies artifact identity and approved model version

**Production inference service**

Loads only approved artifacts with authorized access

### **A security point worth remembering**

A model artifact is not automatically safe just because it was generated by your own training pipeline.

For example, some Python model serialization formats, including pickle-based formats, can execute code when loaded from untrusted sources.

Therefore:

- Only load artifacts from trusted sources.
- Restrict who can publish model artifacts.
- Verify artifact provenance and integrity.
- Scan and review dependencies.
- Avoid loading untrusted serialized models.

This is the ML equivalent of taking software supply-chain security seriously.

## **5. What is ML reproducibility?**

Now we come to one of the most important topics for MLOps engineers.

Imagine you trained a model last Monday.

It achieved:

```
Model: pod-failure-model
Accuracy: 96%
F1 score: 0.91
```

You deployed it and everything worked.

Three weeks later, someone asks:

"Can we train the exact same model again?"

You execute the same Python script, but the result is different:

```
Model: pod-failure-model
Accuracy: 92%
F1 score: 0.84
```

What changed?

Possibilities:

- Dataset changed.
- Python version changed.
- Scikit-learn version changed.
- Features changed.
- Model hyperparameters changed.
- Random seed changed.
- Preprocessing logic changed.
- Training code changed.
- Training environment changed.

This is a reproducibility problem.

### **Definition**

**ML reproducibility** means being able to trace and, as far as practical, recreate a model training run using its recorded data, code, configuration, dependencies and environment.

Let's make this concrete.

For every model training run, you should be able to answer:

1. Which dataset was used?
2. Which version of the training code was executed?
3. Which hyperparameters were used?
4. Which model architecture and configuration were used?
5. Which Python and library versions were used?
6. Which metrics were produced?
7. Which model artifact was generated?
8. Which environment performed the training?

This is very similar to reproducing a software build.

In DevOps, you don't just want to know that an image exists. You want to know which source commit, dependencies and build configuration produced it.

ML adds the dataset and experiment configuration to that equation.

## **6. What exactly should we version?**

This is an essential MLOps concept.

A model is not just a `.pkl`, `.joblib`, `.pt` or `.safetensors` file.

A production ML system has multiple versioned components.

**The reproducible ML experiment**

**Dataset version**

Which training data?

**Code version**

Which source commit?

**Configuration**

Which parameters and settings?

**Dependencies**

Which libraries and versions?

**Metrics**

What were the results?

**Model artifact**

What was produced?

**Reproducible training run**

Traceable, auditable and repeatable experiment

Let's examine these components.

### **A. Dataset versioning**

Suppose you train a model using:

```
dataset_v1.csv
```

Later, the data team fixes incorrect labels and adds more observations:

```
dataset_v2.csv
```

These are two different training datasets.

Even if the Python code remains identical, the resulting model may differ.

You need to record which dataset version was used for each run.

Tools worth knowing:

- DVC
- LakeFS
- Object storage with immutable versioning and recorded dataset manifests.

For your infrastructure background, think of dataset versioning as versioning a build input.

### **B. Code versioning**

Your training code should live in Git.

For every training run, record the commit SHA.

Example:

```
Repository: pod-failure-ml
Branch: main
Commit: a81d9f2
```

This lets you identify exactly which code was used.

Git is not necessarily where you should store huge datasets or model binaries. It should track the code and the metadata needed to identify those external artifacts.

### **C. Configuration versioning**

Consider this training configuration:

```
model:
  algorithm: logistic_regression
  class_weight: balanced
  max_iterations: 1000

training:
  test_size: 0.2
  random_state: 42

features:
  - cpu
  - memory
  - restarts
```

If someone changes the class-weight setting or training split, the experiment may produce a different result.

Store the configuration alongside the run.

### **D. Dependency versioning**

Imagine:

```
Python 3.11
scikit-learn 1.x
pandas 2.x
numpy 2.x
```

A different library version can change behavior, compatibility or numerical results.

Use:

- `pyproject.toml`
- `poetry.lock`
- `requirements.txt` with pinned versions
- Container images with pinned base-image versions and recorded digests.

You already use Poetry and Docker, so this part fits directly into your existing engineering skill set.

### **E. Model artifact versioning**

After training, you might generate:

```
pod_failure_model.joblib
```

But what if you train another version?

```
pod_failure_model_v1.joblib
pod_failure_model_v2.joblib
pod_failure_model_v3.joblib
```

Simply renaming files is not a complete model lifecycle strategy.

You need metadata such as:

- Model name.
- Model version.
- Training run ID.
- Dataset reference.
- Code commit.
- Evaluation metrics.
- Artifact location.
- Approval or deployment status.

This is where a model registry becomes useful. We'll cover it in detail in the MLOps phase.

## **7. MLflow: Experiment tracking for ML engineers**

Let's introduce a tool you'll encounter frequently in MLOps.

!![Aprende a programar desde tu casa](https://images.openai.com/static-rsc-4/B0i4FjKtuevBCMrh2NGSeCmQrkVoHHDsHRUZjGH9wHaqtr9GzH8PY8Ni6ih7XxltdsuJZQzXGHaoKqwvNBKDY8GmfWFdeQhTVYXZRi5kdhvBVEDkUpp8oyX__3UvivrIyJNi9R9XdkZnYj0vygLQ_3w-ElxuzxNPUVqqpj58mPo?purpose=inline)

**MLflow**

An open-source platform for tracking experiments, managing model artifacts and organizing the model lifecycle.

[Official MLflow website](https://mlflow.org/?utm_source=chatgpt.com)

Think of MLflow as a tracking system for ML experiments.

In normal software development, you might track:

- Build ID.
- Git commit.
- Docker image tag.
- Test results.
- Deployment status.

In ML, you additionally track:

- Dataset version.
- Model parameters.
- Hyperparameters.
- Evaluation metrics.
- Model artifacts.
- Training run metadata.

### **MLflow architecture**

!![An introduction to MLOps with MLflow](https://images.openai.com/static-rsc-4/0G_dgZX1Bd49vkaPiDfLLMwuzaQNUdHl_3BtICd65ULOXDNZBmiTkvSL3zJ4ybBUJcce-hPLB0POJGQo2uMB8d5hLE5vxNpaBFNM5eabbnxNTUlsEFXStB8Twq7kvsm-qBOCPehtuS_onrzKshRnTjbQAUYbxoSrsPDhzdqcg4o?purpose=inline)

[inseefrlab.github.io](https://inseefrlab.github.io/formation-mlops/slides/en/index.html?utm_source=chatgpt.com)

A simplified architecture:

```
Training Python Script
        |
        v
    MLflow Client
        |
        v
  MLflow Tracking Server
        |
        +----------------------+
        |                      |
        v                      v
  Backend Store          Artifact Store
  (Run metadata)         (Model files,
                          plots, outputs)
```

The backend store records information about the run.

The artifact store stores files generated during the experiment.

For example, these could be:

```
Experiment: pod-failure-prediction

Run ID: 8f21a

Parameters:
  algorithm: logistic_regression
  class_weight: balanced
  max_iter: 1000

Metrics:
  accuracy: 0.96
  precision: 0.91
  recall: 0.88
  f1_score: 0.89

Artifacts:
  model.joblib
  confusion_matrix.png
  classification_report.json
  preprocessing_pipeline.joblib
```

This is much more useful than simply having a model file somewhere in S3.

### **A small MLflow example**

```
import mlflowimport mlflow.sklearnfrom sklearn.linear_model import LogisticRegressionfrom sklearn.metrics import accuracy_scoremlflow.set_experiment("pod-failure-prediction")with mlflow.start_run():    model = LogisticRegression(        class_weight="balanced",        max_iter=1000    )    model.fit(X_train, y_train)    predictions = model.predict(X_test)    accuracy = accuracy_score(        y_test,        predictions    )    # Track configuration    mlflow.log_param("algorithm", "logistic_regression")    mlflow.log_param("class_weight", "balanced")    mlflow.log_param("max_iter", 1000)    # Track evaluation    mlflow.log_metric("accuracy", accuracy)    # Store the trained model    mlflow.sklearn.log_model(        model,        name="pod-failure-model"
```

Notice the pattern:

1. Start an experiment run.
2. Train the model.
3. Record parameters.
4. Record metrics.
5. Store the model artifact.

The tracking calls can be expanded to include the dataset reference, source commit, preprocessing pipeline and other metadata.

### **Where does MLflow fit into your infrastructure?**

You could deploy MLflow as a service on Kubernetes.

For example:

```
Kubernetes Cluster
|
+-- mlflow namespace
|   |
|   +-- MLflow Tracking Server
|   |
|   +-- PostgreSQL
|       (Backend metadata store)
|
+-- Object Storage
    |
    +-- S3
        (Model artifacts)
```

Your training pipeline connects to the tracking server.

The server records experiment information, while model artifacts are stored in object storage.

This is a very practical platform engineering use case.

You should understand how to:

- Deploy MLflow.
- Configure its backend database.
- Configure S3 or compatible artifact storage.
- Secure the tracking endpoint.
- Manage credentials and secrets.
- Back up metadata and artifacts.
- Monitor availability and storage growth.

We will build this properly in the MLOps phase rather than turning this lesson into a full MLflow deployment tutorial.

## **8. What is data lineage?**

Another important term is **data lineage**.

Data lineage answers:

- Where did the dataset come from?
- Which transformations were applied?
- Which training run used it?
- Which model was generated?
- Where was that model deployed?

Think of it as a traceable history of your ML artifacts.

For example:

```
Raw Kubernetes Metrics
          |
          v
   Data Preprocessing
          |
          v
   Dataset Version 3
          |
          v
   Training Run 104
          |
          v
   Model Version 2.1
          |
          v
   Production Deployment
```

Now imagine an incident occurs and the model begins generating incorrect predictions.

You should be able to trace:

```
Production model
      |
      v
Model version 2.1
      |
      v
Training run 104
      |
      v
Dataset version 3
      |
      v
Git commit a81d9f2
```

You can investigate the exact training inputs and configuration.

### **Why lineage matters**

| Situation           | How lineage helps                                 |
| ------------------- | ------------------------------------------------- |
| Model quality drops | Identify which training data and code produced it |
| Bad deployment      | Identify the model artifact and training run      |
| Dataset corruption  | Identify models trained on affected data          |
| Security audit      | Trace the origin and access history of artifacts  |
| Rollback            | Identify a previously approved model version      |

For AI infrastructure engineers, lineage is a platform capability—not just an ML concept.

## **9. Reproducibility versus determinism**

These two terms are related, but they are not identical.

**Reproducibility** means having enough recorded information to repeat or investigate a training run.

**Determinism** means that the same inputs and operations produce exactly the same outputs.

For example:

```
Dataset: dataset_v1
Code: commit_a81d9f2
Configuration: config_v1
Environment: image_digest_123
Random seed: 42
```

Recording these details makes the experiment traceable.

But even then, exact output equality isn't guaranteed in every environment.

Why?

- GPU operations may be nondeterministic.
- Floating-point calculations can vary.
- Different hardware or libraries can produce numerical differences.
- Distributed training can introduce variation in operation ordering.
- Some algorithms have stochastic behavior.

You can improve reproducibility by controlling random seeds, pinning dependencies, recording hardware and software details, and using deterministic operations where supported.

But don't promise bit-for-bit identical results unless the full execution environment supports it.

### **The key takeaway**

You don't need to guarantee that every ML experiment produces identical results.

You need to make experiments traceable, repeatable as far as practical, and explainable when differences occur.

## **10. Putting responsible AI and reproducibility into CI/CD**

Now let's connect everything to a workflow you already understand: Jenkins and GitOps.

Imagine your team has a pipeline that trains a model and deploys it to Kubernetes.

A production-ready pipeline should have controls beyond simply running `python train.py`.

**Responsible ML delivery pipeline**

1. **Source and data validation**

   Validate schemas, dataset provenance, access controls and data quality.
2. **Reproducible environment**

   Use pinned dependencies, container images and recorded configuration.
3. **Model training**

   Execute training and record the dataset version, code SHA and run ID.
4. **Evaluation gates**

   Validate model metrics, required thresholds and agreed safety checks.
5. **Artifact and lineage checks**

   Register the artifact, record its provenance and verify integrity.
6. **Approval and deployment**

   Promote only approved model versions through your deployment workflow.
7. **Production monitoring**

   Monitor service health, prediction behavior, data drift and model quality when labels become available.

### **Example Jenkins pipeline structure**

This is an architectural example, not a complete executable pipeline.

```
pipeline {
    agent { label 'linux-k8s' }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Data') {
            steps {
                sh 'python validate_data.py'
            }
        }

        stage('Build Training Image') {
            steps {
                sh 'docker build -t pod-ml-trainer:${BUILD_NUMBER} .'
            }
        }

        stage('Train Model') {
            steps {
                sh 'python train.py'
            }
        }

        stage('Evaluate Model') {
            steps {
                sh 'python evaluate.py'
            }
        }

        stage('Validate Artifact') {
            steps {
                sh 'python validate_artifact.py'
            }
        }

        stage('Register Model') {
            steps {
                sh 'python register_model.py'
            }
        }

        stage('Deploy Approved Model') {
            steps {
                echo 'Deploy approved model through GitOps'
            }
        }
    }
}
```

In a real implementation, the stages would also need proper artifact handoff, immutable image references, credential handling, evaluation thresholds, approvals and failure handling.

### **A particularly important design decision**

Don't automatically deploy every newly trained model.

A new model should be evaluated and compared with the currently deployed version.

For example:

```
Current Production Model
       |
       v
Current evaluation baseline

New Candidate Model
       |
       v
Evaluation
       |
       v
Compare results
       |
       +---- Fails requirements ---> Reject
       |
       +---- Passes requirements --> Approval
                                      |
                                      v
                                   Deploy
```

This is how you prevent an experiment from accidentally becoming a production release.

## **11. What you should know as an AI Infrastructure engineer**

Let's separate the knowledge into three levels.

Must understand deeply

**Reproducibility and lineage**

- Dataset, code, configuration and dependency versioning.
- Model artifacts and their provenance.
- MLflow experiment tracking.
- Reproducible containers and training environments.
- Model approval, promotion and rollback.

Understand conceptually

**Responsible AI**

- Bias and fairness.
- Explainability.
- Data privacy.
- Responsible model usage.
- Model evaluation and governance.

Know it exists

**Specialist techniques**

- SHAP and LIME internals.
- Advanced fairness algorithms.
- Formal explainability methods.
- Advanced statistical reproducibility techniques.

You should be able to deploy the tools, integrate their outputs into pipelines and troubleshoot the platform. You don't need to become the person researching new fairness algorithms.

## **12. Lesson 11 — Knowledge check**

**Knowledge check**

0/8 answered

**1. What is responsible AI?**

Making every model run on a GPU

Building and operating AI systems with safety, fairness, transparency, privacy and accountability

Maximizing accuracy regardless of consequences

Removing human involvement from AI systems

**2. Your pod failure model flags workloads in one region much more frequently. What should you investigate?**

Immediately delete the region feature

Assume the region is causing failures

Investigate data quality, sampling, workload differences and feature behavior

Increase the number of model parameters

**3. What is local explainability?**

Understanding an individual prediction

Understanding only the model's training duration

Measuring GPU utilization

Versioning the dataset

**4. Which security control helps restrict who can access model artifacts in S3?**

Batch size

IAM policies and bucket policies

Learning rate

Dropout

**5. What information is important for reproducing an ML training run?**

Only the model filename

Only the Python script

Dataset version, code, configuration, dependencies and environment details

Only the accuracy score

**6. What does MLflow primarily provide?**

Kubernetes GPU scheduling

Experiment tracking, artifact management and model lifecycle capabilities

A replacement for Git

Automatic data labelling

**7. What is data lineage?**

The number of parameters in a model

The history and origin of data and artifacts through the ML lifecycle

The GPU memory consumed by training

The number of epochs

**8. Why shouldn't every newly trained model be deployed automatically?**

Models cannot be deployed through CI/CD

A candidate must pass evaluation and release controls before production promotion

Training always changes the Python version

Model registries cannot store multiple models

Submit answers

## **13. Practical exercise — Design a reproducible ML platform**

Let's use your DevOps experience.

Your team has a pod failure prediction model. It is trained through Jenkins and deployed to Kubernetes.

Currently:

- Training code is in GitHub.
- Datasets are stored in S3.
- Model files are stored in S3.
- The inference API runs on EKS.
- Training is performed in Docker.
- Jenkins runs the pipeline.

Your task is to design the missing MLOps capabilities.

**Part A — Reproducibility**

Explain how you would record:

1. Dataset version.
2. Git commit SHA.
3. Python and dependency versions.
4. Training configuration.
5. Docker image digest.
6. Model artifact version.

**Part B — MLflow architecture**

Where would you deploy MLflow?

Explain:

- Tracking server.
- Backend metadata database.
- Artifact storage.
- Authentication and access control.

**Part C — Production security**

How would you protect:

- Training datasets?
- Model artifacts?
- Credentials?
- MLflow access?

**Part D — Deployment governance**

Suppose a new model performs worse than the currently deployed model.

How should your Jenkins and GitOps workflows prevent it from reaching production?

You don't need to write code. I want to see the architecture and engineering decisions.

## **Lesson 11 summary**

| Concept             | What you should remember                                        |
| ------------------- | --------------------------------------------------------------- |
| Responsible AI      | Safe, fair, transparent and accountable AI                      |
| Bias                | Systematic distortion in model behavior                         |
| Explainability      | Understanding model predictions                                 |
| Data privacy        | Protecting sensitive data throughout the ML lifecycle           |
| Reproducibility     | Recording everything needed to recreate or investigate training |
| Dataset versioning  | Knowing exactly which data produced a model                     |
| Experiment tracking | Recording parameters, metrics and artifacts                     |
| MLflow              | Experiment tracking and model lifecycle platform                |
| Data lineage        | Traceability from data source to production model               |
| Model governance    | Evaluation, approval, deployment and rollback controls          |

**The most important takeaway:** A production model must be more than accurate. It must be traceable, secure, reproducible, auditable and safely deployable.

## **Phase 1 completion checkpoint**

That's the end of the planned Phase 1 lesson sequence!

You have now covered the foundational concepts from AI and traditional ML through model training, feature engineering, data quality, imbalanced datasets, responsible AI and reproducibility.

However, Phase 1 isn't automatically complete yet. We still have outstanding knowledge checks and practical exercises from earlier lessons, including Lessons 4, 5, 8, 9, 10 and 11.

We'll review the outstanding work rather than silently marking lessons complete.

**Next phase: Phase 2 — Mathematics and Data Fundamentals.**

We'll approach it from an engineering perspective:

- Tensors and shapes because PyTorch and GPUs operate on tensors.
- Matrix multiplication because neural networks and attention use it.
- Statistics because you need to interpret model metrics and monitoring.
- Probability because model predictions and uncertainty depend on it.
- Gradients because training and optimization use them.

No research-level derivations. We'll focus on understanding what happens inside the systems you'll deploy and operate. 💪