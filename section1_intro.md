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
