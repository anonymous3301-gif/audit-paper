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
