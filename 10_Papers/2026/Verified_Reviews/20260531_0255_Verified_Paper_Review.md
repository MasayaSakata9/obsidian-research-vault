---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-31_01-40-19.md
sha256: e26b75fc66941e877a6330247c30abb0be76ba807be43dca2179f50978172e7c
---

## Verification Verdict
- **Verdict:** VERIFIED_WITH_CORRECTIONS
- **Source-read level:** full text excerpt (153,267 chars) + 4 targeted snippets
- **Main reason:** Summary is factually accurate against the provided source context. Minor corrections needed on code availability confirmation and one nuanced clarification about the "fewer than 5000" claim.

## Corrections
| Reader claim | Status | Correction / note | Evidence |
|---|---|---|---|
| Code publicly available — 要確認 | CORRECT to flag | The full excerpt contains no GitHub URL, supplementary code statement, or data release mention. Reader correctly marked this unconfirmed. | No code reference in any excerpt section |
| Training data < 5000 examples sufficient | MINOR IMPRECISE | Abstract says "fewer than 5000 examples already suffices" — this is the total dataset size, not necessarily the training split. Section 2.1 says ~1000 per scenario × 5 scenarios ≈ 5000 total. The reader should distinguish "total dataset" from "training set". | Abstract + Section 2.1 |
| FNO-L2 variant results — 要確認 | CORRECT to flag | Section 3.1 mentions FNO-L2 in training dynamics discussion but provides no separate quantitative table entry. Reader correctly flagged this. | targeted_snippets[baselines] |
| Physics-informed loss equation details — 要確認 | CORRECT to flag | Section 2.3 is described in the excerpt as introducing the loss function, but the actual mathematical formula is cut off or not in the excerpt. | Section 2.3 mentioned but formula absent in excerpt |
| FNO architecture depth/channel count — 要確認 | CORRECT to flag | Not present in the excerpt. | Not in excerpt |
| 67× speedup, 3.9% error, 6.1% H error, 10.2% E error | VERIFIED | All match abstract and Table 1 exactly. | Abstract + Table 1 in targeted_snippets[baselines] |
| fdtdx, grid (128,128,64), 50nm, periodic/PML BC | VERIFIED | All confirmed in Section 2.2.2. | targeted_snippets[code] |
| Five structural scenarios | VERIFIED | Listed in Section 2.1. | Section 2.1 in excerpt |

## Verifier 1: Factual Check
- **Metadata:** ✓ All correct — title, authors (4), arXiv ID 2512.15694, date 2025-12-17, venue physics.optics. DOI correctly marked empty.
- **Method:** ✓ FNO + FNO-L2 with physics-informed Maxwell residuals; FDTD baseline via fdtdx. All confirmed.
- **Inputs / outputs:** ✓ Input: 3D εᵣ distribution. Output: full 3D E and H fields. Correct.
- **Baselines:** ✓ 3D-WaveY-Net (CNN-based), FNO-SD (single-domain). Table 1 values all verified.
- **Metrics:** ✓ e_diffrr, e_L1(H_d), generalization, resolution independence, TFLOPS. All match Table 1.
- **Quantitative results:** ✓ 3.9% diffraction error, 6.1% magnetic, 10.2% electric, 67× speedup — all verified against abstract + Table 1.
- **Code / data availability:** ✓ Reader correctly marks this as unconfirmed — no code reference found in excerpt.

## Verifier 2: Research-Fit Check
- **Relevance to current research:** The reader's assessment of **partial fit** is accurate and appropriately cautious. The paper validates FNO for EM surrogate modeling in general, but the domain gap (optical metasurfaces → 2.4 GHz indoor WiFi) is real and substantial.
- **What is directly usable:**
  - Architecture choice justification (FNO > CNN for EM problems)
  - Physics-informed loss concept (Maxwell residuals in training)
  - Generalization insight (multi-domain training is critical)
  - Quantitative performance targets (sub-5% error, 67× speedup)
- **What is only background / not directly transferable:**
  - Specific FNO hyperparameters (depth, channels, Fourier modes) — optimized for 3D optical scale
  - Boundary condition handling (periodic ≠ room walls)
  - Material model (lossless dielectrics ≠ conductive walls/furniture)
  - Source modeling (fixed plane wave ≠ arbitrary source positions)
- **Risks if used in thesis:**
  1. **Overclaiming transferability:** Saying "this paper proves FNO works for WiFi" would be wrong. It proves FNO works for 3D optical EM — the WiFi extension is a hypothesis.
  2. **Ignoring σ (conductivity):** The paper's physics-informed loss does not include conductivity terms. WiFi environments need σ ≠ 0.
  3. **Boundary condition mismatch:** Periodic BCs in the paper vs. absorbing/reflective walls in WiFi.
  4. **Scale difference:** 3–4 orders of magnitude in wavelength means different discretization challenges.
  5. The reader's "failure modes" section is well-reasoned but somewhat speculative — these are the reader's own projections, not findings from the paper itself. This should be clearly marked as conjecture.

## Final Verified Summary
- **Title:** Physics-informed Neural Operators for Predicting 3D Electromagnetic Fields Transformed by Metasurfaces
- **One-paragraph summary:** Furat et al. (2025) demonstrate that Fourier Neural Operators (FNO) can serve as fast, accurate 3D surrogate models for predicting electromagnetic fields transformed by metasurfaces. Trained on ~5000 synthetic examples generated via stochastic geometry and simulated with FDTD (fdtdx), the physics-informed FNO incorporates Maxwell residuals into the loss function. It achieves 3.9% relative error in diffraction efficiency, 6.1% error in magnetic field, and 10.2% in electric field — outperforming a 3D CNN baseline (3D-WaveY-Net) across all metrics. The model provides 67× speedup over FDTD, generalizes to unseen geometries, and supports resolution-independent inference.
- **Usefulness for the current research:** **Medium priority.** Validates the core hypothesis that neural operators can surrogate Maxwell solvers with high fidelity. The physics-informed loss design (Maxwell residuals) is conceptually transferable to 2D TEz WiFi problems. However, the domain mismatch (optical vs. radio, lossless vs. conductive, periodic vs. room boundaries) means specific architectures and hyperparameters cannot be copied directly. Main value: architecture validation, loss design inspiration, and performance benchmark targets.
- **Implementation implication:** A 2D FNO with physics-informed Maxwell residuals is a viable approach for WiFi surrogate modeling. Key adaptations needed: (1) add conductivity σ as input channel, (2) include source position as input, (3) reformulate physics loss for 2D TEz with conductive media, (4) use absorbing/reflective BCs instead of periodic. Minimal experiment: train on ~2000 WiFi samples comparing data-only vs. physics-informed loss.
- **Remaining 要確認 items:**
  1. Exact physics-informed loss equation (Section 2.3 formula not in excerpt)
  2. Code availability (not found in excerpt)
  3. FNO-L2 quantitative results (mentioned but no numbers)
  4. FNO architecture hyperparameters (depth, channels, Fourier modes)
  5. Journal submission status (still arXiv-only as of metadata)
