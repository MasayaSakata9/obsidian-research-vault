# Search Policy

## 調査目的

Meep/FDTDで生成した2.4GHz屋内WiFi電波伝搬データに対して、FNOベースのサロゲートモデル精度を改善する論文・手法を継続的に探索する。

特に以下を改善できる手法を優先する。

- 線形スケール `Ez` MAE
- 線形スケール `Ez` 最大絶対誤差
- source-near error
- wall-shadow error
- far-field error
- low-field error

## 優先する情報源

- arXiv
- OpenAlex
- Semantic Scholar
- IEEE Xplore
- Journal of Computational Physics
- SIAM Journal on Scientific Computing
- NeurIPS / ICML / ICLR
- SciML系Workshop
- PhysicsNeMo / NVIDIA Modulus documentation
- GitHub実装

## 論文保存の必須メタデータ

各論文ノートには必ず以下を記録する。

- title
- authors
- year
- venue / journal / conference / arXiv
- DOI
- URL
- PDF URL
- code URL
- 対象物理現象
- 入力データ形式
- 出力field形式
- 使用モデル
- FNO / neural operatorとの関係
- sparse observationの有無
- multi-fidelityの有無
- electromagnetic / wave / Helmholtz / Maxwellとの関係
- 評価指標
- 本研究への関連度: High / Medium / Low
- 本研究で使えそうな要素
- 使う際の注意点
- 除外理由または保留理由

## 関連度判定基準

### High Priority

- 2D/3D電磁界、波動場、Helmholtz、Maxwell、FDTD、radio propagation、WiFi propagationに近い。
- FNOまたはneural operatorでfull-field prediction / reconstructionを扱う。
- sparse observation、line/profile、coarse field、low-fidelity fieldからfull fieldを再構成する。
- source-near、wall-shadow、far-field、weak-fieldなど領域別誤差改善に関係する。
- 現在の入力 `eps_r`, `sigma`, `source_map` と出力 `ez_steady` に実装可能。
- 直接2D FNO baselineと公平比較できる。

### Medium Priority

- 別物理分野だが、低次元→高次元、multi-fidelity、feature loss、uncertainty weightingが転用できる。
- DeepONet、Graph Neural Operator、Transformer系など、FNOの補助として使えそう。
- 物理場super-resolutionやcoarse-to-fine reconstructionに関係する。

### Low Priority

- RSSI/path-loss predictionのみ。
- 一般画像処理のみ。
- 通信路推定のみ。
- pure temporal forecasting中心。
- 物理場評価指標がないGAN/diffusion系。
- toy PDEのみのPINN論文。

## 優先的に探す手法カテゴリ

- Fourier Neural Operator for electromagnetic wave propagation
- Neural operator for Maxwell equations
- Neural operator for Helmholtz equation
- FNO for FDTD surrogate modeling
- Indoor radio propagation surrogate modeling
- Sparse-observation-to-field reconstruction
- RecFNO / sparse-input FNO
- Multi-fidelity neural operator
- Low-resolution to high-resolution residual learning
- Coarse-to-fine wavefield reconstruction
- Wavefield super-resolution
- Source-conditioned neural operator
- Boundary/material-conditioned FNO
- Region-weighted loss
- Uncertainty-weighted loss
- Feature loss for physical fields

## 除外条件

- FNO baselineとの公平比較ができない手法。
- Meep/FDTD teacherと矛盾するPDE residual、境界条件、source条件を強制する手法。
- 現在のデータ `|Ez|` / `ez_steady` だけでは実装困難な複素場・位相前提の手法。
- 単一1Dラインだけからfull 2D fieldを復元する前提が強い手法。
- 評価が見た目だけで、MAE / max error / region-wise errorがない手法。
- 卒論期間内にbaseline比較が困難な高コスト手法。

## 検証ルール

- 新手法は必ず直接2D FNO baselineと同一train/test splitで比較する。
- 主指標は線形 `Ez` MAEと線形 `Ez` 最大誤差。
- dB MAE、source-near、wall-shadow、far-field、low-fieldは補助指標として確認する。
- training lossだけで成功判定しない。
- visual comparisonでは、干渉縞、壁裏シャドウ、source近傍ピーク、遠方弱電界を確認する。
- test casesが少ない評価は暫定結果として扱う。
- 推測で有効性を断定しない。未検証のものは「仮説」と明記する。

## Obsidian保存ルール

- 論文候補・未整理の調査結果は `00_Inbox/` に保存する。
- 個別の論文ノートは `10_Papers/2026/` に保存する。
- 分野全体の整理、研究マップ、手法分類は `20_Literature_Maps/` に保存する。
- 日次の調査まとめは `30_Daily_Reviews/` に保存する。
- 週次の研究レビュー、読むべき論文リスト、研究方針の更新は `40_Weekly_Reviews/` に保存する。
- 研究アイデア、卒論テーマ候補、実験案は `50_Ideas/` に保存する。
- Hermesに渡す長期指示・プロンプトは `80_Prompts/` に保存する。
- テンプレートは `90_Templates/` に保存する。
- 既存ファイルの上書きは禁止。追記または新規ファイル作成を基本とする。
- 秘密情報、認証情報、トークン、private keyは保存しない。

## Hermes Agentへの注意事項

- 研究テーマを勝手に拡張しない。
- 主軸は「2.4GHz屋内WiFi電波伝搬のMeep/FDTDサロゲート」である。
- FNOを現時点の基準モデルとして扱う。
- RSRNet / MFRNet / CoFFe / GLUは、低次元補助・feature loss・multi-fidelity・sparse reconstruction・importance weightingの発想源として読む。
- 単一 `y=1.0m` ラインのhard 1D→2D curriculumは失敗済みなので繰り返さない。
- source-near error、wall-shadow error、far-field error、low-field errorに効く論文を優先する。
- 新しい論文を採用候補にする場合は、現在のデータ形式で実装できるか、FNO baselineと公平比較できるか、主評価指標を改善しそうかを書く。
- 不明点は「要確認」として残す。
