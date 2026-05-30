---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-30_13-39-25.md
sha256: 8e8500498d3cf68b4df52ffff80514356f307c23c3544197332e755032833fbd
---

## Verification Verdict
- **Verdict:** VERIFIED_WITH_CORRECTIONS
- **Source-read level:** abstract only (NeurIPS web excerpt + Crossref metadata; full text not available)
- **Main reason:** The reader summary is largely faithful to the abstract. One factual correction needed (author ordering). The "Research Usefulness" and Specialist assessments contain reasonable inference explicitly labeled as such, but several items remain unverifiable from abstract-only access.

---

## Corrections
| Reader claim | Status | Correction / note | Evidence |
|---|---|---|---|
| Authors: "Zipeng Xiao, Siqi Kou, Zhongkai Hao, Bokai Lin, Zhijie Deng" | **CORRECTION** | Crossref lists: "Zhijie Deng, Zhongkai Hao, Siqi Kou, Bokai Lin, Zipeng Xiao". The NeurIPS page excerpt also shows "Zipeng Xiao, Siqi Kou, Zhongkai Hao, Bokai Lin, Zhijie Deng" — so the reader matches the NeurIPS page ordering. Crossref has a different order. Both are likely correct (different sorting conventions). No substantive error. | Crossref metadata vs. NeurIPS page |
| "up to 31% average improvement compared to competing neural operator baselines" | VERIFIED | Matches abstract exactly: "up to 31% average improvement compared to competing neural operator baselines" | NeurIPS web excerpt |
| Two implementations: KAN-based and MLP+orthogonal embedding | VERIFIED | Abstract: "two implementations of AM-FNO, based on... KAN and MLPs equipped with orthogonal embedding functions respectively" | NeurIPS web excerpt |
| "Code URL: 要確認" | VERIFIED | No code URL in abstract or Crossref metadata; NeurIPS page has "Paper" and "Supplemental" links but content not fetched | Source context |
| AM-FNO addresses parameter explosion from per-mode parameters | VERIFIED | Abstract: "FNOs employ separate parameters for different frequency modes... results in an undesirably large number of parameters" | NeurIPS web excerpt |
| Specialist 1–3 assessments (WiFi/FDTD relevance, implementation cost, etc.) | ACCEPTABLE INFERENCE | All speculative claims are explicitly labeled as inference ("推論であり、論文で直接検証されているわけではない"). Not overstatements. | Reader explicitly qualifies |
| Evidence Table confidence levels | VERIFIED | Appropriately calibrated — High for abstract-confirmed claims, Low for unconfirmed items | Consistent with source-read level |

---

## Verifier 1: Factual Check

**Metadata:**
- ✅ Title correct: "Amortized Fourier Neural Operators"
- ✅ Authors correct (ordering differs between Crossref and NeurIPS page — both valid)
- ✅ Year: 2024
- ✅ Venue: NeurIPS 2024, Main Conference Track
- ✅ DOI: 10.52202/079017-3651
- ⚠️ arXiv ID: reported as "なし" — could not verify; no arXiv metadata in source context. An arXiv preprint may exist but was not fetched.

**Method:**
- ✅ Accurately describes the core problem: per-mode parameters → parameter explosion in high-dimensional PDEs
- ✅ Correctly describes the solution: amortized neural parameterization of kernel function
- ✅ Two implementations correctly identified: AM-FNO-KAN and AM-FNO-MLP (with orthogonal embedding)
- ✅ "Arbitrarily many frequency modes with fixed parameters" — matches abstract wording

**Inputs / outputs:**
- ✅ Correctly notes these are NOT specified in the abstract
- ✅ Reasonably infers PDE general input/output form

**Baselines:**
- ✅ Correctly states specific baseline names are NOT in the abstract
- ✅ Notes only "competing neural operator baselines" is mentioned

**Metrics:**
- ✅ Correctly states specific metrics are NOT in the abstract

**Quantitative results:**
- ✅ "up to 31% average improvement" — exact match with abstract
- ✅ Correctly notes which task/metric is unknown

**Code / data availability:**
- ✅ Correctly marked as 要確認
- ✅ Notes NeurIPS supplemental material should be checked

---

## Verifier 2: Research-Fit Check

**Relevance to current research (Meep/FDTD 2.4GHz indoor WiFi FNO surrogate modeling):**
- The connection is **indirect but plausible**. AM-FNO targets the exact bottleneck the current research faces: `fno_modes=256` limiting high-frequency representation (shadow boundaries, source-near gradients).
- The reader correctly frames this as *inference*, not direct evidence. No Helmholtz/EM evaluation is mentioned in the abstract.
- **Not overstated** — the reader consistently uses "可能性がある", "推論であり", "要確認".

**What is directly usable:**
- The architectural insight (amortized parameterization → more modes without parameter explosion) is directly relevant to the mode-limitation problem.
- The MLP-based variant (AM-FNO-MLP) is the most practical starting point for experimentation.

**What is only background:**
- The KAN-based variant is interesting but speculative for this use case — KAN training stability and inference overhead are unknown.
- The "31% improvement" claim cannot be contextualized without knowing which PDEs/metrics were tested.

**Risks if used in thesis:**
1. **No EM/Helmholtz validation:** The paper does not appear to test on electromagnetic PDEs. Applying AM-FNO to Helmholtz equations (complex-valued, high wavenumber, PML boundary conditions) is untested territory.
2. **Complex output handling:** Helmholtz solutions are complex-valued. Whether AM-FNO natively supports complex outputs or requires real/imaginary splitting is unknown from the abstract.
3. **Computational overhead:** KAN-based AM-FNO may have higher inference cost than standard FNO, undermining the "surrogate model" premise.
4. **Abstract-only access:** Without full text, the actual datasets, baselines, and metrics remain unknown. The "31% improvement" could be on easy PDEs (Darcy flow) rather than wave equations.

---

## Final Verified Summary

**Title:** Amortized Fourier Neural Operators (AM-FNO)

**One-paragraph summary:**
NeurIPS 2024 paper proposing AM-FNO, a variant of Fourier Neural Operators that replaces per-frequency-mode parameters with an amortized neural parameterization of the kernel function, enabling arbitrarily many frequency modes with a fixed parameter count. Two implementations are provided: KAN-based (AM-FNO-KAN) and MLP-based with orthogonal embeddings (AM-FNO-MLP). Evaluated on diverse PDE datasets across multiple domains, reporting up to 31% average improvement over competing neural operator baselines. Abstract only — specific datasets, baselines, metrics, and code availability not confirmed.

**Usefulness for current research:**
Medium-High. Directly addresses the mode-limitation bottleneck (`fno_modes=256`) in the current 2D FNO baseline for indoor WiFi field prediction. The ability to use more frequency modes without parameter explosion could improve high-frequency error modes (shadow boundaries, source-near gradients). However, no electromagnetic/Helmholtz validation is reported, and complex-valued output handling is unconfirmed.

**Implementation implication:**
Medium cost. The AM-FNO-MLP variant is the most practical entry point — requires replacing the FNO kernel module with an amortized parameterization + orthogonal embedding MLP. The KAN variant requires additional dependency evaluation. A minimal experiment: swap the kernel in the existing 2D FNO training pipeline and test with `fno_modes=512` or `1024`.

**Remaining 要確認 items:**
1. Full text / arXiv preprint — need to identify actual datasets (Navier-Stokes? Darcy? Helmholtz?), baselines (FNO, U-FNO, Geo-FNO?), and metrics (L2, H1, relative error?)
2. Code availability — check NeurIPS supplemental material and GitHub
3. Complex-valued output support — critical for Helmholtz equation
4. Computational cost comparison — especially for KAN variant
5. Frequency mode sensitivity analysis — does "arbitrarily many" hold in practice?
