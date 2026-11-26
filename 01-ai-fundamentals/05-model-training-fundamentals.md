# Lesson 5 — Model Training Fundamentals

> **Visual companion:** [Open the HTML lesson](./05-model-training-fundamentals.html)

**Phase:** 1 — Artificial Intelligence Fundamentals  
**Status:** Taught; knowledge check 7/8; practical exercise pending

## Learning objectives

- Explain model parameters, weights, and biases.
- Distinguish parameters from hyperparameters.
- Explain loss, optimization, and gradient descent.
- Distinguish a training step, batch, and epoch.

## 1. What does a model learn?

Imagine training a model to predict whether a Kubernetes pod is likely to fail. It receives historical examples containing CPU usage, memory usage, and restart count.

During training, the model adjusts internal numerical values so its predictions become more accurate on the training objective. Those learned values are called **parameters**.

## 2. Parameters, weights, and biases

A **parameter** is an internal value learned by a model during training. In a simple model, parameters include weights and a bias.

- **Weights** determine how strongly an input contributes to a calculation.
- **Bias** is an additional learned value that shifts the result independently of the input values.

Illustrative example:

`score = (CPU × 0.4) + (Memory × 0.5) + (Restarts × 0.1)`

These values are illustrative weights, not a real trained model or proof of causation.

A simple linear model can be written as:

`y = w₁x₁ + w₂x₂ + b`

Here, `w₁` and `w₂` are weights, `x₁` and `x₂` are inputs, and `b` is the bias parameter.

**Important:** the learned bias parameter in this equation is not the same as *high bias* in the bias–variance discussion from Lesson 4.

## 3. Parameters vs. hyperparameters

| Type | Meaning | Examples |
|---|---|---|
| Parameters | Values learned from data during training | Weights and biases |
| Hyperparameters | Settings chosen by the engineer or tuning process | Learning rate, batch size, number of epochs |

The learning rate controls how large an update the optimizer attempts. Batch size controls how many training examples are processed together for an update. Epochs specify how many passes through the training dataset are made.

## 4. Loss function

A **loss function** measures how far a model's predictions are from the target values according to a chosen objective.

For example, if a model predicts a pod-failure probability of 0.9 for an example whose actual label is 1 (failure), that prediction is close to the target. If it predicts 0.1, it is far from the target and should receive a larger loss under an appropriate classification loss.

Lower loss generally indicates better performance on the same objective and dataset, but training loss alone does not prove that a model generalizes well. Validation performance matters too.

Binary cross-entropy is one commonly used loss function for binary classification.

## 5. Optimization and gradient descent

**Optimization** is the process of adjusting model parameters to reduce the loss.

Gradient descent, at a high level:
1. The model makes predictions.
2. The loss function measures prediction error.
3. Gradients indicate how changing parameters affects the loss.
4. The optimizer updates parameters in a direction intended to reduce loss.
5. The process repeats.

A simplified update rule is:

`w_new = w_old − η × (∂L / ∂w)`

Where `w` is a parameter, `L` is the loss, `∂L / ∂w` is the gradient of loss with respect to that parameter, and `η` (eta) is the learning rate.

If the learning rate is too high, updates may overshoot useful values and training can become unstable or fail to converge. If it is too low, training may be very slow.

The **optimizer** performs parameter updates; gradient descent is one optimization approach.

## 6. Steps, batches, and epochs

- **Batch:** a group of training examples processed together.
- **Step / iteration:** typically one optimizer update.
- **Epoch:** one complete pass through the training dataset.

Example:
- Training records: 10,000
- Batch size: 100
- Batches per epoch: 10,000 ÷ 100 = 100
- Epochs: 20
- Approximate optimizer steps: 100 × 20 = 2,000

This assumes the dataset is divisible by the batch size, all batches are used, and one optimizer update occurs per batch.

## 7. Infrastructure impact

Training consumes compute, memory, storage, and sometimes substantial network bandwidth:
- **CPU/GPU:** performs data processing and model computations.
- **RAM/VRAM:** holds batches, model parameters, intermediate activations, and optimizer state.
- **Storage:** holds datasets, checkpoints, logs, and model artifacts.
- **Networking:** transfers data between workers or storage systems in distributed training.

From a DevOps perspective, training is commonly run as a repeatable workload (for example, a Kubernetes Job) with explicit CPU/GPU and memory requests, data access, logs, retry behavior, and artifact persistence. A pod restarting does not automatically mean training state is safely checkpointed.

## 8. Practical exercise

Scenario:

**Initial model values**
- CPU weight: 0.2
- Memory weight: 0.3
- Restart weight: 0.1

**Training configuration**
- Learning rate: 0.01
- Batch size: 100
- Training records: 10,000
- Epochs: 20

Answer:
1. Which listed values are model parameters?
2. Which listed values are hyperparameters?
3. How many batches are there in one epoch?
4. Approximately how many optimizer steps occur across 20 epochs?
5. What is the purpose of the loss function?
6. What can happen if the learning rate is too high?
7. Who or what updates the weights during training?

### Answer guide

1. Parameters: the three weights (0.2, 0.3, 0.1).
2. Hyperparameters: learning rate, batch size, and number of epochs.
3. 100 batches per epoch.
4. Approximately 2,000 optimizer steps.
5. It quantifies prediction error against the target under the selected objective.
6. Updates can overshoot and make training unstable or prevent convergence.
7. The optimizer updates the weights using gradient information.

## 9. Knowledge check

The knowledge check was submitted with a score of **7/8**. The missed question was the batch/step calculation: 10,000 records with a batch size of 100 gives **100 steps per epoch**, assuming one update per batch.

## 10. Interview questions

1. What is the difference between a model parameter and a hyperparameter?
2. What is the role of weights and bias?
3. What does a loss function measure?
4. What does an optimizer do?
5. What is gradient descent, conceptually?
6. What happens when the learning rate is too high or too low?
7. What is the difference between a batch, a step, and an epoch?
8. If a training job runs out of GPU memory, what configuration or workload factors would you investigate?

## Progress

- Lesson taught: Yes
- Knowledge check: 7/8
- Practical exercise: Pending user's written answers and review
- Completion status: **Not yet complete**
