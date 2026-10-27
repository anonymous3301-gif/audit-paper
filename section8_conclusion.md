# 8 Conclusion

A gate is worth its complexity only conditionally. On a 16-cell capacity/load grid, Kaczmarz-based delta-rule memory improves over L2-const at low-to-moderate load ($0.125 \lesssim n/d \lesssim 1$), but the advantage vanishes at high load ($n/d \geq 8$). The gate's value is budget-dependent: it is worth $+0.0729$ at 1500 steps but $+0.0030$ at 3000 steps. Optimizing $\beta$ in the L2 family does not close the gap to Kaczmarz; key normalization is necessary. The gate's shape co-adapts and determines what transfers, but the co-adaptation is a 1500-step phenomenon. The collapse mechanism in GDN remains open; a scalar retention factor produces a collapse or a mixed outcome, but not GDN's three-mode distribution.

The result is a conditional, quantitative audit, not a claim that Kaczmarz is universally superior.
