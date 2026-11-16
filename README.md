# OpenGP — Proposal Showcase

Static showcase website for our final-year project proposal: **Peer-to-Peer Volunteer Computing System for Distributed LLM Inference**.

## Overview

OpenGP enables organizations to run non-interactive batch LLM inference across heterogeneous volunteer consumer GPUs without out-of-memory crashes or losing progress when nodes disconnect. Key mechanisms include:

- **Active / Passive Architecture:** Pipeline-parallel GPU inference backed by GPU-free passive peers storing `(N, K)` Reed-Solomon erasure-coded activation shards indexed in a Kademlia DHT.
- **Query-Aware KV Cache Streaming:** Keeps the full KV cache in CPU RAM and streams only the top 5–10% relevant entries into GPU VRAM per token.

## Team

- **Pratheep S.**
- **Sirithar R.**
- **Suriya G.**
- **Vithurshan S.**

**Supervisor:** Dr. Adeesha Wijayasiri

## Tech Stack

Plain HTML5, CSS3 (custom properties, mobile-first Grid/Flex, self-hosted `InterVariable.woff2`), and progressive-enhancement vanilla JS. Zero frameworks, zero build steps, and zero external runtime requests.

## Run Locally

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000/` (or serve the parent directory to test sub-path hosting at `/proposal-showcase/`).

## Deployment (GitHub Pages)

1. Push the `main` branch to GitHub.
2. In the repository, go to **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**, choose **`main`** and **`/ (root)`**, then save.
4. `.nojekyll` is included at the repository root so static assets are served directly.

## Credits

Content is derived from the OpenGP Final Year Project Proposal presentation. Typography uses the self-hosted [Inter](https://rsms.me/inter/) variable font.
