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
