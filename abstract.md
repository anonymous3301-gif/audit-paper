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
