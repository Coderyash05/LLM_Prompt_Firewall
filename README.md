# AdaptiveGuard: Self-Learning Multi-Layer Firewall for Prompt Injection Defense

## Overview

AdaptiveGuard is a self-learning multi-layer prompt firewall that protects LLMs against prompt injection attacks.

## Features

- Three-layer cascaded detection (Static Rules + Embedding Memory + LLM-as-Judge)
- Self-learning feedback loop with dual-update mechanism
- Zero false positive rate achieved
- No GPU required (Groq API only)

## Quick Start

1. Click the "Open in Colab" button above
2. Set up your Groq API key (using Colab Secrets)
3. Run all cells (Runtime → Run all)

## Results

| Metric | Round 0 | Round 1 | Round 2 |
|--------|---------|---------|---------|
| TPR | 76.0% | 37.0% | 37.0% |
| FPR | 0.0% | 0.0% | 0.0% |
| Precision | 100% | 100% | 100% |
| Memory | 0 | 100 | 200 |
| Total Rules | 24 | 89 | 141 |

## Paper

Full paper included in the repository.

## License

MIT
