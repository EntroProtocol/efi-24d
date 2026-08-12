# The 24 Entropy Dimensions — Mathematical Details

## Core Dimensions (D1-D6)

### D1: Shannon Entropy
**Formula:** H(X) = -Σ p(x) log₂ p(x)

Measures the average information content of the bytecode byte distribution. Safe contracts have predictable opcode distributions; vulnerable contracts often show anomalous entropy values.

### D2: Normalized Shannon Entropy
**Formula:** H_norm = H(X) / log₂(n)

Shannon entropy normalized to [0,1] range, making it scale-invariant. Allows comparison between contracts of different sizes.

### D3: Kolmogorov Complexity
**Formula:** K(x) ≈ |compress(x)| / |x|

Approximated via DEFLATE compression ratio. Measures algorithmic complexity — how much information is truly irreducible. Vulnerable contracts often have anomalous complexity signatures.

### D4: Permutation Entropy
**Formula:** PE = -Σ p(π) log₂ p(π)

Measures the entropy of ordinal patterns in local byte sequences. Captures local order structure that global entropy misses.

### D5: Spectral Entropy
**Formula:** SE = -Σ |F(k)|² log₂ |F(k)|²

FFT-based frequency domain analysis. Detects periodic patterns in opcode sequences that indicate repetitive or structured code.

### D6: Min-Entropy
**Formula:** H_∞ = -log₂ max(p(x))

Worst-case entropy. Sensitive to the most probable byte value. Detects opcode bias — contracts dominated by a single opcode category.

---

## Advanced Dimensions (D7-D12)

### D7: Sample Entropy
Measures self-similarity in the bytecode signal. Low sample entropy = repetitive patterns = potential vulnerability indicator.

### D8: Approximate Entropy
ApEn quantifies complexity and regularity. More regular bytecodes (lower ApEn) suggest simpler, potentially more auditable contracts — but anomalies can indicate hidden complexity.

### D9: Lempel-Ziv Complexity
Algorithmic information density via LZ76. Counts distinct substrings needed to reconstruct the bytecode. Higher complexity = more diverse operations.

### D10: Tsallis Entropy (q=2)
**Formula:** S_q = (1 - Σ p(x)^q) / (q - 1)

Non-extensive generalization of Shannon entropy. Sensitive to rare events — useful for detecting unusual opcode patterns that Shannon entropy averages out.

### D11: Rényi Entropy (α=2)
**Formula:** H_α = (1/(1-α)) log₂ Σ p(x)^α

Generalized entropy family. α=2 weights more frequent opcodes heavier — complementary to Tsallis which weights rare events.

### D12: Higuchi Fractal Dimension
Estimates fractal dimension of the bytecode signal. Higher fractal dimension = more complex, self-similar structure.

---

## Elite Dimensions (D13-D18)

### D13: DFA Hurst Exponent
Detrended Fluctuation Analysis. H > 0.5 = persistent (trending) patterns. H < 0.5 = anti-persistent. H ≈ 0.5 = random. Vulnerable contracts often show anomalous persistence.

### D14: SVD Entropy
Singular Value Decomposition of the opcode trajectory matrix. Captures multi-dimensional structure in the bytecode signal.

### D15: Opcode Diversity
Normalized count of unique opcodes. Low diversity = limited functionality. Very high diversity = complex interactions that may hide vulnerabilities.

### D16: 2-Gram Entropy
Entropy of opcode bigram (pair) transitions. Captures sequential patterns: PUSH1-PUSH1, CALL-STOP, etc.

### D17: 3-Gram Entropy
Entropy of opcode trigram sequences. Captures longer patterns that 2-grams miss.

### D18: 4-Gram Entropy
Entropy of opcode quadgram sequences. The longest n-gram we compute — captures complex multi-step patterns.

---

## Revolutionary Dimensions (D19-D24) — v4.0 BRUTAL

### D19: Markov Transition Entropy
**Method:** Build 256×256 opcode state transition matrix, compute entropy of transition probability distribution.

This is the most powerful single dimension. It captures the **control flow predictability** of a contract. Safe contracts (like WETH) have low Markov entropy (predictable transitions). Complex protocols (like Uniswap V2 Router) have higher Markov entropy. Anomalies in Markov entropy indicate unusual control flow patterns.

**Key insight:** D19 Entropy Rate distinguishes DAO governance contracts (0.145) from standard ERC20 tokens (0.063) — a 2.3× separation.

### D20: Wavelet Packet Entropy
**Method:** Multi-resolution wavelet decomposition (Daubechies D4), entropy of each frequency band.

Captures structural patterns at multiple scales simultaneously. FFT (D5) only captures global frequency content. Wavelet entropy captures **time-localized** patterns — when and where in the bytecode unusual structures appear.

### D21: Opcode Graph Entropy
**Method:** Build directed graph of opcode transitions, compute degree distribution entropy.

Treats the bytecode as a directed graph where nodes are opcodes and edges are transitions. The entropy of the degree distribution reveals the **topology** of the control flow graph. Hub opcodes (high in-degree) indicate frequently called operations — anomalies suggest potential attack surfaces.

### D22: Entropy Rate
**Formula:** h = H(X_n | X_{n-k:n-1})

The **true information generation rate** — conditional entropy of the next opcode given the previous k opcodes. This is fundamentally different from D1 (Shannon) which ignores order. Low entropy rate = predictable = potentially exploitable. High entropy rate = complex = potentially hiding vulnerabilities.

### D23: Storage Pattern Entropy
**Method:** Extract SLOAD/SSTORE opcode sequences, compute entropy of storage access patterns.

Storage access patterns reveal how a contract manages state. Repetitive storage patterns (low entropy) suggest simple state management. Diverse patterns (high entropy) suggest complex state interactions that may contain race conditions or storage collisions.

### D24: Cross-Contract Z-Score
**Method:** Compute z-score of each dimension against a peer group baseline of 73 contracts.

This dimension enables **comparative analysis**. Rather than evaluating a contract in isolation, EFI 24D compares each dimension against a corpus of known contracts. Contracts that deviate by >2σ in any dimension are flagged. This is the foundation of the ML anomaly detector.

---

## Killed Dimensions (removed in v4.0)

Three dimensions from v3.0 were removed after testing showed zero discrimination:

| Dimension | Mean | Std | Verdict |
|:---------|:-----|:----|:--------|
| Mutual Information (D8 old) | 0.01 | 0.005 | DEAD — no variance |
| Granger Causality (D9 old) | 0.01 | 0.01 | DEAD — no variance |
| Bubble Entropy (D16 old) | 0.99 | 0.005 | DEAD — saturated |

These dimensions were replaced by the revolutionary D19-D24 dimensions.
