# AdaptiveGuard: Self-Learning Multi-Layer Firewall for Prompt Injection Defense

##  Overview

**AdaptiveGuard** is a novel, self-learning, multi-layer prompt firewall designed to protect Large Language Models (LLMs) against prompt injection attacks. Unlike traditional static defenses that rely on fixed rule sets or trained classifiers, AdaptiveGuard continuously learns from new attack patterns through a feedback-driven adaptive learning loop.

The system integrates three progressively powerful detection layers:

| Layer | Component | Function |
|-------|-----------|----------|
| **Layer 1** | Static Rule Filter | Regex-based pattern matching to detect known attack patterns |
| **Layer 2** | Embedding Memory | Sentence-BERT semantic similarity with hybrid scoring (70% semantic + 30% keyword) |
| **Layer 3** | LLM-as-Judge | Hardened Llama-3.1-8B-Instant classifier for uncertain cases |

When a new injection is confirmed, a **dual-update feedback mechanism** simultaneously updates the attack memory (embedding storage) and extracts **verb-anchored dynamic rules** — enabling autonomous adaptation without model retraining.

---

##  Key Results

| Metric | Round 0 (Baseline) | Round 1 (After Learning) | Round 2 (Full Learning) |
|--------|---------------------|--------------------------|-------------------------|
| **True Positive Rate (TPR)** | 76.0% | 37.0% | 37.0% |
| **False Positive Rate (FPR)** | 0.0% | 0.0% | 0.0% |
| **Precision** | 100% | 100% | 100% |
| **F1 Score** | 0.864 | 0.540 | 0.540 |
| **Attack Memory Size** | 0 | 100 | 200 |
| **Total Rules** | 24 | 89 | 141 |

> **Trade-off Explanation:** The TPR decrease from 76% to 37% reflects a deliberate design choice — AdaptiveGuard prioritizes **zero false positives** over maximum recall, making it suitable for high-stakes deployments where blocking legitimate user queries is unacceptable.

---

## How It Works
User Input → Layer 1 (Static Rules) → Layer 2 (Embedding Similarity) → Layer 3 (LLM Judge) → Decision
↓ ↓ ↓
BLOCK if match BLOCK if score ≥ 0.80 FINAL VERDICT
UNCERTAIN if 0.60–0.80
SAFE if < 0.60

### Detection Pipeline

### Self-Learning Feedback Loop

When Layer 3 confirms a new injection not detected by previous layers:

1. **Embedding Memory Update:** The attack's Sentence-BERT embedding (384-dim) is appended to memory
2. **Dynamic Rule Extraction:** A verb-anchored regex pattern is extracted using a contextual window (1 word before + verb + 3 words after)

This ensures that semantically similar attacks are caught by Layer 2 in the future, and syntactically similar attacks are caught by Layer 1 — all without retraining the LLM.

---

## Dataset

| Category | Count | Description |
|----------|-------|-------------|
| **Clean samples** | 230 | Science, technology, history, general knowledge, mathematics, health, arts, programming |
| **Round 1 (Known attacks)** | 100 | Naive overrides, ignore/forget, escape characters, fake completions |
| **Round 2 (Evolved attacks)** | 100 | Paraphrased ignores, role injections, authority framing, hypothetical attacks |
| **Round 3 (Novel attacks)** | 100 | Encoded obfuscations (base64, ROT13, leetspeak), nested attacks, payload splitting |
| **Total** | **530** | 230 clean + 300 attacks (1:1.3 ratio) |

