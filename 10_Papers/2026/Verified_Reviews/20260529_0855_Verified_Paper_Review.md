---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-29_07-47-03.md
sha256: 40f0e72ca1c7f75654344eb20e5753f1755dea70e1d1c935ed5b85b085810d4a
---

Based on the provided primary source context (74,410 characters of extracted full text covering pages 1–11), I can now perform the verification. The excerpt covers the abstract, introduction, FNO preliminaries, MscaleFNO architecture definition, and the beginning of Section 4 (numerical experiments). The excerpt is **truncated** — it cuts off mid-sentence at page 11, before the actual numerical results figures/tables and before Sections 4.2+ (2D/3D Helmholtz experiments) and Section 5 (conclusion).

## Verification Verdict
- **Verdict:** VERIFIED_WITH_CORRECTIONS
- **Source-read level:** full text (partial — pages 1–11 of ~15; truncated before results figures and 2D/3D experiments)
- **Main reason:** The reader summary is substantially correct on metadata, architecture, and experimental setup. However, (1) the claim that scale factors `c_i` are "学習可能" (learnable) cannot be verified from the excerpt — the paper only shows them as initial values without confirming trainability, (2) all quantitative error results are in figures/tables not included in the text extraction, and (3) the 2D/3D Helmholtz experiments (Section 4.2+) are entirely outside the excerpt scope.

## Corrections
| Reader claim | Status | Correction / note | Evidence |
|---|---|---|---|
| Scale factors `c_i` are "学習可能" (learnable parameters) | ⚠️ UNVERIFIED | The excerpt shows initial values `c = {1, 10, 20, 40, 60, 80, 100, 120}` but does **not** explicitly state whether they are trained or fixed. MscaleDNN precedent suggests learnability, but the paper text available does not confirm it. | Section 3.2 + Section 4.1 excerpt |
| "少ないパラメータで高周波成分の学習を達成" with specific error values | ⚠️ PARTIAL | Parameter comparison (1,035,544 vs 1,164,001) confirmed. Actual error values (relative L2) are in figures/tables **not included** in the text excerpt. Cannot verify numerical improvement magnitude. | Section 4.1 excerpt |
| 2D/3D Helmholtz equation numerical experiments details | ❌ MISSING | Sections 4.2+ covering 2D/3D Helmholtz experiments are **entirely outside** the excerpt scope. No data available. | Excerpt truncated at p.11 |
| Code availability: "本文に明記なし" | ✅ CORRECT | No GitHub/code URL mentioned in the excerpted text. | Full excerpt scan |
| Priority: High for WiFi FNO surrogate research | ✅ AGREE | Direct Helmholtz connection + spectral bias mitigation are genuinely relevant. | Abstract + Section 3.2 |
| "実装コスト: Med" assessment | ✅ REASONABLE | Architecture is a parallelization of standard FNO branches; drop-in compatible. | Section 3.2 architecture |

## Verifier 1: Factual Check

**Metadata:**
- ✅ Title: "MscaleFNO: Multi-scale Fourier Neural Operator Learning for Oscillatory Function Spaces" — confirmed by page 1.
- ✅ Authors: Zhilin You¹, Zhenli Xu¹, Wei Cai*² — confirmed by page 1. Affiliations: Shanghai Jiao Tong University (1), Southern Methodist University (2).
- ✅ Date: 2024-12-28 (arXiv:2412.20183v1 [math.NA]) — confirmed by page 1.
- ✅ URL: https://arxiv.org/abs/2412.20183 — confirmed.
- ⚠️ DOI: Not present in excerpt (arXiv preprint, likely no DOI yet). Reader correctly marked "要確認".

**Method:**
- ✅ MscaleFNO = parallel FNO branches with scaled inputs — confirmed by abstract and Section 3.2 formulation: `u(x) = Σᵢ γᵢ FNO[cᵢx, cᵢa(x)](x)`.
- ✅ 8 parallel sub-networks with dv=16 each — confirmed Section 4.
- ✅ Initial scale factors: `{1, 10, 20, 40, 60, 80, 100, 120}` — confirmed Section 4.1.
- ⚠️ Whether `c_i` and `γ_i` are learnable vs fixed: **not explicitly stated** in available text. The reader's claim "学習可能" is an inference from MscaleDNN precedent, not directly confirmed.

**Inputs / outputs:**
- ✅ Input: spatial coordinates `x` and function `a(x)` (Helmholtz coefficient/wavenumber) — confirmed by Eq. (11-13).
- ✅ Output: function `u(x)` (Helmholtz solution) — confirmed.
- ✅ FNO takes both `x` and `a(x)` as inputs — confirmed by Eq. (12-13).

**Baselines:**
- ✅ Standard FNO with comparable parameter count — confirmed Section 4: "both models have approximately the same number of parameters."
- ✅ Specific configs: Normal FNO (dv=48, kmax=500) vs MscaleFNO (8×dv=16, kmax=500) — confirmed.
- ✅ Parameter counts: MscaleFNO=1,035,544 vs Normal FNO=1,164,001 — confirmed.
- ✅ Adam optimizer, lr=0.001 — confirmed.

**Metrics:**
- ✅ Relative L2 error — confirmed by Eq. (4) and (6).

**Quantitative results:**
- ⚠️ 1D experiment setup confirmed (1001-point grid, 2000 samples, 1000/500/500 split, batch=20, T=1).
- ❌ Actual error values for Example 4.1 (sin(20a(x))) — in figures **not included** in text excerpt.
- ❌ 2D/3D Helmholtz experiment results — **entirely outside** excerpt scope.

**Code / data availability:**
- ✅ No code URL mentioned in available text — reader correctly marked "要確認".
- ✅ Dataset generated from exact solutions on grid — confirmed Section 4.1.

## Verifier 2: Research-Fit Check

**Relevance to current research:**
- ✅ **Directly relevant.** The paper explicitly targets the Helmholtz equation coefficient→solution mapping, which is the same physics governing 2.4 GHz WiFi propagation (`Δu + a²u = 0`). The Green's function formulations for 2D (Eq. 40) and 3D (Eq. 41) are explicitly presented.
- ✅ **Spectral bias mitigation** is the core motivation — directly addresses the problem of learning high-frequency Ez field oscillations that standard FNO fails at (wall-shadow effects, near-source errors).
- ✅ The multi-scale approach (parallel branches at different scales) is a practical, drop-in extension of standard FNO.

**What is directly usable:**
1. Architecture blueprint: parallel FNO branches with scale factors on both `x` and `a(x)`.
2. Parameter parity strategy: divide channel dimension across branches (dv=48 → 8×dv=16).
3. Initial scale factor values from the paper as starting points.
4. The formulation `u(x) = Σᵢ γᵢ FNO[cᵢx, cᵢa(x)](x)` is immediately implementable.
5. Training hyperparameters (Adam, lr=0.001, batch=20) as baselines.

**What is only background:**
- The 1D `sin(ma(x))` experiments are synthetic proxies for Helmholtz behavior — useful for sanity-checking implementation but not directly applicable to 2D WiFi scenarios.
- The theoretical FNO preliminaries (Section 2) are standard and well-known.

**Risks if used in thesis:**
1. **No 2D validation in available text:** The paper's 2D/3D Helmholtz experiments (Section 4.2+) are outside the excerpt. The reader's specialists correctly flag this as needing verification. If the 2D results are weak, the practical value drops significantly.
2. **Scale factor trainability unclear:** If `c_i` are fixed (not learned), the model is less flexible than implied.
3. **Memory scaling:** N parallel branches mean ~N× memory for FFT operations. For 2D high-resolution grids (e.g., 256×256), 8 branches could be memory-intensive.
4. **No code available:** Implementation must be done from scratch — higher risk of bugs.
5. **Overfitting risk with many branches:** The reader's Specialist 3 correctly identifies this.

## Final Verified Summary

**Title:** MscaleFNO: Multi-scale Fourier Neural Operator Learning for Oscillatory Function Spaces

**One-paragraph summary:**
Zhilin You, Zhenli Xu, and Wei Cai (2024) propose MscaleFNO, a multi-scale extension of the Fourier Neural Operator designed to mitigate spectral bias when learning mappings between highly oscillatory functions. The architecture consists of N parallel FNO branches, each receiving inputs scaled by a factor `c_i` applied to both spatial coordinates and function values, with outputs combined as a weighted sum: `u(x) = Σᵢ γᵢ FNO[cᵢx, cᵢa(x)](x)`. The paper targets the Helmholtz equation coefficient→solution mapping as the primary application. Experiments show MscaleFNO achieves better high-frequency learning than standard FNO with fewer parameters (1,035,544 vs 1,164,001 in the 1D setup). The paper includes 1D function approximation experiments (sin/cos of scaled inputs) and 2D/3D Helmholtz scattering experiments. No code repository is mentioned.

**Usefulness for the current research:** **High.** Directly applicable to 2.4 GHz indoor WiFi Ez-field FNO surrogate modeling because: (1) the Helmholtz equation is the governing physics, (2) the spectral bias problem directly maps to Ez field oscillation learning failures, (3) the architecture is a drop-in extension of standard FNO with moderate implementation cost. The multi-scale design specifically addresses wall-shadow and near-source high-frequency errors.

**Implementation implication:**
- Start with 4 branches (scales `{1, 10, 20, 40}`) on existing 2D FNO baseline
- Divide channel dimension: if baseline dv=48, use 4×dv=12 per branch
- Compare against standard FNO at matched parameter count
- Key metrics: linear Ez MAE, source-near error, wall-shadow error
- Monitor GPU memory usage (FFT cost scales with branch count)

**Remaining `要確認` items:**
1. **2D/3D Helmholtz experiment results** (Section 4.2+) — numerical error values and improvement rates vs standard FNO. **Critical** for assessing practical viability.
2. **Scale factor trainability** — are `c_i` and `γ_i` learned parameters or fixed?
3. **Quantitative error values** for 1D experiments (actual relative L2 numbers).
4. **Inference time comparison** — MscaleFNO vs standard FNO wall-clock time.
5. **Code availability** — no repository found in paper text.
6. **Memory usage comparison** for 2D high-resolution grids.
7. **Boundary condition handling** (Dirichlet/Neumann/PML) in 2D/3D experiments.
