# AI Infrastructure, MLOps & LLMOps — Master Roadmap and Progress

> **Canonical roadmap:** This is the single source of truth for the learning journey, lesson order, scope and progress. We will follow this path without repeatedly redesigning it. A change is made only when there is a clear technical reason, and any change is recorded here.

## Learning destination

Become capable of designing, building, deploying, operating, securing, scaling and optimizing production AI infrastructure and MLOps/LLMOps platforms.

This curriculum is designed for an experienced Senior DevOps Engineer transitioning into **AI Infrastructure, MLOps and LLMOps**. It builds on existing strengths in Linux, Docker, Kubernetes, cloud infrastructure, Terraform, Jenkins, GitOps, security and observability.

**This is not a data-science or ML-research curriculum.** You will learn the ML and LLM concepts needed to make sound infrastructure decisions, work effectively with ML engineers, and operate AI systems in production. Mathematics is practical and intuition-led; we will not spend time on research-level derivations or implementing algorithms from scratch.

## How progress is tracked

Each lesson has an explicit status:

- [ ] **Planned** — in the curriculum, not yet taught.
- [~] **Taught** — lesson content covered; knowledge check and/or practical work still needs completion or review.
- [x] **Learner-confirmed complete** — you reported completing the required work; review may still be pending.
- [✓] **Reviewed complete** — knowledge check and practical work have been reviewed.

A lesson is not marked reviewed complete just because its content has been read. We will update the status after the actual knowledge check and practical exercise. Phase completion requires its exit criteria and practical outcome to be met.

Every lesson should include:
1. Clear first-principles explanations and accurate terminology.
2. Architecture diagrams or workflows where they improve understanding.
3. Examples grounded in real infrastructure and production operations.
4. The AI Infrastructure / MLOps engineering perspective.
5. A knowledge check.
6. A practical exercise or implementation task.
7. Explicit completion criteria.

---

# The canonical six-phase journey

## Phase 1 — Essential AI/ML and LLM Foundations

**Goal:** Understand the model lifecycle and the essential ML, deep-learning and LLM concepts required to deploy and operate AI workloads. Learn enough mathematics to understand tensors, model execution, metrics and GPU workloads—not to become a data scientist.

| Lesson | Topic | Essential coverage | Status |
|---|---|---|---|
| 01 | What Is Artificial Intelligence? | AI vs ML vs deep learning vs generative AI; learning paradigms; practical use cases | [✓] Reviewed complete — knowledge check 6/6 |
| 02 | Traditional Machine Learning | ML workflow; datasets, features and labels; training, validation and testing; common algorithm families | [✓] Reviewed complete — knowledge check and practical reviewed |
| 03 | Classification, Regression and Model Evaluation | Classification vs regression; confusion matrix; accuracy, precision, recall and F1 | [✓] Reviewed complete — knowledge check 7/8; practical calculations reviewed |
| 04 | Overfitting, Underfitting, Bias and Variance | Generalization; training vs validation behavior; regularization; early stopping; data leakage intuition | [~] Taught — knowledge check 6/8; practical exercise pending |
| 05 | Model Training Fundamentals | Parameters, weights, bias, hyperparameters, loss, optimizers, batches, steps and epochs | [~] Taught — knowledge check 7/8; practical exercise pending |
| 06 | Neural Networks and the Perceptron | Neurons, layers, weights, bias, activation functions and the perceptron | [x] Learner-confirmed complete |
| 07 | How Neural Networks Learn | Forward propagation, loss, gradients, backpropagation, chain-rule intuition and optimizers | [x] Learner-confirmed complete — practical and knowledge check reported complete; review not recorded |
| 08 | Feature Types and Feature Engineering | Numerical/categorical, continuous/discrete, nominal/ordinal; encoding; feature selection; feature pipelines; training-serving consistency | [~] Taught — knowledge check and practical exercise pending review |
| 09 | Data Preprocessing and Data Quality | Missing values, outliers, scaling, normalization, standardization, data validation and preprocessing pipelines | [~] Taught — knowledge check and practical exercise pending |
| 10 | Model Baselines and Imbalanced Datasets | Baselines, class weights, sampling, ROC-AUC, PR-AUC and metric selection | [~] Taught — knowledge check and practical exercise pending |
| 11 | Responsible AI and ML Reproducibility | Bias, fairness, explainability, privacy, lineage, experiment tracking, artifact/configuration versioning and governance | [~] Taught — knowledge check and practical exercise pending |
| 12 | Tensors, Shapes, Data Types and Devices | Scalars, vectors, matrices, tensors, dimensions, shapes, indexing, data types, CPU/GPU devices and memory intuition | [ ] Planned |
| 13 | Linear Algebra for Model Execution | Dot products, matrix multiplication, dimensions, transposes and how neural-network layers use these operations | [ ] Planned |
| 14 | Statistics and Probability for ML Operations | Mean, variance, standard deviation, distributions, correlation, probability, conditional probability and uncertainty; interpreting model metrics | [ ] Planned |
| 15 | Optimization and Gradient Intuition | Derivatives, partial derivatives, gradients, gradient descent, learning rate and numerical stability at an engineering level | [ ] Planned |
| 16 | Tokens and Tokenizers | Text-to-token conversion, token IDs, vocabulary, subword tokenization, context windows and token accounting | [ ] Planned |
| 17 | Embeddings and Vector Representations | Embedding vectors, dimensions, semantic similarity, positional information and why embeddings matter to retrieval and LLMs | [ ] Planned |
| 18 | Attention and Transformer Architecture | Queries, keys, values, self-attention, multi-head attention, masks, transformer blocks, residual connections and normalization | [ ] Planned |
| 19 | LLM Model Concepts and Inference Overview | Next-token prediction, model parameters and configuration, checkpoints, model families, context length, training vs inference and generation flow | [ ] Planned |

**Practical outcome:** Train or use a small model, evaluate it, save and reload its artifact, and run inference. Also inspect tensors, tokenization and a small open model's configuration.

**Infrastructure lens:** Model artifacts, data and tensor shapes, CPU/GPU execution, memory requirements, model inputs/outputs, reproducibility and inference as a production workload.

**Phase 1 exit criteria:** Explain the ML lifecycle; distinguish training from inference; distinguish features, parameters and hyperparameters; interpret common metrics; explain overfitting and class imbalance; understand tensors and matrix operations; explain tokenization, embeddings and the high-level Transformer flow.

## Phase 2 — Python, Docker and Local AI Deployment

**Goal:** Move from understanding models to running pretrained models as services. Build and operate local inference workloads before introducing Kubernetes complexity.

| Lesson | Topic | Essential coverage | Status |
|---|---|---|---|
| 01 | Python for AI Platform Workloads | Python environments, packaging, dependency locking, project structure, logging, exceptions and basic profiling | [ ] Planned |
| 02 | Pretrained Model Lifecycle | Model repositories, model cards, configs, weights, tokenizers, downloads, gated access, checksums and trusted sources | [ ] Planned |
| 03 | Loading Models and Running Inference | Model loading, input preparation, inference mode, output handling, CPU execution and memory behavior | [ ] Planned |
| 04 | Building an Inference API with FastAPI | Request/response schemas, validation, model lifecycle, health/readiness endpoints, API versioning and error handling | [ ] Planned |
| 05 | Docker for Model Serving | Image design, pinned dependencies, reproducible builds, non-root execution, model/image separation and image size | [ ] Planned |
| 06 | Model Storage, Cache and Persistence | Model cache, persistent volumes, download strategy, startup time, artifact storage and avoiding repeated downloads | [ ] Planned |
| 07 | CPU vs GPU Inference | Compute differences, memory/VRAM, batching basics, device selection and realistic local hardware limits | [ ] Planned |
| 08 | GPU Containers and Runtime Compatibility | NVIDIA Container Toolkit, host driver vs container CUDA runtime, compatibility checks and GPU visibility | [ ] Planned |
| 09 | Reliable Inference Services | Concurrency, async requests, queues, backpressure, timeouts, graceful shutdown, health checks and resource limits | [ ] Planned |
| 10 | Local LLM Serving Project | Run a small open model locally; expose generation API; test streaming, caching, errors and resource use | [ ] Planned |

**Practical outcome:** Build a Dockerized inference API and a local LLM service. Exercise model downloads, caching, API requests and operational failure cases.

**Infrastructure lens:** Container image design, reproducible environments, model artifact lifecycle, memory limits, startup/readiness behavior, concurrency and service reliability.

**Hardware note:** The MacBook M1 with 8 GB RAM is suitable for small CPU/local experiments, but it is not an NVIDIA CUDA environment. CUDA and NVIDIA GPU work will use a suitable remote or cloud GPU environment when required.

## Phase 3 — Kubernetes and Cloud AI Infrastructure

**Goal:** Deploy and operate AI workloads on Kubernetes, including GPU-enabled workloads. Apply existing Kubernetes and cloud engineering skills to AI-specific scheduling, storage, networking and reliability problems.

| Lesson | Topic | Essential coverage | Status |
|---|---|---|---|
| 01 | AI Workload Patterns on Kubernetes | Online inference services vs batch training/inference jobs; Deployments, Jobs, Stateful workloads and workload lifecycle | [ ] Planned |
| 02 | AI Cluster and GPU Node Architecture | CPU/GPU node pools, instance families, quotas, capacity, drivers, node preparation and failure domains | [ ] Planned |
| 03 | NVIDIA Kubernetes Stack | Device plugin, NVIDIA GPU Operator, container runtime integration, drivers and GPU resource discovery | [ ] Planned |
| 04 | GPU Scheduling and Placement | GPU resource requests, node selectors, affinity, taints/tolerations, topology awareness and scheduling failures | [ ] Planned |
| 05 | Model and Dataset Storage | PVCs, CSI, object storage, model download/cache patterns, local NVMe considerations and storage throughput | [ ] Planned |
| 06 | Networking and Service Exposure | Services, ingress/Gateway API, load balancing, DNS, TLS, NetworkPolicies and inference traffic paths | [ ] Planned |
| 07 | Scaling AI Workloads | HPA, KEDA, custom metrics, queue depth, concurrency, scale-to-zero trade-offs and GPU scaling limitations | [ ] Planned |
| 08 | Reliability and Rollouts | Probes, startup/warm-up, graceful termination, PDBs, rolling updates, model loading failures and rollback | [ ] Planned |
| 09 | Infrastructure as Code for AI | Terraform for GPU nodes, IAM, networking, storage, secrets integration, quotas and environment provisioning | [ ] Planned |
| 10 | Kubernetes AI Deployment Project | Deploy an inference service with manual YAML first, then Helm; add storage, scheduling, security and monitoring basics | [ ] Planned |

**Practical outcome:** Deploy an inference workload to Kubernetes, expose it, persist or retrieve model artifacts, and operate it reliably. Then deploy a GPU-enabled workload on a compatible cluster.

**Infrastructure lens:** Kubernetes scheduling, GPU allocation, node pools, storage performance, network paths, autoscaling, cloud quotas and operational resilience.

## Phase 4 — MLOps and LLMOps Lifecycle Automation

**Goal:** Build repeatable, traceable and governed ML/LLM delivery workflows using familiar CI/CD and GitOps practices.

| Lesson | Topic | Essential coverage | Status |
|---|---|---|---|
| 01 | MLOps and LLMOps Architecture | ML lifecycle vs LLM application lifecycle; training, evaluation, registry, deployment and monitoring responsibilities | [ ] Planned |
| 02 | Experiment Tracking with MLflow | Tracking server, experiments, runs, parameters, metrics, artifacts, backend store and artifact store | [ ] Planned |
| 03 | Dataset and Artifact Versioning | Dataset manifests, DVC concepts, immutable artifacts, model versions, checksums and provenance | [ ] Planned |
| 04 | Model Registry and Lineage | Registry concepts, model versions/aliases, run lineage, approval state, promotion and rollback | [ ] Planned |
| 05 | Training and Evaluation Workloads | Containerized training jobs, Kubernetes Jobs, resource requests, artifacts, checkpoints and failure recovery | [ ] Planned |
| 06 | Workflow Orchestration | Pipeline DAGs, dependencies, retries, caching, parameters and artifact handoff; Kubeflow Pipelines or Argo Workflows where appropriate | [ ] Planned |
| 07 | CI/CD for Models | Jenkins integration, data validation, tests, training triggers, evaluation gates, artifact publishing and immutable references | [ ] Planned |
| 08 | GitOps and Model Deployment | ArgoCD, environment promotion, deployment manifests, model version pinning, canary/rollback and release governance | [ ] Planned |
| 09 | LLM Evaluation and Release Gates | Evaluation datasets, prompt/model regression, quality checks, safety checks, offline evaluation and controlled promotion | [ ] Planned |
| 10 | MLOps End-to-End Project | Build a pipeline from source/data validation through training, MLflow tracking, registry, approval and Kubernetes deployment | [ ] Planned |

**Practical outcome:** Build an automated ML delivery pipeline using Docker, Jenkins, MLflow, object storage, Kubernetes and ArgoCD. Understand how the same lifecycle principles extend to LLM applications.

**Infrastructure lens:** Control planes and metadata stores, artifact storage, pipeline workers, reproducible builds, credentials, permissions, auditability, deployment gates and rollback.

## Phase 5 — Production LLM Serving and GPU Performance Engineering

**Goal:** Understand and operate high-performance LLM inference systems. This is the dedicated deep dive into inference internals, serving engines, GPU memory and performance.

| Lesson | Topic | Essential coverage | Status |
|---|---|---|---|
| 01 | LLM Inference Request Lifecycle | Request admission, tokenization, scheduling, model execution, generation, streaming and response completion | [ ] Planned |
| 02 | Prefill and Decode | Prompt processing, autoregressive token generation, compute patterns and why the two stages behave differently | [ ] Planned |
| 03 | KV Cache and Memory Management | Key/value cache, memory growth with context/concurrency, cache allocation, fragmentation and capacity trade-offs | [ ] Planned |
| 04 | Inference Performance Metrics | Time to first token (TTFT), inter-token latency, tokens/sec, throughput, concurrency, queue time and tail latency | [ ] Planned |
| 05 | Batching and Request Scheduling | Static/dynamic/continuous batching, admission control, queueing, fairness and throughput/latency trade-offs | [ ] Planned |
| 06 | vLLM Architecture and Deployment | vLLM serving architecture, OpenAI-compatible API, streaming, model loading, configuration and Kubernetes deployment | [ ] Planned |
| 07 | GPU Architecture and CUDA for Serving | GPU compute, VRAM, memory bandwidth, drivers, CUDA runtime, kernel execution and compatibility troubleshooting | [ ] Planned |
| 08 | Precision and Quantization | FP32, FP16, BF16, INT8, INT4, quantization trade-offs, model formats and quality/performance implications | [ ] Planned |
| 09 | Multi-GPU Inference | Tensor parallelism, pipeline parallelism concepts, NCCL, GPU topology, communication overhead and placement | [ ] Planned |
| 10 | Benchmarking and Optimization | Workload design, load testing, profiling, bottleneck analysis, throughput tuning, capacity estimation and cost per token | [ ] Planned |
| 11 | Serving Engine Trade-offs | vLLM, llama.cpp and NVIDIA Triton; when each is appropriate and what operational differences matter | [ ] Planned |

**Practical outcome:** Deploy an open LLM with vLLM, expose its API, generate concurrent traffic, measure TTFT/latency/throughput, inspect GPU memory and tune serving configuration.

**Infrastructure lens:** GPU memory capacity, KV cache, batching efficiency, GPU utilization, multi-GPU topology, serving latency, throughput and cost per token.

## Phase 6 — Production AI Platform Engineering

**Goal:** Operate a secure, observable, reliable, multi-tenant and cost-aware AI platform. Combine AI infrastructure, MLOps and LLMOps into production architecture.

| Lesson | Topic | Essential coverage | Status |
|---|---|---|---|
| 01 | AI Observability | Metrics, logs, traces, OpenTelemetry, Prometheus, Grafana, inference metrics, GPU metrics and correlation IDs | [ ] Planned |
| 02 | SLOs, Reliability and Resilience | Availability, latency/error SLOs, capacity, high availability, failure recovery, disaster recovery and incident response | [ ] Planned |
| 03 | Autoscaling and Capacity Engineering | GPU utilization, queue-aware scaling, replica sizing, warm pools, capacity reservations and load forecasting | [ ] Planned |
| 04 | AI Platform Security | IAM/RBAC, workload identity, secrets, KMS, network isolation, image/artifact provenance, model supply chain and access audit | [ ] Planned |
| 05 | LLM Application Security | Prompt injection, sensitive-data leakage, unsafe tool execution, guardrail boundaries, input/output validation and sandboxing concepts | [ ] Planned |
| 06 | Multi-Tenancy and Platform Governance | Tenant isolation, quotas, rate limits, resource fairness, namespaces, policy controls and usage attribution | [ ] Planned |
| 07 | AI FinOps | GPU cost drivers, utilization, idle capacity, right-sizing, spot/on-demand trade-offs, cost per request/token and chargeback | [ ] Planned |
| 08 | RAG Infrastructure and Operations | Ingestion pipeline, embeddings, vector database/index, retrieval flow, persistence, backups, access control and retrieval observability | [ ] Planned |
| 09 | Agent and Tool-Using Workload Operations | Tool execution boundaries, worker queues, state, timeouts, retries, sandboxing, tracing and resource controls | [ ] Planned |
| 10 | Production AI Platform Capstone | Design and implement a secure, observable, scalable AI platform with model serving, MLOps workflow, GitOps, SLOs and FinOps | [ ] Planned |

**Practical outcome:** Deliver a portfolio-grade AI platform architecture and implementation that brings together model delivery, GPU inference, Kubernetes, observability, security, scaling and cost controls.

**Infrastructure lens:** Production ownership: reliability, security, multi-tenancy, capacity, cost, upgrades, disaster recovery and operational runbooks.

---

## Portfolio projects

These projects are the practical checkpoints for the journey. We will build them in sequence and reuse components rather than starting from scratch each time.

| Project | What it demonstrates | Target phase |
|---|---|---|
| 01 — Dockerized ML Inference Service | Pretrained/small model, FastAPI, Docker, model artifact loading and health endpoints | 2 |
| 02 — Local LLM API | Model/tokenizer loading, generation API, streaming, cache and resource observation | 2 |
| 03 — Kubernetes Inference Platform | Deployment, Service, storage, configuration, probes, Helm and GitOps basics | 3 |
| 04 — GPU-Enabled Kubernetes Workload | GPU Operator/device plugin, scheduling, GPU visibility and resource troubleshooting | 3 |
| 05 — End-to-End MLOps Pipeline | Jenkins, MLflow, versioned artifacts, evaluation gates, registry and ArgoCD deployment | 4 |
| 06 — Production vLLM Serving | LLM deployment, concurrent load test, TTFT/throughput metrics, GPU memory and tuning | 5 |
| 07 — Production AI Platform | Observability, security, multi-tenancy, scaling, RAG operations, SLOs and FinOps | 6 |

## Core technology stack

We will learn a focused stack deeply instead of collecting tools without a reason.

| Area | Primary tools / concepts |
|---|---|
| Languages and APIs | Python, FastAPI, HTTP, async processing |
| Containers | Docker, OCI images, NVIDIA Container Toolkit |
| Orchestration | Kubernetes, Helm |
| Cloud and IaC | AWS/GCP, Terraform, IAM, object storage, networking and GPU instances |
| Model ecosystem | Hugging Face Hub, model/tokenizer/config artifacts, Safetensors, GGUF |
| Inference | vLLM, llama.cpp; Triton as an alternative architecture |
| MLOps | MLflow, model registry, dataset/artifact versioning |
| Pipelines and delivery | Jenkins, Kubernetes Jobs, Argo Workflows/Kubeflow Pipelines where justified, ArgoCD |
| Observability | Prometheus, Grafana, OpenTelemetry |
| RAG infrastructure | Embeddings, vector search, Qdrant/FAISS or another selected vector store |

We will introduce other tools only when they solve a concrete problem. KServe, Kueue, Ray Serve, Kubeflow and similar systems are useful technologies to understand, but they are not all mandatory prerequisites.

## Cross-cutting engineering topics

These are revisited in the phase where they matter:

- **Linux and systems:** processes, memory, filesystems, networking, profiling and debugging.
- **Python engineering:** packaging, environments, dependency locking, testing and async execution.
- **Networking:** DNS, TLS, HTTP/gRPC concepts, proxies, load balancing, ingress and service-to-service communication.
- **Storage:** object storage, PVCs, caching, throughput, persistence, backup and recovery.
- **Security:** IAM, RBAC, secrets, encryption, network policy, image scanning and artifact provenance.
- **Reliability:** health checks, graceful shutdown, retries, timeouts, queues, backpressure, SLOs and incident response.
- **Observability:** metrics, logs, traces, dashboards, alerting and workload-level telemetry.
- **FinOps:** utilization, capacity, right-sizing, cost attribution and cost per workload.
- **Infrastructure as Code and GitOps:** Terraform, Jenkins, ArgoCD, immutable artifacts and controlled promotion.

## Explicit scope boundaries

To keep the journey focused on the target role:

**We will cover:** ML lifecycle fundamentals; practical math; model artifacts; tokenization, embeddings and Transformers; inference internals; GPU infrastructure; Kubernetes AI workloads; MLOps/LLMOps automation; evaluation; RAG/agent infrastructure; production observability, security, reliability and FinOps.

**We will not make core prerequisites out of:** research-level statistics, deriving ML algorithms in depth, advanced feature-engineering specialization, developing novel neural-network architectures, training foundation models from scratch at research scale, or building data-science portfolios. We may study specialist topics when they directly affect an infrastructure design decision.

## Current progress snapshot

- **Phase 1 lessons 01–03:** reviewed complete.
- **Phase 1 lessons 04–05:** taught; practical work pending.
- **Phase 1 lesson 06:** learner-confirmed complete.
- **Phase 1 lesson 07:** learner-confirmed complete; review not recorded.
- **Phase 1 lesson 08:** taught; knowledge check and practical review pending.
- **Phase 1 lessons 09–10:** taught; knowledge checks and practical exercises pending.
- **Phase 1 lesson 11:** taught; knowledge check and practical exercise pending.
- **Phase 1 lessons 12–19:** planned.
- **Phases 2–6:** planned.

## Rules for continuing the journey

1. This six-phase roadmap is the canonical path. Do not create alternate roadmaps or reorder phases casually.
2. Before starting a lesson, identify its phase, lesson number, prerequisites and practical outcome.
3. Teach at the depth required for AI Infrastructure / MLOps / LLMOps engineering, using DevOps and production-system analogies.
4. Do not skip essential concepts just to reach tools quickly. Explain why each concept matters to deployment, operations, performance, security or cost.
5. Do not turn lessons into data-science or research courses.
6. Keep lesson numbering stable. If a genuinely necessary topic is discovered, add it to the most relevant phase and explain why; do not silently restructure the journey.
7. After a lesson, update this file's status only according to the progress definitions above.
8. When a lesson is marked complete, preserve the knowledge-check result and practical-review outcome where known.
9. Individual lesson documents may be added as we study them. This file remains the master index and progress tracker.
10. The next learning activity is to close the outstanding Phase 1 knowledge checks and practical exercises, then continue with the remaining Phase 1 lessons in order.
