# Feature vs Parameter vs Hyperparameter

These three terms are closely related in machine learning, but they mean very different things.

The most important idea to remember:

- A **feature** is an input the model receives.
- A **parameter** is a value the model learns during training.
- A **hyperparameter** is a setting we choose to control the model or its training process.

## 1. What is a feature?

A feature is an input variable used by a machine learning model to make a prediction.

Imagine we want to predict whether a Kubernetes pod is likely to fail. We might collect:

| Feature | Example value |
|---|---:|
| CPU usage | 85 |
| Memory usage | 920 |
| Container restarts | 4 |

These measurements are features. They describe the observation we want the model to evaluate.

In a dataset, one row might look like this:

```text
CPU usage | Memory usage | Restarts | Failure
   85     |     920      |    4     |   Yes
```

The first three columns are features. The final column is the label (the answer the model is being trained to predict).

A feature is not something the model learns. It is information supplied to the model.

## 2. What is a parameter?

A parameter is a value inside the model that is learned from training data.

The most familiar parameters in a neural network are:

- **Weights** — control how strongly inputs contribute to a neuron's calculation.
- **Biases** — adjustable values added to the weighted sum.

Consider this simplified equation:

```text
y = w1*x1 + w2*x2 + w3*x3 + b
```

Where:

- `x1`, `x2`, `x3` are the input features.
- `w1`, `w2`, `w3` are weights (learned parameters).
- `b` is the bias (also a learned parameter).
- `y` is the resulting value before any further activation or output processing.

For example:

```text
y = 0.8*CPU + 0.3*Memory + 1.5*Restarts - 2
```

Here, the model applies each weight to its corresponding feature:

- CPU feature is multiplied by 0.8.
- Memory feature is multiplied by 0.3.
- Restarts feature is multiplied by 1.5.
- The bias of -2 is added.

**So, is a weight applied to a feature, or is it a parameter?**

Both statements are correct, but they describe different things:

- A weight is a **parameter** because it is learned and stored inside the model.
- That weight is **applied to a feature** when the model performs its calculation.

During training, the model adjusts its weights and biases to reduce its prediction error.

One caution: a larger weight does not automatically mean a feature is more important. Feature scaling, correlations between features, and interactions between them all affect how weights should be interpreted.

## 3. What is a hyperparameter?

A hyperparameter is a configuration value chosen by the engineer or researcher. It controls the model architecture or the training process.

Unlike weights and biases, ordinary training does not directly learn hyperparameters through backpropagation.

Examples include:

| Hyperparameter | What it controls |
|---|---|
| Learning rate | How large an update the optimizer makes |
| Batch size | Number of training examples processed in one batch |
| Number of epochs | How many times training iterates over the dataset |
| Number of hidden layers | The depth of the neural network |
| Neurons per layer | The width of each layer |
| Dropout rate | The fraction of activations randomly dropped during training |

We choose these values, train the model, evaluate its performance, and may tune them to get better results.

## 4. Quick comparison

| Concept | Meaning | Example |
|---|---|---|
| Feature | Input data supplied to the model | CPU usage, memory, restarts |
| Parameter | Value learned by the model | Weights and biases |
| Hyperparameter | Setting chosen to configure the model or training | Learning rate, batch size, number of layers |

A useful way to think about it:

```text
                 YOU CONFIGURE
                      |
                Hyperparameters
          (learning rate, batch size,
              layers, neurons)
                      |
                      v
Features ------> MODEL TRAINING ------> Learned parameters
(CPU, memory,     (optimization)         (weights, biases)
 restarts)                                      |
                                                v
                                      Trained model artifact
                                                |
                                                v
                                       Inference / prediction
```

The features are supplied as data. Hyperparameters configure how the model is built and trained. Training adjusts the model's parameters.

## 5. How this looks in PyTorch

Consider a small neural network with three input features, one hidden layer containing 128 neurons, and one output.

```python
import torch
import torch.nn as nn

class PodFailureModel(nn.Module):
    def __init__(self):
        super().__init__()

        self.network = nn.Sequential(
            nn.Linear(3, 128),
            nn.ReLU(),
            nn.Linear(128, 1)
        )

    def forward(self, x):
        return self.network(x)

model = PodFailureModel()
```

Let's classify the pieces:

- **3 input features:** CPU usage, memory usage, and restart count.
- **128 hidden neurons:** an architectural choice (hyperparameter).
- **Weights and biases inside the Linear layers:** learnable parameters.
- **ReLU:** the chosen activation function; the architecture/design choice is configured by us.

You can inspect the model's learnable parameters:

```python
for name, parameter in model.named_parameters():
    print(name, parameter.shape, parameter.numel())
```

To count the total number of learnable parameters:

```python
total_parameters = sum(
    parameter.numel()
    for parameter in model.parameters()
)

print("Total parameters:", total_parameters)
```

The `nn.Linear(3, 128)` layer has a weight matrix with shape `(128, 3)` and a bias vector with shape `(128,)`. The next `nn.Linear(128, 1)` layer has a weight matrix with shape `(1, 128)` and a bias vector with shape `(1,)`.

## 6. How this connects to LLMs

When you hear that an LLM has 7 billion, 70 billion, or hundreds of billions of parameters, this primarily refers to the learned numerical values (especially weights) inside its neural network.

It does **not** mean the model has billions of input features.

For an LLM:

- **Input:** tokens representing the prompt.
- **Parameters:** learned weights in the transformer layers, including attention and feed-forward components.
- **Hyperparameters / architecture choices:** number of layers, hidden dimension, attention heads, vocabulary size, context length, and training configuration.

The exact terminology can vary slightly by context. For example, context length is an architecture or runtime configuration, while learning rate and batch size are training hyperparameters.

## 7. Knowledge check

Try answering these without looking back:

1. In a dataset containing CPU usage, memory usage, and restart count, which items are the features?
2. Are weights features, parameters, or hyperparameters?
3. What does a weight do to an input feature in a neuron's calculation?
4. Who or what determines the values of weights during ordinary neural-network training?
5. Is learning rate a parameter or a hyperparameter?
6. Is the number of hidden layers a learned parameter or an architectural hyperparameter?
7. When someone says an LLM has billions of parameters, what are they referring to?
8. If we change the learning rate but keep the dataset the same, have we changed the features?

## Key takeaway

**Features go into the model. Parameters are learned inside the model. Hyperparameters configure the model and the way it learns.**

A weight is a parameter, and that parameter is applied to an input feature during the model's computation.
