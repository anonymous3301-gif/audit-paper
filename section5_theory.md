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
