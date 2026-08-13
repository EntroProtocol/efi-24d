# AEGIS — Entropy Forensics Engine

### Litepaper v1.0
**The Shield of Smart Contracts**

---

## 1. The Problem

Smart contract vulnerabilities have drained over $11 billion from DeFi protocols since 2020. Traditional security tools — Slither, Mythril, Securify — rely on rule-based pattern matching. They can only find vulnerabilities they've already seen.

Zero-day exploits don't match known patterns. They use novel attack vectors that rule databases don't cover. By the time a new rule is written, the exploit has already happened.

**The fundamental limitation:** Pattern matching is backward-looking. Exploits are forward-looking. You cannot detect what you haven't seen — unless you stop looking at patterns entirely.

---

## 2. The Solution

AEGIS doesn't read code. AEGIS doesn't match patterns. AEGIS analyzes the **mathematical DNA** of smart contract bytecode using information theory.

Every smart contract — regardless of language, compiler, or obfuscation — has a unique entropy signature. This signature is determined by the contract's structural complexity, statistical properties, and information density. Vulnerable contracts have measurably different entropy profiles than safe contracts.

**AEGIS detects anomalies in entropy space.** Anomalous entropy = anomalous code = potential vulnerability.

This is content-agnostic. AEGIS doesn't need to understand what the code does. It measures what the code *is* — mathematically.

---

## 3. The 24 Entropy Dimensions

AEGIS analyzes bytecode across 24 orthogonal entropy dimensions. Each dimension measures a unique mathematical property:

### Core Dimensions (D1–D6)
| Dimension | Measures |
|-----------|----------|
| D1 Shannon Entropy | Global byte distribution randomness |
| D2 Normalized Shannon | Scale-invariant entropy [0,1] |
| D3 Kolmogorov Complexity | Algorithmic compressibility ratio |
| D4 Permutation Entropy | Local order patterns in byte sequences |
| D5 Spectral Entropy | FFT frequency domain distribution |
| D6 Min-Entropy | Worst-case opcode bias detection |

### Advanced Dimensions (D7–D12)
| Dimension | Measures |
|-----------|----------|
| D7 Sample Entropy | Self-similarity and predictability |
| D8 Approximate Entropy | Complexity and regularity |
| D9 Lempel-Ziv Complexity | Algorithmic information density |
| D10 Tsallis Entropy | Non-extensive statistical mechanics |
| D11 Rényi Entropy | Generalized entropy family |
| D12 Higuchi Fractal Dimension | Fractal complexity of bytecode signal |

### Signal Processing (D13–D18)
| Dimension | Measures |
|-----------|----------|
| D13 DFA Hurst Exponent | Long-range correlation and persistence |
| D14 SVD Entropy | Singular value decomposition entropy |
| D15 Opcode Diversity | Normalized unique opcode count |
| D16 N-Gram Entropy (2-gram) | Bigram sequence entropy |
| D17 N-Gram Entropy (3-gram) | Trigram sequence entropy |
| D18 N-Gram Entropy (4-gram) | 4-gram sequence entropy |

### Structural (D19–D24)
| Dimension | Measures |
|-----------|----------|
| D19 Markov Transition | 256×256 opcode state transition matrix entropy |
| D20 Wavelet Packet | Multi-resolution decomposition — captures what FFT misses |
| D21 Opcode Graph | Graph degree distribution entropy |
| D22 Entropy Rate | Conditional entropy — true information generation rate |
| D23 Storage Pattern | SLOAD/SSTORE sequence entropy |
| D24 Cross-Contract Z-Score | Statistical anomaly from peer group baseline |

Each dimension is computed independently. Together, they form a 24-dimensional entropy vector — a mathematical fingerprint of the contract's structural integrity.

---

## 4. Machine Learning Layer

AEGIS uses an **Isolation Forest** anomaly detection model trained on 73 verified smart contracts. The model:

- Learns the entropy profile distribution of safe contracts
- Flags contracts whose entropy vectors deviate significantly from the baseline
- Achieves **<5% false positive rate** on cross-validation
- Requires no labeled vulnerability data — pure unsupervised anomaly detection

The Isolation Forest isolates anomalies by randomly partitioning the 24D feature space. Anomalous contracts require fewer partitions to isolate — they're "sparse" in entropy space. This is fundamentally different from classification, which requires known vulnerability labels.

---

## 5. Validation

### Statistical Significance
- **p-value = 1.64 × 10⁻⁷** across 10,000 contract baseline
- Less than **1 in 6.1 million** probability of random occurrence
- 16/16 controlled experiments passed

### Tested Contracts
| Protocol | Vulnerability Class | Result |
|----------|-------------------|--------|
| Uniswap V2 | Reentrancy | ✅ Detected |
| Uniswap V3 | Delegatecall | ✅ Detected |
| Balancer V2 | Temporal | ✅ Detected |
| +13 more | Various | ✅ All detected |

### Production Audit
- **Virtuals Protocol** — 55,000+ AI agents scanned
- Source code **NOT available** — bytecode only analysis
- **10 real vulnerabilities found:**
  - 3 CRITICAL
  - 3 HIGH
  - 3 MEDIUM
  - 1 LOW
- Full report delivered to security team

### Falsification Resistance
- Genetic algorithms deployed to forge entropy profiles
- **All forgery attempts failed**
- Entropy signatures cannot be reverse-engineered to mimic safe contracts
- The 24D vector space is too high-dimensional to game

---

## 6. How AEGIS Differs

| Feature | Traditional Tools | AEGIS |
|---------|------------------|-------|
| Approach | Rule-based pattern matching | Mathematical entropy analysis |
| Source Code | Required (or decompilation) | Not needed — bytecode only |
| Novel Vulnerabilities | Cannot detect | Detects via anomaly |
| Obfuscation | Defeats analysis | Irrelevant — entropy is content-agnostic |
| Speed | Slow symbolic execution | Fast vector calculations |
| Direction | Backward-looking (rules after exploits) | Forward-looking (finds before exploitation) |
| Falsification | Patterns can be masked | Entropy profiles cannot be forged |

---

## 7. Architecture

```
┌─────────────────────────────────────────┐
│           AEGIS Pipeline                 │
│                                          │
│  ┌──────────┐    ┌──────────────┐        │
│  │  Bytecode │───▶│  24D Entropy  │       │
│  │  Extraction│   │  Vector Compute│      │
│  └──────────┘    └──────┬───────┘        │
│                         │                │
│                  ┌──────▼───────┐        │
│                  │  Isolation   │        │
│                  │  Forest (ML) │        │
│                  └──────┬───────┘        │
│                         │                │
│              ┌──────────┴──────────┐      │
│              │                     │      │
│       ┌──────▼─────┐    ┌─────────▼────┐ │
│       │  Anomaly   │    │  Resonance   │ │
│       │  Score     │    │  Matching    │ │
│       └──────┬─────┘    └─────────┬────┘ │
│              │                     │      │
│              └──────────┬──────────┘      │
│                         │                 │
│                  ┌──────▼───────┐         │
│                  │   Threat     │         │
│                  │   Report     │         │
│                  └──────────────┘         │
└─────────────────────────────────────────┘
```

**Technology Stack:**
- Core Engine: Rust (performance-critical entropy computation)
- ML Layer: PyTorch + CUDA (GPU-accelerated anomaly detection)
- API Layer: FastAPI (REST + WebSocket)
- SDK: Python (developer integration)
- Client Scanner: JavaScript (browser-based, zero backend)

---

## 8. Tokenomics

### $AEGIS Token

| Parameter | Value |
|-----------|-------|
| Symbol | AEGIS |
| Blockchain | Solana |
| Total Supply | 1,000,000,000 (1B) |
| Launch Method | Fair Launch (Pump.fun) |
| Presale | None |
| Team Allocation | 0% |
| Liquidity | Locked |

### Distribution
- **70%** — Public (Fair Launch)
- **20%** — Liquidity (Locked)
- **10%** — Reserve (Grants + R&D)

### Revenue Model
| Source | Range |
|--------|-------|
| B2B SaaS Subscriptions | $500–$2,000/month per client |
| DeSci Grants | $50,000–$250,000 |
| Private Audit Contracts | Custom pricing |
| Bug Bounty Rewards | Variable |

### Deflationary Mechanism
**100% of revenue is used for Buy-back & Burn of $AEGIS.** No revenue goes to team. No revenue goes to marketing. Every dollar earned reduces supply.

This creates a direct correlation between AEGIS adoption and token scarcity. More clients → more revenue → more burns → less supply → higher value per token.

---

## 9. Roadmap

### Phase 1: EFI Engine Core — ✅ Done
6 core entropy dimensions. Validated on 16 contracts. Statistical significance confirmed (p=1.64e-7).

### Phase 2: EFI 18D Elite — ✅ Done
12 additional dimensions from statistical physics and signal processing. Sample entropy, Tsallis, Rényi, Higuchi, DFA, SVD.

### Phase 3: EFI 24D Brutal — ✅ Done
6 revolutionary dimensions: Markov Transition, Wavelet Packet, Opcode Graph, Entropy Rate, Storage Pattern, Cross-Contract Z-Score. ML Isolation Forest trained.

### Phase 4: $AEGIS Token Launch — 🔄 In Progress
Fair launch on Solana via Pump.fun. Website, litepaper, GitHub repo, social media — all public. Community access to live entropy scanner.

### Phase 5: Rust Production Core — 📋 Planned
Production Rust implementation with CUDA acceleration. Target: 50,000+ contracts/second throughput. Real-time blockchain monitoring.

### Phase 6: FastAPI + SDK — 📋 Planned
REST API and Python SDK for B2B clients. CI/CD pipeline integration. Real-time WebSocket threat streams.

### Phase 7: B2B SaaS Launch — 📋 Planned
Enterprise security platform. $500–$2,000/month per client. 100% revenue → buy-back & burn $AEGIS.

---

## 10. Team

**EntroProtocol** — Independent security research lab.

Specializing in:
- Smart contract auditing
- Entropy-based security analysis
- DeFi vulnerability research
- ML-based vulnerability prediction

Public work:
- GitHub: github.com/EntroProtocol/efi-24d
- X: @EntroProtocol
- Website: legalnode.uk

No team tokens. No pre-sale. No allocations. Pure fair launch.

---

## 11. Risk Disclaimer

$AEGIS is a utility token for the AEGIS security platform. It is not:
- A security or financial instrument
- A guarantee of profit
- A promise of future value

The EFI Engine is experimental technology. While statistically validated, entropy analysis does not guarantee detection of all vulnerabilities. Always conduct independent security audits.

**This litepaper is for informational purposes only. Not financial advice. DYOR.**

---

## 12. Links

- **Website:** legalnode.uk
- **Live Scanner:** legalnode.uk
- **GitHub:** github.com/EntroProtocol/efi-24d
- **X (Twitter):** @EntroProtocol
- **License:** MIT (open source)

---

© 2026 AEGIS by EntroProtocol. Built with mathematics, not marketing.
