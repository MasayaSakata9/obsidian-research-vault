---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-30_16-40-23.md
sha256: a3a5cf30d7272a2827b6fc843c0ed396d9c196b6fa045adb00605e165cb77219
---

## Verification Verdict
- Verdict: **VERIFIED_WITH_CORRECTIONS**
- Source-read level: full text (73,690 chars extracted from PDF)
- Main reason: Core claims (architecture, error rates, parameter counts, code availability) are all confirmed against the primary-source excerpt. Two minor corrections needed: (1) Navier-Stokes epoch reduction is 30% not ~25%, and (2) baseline FNO errors include ±0.5% variance not reflected in the summary.

## Corrections
| Reader claim | Status | Correction / note | Evidence |
|---|---|---|---|
| 訓練 epoch 約 25% に短縮 | PARTIAL | Burgers/Darcy は 150/600 = 25% で正しいが、Navier-Stokes は 180/600 = 30%。全体的に 25–30% の範囲 | Table 2: MgFNO N-S = 60+60+60 = 180 epochs |
| Burgers 1.60%, Darcy 0.9%, N-S 1.28% (FNO baseline) | MINOR | 論文では `±0.5%` の分散が付く（Burgers: 1.60% ± 0.5%, Darcy: 0.9% ± 0.5%, N-S: 1.28% ± 0.5%） | Table 2 in targeted_snippets["baselines"] |
| 3 層 V-cycle 多グリッド | VERIFIED | Abstract + Section 4.2 で明記 | "three-level hierarchy where: the coarse-level network, the intermediate network and the fine-level network" |
| パラメータ数約 3 倍 | VERIFIED | Burgers: 582K→1.75M (3.0x), Darcy: 2.37M→7.1M (3.0x), N-S: 929K→2.79M (3.0x) | Table 2 |
| ゼロショット超解像 | VERIFIED | Fig 9: 64×64→256×256 の例が記載 | targeted_snippets["data"] |
| コンパクトサポート活性化関数 | VERIFIED | Section 1 + Section 3 で言及 | "we incorporate compact support activation functions" |
| Loss = L2 相対誤差 | VERIFIED | Eq. 5.1 に一致 | targeted_snippets["baselines"] |
| 電磁界系への直接的適用は未検証 | VERIFIED | 本文に Maxwell/Helmholtz 言及なし | 全文スキャン結果 |

## Verifier 1: Factual Check
- **Metadata**: ✅ 正しい。Title, Authors (Zi-Hao Guo, Hou-Biao Li*), Year (2024, v3: May 2025), arXiv ID (2407.08615), venue (arXiv math.NA preprint), code URL すべて確認済み。
- **Method**: ✅ 3 層 V-cycle 多グリッド FNO、逐次訓練 + パラメータ転送、コンパクトサポート活性化関数 — すべて Abstract + Section 4.2 で確認。
- **Inputs/outputs**: ⚠️ 読者サマリの `(eps_r, sigma, source_map) → ez_steady` マッピングは論文の記載ではなく、現在の研究への転用提案。これは「Implementation」セクションにあるので問題ないが、論文自体の記述ではないことを明確にする必要がある。
- **Baselines**: ✅ 標準 FNO (600 epoch) — Table 2 で確認。ただし分散 ±0.5% の記載がサマリから抜けている。
- **Metrics**: ✅ L2 相対誤差 `||ŷ−y||²/||y||²` の平均 — Eq. 5.1 で確認。
- **Quantitative results**: ✅ 誤差値 (0.17%, 0.28%, 0.22%) と削減率 (89%, 71%, 83%) すべて一致。Navier-Stokes の epoch 削減率は 30% で「約 25%」は若干の過小評価。
- **Code/data availability**: ✅ GitHub URL 記載あり、Abstract で明言。

## Verifier 2: Research-Fit Check
- **Relevance to current research**: 間接的だが理論的に適合。MgFNO は FNO の訓練加速版であり、現在の 2D FNO ベースラインの直接的上位互換候補。多グリッドの「高周波成分を粗グリッドで処理」する設計思想は、室内 WiFi 伝播で重要な source-near error（高周波）と wall-shadow error の低減に理論的に適合する。ただし、論文が検証する PDE（Burgers, Darcy, N-S）はすべて流体力学系であり、電磁界系（Maxwell/Helmholtz 方程式）での適用は未検証。
- **What is directly usable**: (1) MgFNO の 3 層逐次訓練アーキテクチャのコード（GitHub 公開）、(2) ゼロショット超解像機能（粗グリッド訓練→細グリッド推論）、(3) パラメータ転送ロジック。これらは既存の FNO コードベースに追加可能。
- **What is only background**: 多グリッド手法の理論的基礎（Section 2）、F-Principle の議論（Section 3）は一般的な背景知識。W-cycle 変種は言及されるのみで実験結果なし。
- **Risks if used in thesis**: (1) 電磁界系での振る舞いは未検証 — 室内 WiFi の不規則幾何・多重散乱環境では多グリッドの誤差減衰仮定が崩れる可能性。(2) パラメータ数 3 倍増加は GPU メモリ制約環境で問題。(3) 複素数出力（Ez の位相）には実数 FNO の拡張が必要。(4) 3 層逐次訓練で失敗した層のパラメータ転送で誤差が累積するリスク。(5) 論文は arXiv preprint であり、ジャーナル査読を経ていない。

## Final Verified Summary
- **Title**: MgFNO: Multi-grid Architecture Fourier Neural Operator for Parametric Partial Differential Equations
- **One-paragraph summary**: 本論文は、従来の FNO の訓練を加速する多グリッド FNO（MgFNO）を提案する。3 層 V-cycle アーキテクチャ（粗グリッド→中間→細グリッド）で逐次訓練し、各層間でパラメータを転送する。F-Principle（DNN が低周波→高周波順に学習する特性）と多グリッド手法（高周波→低周波の誤差減衰）を組み合わせることで、Burgers 方程式（0.17%）、Darcy 流（0.28%）、Navier-Stokes 方程式（0.22%）で標準 FNO 比 71–89% の誤差削減を達成。訓練 epoch は約 25–30% に短縮されるが、パラメータ数は約 3 倍に増加。ゼロショット超解像を維持。コンパクトサポート活性化関数を採用。
- **Usefulness for the current research**: **High**。現在の 2D FNO ベースラインの直接的上位候補。訓練速度 4 倍高速化 + 誤差 70–90% 削減の両方を達成しており、WiFi 電磁界 surrogate の実用化に寄与する可能性が高い。多グリッド構造は source-near error と wall-shadow error の低減に理論的に適合。ただし、電磁界系での検証は別途必要。
- **Implementation implication**: 既存 FNO コードベースに 3 層逐次訓練ループとパラメータ転送ロジックを追加するだけで実装可能（Med cost）。GitHub コードが公開されている。解像度レベルを `64×64 → 128×128 → 256×256` のように設定可能。複素数出力対応が必要なら実数 FNO の拡張手法を別途調査。
- **Remaining `要確認` items**:
  1. DOI — arXiv preprint であり、ジャーナル掲載 DOI は未定
  2. 電磁界系（Helmholtz 方程式）での適用可能性 — 論文内で言及なし、別途検証必要
  3. 複素数出力対応方法 — 実数 FNO の拡張が必要
  4. 室内 WiFi 不規則幾何環境での一般化能力 — 未検証
