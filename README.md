# Awesome-Model-Inference-Servers

# Top Model Inference Servers Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on PagedAttention, TensorRT Compilation & Hardware-Specific Throughput*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Model Inference Servers**. These are the serving engines and platforms that run trained models in production — handling request batching, KV cache management, and hardware acceleration — turning a checkpoint on disk into a low-latency API endpoint.

**Examples** include vLLM, SGLang, NVIDIA Triton, TensorRT-LLM, TGI, Ollama, and the commercial cloud inference providers (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, GPU-agnostic serving, and transparent performance characteristics — with a critical caveat: **TGI entered maintenance mode on December 11, 2025**, and Hugging Face now recommends vLLM or SGLang for new deployments [citation:11][citation:13].

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Google Cloud TPU (vLLM TorchTPU)** — **The managed TPU inference path** with vLLM's standard CLI, OpenAI-compatible server, scheduler, and KV cache manager running unchanged on TPU [citation:4]. **Pallas kernels for attention, MoE, and quantized matmul** push TPU performance in open source [citation:4]. **Best for teams wanting vLLM's ecosystem on Google TPU** without maintaining separate model code.

- **AWS Inferentia / Trainium** — AWS's custom silicon for inference and training. Managed through SageMaker and Neuron SDK. **Best for AWS-native teams** wanting cost-optimized inference on AWS silicon.

- **Cerebras Cloud** — **The speed leader** with wafer-scale engines. **gpt-oss-120b at 1,720.7 tokens/second** — 49.1x faster than DeepInfra's 35.0 t/s [citation:10]. **Best for latency-critical applications** where throughput justifies premium pricing ($0.39 vs CoreWeave's $0.04 per 1M tokens) [citation:10].

- **GroqCloud** — **LPU-based inference** with listed speeds of 1,000 tok/s [citation:5]. **gpt-oss-120b at 474.1 t/s** with 4.88s latency [citation:10]. **Best for teams wanting fast inference** without managing hardware.

- **SambaNova** — **RDU-based inference** at 708.2 t/s for gpt-oss-120b [citation:10]. **Best for enterprises** wanting high throughput on custom silicon.

## Open-Source GitHub Projects

- **[vLLM](https://github.com/vllm-project/vllm)** — **The general-purpose production default for self-hosted LLM serving** — Apache 2.0 licensed, **77K+ GitHub stars** [citation:6]. **PagedAttention** is the core innovation, managing KV cache in non-contiguous blocks to eliminate memory fragmentation and enable high-throughput batching [citation:6]. **The broadest hardware and quantization support** — runs on NVIDIA, AMD, and now Google TPU via the TorchTPU plugin [citation:4][citation:13]. **OpenAI-compatible API**, continuous batching, and the largest ecosystem of the serving engines [citation:6]. **Best for most teams** starting a self-hosted LLM deployment — the safest default when you want broad hardware support and a mature community.

- **[SGLang](https://github.com/sgl-project/sglang)** — **The agentic and prefix-heavy specialist** — Apache 2.0 licensed, **20K+ GitHub stars** [citation:6]. **RadixAttention** reuses shared prompt prefixes across requests — the key advantage for multi-turn agents, RAG pipelines with shared context, and structured output workloads [citation:6]. **Native compressed FSM** for the fastest structured/JSON generation among the major engines [citation:6]. **400,000+ GPUs in production** including xAI, LinkedIn, Cursor, Oracle, GCP, Azure, and AWS [citation:13]. **Best for agentic workloads** where context keeps growing and tool outputs repeat prefixes.

- **[NVIDIA Triton Inference Server](https://github.com/triton-inference-server/server)** — **The enterprise multi-model serving platform** — BSD-3-Clause licensed. **Supports PyTorch, TensorFlow, ONNX, TensorRT, OpenVINO, and Python backends** in one server [citation:2]. **Dynamic batching across different model types**, ensemble pipelines, and production-grade monitoring [citation:11]. **For LLM workloads, typically runs with the TensorRT-LLM backend** — maximum NVIDIA performance but requires engine compilation and version pinning [citation:11]. **Trade-off**: benchmarks show **Triton's vLLM backend adds overhead** compared to standalone vLLM at higher QPS due to response packaging [citation:11]. **Best for enterprises needing multi-model pipelines** (LLM + embedding + reranker) with MLOps teams experienced in TensorRT.

- **[TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM)** — **The peak NVIDIA throughput engine** — Apache 2.0 licensed. **Fastest kernels on NVIDIA hardware** with FP8/FP4 quantization support [citation:1]. **Requires engine compilation** — a step that adds friction for rapid model iteration and version pinning [citation:1]. **Best for teams with dedicated engineers** extracting maximum throughput from NVIDIA Hopper/Blackwell GPUs.

- **[Ollama](https://github.com/ollama/ollama)** — **The dev-laptop one-command install** — MIT licensed. **Dead-simple local inference** for prototyping and development [citation:1]. **15-30% slower than llama.cpp** due to Go + CGo + HTTP serialization, with a **3x throughput gap under production-like load** [citation:13]. **Not built for concurrency** [citation:1]. **Best for local development and prototyping**, not production serving.

- **[llama.cpp](https://github.com/ggerganov/llama.cpp)** — **The CPU-first engine** — MIT licensed. **Fastest on CPU** with the widest model support and full control over quantization and threading [citation:13]. **No other engine is competitive on CPU** [citation:13]. **Best for CPU-only environments, edge devices, and Apple Silicon.**

- **[TGI (Text Generation Inference)](https://github.com/huggingface/text-generation-inference)** — **In maintenance mode as of December 11, 2025** [citation:11][citation:13]. **Historically strong observability** (Prometheus, OpenTelemetry) and deep Hugging Face ecosystem integration [citation:11]. **Only bug fixes and documentation improvements accepted** — no new features [citation:11]. **Do not start new projects on TGI** — migrate to vLLM or SGLang, both of which support OpenAI-compatible APIs to minimize client changes [citation:11].

- **[Ray Serve](https://github.com/ray-project/ray)** — **The orchestration layer** — Apache 2.0 licensed. **Multi-model routing, canaries, and autoscaling** across model serving [citation:1]. **Runs on Kubernetes with GPU node pools** — the real cost is the cluster, drivers, autoscaling, and environment lifecycle around it [citation:1]. **Best for teams serving many models** needing routing and scale-to-zero.

- **[KServe](https://github.com/kserve/kserve)** — **Kubernetes-native model serving** — Apache 2.0 licensed. **Standard inference protocol and scale-to-zero** on Kubernetes. **Best for teams already running Kubernetes** wanting declarative model deployment.

### Additional Strong Open-Source Options

- **SGLang-TorchTPU** — SGLang's full serving stack on Google Cloud TPU via a TorchTPU plugin. **RadixAttention, speculative decoding, KV cache offloading, and disaggregated serving** all supported on TPU [citation:14]. **Proven in production at Meta** [citation:14].
- **EIE (Elyne Inference Engine)** — Policy-driven multi-model GGUF serving with **TurboQuant KV cache compression** and **GPU-agnostic backend abstraction** (CUDA, ROCm, CPU). Apache 2.0 [citation:3]. **Best for local multi-model serving on a single GPU**.
- **KernelLoom** — Local GGUF and OpenVINO serving with **OpenAI-compatible API**, streaming, and benchmarking CLI [citation:8].

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- **TGI entered maintenance mode on December 11, 2025** — Hugging Face explicitly recommends vLLM or SGLang for new deployments [citation:11][citation:13]. Existing TGI deployments still work but receive no new features.
- **The choice moves cost per token by tens of percent, but idle GPUs and missing autoscaling move the total bill by multiples** — fix operations before relitigating vLLM versus TGI [citation:1].
- **Benchmarks are workload-dependent**: TensorRT-LLM often leads raw NVIDIA throughput, SGLang leads shared-prefix and agent workloads, and vLLM is strong across the board with the broadest hardware support [citation:1]. The ranking flips with model size, sequence length, concurrency, and GPU generation [citation:1].
- **Triton adds overhead for single-LLM serving** compared to standalone vLLM at high QPS — it shines in multi-model pipelines, not simple chat endpoints [citation:11].
- Self-hosted open-source solutions require GPU node pools, NVIDIA GPU Operator, model caches on PVC or object storage, autoscaling (HPA/KEDA), and readiness probes tuned to model load time [citation:1].

---

**Made for ML engineers choosing serving engines that match their hardware, scale, and workload patterns.**
Let's make model inference serving more open, hardware-aware, and workload-appropriate.
