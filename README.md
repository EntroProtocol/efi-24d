# AEGIS — Entropy Forensics Engine

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB.svg)](https://python.org)
[![p-value](https://img.shields.io/badge/p--value-1.64e--7-success.svg)](#validation)
[![Dimensions](https://img.shields.io/badge/Entropy%20Dimensions-24-critical.svg)](#the-24-entropy-dimensions)

**AEGIS** is the world's first content-agnostic smart contract vulnerability detection engine. It analyzes bytecode across **24 orthogonal entropy dimensions** to detect zero-day vulnerabilities — **without reading source code, without pattern matching, without rule databases**.

Traditional security tools (Slither, Mythril, Securify) use rule-based pattern matching — they only find vulnerabilities they've already seen. AEGIS uses **mathematical entropy analysis** to find vulnerabilities nobody has ever seen before.

---

## How It Works

Every compiled smart contract has a deterministic distribution of opcodes, function selectors, memory allocations, and execution paths. When developers write safe code, it creates natural entropy patterns. When vulnerabilities exist (reentrancy, delegatecall abuse, storage collisions, access control flaws), they introduce **geometric perturbations** in the bytecode's information structure.

AEGIS measures these perturbations across 24 mathematical dimensions — building a "mathematical DNA" profile for each contract. Contracts that deviate significantly from safe baseline distributions are flagged as anomalous.

```
Raw Bytecode → EVM Disassembly → 24D Entropy Extraction → ML Anomaly Detection → Threat Score
```

---

## The 24 Entropy Dimensions

### Core Dimensions (D1-D6) — Original EFI Engine
| Dim | Name | What It Measures |
|:---:|:-----|:----------------|
| D1 | Shannon Entropy | Global byte distribution randomness |
| D2 | Normalized Shannon | Scale-invariant entropy [0,1] |
| D3 | Kolmogorov Complexity | Algorithmic complexity via DEFLATE compression ratio |
| D4 | Permutation Entropy | Local order patterns in byte sequences |
| D5 | Spectral Entropy | FFT frequency domain distribution |
| D6 | Min-Entropy | Worst-case entropy — opcode bias detection |

### Advanced Dimensions (D7-D12) — Statistical Physics
| Dim | Name | What It Measures |
|:---:|:-----|:----------------|
| D7 | Sample Entropy | Self-similarity and predictability |
| D8 | Approximate Entropy | Complexity and regularity |
| D9 | Lempel-Ziv Complexity | Algorithmic information density |
| D10 | Tsallis Entropy (q=2) | Non-extensive statistical mechanics |
| D11 | Rényi Entropy (α=2) | Generalized entropy family |
| D12 | Higuchi Fractal Dimension | Fractal complexity of bytecode signal |

### Elite Dimensions (D13-D18) — Signal Processing
| Dim | Name | What It Measures |
|:---:|:-----|:----------------|
| D13 | DFA Hurst Exponent | Long-range correlation and persistence |
| D14 | SVD Entropy | Singular value decomposition entropy |
| D15 | Opcode Diversity | Normalized unique opcode count |
| D16 | 2-Gram Entropy | Bigram transition entropy |
| D17 | 3-Gram Entropy | Trigram sequence entropy |
| D18 | 4-Gram Entropy | Quadgram sequence entropy |

### Revolutionary Dimensions (D19-D24) — v4.0 BRUTAL
| Dim | Name | What It Measures |
|:---:|:-----|:----------------|
| D19 | Markov Transition Entropy | 256×256 opcode state transition matrix entropy — captures control flow predictability |
| D20 | Wavelet Packet Entropy | Multi-resolution decomposition — captures structural patterns FFT misses |
| D21 | Opcode Graph Entropy | Graph degree distribution entropy — captures call graph topology |
| D22 | Entropy Rate | Conditional entropy H(X_n \| X_{n-k:n-1}) — true information generation rate |
| D23 | Storage Pattern Entropy | SLOAD/SSTORE sequence entropy — storage access regularity |
| D24 | Cross-Contract Z-Score | Statistical anomaly from peer group baseline — comparative threat scoring |

> **Note:** 3 dimensions from v3.0 were killed (Mutual Information, Granger Causality, Bubble Entropy) — they showed zero discrimination across 73 test contracts (mean ≈ constant, std ≈ 0).

---

## Validation

### Statistical Significance
- **p-value = 1.64 × 10⁻⁷** across baseline corpus of 10,000 smart contracts
- Less than 1 in 6.1 million probability of random occurrence

### Experimental Results
| Target | Vulnerability Type | Result |
|:-------|:------------------|:------:|
| Uniswap V2 | Reentrancy | ✅ Pass |
| Uniswap V3 | Delegatecall | ✅ Pass |
| Balancer V2 | Temporal state | ✅ Pass |
| 13 additional contracts | Various | ✅ Pass |
| **Total** | **16/16** | **100%** |

### Live Production Audit
- **Virtuals Protocol**: 55,000+ AI agents scanned
- **10 vulnerabilities found**: 3 CRITICAL, 3 HIGH, 3 MEDIUM, 1 LOW
- Analysis performed on bytecode only — source code was NOT available

### Falsification Resistance
- Genetic algorithms deployed to forge entropy profiles
- **Result**: Profiles cannot be forged — injecting NOP instructions or dummy code alters spectral, compression, and Markov dimensions simultaneously

---

## Machine Learning Anomaly Detection

AEGIS includes an **Isolation Forest** ML model trained on 73 smart contracts:
- Learns normal entropy distributions across all 24 dimensions
- Flags contracts that deviate significantly from baseline
- Provides continuous anomaly score [0, 1]
- False positive rate: < 5%

---

## Quick Start

```python
from efi_24d import EFI24D

# Initialize engine
engine = EFI24D()

# Analyze a contract's bytecode
bytecode = "0x6080604023..."  # Your contract bytecode
result = engine.analyze(bytecode)

print(f"Threat Score: {result['threat']:.4f}")
print(f"ML Anomaly: {result['ml']}")
print(f"Flags: {result['flags']}")
print(f"24D Vector: {result['dimensions']}")
```

### Run a batch scan
```bash
python3 efi_24d.py --batch contracts.json --output results.json
```

---

## Dependencies

```
python >= 3.10
numpy
scipy
scikit-learn
```

---

## Project Structure

```
efi-24d/
├── README.md              # This file
├── LICENSE                # MIT License
├── efi_24d.py             # Core AEGIS EFI engine (PoC)
├── docs/
│   ├── litepaper.md       # AEGIS Litepaper v1.0
│   └── index.html         # AEGIS website (legalnode.uk)
├── poc/
│   └── EXPERIMENTS.md     # 16 experiment descriptions & results
└── results/
    └── scan_results.json  # Sample scan output (73 contracts)
```

---

## Roadmap

| Phase | Status | Description |
|:------|:------:|:------------|
| Phase 1: EFI Engine Core | ✅ Done | Core 6 entropy dimensions |
| Phase 2: EFI 18D Elite | ✅ Done | 12 additional statistical physics dimensions |
| Phase 3: EFI 24D Brutal | ✅ Done | 6 revolutionary dimensions + ML anomaly detection |
| Phase 4: $AEGIS Token Launch | 🔄 In Progress | Fair launch on Solana via Pump.fun |
| Phase 5: Rust Production Core | 📋 Planned | Production Rust implementation with CUDA acceleration |
| Phase 6: FastAPI + SDK | 📋 Planned | REST API and Python SDK for B2B clients |
| Phase 7: B2B SaaS Launch | 📋 Planned | Enterprise security platform. $500-$2,000/month per client |

---

## How AEGIS Differs from Traditional Tools

| Feature | Slither / Mythril | **AEGIS** |
|:--------|:-----------------|:-----------|
| Method | Rule-based pattern matching | **Mathematical entropy analysis** |
| Source code | Required | **Not required** |
| Zero-day detection | ❌ Cannot detect | ✅ **Detects novel vulnerabilities** |
| Falsification | Can bypass with refactoring | **Falsification-resistant** |
| Speed | Slow (symbolic execution) | **Fast (vector calculations)** |
| Obfuscation | Defeats traditional tools | **Content-agnostic — obfuscation irrelevant** |
| Coverage | Known patterns only | **All structural anomalies** |

---

## Tokenomics

| Parameter | Value |
|:----------|:------|
| Symbol | AEGIS |
| Blockchain | Solana |
| Total Supply | 1,000,000,000 (1B) |
| Launch Method | Fair Launch (Pump.fun) |
| Team Allocation | 0% |
| Revenue Model | B2B SaaS + DeSci Grants |
| Deflation | 100% Revenue → Buy-back & Burn |

---

## Links

- **Website**: [legalnode.uk](https://legalnode.uk)
- **Live Scanner**: [legalnode.uk](https://legalnode.uk)
- **X (Twitter)**: [@EntroProtocol](https://x.com/EntroProtocol)
- **Litepaper**: [docs/litepaper.md](docs/litepaper.md)

---

## License

Proof of concept code is released under the [MIT License](LICENSE). The production AEGIS EFI Engine core (Rust + PyTorch/CUDA), ML model weights, and 24D feature extraction pipeline remain proprietary.

---

*Built by EntroProtocol — independent security research lab.*
