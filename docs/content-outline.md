# Content Outline — OpenGP Proposal Showcase

Derived exclusively from `source/OpenGP_-_Prposal_Presentation-Presented.pdf` (50 slides). Student index numbers are omitted per privacy rules.

## 1. Slide Deck Outline (Paraphrased)

| Slides | Topic | Key Points & Figures |
|---|---|---|
| 1–2 | Title & Outline | **Peer-to-Peer Volunteer Computing System for Distributed LLM Inference**. Team OpenGP (Pratheep S., Sirithar R., Suriya G., Vithurshan S.), Supervisor: Dr. Adeesha Wijayasiri. 11-part presentation outline. |
| 3–7 | Introduction | Stateful two-phase LLM inference (Prefill: parallel, compute-bound, TTFT; Decode: autoregressive, memory-bandwidth-bound, TPOT). Memory constraints: model weights + growing KV cache (e.g., Llama 3.3 70B FP16 requires 140 GB weights + 40 GiB KV cache at 128K context, exceeding 24 GB consumer GPUs). Pipeline parallelism splits layers sequentially across nodes. Volunteer computing pools heterogeneous, dynamic (churn-prone) devices (e.g., BOINC, GIMPS). |
| 8–11 | Motivation & Problem | High GPU costs hinder AI access outside well-funded institutions while consumer GPUs sit idle. Existing distributed LLM systems target datacenter GPUs or interactive chat, lacking fault-tolerant batch inference for consumer devices. **Problem:** How to run LLM inference across volunteer personal devices without OOM crashes or losing progress on unexpected disconnections. |
| 12–19 | Literature Review | Taxonomy across Scheduling (Parallax, HexGen, SpotServe), Fault Tolerance (Petals), KV/Memory Management (FlexGen, Prima.cpp, DeServe), and Interactive vs Batch workloads. Detailed comparison tables + Petals activation-caching diagram. Gap: no existing system combines consumer device support, distributed execution, in-flight fault tolerance, low VRAM pressure, and batch workloads. |
| 20–21 | Project Objectives | (1) P2P volunteer system for distributed LLM batch inference; (2) Error-code-based fault tolerance for node churn; (3) KV-cache offloading to reduce VRAM load; (4) Baseline evaluation. |
| 22–26 | Proposed Methodology | Quantitative experimental study with 3 formal hypotheses: H1 Recovery Latency (vs Petals), H2 Context Capacity (vs FlexGen), H3 Throughput Resilience (vs Petals/DeServe). **Architecture:** Gateway Node + Active Compute Network (GPU pipeline) + Passive Network (non-GPU nodes storing Reed-Solomon (N, K) erasure-coded activation shards on RAM/SSD) + Kademlia DHT (NS1: placement & liveness; NS2: shard index & context store). **Query-Aware KV Cache Streaming:** full KV cache in CPU RAM, fast GPU summary-table filter streams only top 5–10% to VRAM. |
| 27–29 | Preliminary Experiment | 2-node testbed over University TCP network: RTX 3060 (6 GB) + RTX 4070 Ti (16 GB), Qwen2.5-7B-Instruct (28 layers, FP16, 100% VRAM). Tested 1, 3, 5, 7, 9 hops across Small/Medium/Large workloads. Finding: at ≥ 5 hops, inter-token latency exceeds 100 ms (decode drops from ~19–21 tok/s to ~8–10 tok/s), justifying non-interactive batch inference over interactive chat. |
| 30–31 | Scope | **In Scope:** Non-interactive batch workloads, bounded-VRAM KV streaming, error-coded crash recovery, heterogeneous volunteer devices/networks, Kademlia DHT. **Out of Scope:** Real-time interactive chat, model fine-tuning/reconfiguration. |
| 32–33 | Evaluation Plan | H1: Node crash mid-inference → Time to Resume vs Petals. H2: Input context length → Max context processed vs FlexGen. H3: 0%/10%/20% node dropouts → Aggregate throughput (tokens/sec) vs Petals & DeServe. |
| 34–35 | Schedule | Apr 2026 – May 2027 Gantt timeline: Literature & Formulation (Apr–Aug 2026), Proposal Evaluation (10 Sep 2026), Design & Implementation (Sep 2026–Jan 2027), Survey Paper (Dec 2026), Testing & Benchmarking (Jan–Feb 2027), FYP Showcase & Posters (Mar 2027), Final Report & Evaluation (Apr–May 2027). |
| 36–37 | Conclusion | Summary of memory/churn challenges and how pipeline parallelism + error-coded activation caching + KV streaming improve batch throughput, reliability, and utilization. |
| 38–42 | References, Thanks, Q&A | 15 academic references covering volunteer computing, distributed LLM serving, KV cache management, and Reed-Solomon coding. |
| 43–50 | Appendix (Reference) | Bandwidth bottleneck heatmap (50 MB/s needed even for INT4 interactive prefill), RAID 5, Reed-Solomon polynomial interpolation, IPFS DHT, vLLM PagedAttention, FlashAttention memory hierarchy. |

## 2. Proposed Website Section Map

Single scrolling page with sticky top navigation and slide-inspired sections:

1. **Hero** (`#hero`): Project title, one-line summary, OpenGP team & supervisor badge, subtle node-graph SVG motif, quick jump CTA. *(Slides 1, 11)*
2. **Background & Motivation** (`#motivation`): LLM Prefill/Decode phases, VRAM & KV-cache bottleneck (key numbers + SVG chart/visual), volunteer computing opportunity, and the Problem Statement callout. *(Slides 4–11)*
3. **Related Work & Gap** (`#related-work`): Interactive/filterable or clean comparison matrix of existing systems (Petals, Parallax, HexGen, SpotServe, FlexGen, Prima.cpp, DeServe, etc.) showing why OpenGP is needed. *(Slides 13–19)*
4. **Objectives & Hypotheses** (`#objectives`): 4 numbered project objectives + 3 formal hypotheses (H1 Recovery Latency, H2 Context Capacity, H3 Throughput Resilience). *(Slides 21, 23)*
5. **System Architecture** (`#architecture`): Custom inline SVG diagrams for (a) System Overview (Gateway, Active Compute Network, Passive Network, Kademlia DHT), (b) Active/Passive Reed-Solomon (N, K) Crash Recovery flow, and (c) Query-Aware KV Cache Streaming (CPU RAM → Summary Filter → Top 5–10% GPU VRAM). *(Slides 24–26)*
6. **Preliminary Experiment** (`#experiment`): 2-node University TCP setup (RTX 3060 6 GB + RTX 4070 Ti 16 GB, Qwen2.5-7B-Instruct), hop-latency findings (≥ 5 hops > 100 ms inter-token latency), and why this justifies batch inference. *(Slides 28–29)*
7. **Scope & Evaluation Plan** (`#evaluation`): In-Scope vs Out-of-Scope two-column cards + H1/H2/H3 evaluation metrics & baselines table. *(Slides 31, 33)*
8. **Project Timeline** (`#timeline`): Clean milestone & phase timeline from Apr 2026 to May 2027. *(Slide 35)*
9. **Team & Supervisor** (`#team`): 4 member cards (names only + placeholder LinkedIn & GitHub links) and supervisor Dr. Adeesha Wijayasiri. *(Slide 1)*
10. **References** (`#references`): Cleanly formatted academic reference list. *(Slides 38–40)*
