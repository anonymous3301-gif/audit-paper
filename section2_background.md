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
