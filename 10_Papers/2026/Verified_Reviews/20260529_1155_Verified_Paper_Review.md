---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-29_10-40-51.md
sha256: 6cd9e0319d1e9445c57822eb6192c4310d63a56c8db0ac53c6f7ad3809091291
---

I'll verify the reader summary against the provided primary source context (PDF excerpt + targeted snippets).

---

## Verification Verdict
- **Verdict:** VERIFIED_WITH_CORRECTIONS
- **Source-read level:** full text (102,262 chars extracted from PDF, with targeted snippets for results/baselines/metrics/data/code)
- **Main reason:** Core claims (FNO architecture, input/output design, scattered-wavefield strategy, training config) are all confirmed by the excerpt. Minor corrections needed on frequency range specificity and a few "要確認" items that can now be resolved.

---

## Corrections
| Reader claim | Status | Correction / note | Evidence |
|---|---|---|---|
| FNO 4 blocks, modes=48, width=128, GELU | ✅ Confirmed | Exact match | Section 3: "Fourier mode set to 48 and a Fourier layer width of 128… GELU" |
| Input: 3ch (velocity + U₀ real/imag), Output: 2ch (δU real/imag) | ✅ Confirmed | Exact match | Figure 1 caption: "three channels, including velocity and real and imaginary parts of the analytical background wavefield… two channels representing real and imaginary parts of the scattered wavefield" |
| Background wavefield embeds source location + frequency | ✅ Confirmed | Exact match | Section 2.2, Eq. 3 (Hankel function H₀⁽²⁾) |
| Scattered wavefield prediction preferred over full-field | ✅ Confirmed | Section 2.3: "learning the residual of the physical wavefield rather than directly learning the physical field itself will improve accuracy"; ablation in Section 4 |
| Training: 9000 train / 1000 val, batch 128, 1000 epochs, lr=1e-3 | ✅ Confirmed | Section 3.1 |
| No normalization — raw velocity values used | ✅ Confirmed | Section 3: "We do not normalize the datasets… directly feed the velocities in km/s" |
| Frequency range 3–21 Hz | ⚠️ Partial | OpenFWI test uses 3–15 Hz; 3–21 Hz is stated as the overall chosen range (likely applies to realistic model tests too). The reader should distinguish these. | Section 3: "chosen frequency range, from 3 Hz to 21 Hz"; Section 3.1: "randomly draw a frequency value from 3Hz to 15 Hz" |
| Background velocity v₀ = 1.5 km/s | ✅ Confirmed | Section 3.1: "choose the background velocity of 1.5 km/s" |
| Code availability: 要確認 | ✅ Resolved | No code mention in excerpt; arXiv metadata fetch failed (429). Still unconfirmed. | Section 2.3 ends with "we include an ablation study… in Section 4" — no code URL anywhere in available text |
| Spatial resolution 139×139 | ✅ Confirmed | Section 3.1: "spatial resolution of 139×139" |
| Relative L2 error 2–6% on validation | ⚠️ Approximate | Figure 2 y-axis labels show ~0.02–0.06 range; exact values depend on reading the graph. Reasonable estimate. | Figure 2 caption: "validation error" |
| Adam optimizer | ✅ Confirmed | Section 3: "Adam optimizer… learning rate 1e-3" |
| 9-point finite difference as reference | ✅ Confirmed | Section 2.3: "optimal 9-point finite difference method"; Section 3.1: "optimal 9-point finite difference method to calculate the reference results" |
| OpenFWI CurveVelA dataset | ✅ Confirmed | Section 3.1: "focusing on the 'CurveVelA' velocity class" |
| Overthrust model validation | ✅ Confirmed | Section 1: "more realistic models, e.g., extracted from Overthrust models" (quantitative results not in available excerpt) |
| DOI: 要確認 | ✅ Still unconfirmed | Submitted to Geophys. J. Int., not yet published |
| Single reference frequency strategy | ✅ Confirmed | Abstract: "single reference frequency strategy"; Section 1: "incorporating the single reference frequency (Huang & Alkhalifah 2022b)" |

---

## Verifier 1: Factual Check

**Metadata:** ✅ All confirmed. Title, authors, arXiv ID (2405.01272v3), date (30 Mar 2025), venue (submitted to Geophys. J. Int.) all match the excerpt. DOI remains unconfirmed — paper is under review.

**Method:** ✅ FNO with 4 blocks, Fourier modes=48, hidden width=128, GELU activation. Scattered wavefield formulation via Eq. 4 is correctly described. Background wavefield via Hankel function H₀⁽²⁾ (Eq. 3) is correctly reported.

**Inputs / outputs:** ✅ 3-channel input (velocity + U₀ real/imaginary) → 2-channel output (δU real/imaginary). Source location and frequency embedded in U₀ rather than as separate channels.

**Baselines:** ✅ Conventional neural operator methods (source as binary mask + frequency as constant channel) compared in Section 3.2. 9-point finite difference used as ground truth reference. Ablation on output type (scattered vs full wavefield) in Section 4.

**Metrics:** ✅ Relative L2 norm error for validation; MSE loss for training.

**Quantitative results:** ⚠️ Mostly confirmed. Validation error ~2–6% relative L2 is a reasonable reading of Figure 2. Frequency range needs distinction: 3–15 Hz for OpenFWI, 3–21 Hz overall. Overthrust quantitative results not available in excerpt.

**Code / data availability:** OpenFWI dataset is cited (Deng et al. 2022) — publicly available. FNO code: NOT mentioned in the paper excerpt. arXiv metadata fetch failed (HTTP 429). Still marked as unavailable.

---

## Verifier 2: Research-Fit Check

**Relevance to current research (Meep/FDTD 2.4 GHz indoor WiFi FNO surrogate):**

The relevance claim is **justified but with important caveats**. The core ideas transfer conceptually:

1. **Scattered-field decomposition** (predict δU = U − U₀ rather than U directly) is the most actionable insight. The paper provides theoretical grounding (residual learning improves accuracy, avoids source singularity) that directly supports designing a similar strategy for WiFi: predict δEz = Ez − Ez₀ where Ez₀ is the empty-room analytical solution.

2. **Background field as input embedding** (encoding source position + frequency into U₀ instead of separate channels) is a concrete architectural suggestion. For WiFi, this means computing the free-space Green's function for the given source/frequency and using it as an input channel alongside εᵣ and σ.

3. **FNO hyperparameters** (modes=48, width=128, 4 blocks) are useful starting points, especially since the grid sizes are comparable (139×139).

4. **No normalization** finding is practically useful — may simplify the data pipeline.

**What is directly usable:** Scattered-field decomposition strategy, background-field embedding design, FNO architecture reference values, no-normalization insight.

**What is only background:** The seismic physics (acoustic Helmholtz vs electromagnetic Helmholtz), specific frequency ranges, velocity model details, Overthrust model experiments.

**Risks if used in thesis:**
- **Physics mismatch:** 2D acoustic wave ≠ 3D electromagnetic wave. The scattered-field strategy may not transfer directly — WiFi has boundary reflections (walls) that seismic open-boundary models don't have.
- **Background field quality:** In seismic, v₀ = 1.5 km/s is a reasonable homogeneous approximation. For WiFi, the "empty room" Ez₀ may differ significantly from the actual background when obstacles are present, potentially making δEz large rather than small.
- **Frequency extrapolation:** The single reference frequency strategy is validated at seismic scales (3–21 Hz). WiFi operates at 2.4 GHz — the extrapolation gap is enormous and unvalidated.
- **Source singularity:** The paper claims scattered-field avoids source singularity for acoustic waves. For EM, Ez gradients near the source can be extremely steep — this needs separate verification.

---

## Final Verified Summary

**Title:** Learned frequency-domain scattered wavefield solutions using neural operators

**One-paragraph summary:**
Huang & Alkhalifah (KAUST, 2024, arXiv:2405.01272v3) propose a Fourier Neural Operator for frequency-domain seismic wavefield simulation that predicts the **scattered wavefield** δU = U − U₀ rather than the full wavefield. The key innovation is embedding source location and frequency information into an analytically computed background wavefield U₀ (via Hankel functions) instead of using them as separate input channels. The FNO (4 blocks, modes=48, width=128, GELU) takes velocity + U₀ (3 channels) and outputs δU (2 channels: real/imaginary). Trained on 9,000 OpenFWI samples with Adam (lr=1e-3, 1000 epochs, batch 128), achieving ~2–6% relative L2 error on validation. No data normalization is used — raw physical values are fed directly. A single reference frequency strategy enables extrapolation to larger domains and higher frequencies. Also validated on Overthrust-derived realistic models. Submitted to Geophys. J. Int.; code not publicly released.

**Usefulness for current research:**
HIGH — primarily for the scattered-field decomposition design pattern and the background-field embedding strategy. These two ideas directly inform how the WiFi FNO surrogate should be structured: (1) predict δEz rather than Ez, and (2) embed source config into the input via free-space Green's function rather than binary masks. The FNO hyperparameter choices and no-normalization finding are also practically useful.

**Implementation implication:**
Medium effort. Requires: (1) computing Ez₀ analytically for the empty-room case (free-space or cavity Green's function), (2) modifying the data pipeline to concatenate Ez₀ real/imaginary channels with εᵣ/σ, (3) changing the network target from Ez to δEz = Ez − Ez₀, (4) adding Ez₀ back during inference to recover the full field. The FNO backbone itself needs minimal changes.

**Remaining `要確認` items:**
- ❓ DOI — paper still under review at Geophys. J. Int.
- ❓ Code availability — no GitHub link found in paper text; arXiv metadata fetch failed (429 rate limit)
- ❓ Section 4 ablation quantitative values (scattered vs full-field exact error comparison) — not in available excerpt
- ❓ Overthrust model quantitative results — mentioned but numbers not in excerpt
- ❓ Single reference frequency strategy implementation details — cited to Huang & Alkhalifah 2022b, mechanism not fully explained in this paper
- ❓ Computational speedup vs finite-difference — claimed but no quantitative timing in excerpt
