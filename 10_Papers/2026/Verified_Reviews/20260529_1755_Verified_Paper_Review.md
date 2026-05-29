---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-29_16-41-21.md
sha256: 565f75cf40811e16062ea704b63299f63fee3e1e166d647efc67058b0bd11cb3
---

I'll verify the reader summary against the provided primary source context (full text excerpt + targeted snippets).

---

## Verification Verdict
- **Verdict:** VERIFIED_WITH_CORRECTIONS
- **Source-read level:** full text excerpt (58,097 chars extracted from PDF)
- **Main reason:** Core method, architecture, quantitative results, and theoretical claims are accurate. Minor corrections needed on UNet input channel description, a misleading framing of "best Conv-FNO variant," an error in the minimal experiment's channel count, and unconfirmed metadata (DOI, venue, code availability for UNet-FNO itself).

---

## Corrections
| Reader claim | Status | Correction / note | Evidence |
|---|---|---|---|
| UNet input channels fixed at 3 | ❌ INCOMPLETE | Input channels are **configurable** (default: 3), not fixed | Sec 5.1: "The input channels are configurable (default: 3)" |
| UNet-FNO is "the best Conv-FNO variant (without LocalFNO extension)" | ⚠️ MISLEADING FRAMING | UNet-LocalFNO actually beats UNet-FNO on 2/3 benchmarks (Darcy: 5.699E-3 vs 7.142E-3; Allen-Cahn: 9.397E-4 vs 2.060E-3). Only Navier-Stokes favors UNet-FNO (1.570E-2 vs 1.636E-2). The combination is generally superior. | Results table |
| Minimal experiment: "16→128→16 channels, 32 output LSF channels" | ❌ ERROR | "16→128→16" is wrong. The UNet goes 16→32→64→128 (encode) → 128→64→32→16 (decode), outputting **32 channels**. The "16" in the reader's description is the bottleneck direction, not the output. | Sec 5.1: "reaching 128 channels at the bottleneck… The output of the UNet is configured to have 32 channels" |
| Code URL: "Conv-FNO/UNet-FNO own code 要確認" | ⚠️ UNCONFIRMED | Paper only cites `https://github.com/neuraloperator/neuraloperator` for the FNO backbone. No separate repo for UNet-FNO is mentioned in the excerpt. | Sec 5.1: "The implementation of the FNO is based on the code available at https://github.com/neuraloperator/neuraloperator" |
| DOI: 要確認 | ⚠️ UNCONFIRMED | No DOI found in excerpt; arXiv metadata fetch failed (HTTP 429). | fetch_errors |
| Venue status: 要確認 | ⚠️ UNCONFIRMED | No submission/acceptance info in excerpt; appears arXiv-only as of v2. | excerpt |
| LocalFNO training time omitted | ⚠️ MISSING CONTEXT | LocalFNO takes 2163s (vs FNO 1207s, UNet-FNO 1492s). UNet-FNO is notably faster than LocalFNO despite better accuracy. This is relevant for the cost-benefit analysis. | Table 5 |
| Data-limited robustness: no specific ablation numbers | ⚠️ LOW CONFIDENCE | Paper claims "exceptional performance even with limited training data" but no specific ablation at 100/200 samples is visible in the excerpt. | Sec 1 contribution bullet; Sec 5 mentions it but numbers not in excerpt |

---

## Verifier 1: Factual Check

**Metadata:**
- ✅ Title, authors, year (2025), arXiv ID (2503.17797v2), date (3 Jun 2025) — all confirmed from excerpt header.
- ⚠️ DOI: not found in excerpt; arXiv metadata fetch failed (HTTP 429 rate limit).
- ⚠️ Venue: no conference/journal submission info visible; appears arXiv preprint only.

**Method:**
- ✅ Conv-FNO = P_θ2 ∘ CAT ∘ CNN_θ1 (Sec 3, Eq. 1) — correct.
- ✅ UNet-FNO: 4-level, 3 encode + bottleneck + 3 decode, 16→32→64→128 channels, bottleneck 128, 32 output channels, kernel size 3, circular padding, ReLU, no bias — all confirmed (Sec 5.1).
- ✅ 4 FNO blocks following UNet — confirmed.
- ✅ Two resizing schemes for resolution invariance — confirmed (Sec 4.1, Eqs. 2–3).
- ✅ Theorem 4.3: optimal Conv-FNO has lower error than FNO across all resolutions — confirmed.

**Inputs / outputs:**
- ✅ Generic PDE input functions → PDE solution functions — correct.
- ✅ Tested on Darcy flow (a→u), Allen-Cahn, Navier-Stokes — confirmed.

**Baselines:**
- ✅ FNO, U-FNO, CNO, LocalFNO all mentioned as baselines in the paper — confirmed.

**Metrics:**
- ✅ Relative L2 error — confirmed via Theorem 4.3 formula using L2 norm.

**Quantitative results:**
- ✅ All three PDE results verified against the results table:
  - Darcy: FNO 1.686E-2, LocalFNO 9.029E-3, UNet-FNO 7.142E-3, UNet-LocalFNO 5.699E-3
  - Allen-Cahn: FNO 2.945E-3, LocalFNO 1.834E-3, UNet-FNO 2.060E-3, UNet-LocalFNO 9.397E-4
  - Navier-Stokes: FNO 5.271E-2, LocalFNO 2.301E-2, UNet-FNO 1.570E-2, UNet-LocalFNO 1.636E-2
- ✅ Training times on A100, 1000 samples: FNO=1207s, LocalFNO=2163s, UNet-FNO=1492s, UNet-LocalFNO=2524s — +24% overhead confirmed.

**Code / data availability:**
- ⚠️ FNO backbone from `neuraloperator` library — confirmed.
- ⚠️ UNet-FNO own implementation: no separate repo cited in excerpt. Likely not open-sourced separately.

---

## Verifier 2: Research-Fit Check

**Relevance to current research (Meep/FDTD 2.4 GHz indoor WiFi FNO surrogate):**
- ✅ The core insight — FNO misses local spatial features — is directly relevant. Indoor WiFi propagation has exactly the features the paper targets: sharp gradients near sources, material boundary transitions, and shadow-region fine structure.
- ⚠️ The paper's PDEs (elliptic Darcy, parabolic Allen-Cahn, parabolic Navier-Stokes) are **non-oscillatory**. Helmholtz/wave equations produce λ/2-scale oscillations that were **not tested**. This is the single biggest gap.
- ⚠️ Complex-valued field output (Ez) is **not addressed** in the paper. Real/imag splitting or modulus+phase encoding would be needed.

**What is directly usable:**
- ✅ UNet-FNO architecture spec is concrete and reproducible (Sec 5.1 gives full hyperparameters).
- ✅ Two resizing schemes for resolution invariance — Scheme 2 (resize only CNN features) is theoretically safer for wavelength-scale grids.
- ✅ Theorem 4.3 provides theoretical justification that Conv-FNO strictly dominates FNO in expressivity.
- ✅ Data-limited robustness is valuable since Meep simulation data generation is expensive.

**What is only background:**
- The toy Conv-FNO experiments (Sec 3.1) with varying kernel sizes are illustrative but not directly useful for implementation.
- The general discussion of LSFs in PDEs is context for the thesis but not an actionable contribution.

**Risks if used in thesis:**
1. **Oscillation mismatch:** Helmholtz solutions oscillate at λ/2 scales. The CNN's local receptive field (kernel=3) may not resolve these without sufficient depth or larger kernels. The paper's success on smooth PDEs doesn't guarantee transfer to wave PDEs.
2. **Complex-valued fields:** No guidance on handling complex Ez. Real/imag split doubles output channels; modulus+phase loses phase continuity at zeros.
3. **Geometry generalization:** If the UNet pre-extractor overfits to training-domain room layouts, it could degrade on unseen geometries — a critical failure mode for surrogate modeling.
4. **Interpolation artifacts:** Resolution-invariance resizing at wavelength scales could corrupt phase information. Scheme 2 is safer but untested for wave PDEs.

---

## Final Verified Summary

**Title:** Enhancing Fourier Neural Operators with Local Spatial Features

**One-paragraph summary:**
Liu et al. (2025, arXiv:2503.17797) propose Conv-FNO, a hybrid architecture that prepends a CNN feature pre-extractor to a standard FNO backbone, concatenating the extracted Local Spatial Features (LSFs) with the original input before Fourier processing. The best-performing variant, UNet-FNO, uses a 4-level UNet (16→32→64→128 channels, 32 output LSF channels) feeding into 4 FNO blocks. Two resizing schemes preserve resolution invariance. Theoretical analysis (Theorem 4.3) proves Conv-FNO strictly dominates FNO in expressivity. Empirically, UNet-FNO reduces relative L2 error by 2.3× on Darcy flow, 1.4× on Allen-Cahn, and 3.4× on Navier-Stokes versus FNO, with only +24% training time overhead. Combining with LocalFNO yields further gains on Darcy and Allen-Cahn. The architecture shows promise for data-limited scenarios. Tested only on non-oscillatory elliptic/parabolic PDEs; wave/Helmholtz equations and complex-valued fields are not addressed.

**Usefulness for current research:** **High.** The LSF concept directly targets error modes critical for indoor WiFi surrogate modeling (source-near gradients, wall-shadow transitions, low-field fine structure). The architecture is concrete, well-specified, and has modest overhead. The main risk is transferability to oscillatory Helmholtz solutions — the paper's PDEs are fundamentally different from wave equations.

**Implementation implication:** Replace the 2D FNO's pointwise lifting layer with a UNet pre-extractor (config as Sec 5.1). Use Scheme 2 for resolution invariance. Handle complex Ez via real/imag channel split. Estimated 1–2 weeks for a working prototype. Key experiments: compare source-near MAE, wall-shadow MAE, and far-field MAE against the existing 2D FNO baseline on 500 Meep configurations.

**Remaining 要確認 items:**
1. **DOI** — not found; arXiv metadata fetch failed (HTTP 429).
2. **Venue/submission status** — appears arXiv-only as of v2 (Jun 2025).
3. **UNet-FNO open-source code** — only FNO backbone repo cited; own implementation likely not publicly available.
4. **Data-limited ablation specifics** — claimed but no concrete numbers at 100/200 samples visible in excerpt.
5. **Computational complexity (FLOPs/memory)** — not quantified beyond training time.
