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
