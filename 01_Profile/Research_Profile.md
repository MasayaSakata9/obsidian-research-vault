# Research Profile

## 研究テーマ

Meep/FDTDで生成した2.4GHz屋内WiFi電波伝搬データを、Fourier Neural Operator（FNO）を中心とするニューラルサロゲートモデルで高速・高精度に近似する研究。

対象は、コンクリート壁を含む2D室内環境における定常状態の `Ez` / `|Ez|` 電界分布。

現在の主軸は、直接2D FNOを基準モデルとし、低次元伝搬情報・補助損失・局所ガイド・multi-fidelity情報によって精度を改善すること。

## 研究目的

- Meep/FDTDによる高コストなWiFi伝搬シミュレーションを、学習済みサロゲートモデルで高速に予測する。
- 入力された壁配置・材料分布・送信点から、2D電界分布を予測する。
- 特に、干渉縞、壁裏シャドウ、遠方弱電界、source近傍ピークを高精度に再現したい。
- 主評価指標は、線形スケール `Ez` の平均絶対誤差と最大絶対誤差。

## 対象としている現象・問題

- コンクリート壁による反射、減衰、回折、壁裏シャドウ。
- マルチパス干渉による干渉縞。
- source近傍の急峻な高振幅領域。
- weak-field / far-field / wall-shadow領域での誤差。
- FDTD計算コストをニューラルサロゲートで置き換える問題。

## 現在の研究仮説

- 直接2D FNOは強い基準モデルだが、低次元情報や物理的に意味のある補助損失を組み合わせれば、source-near error、wall-shadow error、low-field error、最大誤差を改善できる可能性がある。
- 単一の `y=1.0m` ラインから2D fieldを復元する単純な1D→2Dカリキュラムは不十分。
- FNOを2D backboneとして維持し、低次元情報は補助損失・条件付け・feature fusion・residual learningとして使う方針が有望。

## 使用中・使用予定の手法

### 使用中

- Meep/FDTDによる教師データ生成
- PyTorch
- NVIDIA PhysicsNeMo / Modulus Sym
- 2D Fourier Neural Operator（FNO）
- 入力: `eps_r`, `sigma`, `source_map`
- 出力: `ez_steady` / `|Ez|`
- dB変換・正規化
- weighted L2 loss
- weak-field / wall-shadow / far-field / source-near weighting
- 共通test splitによるモデル比較

### 調査・検討対象

- FNO + multi-line auxiliary loss
- FNO + source-neighborhood line/cross/patch auxiliary loss
- FNO + field feature loss
- Sparse-observation-to-field FNO
- RecFNO / sparse-input FNO
- Multi-fidelity neural operator
- Low-resolution to high-resolution residual FNO
- Coarse-to-fine field reconstruction
- Wavefield super-resolution
- Validation-error-driven weighting
- Uncertainty weighting
- Multi-scale FNO / AFNO / Nested FNO

## 現在困っていること

- 低次元情報を入れても、直接2D FNO baselineを明確に超える設計がまだ確定していない。
- 単一 `y=1.0m` ラインは情報量が不足していた。
- source-near領域の最大誤差が残りやすい。
- wall-shadow、far-field、low-fieldなど領域別の改善と、全体MAE改善を両立したい。
- 卒論として十分な精度の数値目標は要確認。

## 探してほしい手法・論文

- Neural operatorによる電磁波・波動場・Helmholtz / Maxwell surrogate modeling
- FNOによる2D/3D wave propagation surrogate
- FDTD surrogate modeling
- Indoor radio propagation surrogate
- WiFi / RF / electromagnetic full-field prediction
- Sparse observations to full-field reconstruction
- Multi-fidelity neural operator
- Low-fidelity to high-fidelity residual learning
- Coarse-to-fine field reconstruction
- Wavefield super-resolution
- Source-conditioned neural operator
- Boundary/material-conditioned FNO
- Region-weighted loss / uncertainty-weighted loss
- Field feature loss

## 優先キーワード

### 英語

- WiFi propagation surrogate model
- indoor radio propagation surrogate
- electromagnetic field surrogate learning
- FDTD surrogate model
- Meep FDTD machine learning surrogate
- Fourier Neural Operator electromagnetic waves
- FNO wave propagation
- neural operator for Maxwell equations
- neural operator for Helmholtz equation
- sparse observation full-field reconstruction
- sparse-input Fourier Neural Operator
- RecFNO
- multi-fidelity neural operator
- low-fidelity high-fidelity residual learning
- coarse-to-fine field reconstruction
- wavefield super-resolution
- source-near error
- wall-shadow error
- weak-field weighting
- uncertainty-aware loss weighting

### 日本語

- WiFi 電波伝搬 サロゲートモデル
- FDTD サロゲートモデル
- フーリエニューラルオペレータ 電磁波
- Neural Operator 電磁界
- Helmholtz 方程式 サロゲート
- Maxwell 方程式 ニューラルオペレータ
- 波動場 予測 FNO
- スパース観測 全場再構成
- 低次元情報 物理場再構成
- 壁裏シャドウ 電波伝搬
- source-near error 電波
- multi-fidelity サロゲート

## 除外したい研究・手法

- FNO baselineと公平比較できない全面置換手法。
- 単一 `y=1.0m` ラインだけに依存する1D→2D hard switching。
- 学習lossだけを改善し、線形 `Ez` MAE・最大誤差を確認しない研究。
- Meep/FDTDのsource、PML、材料、境界条件、位相情報と整合しないPDE residual / PINOの安易な導入。
- 一般画像生成・CNN super-resolutionのみで、電磁波・波動場・operator learningとの接続が薄い研究。
- RSSI/path-loss predictionのみで、2D full-field predictionを扱わない研究。

## 評価基準

### 主指標

- 線形スケール `Ez` MAE
- 線形スケール `Ez` 最大絶対誤差

### 補助指標

- dB MAE
- dB max error
- source-near MAE / max error
- wall-shadow MAE
- far-field error
- low-field error
- 干渉縞、壁裏シャドウ、source近傍ピークの視覚的再現性

### 比較ルール

- 直接2D FNO baselineと同一train/test splitで比較する。
- dB lossが良くても、線形 `Ez` MAE・最大誤差が悪ければ採用しない。
- test caseが少ない結果は暫定扱いにする。

## 実装・実験環境

- Meep/FDTD
- 2.4GHz continuous source
- 2D indoor propagation field
- 800 x 800 grid
- PyTorch
- NVIDIA PhysicsNeMo / Modulus Sym
- FNO
- Hydra config
- RTX 6000 Ada / B200クラスタ候補

## 卒論・研究としてのゴール

- Meep/FDTDの2.4GHz屋内WiFi伝搬をFNOサロゲートで高速予測できることを示す。
- 直接2D FNO baselineを基準に、低次元伝搬情報・局所ガイド・multi-fidelity・feature lossなどで改善を示す。
- 「Propagation-guided FNO for indoor electromagnetic surrogate modeling」として、どの低次元情報が有効かを整理する。

## Hermes Agentへの運用メモ

- 研究テーマを勝手に拡張しない。
- FNOを現時点の基準モデルとして扱う。
- RSRNet / MFRNet / CoFFe / GLUは直接置換ではなく、補助損失・feature fusion・sparse observation・importance weightingの発想源として読む。
- 単一 `y=1.0m` ラインのhard 1D→2D curriculumは失敗済みなので繰り返さない。
- 論文探索では、source-near error、wall-shadow error、far-field error、low-field errorに効きそうな手法を優先する。
- 新しい論文については、現在のデータ形式で実装可能か、FNO baselineと公平比較可能か、主評価指標を改善しそうかを必ず評価する。
- 不明点は推測で埋めず、`要確認` として残す。
