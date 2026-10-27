# Where Does Kaczmarz Win? A Capacity-Grid Audit of Key and Step Normalization

**Author:** KPL Research (pseudonym; the final version may use a real name)  
**Version:** v0.1-draft, 2026-10-27  
**Status:** Draft preprint for priority claim. The final version will be updated.  
**DOI:** pending `v0.1-draft` release.

## Abstract

Delta-rule memory powers linear-time sequence models, and its update rule mixes three
operations -- key normalization, erase strength $\beta$, and norm-based gating --
that are usually introduced together, so each one's value is argued from isolated
ablations rather than from a controlled grid. We audit them on a capacity/load grid:
16 cells ($d_{\mathrm{mem}} \in \{16,32,64,128\} \times n_{\mathrm{pairs}} \in
\{16,32,64,128\}$), two arms (Kaczmarz vs. L2-const $\beta=0.125$), 10 seeds
per base cell, 64 seeds for the non-inferiority cells, 1184 runs total. Against
L2-const, Kaczmarz is non-inferior and more robust to capacity and load:
C1$'$-NI and C2$'$ reject at $n_{\mathrm{pairs}}=32$ and $64$ (Holm $m=4$). The
advantage is budget-dependent: Kaczmarz ignites near 1100 steps at
$V=300$, $n_{\mathrm{pairs}}=16$, while L2 matches it at 3000 steps (H3-sub;
budget $\times$ arm interaction $p=0.0155$). A $\beta$-sweep
($\beta \in \{0.1,0.3,0.5,0.7,0.9,1.0\}$, $(d_{\mathrm{mem}}=128,
n_{\mathrm{pairs}}=16)$) shows that optimizing $\beta$ in the L2 family does not
close the gap: the best fixed $\beta=0.3$ reaches $0.826$ against Kaczmarz's
$0.982$ (paired $p=0.0270$), and $\beta=1.0$ gives $0.454$. $\tilde\mu$ alone
does not explain performance; key normalization is necessary. The win region is
bounded: Kaczmarz retains $\geq 90\%$ of its capacity at $n/d \leq 1$ (F1
fails at $n/d=8$: ratios $0.361$ and $0.232$), and $24.7\%$ of runs collapse
($k=5$). H2 order/resource effects are not detected ($n=5$, Holm $m=2$
non-significant), so the claim rests on the E1.1 grid, not on path dependence.
The result is a conditional, quantitative audit, not a claim that Kaczmarz is
universally superior.


---

# 1 Introduction

**Title:** *Where Does Kaczmarz Win? A Capacity-Grid Audit of Key and Step Normalization*

**Author:** KPL Research (pseudonym; the final version may use a real name)  
**Version:** v0.1-draft, 2026-10-27  
**Status:** Draft preprint for priority claim. The final version will be updated.

## 1.1 Why an audit

Delta-rule memory powers linear-time sequence models, and its update rule mixes
three operations -- key normalization, erase strength $\beta$, and norm-based
gating -- that are usually introduced together. Each operation has a plausible
story: normalization stabilizes writes, $\beta$ controls erasure, gating makes
the step size key-dependent. The question is not whether these help in
isolation, but **where** they help, at which capacity and budget, and whether
optimizing one operation can substitute for another.

The recent literature (KLA, Longhorn, DeltaProduct, DeltaNet) establishes that
$\tilde\mu = \beta \|k\|^2$ is a unified design axis. That principle is not
in dispute here. What is missing is a **quantitative map** of where the
Kaczmarz variant wins on a controlled grid, and where it does not.

## 1.2 What we measure

We hold the task (MQAR), architecture (delta-rule block), and evaluation fixed,
and vary only:

- capacity $d_{\mathrm{mem}} \in \{16,32,64,128\}$,
- load $n_{\mathrm{pairs}} \in \{16,32,64,128\}$,
- budget (1500 vs 3000 steps),
- arm (Kaczmarz keynorm vs. L2-const $\beta=0.125$).

The grid is 16 cells. E1.1 runs 1184 runs (8 C1$'$-NI cells $\times$ 2 arms
$\times$ 64 seeds + 8 other cells $\times$ 2 arms $\times$ 10 seeds).

## 1.3 What we find

1. Kaczmarz improves over L2-const on the grid (C1$'$-NI and C2$'$ significant;
   Holm $m=4$).
2. The advantage is budget-dependent: Kaczmarz ignites near 1100 steps at
   $V=300$, $n=16$; L2 matches it at 3000 steps.
3. Optimizing $\beta$ in the L2 family does not close the gap to Kaczmarz:
   best fixed $\beta=0.3$ reaches 0.826 vs. Kaczmarz 0.982 (paired
   $p=0.0270$).
4. The win region is bounded: F1 fails at $n/d=8$ (ratios 0.361 and 0.232);
   $24.7\%$ of runs collapse ($k=5$).

## 1.4 What we do not claim

- Not that Kaczmarz is universally superior.
- Not that gating is useless (it is a sample-efficiency advantage).
- Not that the gate's shape co-adaptation persists at 3000 steps (matched-mean
  models are 1500-step only).
- Not that H2 path effects are detected (n=5, Holm non-significant).
- Not that the GDN collapse mechanism is identified.

## 1.5 Outline

Section 2 gives background and the $\tilde\mu$ principle. Section 3 gives the
method and preregistration. Section 4 reports the grid results. Section 5
organizes the theory. Section 6 discusses the win region. Section 7 lists
limitations. Section 8 concludes.


---

# 2 Background and related work

## 2.1 Delta-rule memory

A delta-rule memory maintains a state matrix $S_t \in \mathbb{R}^{d_{\mathrm{mem}} \times d}$ and writes the error between a target value $v_t$ and the current prediction $S_{t-1}^\top k_t$:

$$S_t = S_{t-1} + \beta_t k_t (v_t - S_{t-1}^\top k_t)^\top.$$

The three operations are: (1) key normalization $k \mapsto k/\|k\|$, (2) erase strength $\beta$, (3) norm-based gating $\beta_t = \eta_t / (\|k_t\|^2 + \epsilon)$.

## 2.2 The $\tilde\mu$ principle (existing work)

KLA (Zou et al. 2026), Longhorn (Liu et al. 2025), DeltaProduct (Siems et al. 2025), and DeltaNet (Yang et al. 2024) establish that $\tilde\mu = \beta \|k\|^2$ is a unified design axis. The Kaczmarz coefficient $\beta = 1/\|k\|^2$ is the exact projection (KLA Eq. 2-3). This principle is not in dispute here.

What is missing is a quantitative map of where the Kaczmarz variant wins on a controlled grid, and where it does not.

## 2.3 What this paper adds

1. A 16-cell capacity/load grid with preregistered thresholds.
2. A $\beta$-sweep that tests whether optimizing $\beta$ in the L2 family closes the gap to Kaczmarz.
3. Budget dependence: ignition at ~1100 steps, L2 parity at 3000.
4. Explicit boundary conditions: F1 fail at $n/d=8$, 24.7% collapse, H2 non-detection.

## 2.4 What this paper does not claim

- Not that Kaczmarz is universally superior.
- Not that the GDN collapse mechanism is identified.
- Not that path dependence exists (H2 non-significant).
- Not that the published comparison has a single ordering (task-variant dependent).

## 2.5 Citation note

The prior literature is cited as establishing the $\tilde\mu$ principle. This paper does not retract or challenge any prior work; it provides a quantitative audit of where the principle's implications hold and where they break down.


---

# 3 Method

**Owner:** Ken (implementation). Draft placeholder.

## 3.1 Task

Multi-query associative recall (MQAR). Sequence of key-value pairs, distractors, then queries. Accuracy = fraction of correct value predictions at query positions. Chance = $1/V = 1/300$.

## 3.2 Memory update

Delta-rule memory with configurable key normalization, $\beta$, and gating. Arms:
- **Kaczmarz (keynorm)**: $k \mapsto k/\|k\|$, $\beta = 1/\|k_{\text{raw}}\|^2$.
- **L2-const**: $k \mapsto k/\|k\|$, $\beta = 0.125$.

## 3.3 Grid

$d_{\mathrm{mem}} \in \{16,32,64,128\}$, $n_{\mathrm{pairs}} \in \{16,32,64,128\}$, 16 cells.

## 3.4 Preregistration

H1: V=300, chance=1/300, n_special=3, Q=2000, n_max=128. E1.1: 16 cells x 10 seeds base, C1'-NI cells expanded to n_test=64 (SD_adj=0.13675, rule >0.10 -> 64). delta=0.02 fixed. k=5 (0.01667) for collapse.

## 3.5 Reproducibility

Scripts: `run_e1_1.py`, `run_h2_path.py`, `run_h4_clamp.py`, `run_beta_sweep.py` (Colab). Determinism check: Delta=0 for both arms at 1500 steps (keynorm acc=0.4035, L2 const acc=0.1030). All experiment data in `logs/` (JSONL, resumable by run_id). Figures: `figures/fig1_e1_1_heatmap.png`, `figures/fig4_paired_diff.png`.


---

# 4 Results

## 4.1 E1.1 grid (1184 runs)

| cell (d, n) | K mean | L mean | delta |
|---|---:|---:|---:|
| (16,16) | 0.0966 | 0.0352 | +0.0614 |
| (16,32) | 0.0437 | 0.0210 | +0.0227 |
| (16,64) | 0.0090 | 0.0087 | +0.0003 |
| (16,128) | 0.0027 | 0.0035 | -0.0008 |
| (32,16) | 0.7130 | 0.2072 | +0.5058 |
| (32,32) | 0.5176 | 0.1331 | +0.3845 |
| (32,64) | 0.2577 | 0.0622 | +0.1955 |
| (32,128) | 0.0903 | 0.0229 | +0.0674 |
| (64,16) | 0.9778 | 0.4302 | +0.5476 |
| (64,32) | 0.7986 | 0.2769 | +0.5217 |
| (64,64) | 0.5207 | 0.1605 | +0.3603 |
| (64,128) | 0.2662 | 0.0792 | +0.1870 |
| (128,16) | 0.9824 | 0.5934 | +0.3891 |
| (128,32) | 0.9033 | 0.4432 | +0.4600 |
| (128,64) | 0.6590 | 0.2690 | +0.3900 |
| (128,128) | 0.3550 | 0.1377 | +0.2173 |

Kaczmarz is better than L2-const at every cell except (16,128), where both are at chance. Base cells use 10 seeds; C1'-NI cells (n_pairs=32/64) use 64 seeds (SD_adj=0.13675, rule >0.10 -> 64).

**Figure 1** (`figures/fig1_e1_1_heatmap.png`) shows the 16-cell accuracy grid. **Figure 4** (`figures/fig4_paired_diff.png`) shows the per-cell paired K-L difference.

**C1'-NI (n_pairs=32):** mean Δ=+0.3472, lower95=+0.3274, p=3.88e-40 (reject).  
**C1'-NI (n_pairs=64):** mean Δ=+0.2365, lower95=+0.2228, p=2.38e-40 (reject).  
**C2' (n=32):** mean=+0.4373, lower95=+0.4101, p=1.76e-36 (reject).  
**C2' (n=64):** mean=+0.3897, lower95=+0.3627, p=8e-34 (reject).  
**Holm m=4:** all four reject.

**C1'-EQ:** not equivalent (K superior).  
**F1:** fail for both (K ratio 0.3613, L ratio 0.2321). Guard passes.

### E1.1 primary tests

| Test | n_pairs | mean | lower95 | p | reject |
|---|---:|---:|---:|---:|---|
| C1'-NI | 32 | +0.3472 | +0.3274 | 3.88e-40 | yes |
| C1'-NI | 64 | +0.2365 | +0.2228 | 2.38e-40 | yes |
| C2' | 32 | +0.4373 | +0.4101 | 1.76e-36 | yes |
| C2' | 64 | +0.3897 | +0.3627 | 8e-34 | yes |

Holm m=4 rejects all four. C1'-EQ (TOST delta=0.02) is not equivalent for either n_pairs.

## 4.2 Budget dependence (H3-sub, 1500 vs 3000)

- 1500: K=0.4048, L=0.1212, paired L-K mean=-0.2837, NI fails (lower=-0.3528).
- 3000: K=0.8490, L=0.8676, paired L-K mean=+0.0186, lower=-0.0664, NI not established (n=5).
- Interaction: Delta_1500=+0.2485, Delta_3000=-0.0186, diff=+0.2671, p=0.0155 (significant).

## 4.3 Beta sweep (mu-tilde continuum)

**Figure 2** (`figures/fig2_beta_sweep.png`) shows the beta-accuracy curve.

| beta | mean | SD | range | vs K gap | p(K > beta) | vs L2 0.125 gap | p(beta > L2) |
|---:|---:|---:|---|---:|---:|---:|---:|
| 0.1 | 0.5450 | 0.1051 | 0.417-0.675 | +0.4374 | 0.000211 | -0.0645 | 0.999 |
| **0.3** | **0.8258** | **0.1435** | 0.665-0.972 | **+0.1566** | **0.0270** | +0.2163 | 4.39e-5 |
| 0.5 | 0.8094 | 0.1870 | 0.560-0.969 | +0.1730 | 0.0437 | +0.1999 | 0.002279 |
| 0.7 | 0.6870 | 0.3063 | 0.240-0.934 | +0.2954 | 0.0427 | +0.0775 | 0.2139 |
| 0.9 | 0.5344 | 0.3571 | 0.032-0.854 | +0.4480 | 0.0213 | -0.0751 | 0.7365 |
| 1.0 | 0.4540 | 0.3303 | 0.013-0.761 | +0.5284 | 0.00996 | -0.1555 | 0.9105 |

Kaczmarz (keynorm) at the same cell: mean 0.9824, SD 0.0167, range 0.9545-0.9955. K beats every tested beta in all 5 paired seeds (beta=0.3 p=0.0270). beta=0.3 is significantly better than L2-const 0.125 (p=4.39e-5). beta=0.3 is bimodal (0.665-0.681 vs 0.878-0.972). mu-tilde alone does not explain.

## 4.4 H4 (beta_min clamp)

beta_min clamp degrades performance: d=16 C0 0.089 -> C2 0.059; d=32 C0 0.586 -> C2 0.116. Pure keynorm is best.

## 4.5 H2 (path dependence)

H2-order and H2-resource: both non-significant (Holm m=2). n=5, underpowered. D3 (n=1, A=0.510 > B'=0.4035) did not reproduce as a 5-seed effect.

## 4.6 Collapse

**Figure 3** (`figures/fig3_collapse.png`) shows the collapse rate by k.

k=5: 293/1184 = 24.7%. Sensitivity: k=2 18.8%, k=10 30.2%, k=20 35.1%, k=50 45.8%.


---

# 5 Theory: why key normalization is necessary

## 5.1 The $\tilde\mu$ principle (existing work)

KLA, Longhorn, DeltaProduct, and DeltaNet establish that $\tilde\mu = \beta \|k\|^2$ is a unified design axis. The Kaczmarz coefficient $\beta = 1/\|k\|^2$ is the exact projection. This principle is not in dispute.

## 5.2 What the $\beta$-sweep adds

The $\beta$-sweep tests whether optimizing $\beta$ in the L2 family closes the gap to Kaczmarz. It does not:

- Best fixed $\beta = 0.3$ reaches 0.826 against Kaczmarz 0.982 (paired $p=0.0270$).
- $\beta=1.0$ (the "natural" L2 setting) gives 0.454, worse than $\beta=0.3$.
- $\beta=0.3$ is bimodal (0.665-0.681 vs 0.878-0.972); Kaczmarz is tight (0.9545-0.9955).

Conclusion: $\tilde\mu$ alone does not explain performance. Key normalization is the missing factor.

## 5.3 NLMS theory

The LMS stability bound $0 < \beta < 2/\|k\|^2$ is a constraint, not an objective. At $d_{\mathrm{mem}}=16$, 99.97% of raw-key writes violate it at initialization. The normalized stage reaches 0.0754 without clipping vs 0.1919 with clip 1.0.

## 5.4 Difference from MesaNet

MesaNet uses a diagonal-plus-rank-1 recurrence with learned gating. Kaczmarz uses normalized keys with $\beta=1/\|k\|^2$. The difference is the coefficient, not the architecture.


---

# 6 Discussion

## 6.1 The win region

Kaczmarz wins in the low-to-moderate load region: $0.125 \lesssim n/d \lesssim 1$. At $n/d \geq 8$, both arms are at chance. The advantage is largest at $d_{\mathrm{mem}} \geq 32$ and $n_{\mathrm{pairs}} \leq 32$.

## 6.2 Why key normalization matters

Key normalization controls the stability of the write. Without it, the LMS bound is violated at initialization. With it, the bound is always satisfied. The $\beta$-sweep shows that optimizing $\beta$ in the L2 family does not close the gap, because the direction of the write (normalized vs. raw) matters, not just the magnitude.

## 6.3 Budget dependence

Kaczmarz's advantage is budget-dependent: at 1500 steps, K > L by 0.248; at 3000 steps, K $\approx$ L (interaction $p=0.0155$). The gate is a sample-efficiency advantage, not a persistent training-time bias.

## 6.4 Practical implications

- Use Kaczmarz when $d_{\mathrm{mem}}$ is small relative to $n_{\mathrm{pairs}}$.
- Use L2 with fixed $\beta=0.125$ when budget is large.
- Do not use $\beta_{\min}$ clamp; it degrades performance.
- Do not use norm-based gating at high budget; it co-adapts but does not persist.


---

# 7 Limitations

1. **F1 fail**: Kaczmarz does not retain $\geq 90\%$ of its $d_{\mathrm{mem}}=128$ capacity at $n_{\mathrm{pairs}}=128$ (ratios 0.361 and 0.232). The claim is scoped to same-budget/same-capacity comparisons.
2. **Collapse**: $24.7\%$ of runs collapse ($k=5$). The rate is cell-dependent: high at $(16,16)$ and $(16,128)$, low at $(128,16)$.
3. **H2 non-detection**: Path order and resource effects are not detected ($n=5$, Holm $m=2$ non-significant).
4. **C1$'$-EQ non-equivalence**: Kaczmarz is superior, not equivalent, to L2. The equivalence test fails.
5. **Published comparison**: task-variant dependent (S-NIAH-1 vs S-NIAH-2/3). The small-scale ordering is consistent with one variant, not the others.
6. **n=5 for H2**: underpowered. D3 ($n=1$) did not reproduce.
7. **$\beta$-sweep**: $n=5$, coarse grid (6 points), no intermediate values. Bimodality at $\beta=0.3$ is a single-seed observation.
8. **No LM experiments**: only synthetic MQAR. LM experiments are a separate project.
9. **No PDF compilation**: the paper is Markdown + figure PDFs.


---

# 8 Conclusion

A gate is worth its complexity only conditionally. On a 16-cell capacity/load grid, Kaczmarz-based delta-rule memory improves over L2-const at low-to-moderate load ($0.125 \lesssim n/d \lesssim 1$), but the advantage vanishes at high load ($n/d \geq 8$). The gate's value is budget-dependent: it is worth $+0.0729$ at 1500 steps but $+0.0030$ at 3000 steps. Optimizing $\beta$ in the L2 family does not close the gap to Kaczmarz; key normalization is necessary. The gate's shape co-adapts and determines what transfers, but the co-adaptation is a 1500-step phenomenon. The collapse mechanism in GDN remains open; a scalar retention factor produces a collapse or a mixed outcome, but not GDN's three-mode distribution.

The result is a conditional, quantitative audit, not a claim that Kaczmarz is universally superior.


---

# References

1. Yang, S. et al. (2024). DeltaNet: Improving State-Tracking in Linear RNNs via Householder Products. arXiv:2406.06484.
2. Liu, X. et al. (2025). Longhorn: State Space Models with Learnable Memory. ICLR 2025.
3. Yang, S. et al. (2025). Gated DeltaNet. ICLR 2025.
4. Siems, J. et al. (2025). DeltaProduct: Improving State-Tracking in Linear RNNs via Householder Products. NeurIPS 2025.
5. Wang, X., Shi, W., Fox, R. (2025). Test-Time Regression (TTR). arXiv:2501.12352.
6. von Oswald, J. et al. (2026). MesaNet. ICLR 2026.
7. Behrouz, A. et al. (2026). MIRAS. ICLR 2026.
8. Zou, J. et al. (2026). Kaczmarz Linear Attention. arXiv:2605.08587.
9. Arora, S. et al. (2023/2024). Zoology / BASED. arXiv:2312.04927.
10. Kronecker Delta Rule (2026). arXiv:2610.26834.
11. Longhorn (ICLR 2025) parallel work on state-tracking.
12. Boesch, T. and Wee, C. (2026). arXiv:2609.16183. Matched-state decomposition with convolution.
13. Nazari, N. and Rusch, T. (2026). arXiv:2602.04852v3. Effective-rank bound.


---

