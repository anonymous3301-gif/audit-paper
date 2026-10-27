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
