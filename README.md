---
title: Cyber-Redline Arena V2
emoji: 🔴
colorFrom: red
colorTo: gray
sdk: docker
app_port: 7860
pinned: true
license: mit
---

# Cyber-Redline Arena V2 🔴

### Verifiable Reinforcement Learning Training Infrastructure for Multi-Agent Adversarial Cybersecurity
**A Meta OpenEnv Hackathon Submission | Theme: Multi-Agent + Fleet AI Process Supervision**

[![Hugging Face Space](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Space-blue)](https://huggingface.co/spaces/markjoseph2003/cyber-redline-arena)
[![Blog](https://img.shields.io/badge/📖-Blog-red)](BLOG.md)

Cyber-Redline Arena V2 is a high-fidelity, OpenEnv-compliant training ground designed to solve the **Strategic Horizon Problem** in autonomous security LLMs. By combining **SFT-to-GRPO transfer learning** with a **Dual-LLM Adversarial Environment**, we enable agents to master multi-hop attack chains while maintaining operational stealth.

---

## 🔗 Submission Materials
*   **Live Environment**: [Hugging Face Space (Cyber-Redline Arena V2)](https://huggingface.co/spaces/markjoseph2003/cyber-redline-arena) 🚀
*   **Mini-Blog**: [Detailed Technical Writeup (BLOG.md)](BLOG.md)
*   **Video Demo**: [YouTube Video Link](https://www.youtube.com/watch?v=dQw4w9WgXcQ) *(Placeholder)*
*   **Video Script**: [DEMO_VIDEO_SCRIPT.md](DEMO_VIDEO_SCRIPT.md)
*   **Training Notebooks**:
    *   [SFT Training (Colab)](training/colab_sft_training.ipynb)
    *   [GRPO Reinforcement Learning (Colab)](training/colab_grpo_v2.ipynb)
*   **Environment Manifest**: [openenv.yaml](openenv.yaml)

---

## 🚀 The Training Pipeline: SFT-to-GRPO (DeepSeek-style)

V2 implements a specialized two-stage pipeline to overcome the "Cold Start" problem in cyber RL.

### 1. Stage 1: SFT (Bootstrapping Formatting & Heuristics)
The model is initialized with a **LoRA adapter (Rank 16)** trained on 447 expert winning trajectories.
- **Goal**: Internalize the strict JSON action format and basic prerequisite ordering.
- **Evidence**: Loss converged from `0.30` to `0.0024` over 3 epochs.

![Training Curves](results/training_curves.png)

### 2. Stage 2: GRPO (Tactical Advantage Optimization)
We then apply **Group Relative Policy Optimization** to fine-tune the strategic decision-making.
- **Group Rollouts**: For every prompt, GRPO generates 4 samples. It rewards the samples that achieve the highest relative advantage (e.g., choosing `http_get` to minimize detection).
- **Adversarial Loop**: The training loop executes actions against a **Blue Team LLM**, ensuring the model learns to adapt to dynamic defensive maneuvers.

![Reward Curves](results/reward_curves.png)

---

## 🛡️ Adversarial Multi-Agent Architecture

V2 is not a static game; it is a live interaction between three distinct LLM-powered agents.

| Agent | Module | Role | Logic |
|---|---|---|---|
| **Red Team** | `RedTeamAgentLLM` | Attacker | SFT+GRPO Policy with Theory-of-Mind prompts. |
| **Blue Team** | `BlueTeamAgentLLM` | Defender | Adversarial agent that deploys honeypots and isolates nodes. |
| **Fleet AI** | `FleetAIVerifier` | Auditor | **Step-Level Process Supervision** and MITRE technique mapping. |

---

## ⛓️ Environment Mechanics (Deep Dive)

### 1. Composable Rubric Verifiers
Rewards are calculated via five independent axes to eliminate reward hacking.
- **`R_stealth`**: Linear penalty scale (`-0.08 x detection`). At 90+ detection, the agent is hard-expelled (-30 penalty).
- **`R_chain`**: Rewards sequential `Probe -> Exploit` chains. **Probed nodes (status 1) receive a x2 damage multiplier**, incentivizing stealthy preparation over brute-force.
- **`R_objective`**: Scalar rewards for lateral movement (+20) and flag exfiltration (+100).
- **`R_opsec`**: Catastrophic penalties for triggered honeypots (-50) or prerequisite violations (-30).
- **`R_resilience`**: A unique **Chaos Adaptation** reward. Grants +4.0 for successfully pivoting tools after a failure and -3.0 for "hammering" a broken path.

### 2. Chaos Engine (Adversarial Friction)
Every episode features a randomized `chaos_vector` simulating infrastructure instability:
- **`API_RATE_LIMIT`**: 15% probability of a `429` error on any step.
- **`TOOL_FAILURE`**: A specific tool (e.g., `execute_exploit`) is degraded for 3 consecutive steps.

### 3. VaultGuard: Multi-Phase Security
The flag is protected by a secondary security layer in `server/vault.py`:
- **Stage 1: `CODE_GATE`**: Requires a 16-character access code discovered by compromising a specific intermediate node (e.g., `active_dir`).
- **Stage 2: `RATE_LIMITED`**: Activated when Blue Team hits the `ALERT` tier. Enforces a 3-attempt limit; exceeding this triggers a **4-step lockout**.

---

## 📊 Performance Evidence

| Metric | Base Model | SFT Model (V1) | GRPO Model (V2) |
|---|---|---|---|
| **Format Adherence** | 12% | 98% | **100% (JSON Only)** |
| **Tactical Stealth** | Low (Nmap) | Moderate | **High (Probe-First)** |
| **Win Rate** | 0% | 86% | **88% (Near Optimal)** |
| **MITRE Mapping** | N/A | Heuristic | **Live LLM-Verified** |

---

## 📐 Fleet AI: Step-Level Process Supervision

V2 implements **Process Supervision** rather than just Outcome Reward. The `FleetAIVerifier` analyzes every action and generates a **Blended Alignment Score** (60% LLM / 40% Heuristic).
- **Phase Detection**: Maps actions to `RECON`, `LATERAL_MOVEMENT`, `EXPLOITATION`, or `VIOLATION`.
- **Divergence Monitoring**: If a model achieves a high objective reward but a low alignment score, it indicates **Reward Hacking**.

---

## 🛠️ Infrastructure & Tech Stack

- **Framework**: OpenEnv + Gymnasium
- **Training**: Unsloth 4-bit LoRA (RTX 5060 Optimized)
- **Model**: Qwen2.5-3B-Instruct
- **Server**: FastAPI + Uvicorn
- **Dashboard**: Vanilla JS + CSS (Cyberpunk Glassmorphism)

---
*Built by Mark Joseph and Neha Benny for the Meta OpenEnv Hackathon 2026. This environment is designed for verifiable, safe, and strategic autonomous agent research.*

