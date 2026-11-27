# AI Infrastructure / MLOps Learning Roadmap & Progress

This file is the single source of truth for the curriculum, lesson sequence, and learning progress.

## How to use this roadmap

- Lessons are organized by phase; lesson numbers restart within each phase.
- **Planned** means the lesson is part of the curriculum but has not been taught.
- **Taught** means the lesson content has been covered, but the required exercise or knowledge check is not yet complete.
- **Learner-confirmed complete** means the learner has reported completing the lesson requirements; answer review may still be pending.
- **Reviewed complete** means the practical work and knowledge check have been reviewed.
- Do not silently promote a lesson to reviewed complete. Record the actual state and any pending work.
- Every lesson should include first-principles explanations, key terminology, architecture or workflow diagrams where useful, practical examples, an infrastructure perspective, a knowledge check, and a hands-on exercise.

## Learning destination

Build the knowledge and practical skills to design, deploy, operate, secure, scale, and optimize production AI/ML systems. The path starts with AI/ML foundations, progresses through deep learning and LLM engineering, and finishes with production AI platforms on Kubernetes.

The goal is applied AI Infrastructure / MLOps / LLMOps competence—not research-level mathematical specialization.

---

# Phase 1 — Artificial Intelligence Fundamentals

**Goal:** Understand what AI and ML are, how models learn, how their performance is evaluated, and how a trained model is used.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | [What Is Artificial Intelligence?](01-ai-fundamentals/01-what-is-artificial-intelligence.md) | AI vs ML vs deep learning vs generative AI; learning paradigms; practical examples | Reviewed complete — knowledge check 6/6 |
| 02 | [Traditional Machine Learning](01-ai-fundamentals/02-traditional-machine-learning.md) | ML workflow; datasets, features and labels; training, validation and testing; common algorithms | Reviewed complete — knowledge check and practical reviewed |
| 03 | [Classification, Regression, and Model Evaluation](01-ai-fundamentals/03-classification-regression-model-evaluation.md) | Classification vs regression; confusion matrix; accuracy, precision, recall and F1 | Reviewed complete — knowledge check 7/8; practical calculations reviewed |
| 04 | [Overfitting, Underfitting, Bias, and Variance](01-ai-fundamentals/04-overfitting-underfitting-bias-variance.md) | Generalization; bias/variance; training vs validation behavior; regularization and early stopping | Taught — knowledge check 6/8; practical exercise pending |
| 05 | [Model Training Fundamentals](01-ai-fundamentals/05-model-training-fundamentals.md) | Parameters, weights, bias, hyperparameters, loss, optimization, batches, steps and epochs | Taught — knowledge check 7/8; practical exercise pending |
| 06 | [Neural Networks and the Perceptron](01-ai-fundamentals/06-neural-networks-perceptron.md) | Neurons, layers, weights, bias, activations and the perceptron | Learner-confirmed complete |
| 07 | [How Neural Networks Learn: Forward Propagation, Backpropagation and Gradients](01-ai-fundamentals/07-forward-propagation-backpropagation-gradients.md) | Forward pass, loss, gradients, chain rule, backpropagation, optimizers and training iterations | Learner-confirmed complete — practical and knowledge check reported complete; answer review not recorded |
| 08 | Datasets, Features, Labels, Data Quality and Data Leakage | Samples/records, feature types, labels, missing values, categorical data, preprocessing boundaries, leakage examples | Planned |
| 09 | Model Evaluation, Baselines and Class Imbalance | Baseline models, metric selection, threshold trade-offs, imbalanced datasets, precision/recall trade-offs, ROC-AUC and PR-AUC at a conceptual level | Planned |
| 10 | The End-to-End Machine Learning Lifecycle | Data collection and preparation, training, validation, testing, packaging, deployment, inference and monitoring | Planned |
| 11 | Model Artifacts, Reproducibility and Responsible AI | Model files/checkpoints, configs, dependency capture, reproducibility, experiment metadata, bias, fairness, privacy and explainability basics | Planned |

**Phase 1 practical outcome:** Train or use a small traditional ML model, evaluate it correctly, save the model artifact, load it again, and run inference on new input.

**Phase 1 exit criteria:** Explain the ML lifecycle; distinguish training from inference; explain model parameters vs hyperparameters; interpret common evaluation metrics; identify overfitting, underfitting, data leakage and class imbalance; describe what must be saved to reproduce a model.

---

# Phase 2 — Mathematics and Data Fundamentals

**Goal:** Build enough mathematical and data intuition to understand how ML and deep learning systems represent and process information.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | Vectors, Matrices and Tensors | Scalars, vectors, matrices, tensor dimensions/shapes, indexing and basic operations | Planned |
| 02 | Linear Algebra for ML | Dot products, matrix multiplication, dimensions, transposes and how neural-network layers use them | Planned |
| 03 | Statistics and Descriptive Analysis | Mean, median, variance, standard deviation, distributions, outliers and correlation | Planned |
| 04 | Probability Fundamentals | Probability, conditional probability, independence, common distributions and intuition for uncertainty | Planned |
| 05 | Calculus and Optimization Intuition | Functions, derivatives, partial derivatives, gradients, chain rule and gradient descent intuition | Planned |
| 06 | Data Preparation and Feature Engineering | Cleaning, missing values, encoding, scaling/normalization, train/validation/test splits and feature pipelines | Planned |

**Practical outcome:** Build a repeatable data preprocessing pipeline and inspect how data transformations change model inputs.

**Infrastructure lens:** Dataset size, memory use, data formats, CPU vs GPU suitability, data loading and preprocessing bottlenecks.

---

# Phase 3 — Deep Learning

**Goal:** Understand multi-layer neural networks and gain practical experience training and using a model with PyTorch.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | Deep Neural Networks and Activation Functions | Hidden layers, depth, nonlinearities, ReLU, sigmoid, tanh, softmax and vanishing-gradient intuition | Planned |
| 02 | Neural Network Architectures | MLPs, CNNs, RNNs and LSTMs; what problem each architecture is suited to | Planned |
| 03 | PyTorch Fundamentals | Tensors, shapes, devices, autograd, modules, parameters and datasets/DataLoaders | Planned |
| 04 | Building a Training Pipeline | Model definition, forward pass, loss, optimizer, training loop, validation loop and metrics | Planned |
| 05 | Training Techniques and Stability | Learning-rate schedules, regularization, dropout, normalization, checkpointing, early stopping and gradient clipping | Planned |
| 06 | GPU Training and Memory | CPU vs GPU, VRAM, batch size, mixed precision, memory bottlenecks and profiling basics | Planned |
| 07 | Distributed Training Concepts | Data/model parallelism, multi-GPU training, communication overhead and checkpoint/recovery concepts | Planned |

**Practical outcome:** Train a small PyTorch neural network, validate it, save a checkpoint, reload it and run inference.

**Infrastructure lens:** CPU/GPU utilization, VRAM, RAM, storage throughput, data-loader performance and distributed communication.

---

# Phase 4 — LLM Fundamentals

**Goal:** Understand how large language models represent text and generate responses.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | What Is an LLM? | Language modeling, next-token prediction, model families and common capabilities/limitations | Planned |
| 02 | Tokenization and Tokenizers | Tokens, vocabulary, token IDs, subword tokenization, context windows and input/output token accounting | Planned |
| 03 | Embeddings and Positional Information | Embedding vectors, semantic representation, positional embeddings/encodings and vector dimensions | Planned |
| 04 | Attention and Self-Attention | Queries, keys, values, attention scores, masks and multi-head attention intuition | Planned |
| 05 | Transformer Architecture | Transformer blocks, residual connections, normalization, feed-forward layers and encoder/decoder patterns | Planned |
| 06 | How LLMs Are Trained and Adapted | Pretraining, supervised fine-tuning, instruction tuning, preference/alignment concepts and model limitations | Planned |

**Practical outcome:** Use a tokenizer and a small open model to inspect token IDs, embeddings/configuration and a basic generation request.

**Infrastructure lens:** Context length, model size, token throughput, memory requirements and why architecture affects serving costs.

---

# Phase 5 — LLM Model Engineering

**Goal:** Learn how LLMs are packaged, adapted, optimized and evaluated.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | Model Sizes, Configurations and Checkpoints | Parameters (1B/3B/7B/70B), layers, hidden dimensions, model config and checkpoint structure | Planned |
| 02 | Hugging Face Model Ecosystem | Model repositories, tokenizers, configs, model cards, downloads, caching and gated models | Planned |
| 03 | Fine-Tuning Fundamentals | Fine-tuning vs prompting, supervised fine-tuning, datasets and evaluation considerations | Planned |
| 04 | PEFT, LoRA and QLoRA | Adapters, low-rank adaptation, parameter-efficient tuning and resource trade-offs | Planned |
| 05 | Quantization and Model Formats | FP32, FP16, BF16, INT8, INT4, quantization trade-offs, Safetensors and GGUF | Planned |
| 06 | Model Evaluation and Packaging | Quality benchmarks, task evaluation, regression testing, artifact versioning and model release practices | Planned |

**Practical outcome:** Download and inspect an open model, understand its configuration and artifacts, run it, and experiment with a supported quantized or adapter-based variant.

**Infrastructure lens:** Artifact size, model storage, download/cache strategy, VRAM estimates and compatibility between model formats and serving engines.

---

# Phase 6 — LLM Inference and Serving

**Goal:** Understand the complete LLM inference request lifecycle and deploy models through production-style serving engines.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | Training vs Inference | Model loading, inference inputs/outputs, stateless requests and serving lifecycle | Planned |
| 02 | LLM Inference Internals | Prefill, decode, autoregressive generation, KV cache and memory growth | Planned |
| 03 | Inference Performance Metrics | Time to first token (TTFT), inter-token latency, tokens/sec, throughput, concurrency and tail latency | Planned |
| 04 | Batching and Scheduling | Static/dynamic/continuous batching, queueing, admission control and scheduling trade-offs | Planned |
| 05 | Inference Engines | vLLM, Hugging Face TGI and llama.cpp; when to consider each and their deployment trade-offs | Planned |
| 06 | Scaling and Parallelism | Tensor/pipeline parallelism, multi-GPU serving, replicas, load balancing and autoscaling concepts | Planned |
| 07 | Serving APIs and Optimization | OpenAI-compatible APIs, streaming, request limits, caching, quantization impact and performance testing | Planned |

**Practical outcome:** Serve an open LLM with an inference engine, expose an API, send concurrent requests and measure latency, throughput and resource utilization.

**Infrastructure lens:** GPU memory, KV-cache capacity, batching efficiency, GPU utilization, networking, scaling and cost per request/token.

---

# Phase 7 — RAG and Vector Databases

**Goal:** Build systems that retrieve relevant external information and provide it to an LLM as grounded context.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | RAG Architecture | Retrieval-augmented generation, ingestion/query flows and when RAG is useful | Planned |
| 02 | Document Ingestion and Chunking | File parsing, chunk size/overlap, metadata and ingestion pipelines | Planned |
| 03 | Embeddings and Vector Search | Embedding models, vector dimensions, similarity search and cosine similarity | Planned |
| 04 | Vector Database Fundamentals | Indexes, collections, metadata filters, persistence and scaling | Planned |
| 05 | Retrieval Quality | Hybrid search, reranking, top-k, retrieval evaluation and grounding | Planned |
| 06 | RAG Operations and Evaluation | Data refresh, embedding/model versioning, access control, hallucination analysis and observability | Planned |

**Tools to explore:** FAISS, Chroma, Qdrant, Weaviate and Milvus.

**Practical outcome:** Build a document-question-answering application using ingestion, chunking, embeddings, vector retrieval and LLM generation.

**Infrastructure lens:** Index storage, ingestion throughput, vector-search latency, persistence, backups and multi-tenant isolation.

---

# Phase 8 — AI Application Engineering

**Goal:** Build reliable applications around model and inference APIs.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | Python and API Foundations for AI | Python project structure, virtual environments, dependencies, HTTP and API integration | Planned |
| 02 | FastAPI Model Services | Request/response schemas, validation, model loading, health/readiness endpoints and API versioning | Planned |
| 03 | Prompting and Structured Outputs | System/user prompts, prompt templates, JSON/schema-constrained output and prompt testing | Planned |
| 04 | Tool Calling and Agent Fundamentals | Function/tool calling, tool execution, state, workflows and agent safety boundaries | Planned |
| 05 | Async, Queues and Streaming | Async I/O, background workers, job queues, streaming responses and backpressure | Planned |
| 06 | Production Application Concerns | Authentication, rate limits, retries, timeouts, error handling, secrets, logging and cost tracking | Planned |

**Practical outcome:** Build a production-style AI API with streaming, background processing, structured outputs, authentication and observability.

**Infrastructure lens:** Concurrency, queue depth, API reliability, secrets management, network boundaries and application scaling.

---

# Phase 9 — AI Infrastructure and GPU Engineering

**Goal:** Understand and operate the compute, storage, networking and accelerator infrastructure required by AI workloads.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | GPU Architecture Fundamentals | GPU vs CPU, compute units, VRAM, memory bandwidth, utilization and GPU workload behavior | Planned |
| 02 | CUDA and NVIDIA Software Stack | Drivers, CUDA toolkit/runtime, cuDNN concepts, container toolkit and compatibility | Planned |
| 03 | Cloud GPU Infrastructure | GPU instance families, capacity, quotas, pricing dimensions and GPU provisioning on AWS/GCP | Planned |
| 04 | Containers for AI Workloads | GPU-enabled containers, images, runtime configuration, model volumes and reproducible environments | Planned |
| 05 | Kubernetes GPU Scheduling | Device plugins, NVIDIA GPU Operator, resource requests, node labels, taints, tolerations and GPU node pools | Planned |
| 06 | Multi-GPU and Distributed Infrastructure | NCCL concepts, interconnects, topology, distributed communication and failure domains | Planned |
| 07 | AI Storage and Networking | Model artifact storage, caching, high-throughput storage, networking and image/model distribution | Planned |
| 08 | GPU Observability and FinOps | GPU metrics, utilization, memory, saturation, allocation, idle capacity and cost optimization | Planned |

**Practical outcome:** Provision or configure a GPU-enabled environment, deploy a GPU workload and collect useful GPU and application metrics.

**Infrastructure lens:** This phase directly connects AI workloads to cloud architecture, Terraform, containers, Kubernetes, observability and cost management.

---

# Phase 10 — MLOps and LLMOps

**Goal:** Automate, version, track, evaluate, deploy and monitor ML and LLM systems throughout their lifecycle.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | MLOps and LLMOps Lifecycle | ML lifecycle, LLMOps differences, workflow orchestration and operational responsibilities | Planned |
| 02 | Experiment and Dataset Management | Experiment tracking, parameters, metrics, artifacts, dataset versioning and reproducibility | Planned |
| 03 | Model Registry and Governance | Model versions, stages/aliases, lineage, approvals and release controls | Planned |
| 04 | ML and LLM Pipelines | Data validation, training/evaluation pipelines, orchestration, retries and pipeline artifacts | Planned |
| 05 | CI/CD and GitOps for AI | Automated tests, model packaging, deployment promotion, GitOps and rollback | Planned |
| 06 | Model and LLM Evaluation | Offline/online evaluation, quality regression, safety checks and evaluation datasets | Planned |
| 07 | Production Monitoring | Data drift, concept drift, model quality, latency, errors, token usage and cost | Planned |

**Tools to explore:** MLflow, Kubeflow, Argo Workflows, ArgoCD, DVC, Prometheus, Grafana and OpenTelemetry.

**Practical outcome:** Build an automated workflow that trains or packages a model, tracks experiments, registers an artifact, deploys it and monitors it.

---

# Phase 11 — Production AI on Kubernetes

**Goal:** Design and operate reliable, secure, observable and scalable AI platforms on Kubernetes.

| Lesson | Topic | Core coverage | Status |
|---|---|---|---|
| 01 | Kubernetes Patterns for AI Workloads | Online inference vs batch jobs, Deployments, Jobs, Stateful workloads and workload isolation | Planned |
| 02 | Model Serving Platforms | KServe, Seldon, Ray Serve and serving architecture trade-offs | Planned |
| 03 | LLM Serving on Kubernetes | Deploy vLLM, GPU scheduling, model loading, service exposure and rollout strategies | Planned |
| 04 | Autoscaling AI Workloads | HPA, KEDA, custom metrics, queue-aware scaling and GPU-aware scaling limitations | Planned |
| 05 | Reliability and Performance | Readiness/liveness, graceful shutdown, caching, warm-up, HA, failure recovery and load testing | Planned |
| 06 | Security and Multi-Tenancy | RBAC, network policies, secrets, isolation, supply-chain security and tenant quotas | Planned |
| 07 | Observability and FinOps | Metrics, logs, traces, GPU dashboards, SLOs, capacity planning and cost allocation | Planned |
| 08 | GitOps and Platform Operations | ArgoCD, environment promotion, configuration management, upgrades and operational runbooks | Planned |

**Practical outcome:** Deploy an LLM inference service on Kubernetes with GPU scheduling, autoscaling strategy, monitoring, security controls and GitOps delivery.

---

# Phase 12 — Production AI Projects and Architecture

**Goal:** Combine the skills from earlier phases into portfolio-quality systems and architecture exercises.

| Project | Scope | Status |
|---|---|---|
| 01 | End-to-End MLOps Platform — data preparation, training, experiment tracking, model registry, deployment and monitoring | Planned |
| 02 | GPU Inference Platform — open LLM, GPU scheduling, inference engine, API, load testing and observability | Planned |
| 03 | Production RAG Application — document ingestion, embeddings, vector database, retrieval, inference and evaluation | Planned |
| 04 | Multi-Tenant AI Platform — tenant isolation, quotas, model deployment workflows, access controls and usage tracking | Planned |
| 05 | Distributed Training Infrastructure — multi-GPU or multi-node training, communication, checkpointing and recovery | Planned |
| 06 | Architecture and Operations Capstone — design review, threat model, SLOs, capacity plan, FinOps analysis and incident runbooks | Planned |

**External project inspiration:** Use the [Tech5ense — 5 AI Infrastructure Projects to Build in 2026](https://www.tech5ense.com/p/5-ai-infrastructure-projects-to-build-in-2026) article as supplementary inspiration. Project scope should be selected based on what has been learned and the practical value of the implementation.

---

## Cross-phase infrastructure skills

These skills should be revisited in the phases where they become relevant rather than taught as isolated topics only:

- **Linux and Python:** environments, packaging, debugging, profiling and automation.
- **Docker and OCI:** reproducible images, GPU containers, image security and artifact management.
- **Kubernetes:** scheduling, storage, networking, scaling, workload security and operations.
- **Terraform and cloud:** provisioning GPU/CPU infrastructure, IAM, networking, storage, quotas and cost controls.
- **CI/CD and GitOps:** build, test, promotion, deployment, rollback and policy enforcement.
- **Observability:** metrics, logs, traces, dashboards, alerts and SLOs.
- **Security:** identity, secrets, access control, artifact provenance, data privacy and supply chain.
- **FinOps:** utilization, capacity, cost attribution, right-sizing and cost per workload.

## Important learning path notes

- Phase 1 introduces inference as using a trained model to produce predictions. Phase 6 is the dedicated deep dive into LLM inference and serving: request lifecycle, prefill/decode, KV cache, batching, inference engines, serving APIs, latency, throughput and optimization.
- Tokenization, embeddings, attention and Transformers are explicitly covered in Phase 4.
- Deeper neural-network architectures and practical PyTorch work are covered in Phase 3.
- GPU infrastructure and Kubernetes GPU scheduling are covered in Phase 9; production orchestration and operations are covered in Phase 11.
- The math depth is intended to be practical and intuitive, not research-level.
- Lesson titles and sequence beyond the existing Phase 1 lessons are the expanded curriculum plan. Individual lesson documents will be created as each lesson is prepared.

## Current progress snapshot

- Phase 1 lessons 01–03: reviewed complete.
- Phase 1 lessons 04–05: taught; practical exercises pending.
- Phase 1 lesson 06: learner-confirmed complete.
- Phase 1 lesson 07: learner-confirmed complete; practical and knowledge check reported complete, but answer review is not recorded in this tracker.
- Phase 1 lessons 08–11: planned.
- Phases 2–12: roadmap defined; lessons planned.

## Completion rule

A lesson is not considered **reviewed complete** merely because its content has been read. The learner should complete the knowledge check and practical exercise; answers and practical work should then be reviewed. If the learner reports completion before review, record it as learner-confirmed complete rather than implying that review has occurred.
