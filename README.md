# Where Does Kaczmarz Win? A Capacity-Grid Audit of Key and Step Normalization

**Author:** KPL Research (pseudonym; the final version may use a real name)  
**Version:** v0.1-draft, 2026-10-27  
**Status:** **Draft preprint for priority claim. The final version will be updated.**  
**DOI:** pending `v0.1-draft` release (badge will be added after Zenodo issues the DOI).

This repository contains the Markdown manuscript, figures, code, and key result
files for the paper. It is a draft: the text is readable and the claims are
supported by the included artifacts, but it is not a final journal submission.

## Abstract

Delta-rule memory powers linear-time sequence models, and its update rule mixes
three operations -- key normalization, erase strength $\beta$, and norm-based
gating -- that are usually introduced together, so each one's value is argued from
isolated ablations rather than from a controlled grid. We audit them on a
capacity/load grid: 16 cells ($d_{\mathrm{mem}} \in \{16,32,64,128\} \times
n_{\mathrm{pairs}} \in \{16,32,64,128\}$), two arms (Kaczmarz vs. L2-const
$\beta=0.125$), 10 seeds per base cell, 64 seeds for the non-inferiority cells,
1184 runs total. Against L2-const, Kaczmarz is non-inferior and more robust to
capacity and load: C1$'$-NI and C2$'$ reject at $n_{\mathrm{pairs}}=32$ and $64$
(Holm $m=4$). The advantage is budget-dependent: Kaczmarz ignites near 1100
steps at $V=300$, $n=16$, while L2 matches it at 3000 steps (H3-sub;
budget $\times$ arm interaction $p=0.0155$). A $\beta$-sweep
($\beta \in \{0.1,0.3,0.5,0.7,0.9,1.0\}$, $(d_{\mathrm{mem}}=128,
n_{\mathrm{pairs}}=16)$) shows that optimizing $\beta$ in the L2 family does
not close the gap: the best fixed $\beta=0.3$ reaches $0.826$ against Kaczmarz's
$0.982$ (paired $p=0.0270$), and $\beta=1.0$ gives $0.454$. $\tilde\mu$ alone
does not explain performance; key normalization is necessary. The win region is
bounded: Kaczmarz retains $\geq 90\%$ of its capacity at $n/d \leq 1$ (F1
fails at $n/d=8$: ratios $0.361$ and $0.232$), and $24.7\%$ of runs collapse
($k=5$). H2 order/resource effects are not detected ($n=5$, Holm $m=2$
non-significant), so the claim rests on the E1.1 grid, not on path dependence.
The result is a conditional, quantitative audit, not a claim that Kaczmarz is
universally superior.

## Repository contents

| Path | Content |
|---|---|
| `paper.md` | Single-file manuscript (abstract + Sections 1-8) |
| `abstract.md` | Title, author, abstract, draft status |
| `sections/` | Per-section Markdown sources |
| `figures/` | Figure 1-7 PDFs |
| `code/` | Scripts used for the ladder, controls, statistics, and figures |
| `data/` | Key result JSON/Markdown files |
| `PUBLICATION_NOTES.md` | Draft status, known limitations, retraction scope, figure notes |
| `LICENSE` | CC-BY-4.0 for the text and figures |
| `LICENSE-CODE` | MIT for the code |

## Code map

The main entry points are:

- `code/budget_match.py` -- budget-matched four-stage ladder
- `code/conv_ladder.py` -- convolution control
- `code/clip_ablation.py` -- clip-protocol control
- `code/init_eig_diag.py` -- initialization diagnostic
- `code/cross_eval_matched.py`, `cross_eval_d24.py`, `cross_eval_d32.py` -- post-hoc transfer
- `code/tost_matched_mean.py` -- TOST for the matched-mean comparison
- `code/delta_decay_only.py`, `delta_decay_only_v3.py` -- decay-only control
- `code/verify_gdn_stats.py` -- GDN vs Delta statistics
- `code/fig1_trajectory.py` ... `code/fig7_threshold_sweep.py` -- figures
- `code/section6_analysis.py`, `code/seed_stats.py`, `code/npairs_sweep.py`, `code/harmonic.py`, `code/beta_power.py`, `code/rawk_log.py` -- supporting analyses

## Draft status and limitations

- This is **v0.1-draft**. The final version will be updated.
- The GDN collapse mechanism remains open; the paper does not claim to identify
  it.
- The published comparison is task-variant dependent; the paper does not assert
  a single published ordering.
- Figure 7 panel (b) is a step function; the plotted threshold grid is discrete,
  so some intermediate failure counts may be skipped visually. The caption says
  so explicitly.
- The paper is distributed as Markdown (`paper.md`) plus figure PDFs. No
  compiled PDF is included in v0.1-draft.

## License

Text and figures: CC-BY-4.0. Code: MIT. See `LICENSE` and `LICENSE-CODE`.
