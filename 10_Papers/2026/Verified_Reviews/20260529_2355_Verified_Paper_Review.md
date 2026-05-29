---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-29_22-41-59.md
sha256: 2b39157c38c3d3f8257cd299d31785228de33a1a9d1e58cc1e52d6cecb20eb0b
---

## Verification Verdict
- **Verdict:** VERIFIED_WITH_CORRECTIONS
- **Source-read level:** full text excerpt (325,521 chars + 5 targeted snippets)
- **Main reason:** The summary is structurally sound and the method/architecture claims are accurate, but there is a **significant numerical attribution error**: the reported percentage improvements (−18.39%, −16.86%, −21.62%, −17.26%) are calculated **relative to base FNO**, not relative to NO-LIDK⋄ as the summary states. This is a material misrepresentation of the results.

---

## Corrections

| Reader claim | Status | Correction / note | Evidence |
|---|---|---|---|
| "NO-LIDK⋄に対して fRMSE: 19.2%改善（$1.92\times10^{-1}$ vs $2.04\times10^{-1}$）" | ❌ **Incorrect baseline attribution** | The −18.39% figure in Table 1 is computed **against FNO (2.35)**, not NO-LIDK⋄ (2.04). vs FNO: (1.92−2.35)/2.35 = −18.3%. vs NO-LIDK⋄: (1.92−2.04)/2.04 = −5.9%. The summary conflates "strongest baseline" with "denominator for % calculation." | Table 1 snippet; arithmetic verification |
| "cRMSE: 16.9%改善" | ❌ Same error | −16.86% is vs FNO (3.37→2.80). vs NO-LIDK⋄ (3.71→2.80) = −24.5%. | Table 1 |
| "nRMSE: 21.6%改善" | ❌ Same error | −21.62% is vs FNO (5.80→4.55). vs NO-LIDK⋄ (5.96→4.55) = −23.7%. | Table 1 |
| "MaxError: 17.3%改善" | ❌ Same error | −17.26% is vs FNO (8.37→6.93). vs NO-LIDK⋄ (7.6→6.93) = −8.8%. | Table 1 |
| "パラメータ数: 最大50%削減（ローカルブランチ導入による）" | ✅ Verified | Abstract: "reduces the number of trainable parameters by up to 50%" | Abstract excerpt |
| "FNO単独よりもLOGLO-FNOが有意に上回る（fRMSE: 2.35→1.92、約18%改善）" | ✅ Verified | This statement is internally consistent and correct. | Table 1 |
| "波動方程式（wave propagation）が6課題のうちの1つ" | ⚠️ Partially precise | Abstract says "wave propagation" — the specific PDE (linear/nonlinear wave eq.) is not identified in the excerpt. | Abstract |
| "EM特有の物理制約が組み込まれていない" | ✅ Correct inference | Paper covers fluid mechanics, wave propagation, biological pattern formation — no EM/Maxwell discussion found in excerpt. | Full excerpt scan |
| "3Dベクトル場（Ex,Ey,Ez）処理は検討されていない" | ✅ Correct inference | Paper works with scalar fields; "notations naturally extend to 3D spatial data" but no vector field treatment. | Section 3 |
| "Code URL: 要確認" | ✅ Correct | No repository URL in excerpt; only `github.com/neuraloperator/neuraloperator` cited as a footnote for the library. | Full excerpt |
| "DOI: 要確認" | ✅ Correct | PDF header shows OpenReview link but no TMLR DOI. | PDF header |

---

## Verifier 1: Factual Check

**Metadata:**
- Title, authors, affiliations: ✅ All match the PDF header exactly.
- Year/venue: ✅ TMLR 12/2025, arXiv v2 dated 2026-01-10. Correct.
- arXiv ID: ✅ 2504.04260.

**Method:**
- 3-branch architecture (global / local spectral / HFP): ✅ Verified in Section 3, Eq. 2, Figure 1.
- Local spectral convolution on patches: ✅ Section 3.1, Eq. 3.
- HFP via AvgPool→Interpolate high-pass filter: ✅ Section 3.1, Eq. 4.
- Frequency-aware loss with radially binned spectral errors: ✅ Section 3.2, Eq. 5.
- Parameter reduction up to 50%: ✅ Abstract, Section 3.3 referenced.

**Inputs / outputs:**
- Scalar field inputs/outputs: ✅ Correct. Paper operates on Banach spaces of scalar functions.
- Multi-channel input mapping (eps_r+sigma+source_map): ⚠️ Not directly tested in paper but architecturally feasible via lift layer P. Specialist correctly flags this.

**Baselines:**
- FNO, F-FNO, U-FNO, NO-LIDK (3 variants), U-Net (PDEArena), LSM, Transolver: ✅ All confirmed in "baselines" snippet and Section 4.

**Metrics:**
- fRMSE, cRMSE, nRMSE, MaxError, MELR, WLR: ✅ All confirmed in "Training Objective" snippet.

**Quantitative results:**
- **CRITICAL ERROR:** All four percentage improvements (fRMSE −18.39%, cRMSE −16.86%, nRMSE −21.62%, MaxError −17.26%) are **relative to base FNO**, NOT relative to NO-LIDK⋄. The summary incorrectly attributes them to the "strongest baseline." The actual improvement over NO-LIDK⋄ is: fRMSE −5.9%, cRMSE −24.5%, nRMSE −23.7%, MaxError −8.8%.
- Raw numbers (1.92e-1, 2.80e-2, 4.55e-2, 6.93e-2 for LOGLO-FNO): ✅ Verified against Table 1.
- NO-LIDK⋄ numbers (2.04e-1, 3.71e-2, 5.96e-2, 7.6e-2): ✅ Verified.
- FNO numbers (2.35e-1, 3.37e-2, 5.80e-2, 8.37e-2): ✅ Verified.

**Code / data availability:**
- No repository URL in paper: ✅ Confirmed.
- DOI not in header: ✅ Confirmed.

---

## Verifier 2: Research-Fit Check

**Relevance to current research (Meep/FDTD 2.4GHz indoor WiFi FNO surrogate):**
- **Direct relevance: Low to Moderate.** The paper targets Navier-Stokes, diffusion-reaction, wave propagation, and Gray-Scott — none are Maxwell/Helmholtz equations. The connection to EM is indirect and speculative.
- The specialists correctly characterize this as structurally analogous but not directly applicable.

**What is directly usable:**
- Local spectral convolution branch architecture — the patch-based Fourier approach is transferable to any FNO codebase, including EM applications.
- HFP branch (AvgPool→Interpolate high-pass extraction) — simple to implement, provides inductive bias for high-frequency features.
- Frequency-aware loss design (radially binned spectral error) — conceptually transferable to EM-specific error bands (near-field, far-field, shadow regions).

**What is only background:**
- The specific benchmark results (Kolmogorov Flow, turbulence metrics) — not directly applicable to WiFi prediction.
- MELR/WLR energy spectrum metrics — designed for fluid turbulence, not EM fields.
- 5-step AR rollout stability — relevant conceptually but the dynamics (time-dependent Navier-Stokes vs. steady-state Helmholtz) differ fundamentally.

**Risks if used in thesis:**
1. **Overclaiming transferability:** Citing LOGLO-FNO results as evidence for EM performance would be unjustified. The wave propagation benchmark is not Maxwell equations.
2. **Scalar vs. vector field gap:** The paper handles scalar fields. Indoor WiFi Ez prediction is scalar, so this is manageable, but full 3D vector field (Ex, Ey, Ez) would require unverified extension.
3. **Boundary condition mismatch:** EM problems involve PEC/PMC boundaries, dielectric interfaces, and PML absorbing boundaries — none tested in the paper.
4. **Patch size sensitivity:** The local branch's patch size is critical and task-dependent. The paper's ablation (Appendix G) is not fully visible in the excerpt — this is a real implementation risk.
5. **The percentage error correction matters:** If cited in a thesis, the improvement over NO-LIDK⋄ is modest (5.9% on fRMSE), not the 18% claimed. This changes the narrative about how much LOGLO-FNO improves over SOTA.

---

## Final Verified Summary

**Title:** LOGLO-FNO: Efficient Learning of Local and Global Features in Fourier Neural Operators

**One-paragraph summary:**
Kalimuthu et al. (TMLR 12/2025) propose LOGLO-FNO, a three-branch extension of Fourier Neural Operators that adds (i) a local spectral convolution branch operating on spatial patches to capture fine-grained features, (ii) a High-Frequency Propagation (HFP) branch that extracts high-frequency components via AvgPool→Interpolate high-pass filtering and propagates them through channel MLPs, and (iii) a frequency-aware loss penalizing radially binned mid/high-frequency spectral errors. The local branch reduces trainable parameters by up to 50% while maintaining baseline FNO accuracy. Evaluated on 6 PDE benchmarks (Navier-Stokes Kolmogorov Flow, diffusion-reaction, wave propagation, Gray-Scott, and others), LOGLO-FNO outperforms base FNO by ~18% on fRMSE for 2D Kolmogorov Flow. Against the strongest baseline (NO-LIDK⋄), improvements are more modest (~6% on fRMSE). Long rollout stability is also improved. The paper does not address EM/Maxwell equations or vector fields.

**Usefulness for the current research:**
- **Priority: High (for architectural ideas), Low (for direct results).** The local spectral convolution and HFP branches are directly applicable architectural enhancements for an indoor WiFi FNO surrogate. The frequency-aware loss concept is transferable. However, no EM-specific validation exists — all claims about WiFi relevance require experimental verification.

**Implementation implication:**
- **Cost: Medium.** Three incremental additions to an existing 2D FNO codebase: (a) patch-based local Fourier layer, (b) HFP high-pass branch, (c) radially binned spectral loss. The `neuraloperator` library provides a foundation. Patch size selection and frequency-aware loss weighting (λ) require task-specific tuning.

**Remaining `要確認` items:**
1. **DOI:** TMLR publication DOI not found in PDF header.
2. **Code availability:** No repository URL in paper text.
3. **Exact λ values for frequency-aware loss:** Appendix O.8 referenced but not in excerpt.
4. **Patch size ablation details:** Appendix G referenced but not fully visible.
5. **Wave propagation PDE specifics:** Whether linear or nonlinear wave equation.
6. **HFP computational overhead:** Inference time/memory impact not quantified in excerpt.
7. **5-step AR quantitative results:** Table 1 header mentions 5-step but only 1-step data visible in snippet.
