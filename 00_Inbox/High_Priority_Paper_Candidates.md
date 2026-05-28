# High Priority Paper Candidates

> 最終更新: 2026-05-26
> 検索クエリ:
>   1. ti:"Fourier" AND ti:"Neural Operator" AND (wave OR electromagnetic OR "wave propagation" OR scattering)
>   2. ti:"neural operator" AND (wave OR Helmholtz OR Maxwell OR "wave equation")
>   3. ti:"sparse" AND ti:"neural operator" OR ti:"FNO" AND (reconstruction OR inverse OR "data assimilation")

---

## 1. Neural Adjoint Method for Meta-optics: Accelerating Volumetric Inverse Design via Fourier Neural Operators

- **Title**: Neural Adjoint Method for Meta-optics: Accelerating Volumetric Inverse Design via Fourier Neural Operators
- **Authors**: Chanik Kang, Hyewon Suk, Haejun Chung
- **Year**: 2026
- **arXiv ID**: 2604.17425
- **URL**: https://arxiv.org/abs/2604.17425
- **PDF**: https://arxiv.org/pdf/2604.17425
- **Categories**: cs.LG, physics.optics
- **なぜHigh Priorityか**:
  - FNOをMaxwell方程式のFDTDシミュレーションのサロゲートとして直接適用している。本研究（2.4GHz WiFi伝播のFNOサロゲート）と最も直接的に関連する。
  - 入力: 体積誘電率分布 → 出力: 電磁場（adjoint gradient field）。本研究の「入力: eps_r/sigma/source_map → 出力: Ez」のデータフローと構造が類似している。
  - Stage-wise FNOによる高周波成分の段階的学習という手法が、本研究で問題になっているsource-near領域やwall-shadow領域の誤差低減に応用可能。
- **本研究に使えそうな要素**:
  - Stage-wise FNO: 高周波成分を段階的に強調する損失重み付け。本研究で問題のsource-near領域（高周波成分が強い）の誤差低減に使える。
  - FDTDペアデータセットの構築方法: Meep/FDTDでteacherデータを作成する本研究のデータパイプラインの参考になる。
  - Solver-supervised surrogateの概念: FDTDのadjoint解をFNOで近似するアプローチは、本研究の「FDTD→FNOサロゲート」の設計思想と一致。
- **実装に使う場合の注意点**:
  - 3D voxelデータセットを扱っており、本研究の2D設定とは次元が異なる。3D→2Dへの縮約が必要。
  - 逆設計問題（adjoint gradient予測）であり、本研究の順伝播問題（field予測）とは目的が異なる。損失関数の設計はそのまま使えない。
  - 体積誘電率のみを入力としており、導電率(sigma)やsource位置の情報組み込みは要検討。
- **要確認事項**:
  - Stage-wise FNOの具体的な損失関数定義（コード公開があるか）
  - FNOの入力表現が本研究の3ch入力（eps_r, sigma, source_map）に直接適用可能か
  - データセットサイズと学習コストの規模感

---

## 2. Windowed Fourier Propagator: A Frequency-Local Neural Operator for Wave Equations in Inhomogeneous Media

- **Title**: Windowed Fourier Propagator: A Frequency-Local Neural Operator for Wave Equations in Inhomogeneous Media
- **Authors**: Yiyang Cai, Zixuan Qiu, Yunlu Shu, Jiamao Wu, Yingzhou Li, Tianyu Wang, Xi Chen
- **Year**: 2026
- **arXiv ID**: 2603.14289
- **URL**: https://arxiv.org/abs/2603.14289
- **PDF**: https://arxiv.org/pdf/2603.14289
- **Categories**: cs.LG, math.NA
- **なぜHigh Priorityか**:
  - 不均質媒質中の波動方程式をニューラルオペレータで扱う。本研究の「室内不均質環境（壁/家具/誘電体）でのWiFi電波伝播」と物理的に最も近い設定。
  - 周波数局所性（frequency locality）の物理原理に基づく設計。FNOのグローバルフーリエ変換の弱点（局所的な不連続性の扱いの難しさ）を克服する可能性。
  - 重ね合わせの明示的保存により、単純な訓練データ（平面波など）から複雑な波状態への一般化が可能。本研究でデータ生成コストを削減できる可能性。
- **本研究に使えそうな要素**:
  - Windowed Fourier Propagator (WFP) アーキテクチャ自体: 2D FNOの代替または改良版として適用可能。特にwall-shadow領域やsource-near領域の性能向上が期待される。
  - 周波数局所性の考え: 室内WiFi伝播では壁面や家具による散乱が局所的な高周波成分を生む。WFPの周波数ウィンドウ設計がこの問題に適合する可能性がある。
  - 訓練データの単純化: 平面波などの単純な入力から訓練し、複雑なsource配置に一般化するアプローチ。Meepシミュレーションの訓練データ数を減らせる可能性。
- **実装に使う場合の注意点**:
  - 時間依存波動方程式を対象にしており、本研究の定常状態（steady-state）Ezフィールドとは設定が異なる。時間次元の処理が必要。
  - 1D/2Dの波動方程式を扱っているが、本研究のHelmholtz方程式（定常状態の波動方程式）との厳密な対応関係は要検証。
  - WFPの周波数ウィンドウパラメータのチューニングが、本研究の2.4GHz帯域にどのように最適化されるかは不明。
- **要確認事項**:
  - WFPを定常状態のHelmholtz方程式に適用する際の具体的な変換方法
  - 周波数ウィンドウサイズと室内WiFi伝播の空間スケール（800x800グリッド）の適合性
  - コード公開の有無とPyTorch実装の詳細

---

## 3. Frequency Bias and OOD Generalization in Neural Operators under a Variable-Coefficient Wave Equation

- **Title**: Frequency Bias and OOD Generalization in Neural Operators under a Variable-Coefficient Wave Equation
- **Authors**: Runlong Xie, An Luo
- **Year**: 2026
- **arXiv ID**: 2605.12997
- **URL**: https://arxiv.org/abs/2605.12997
- **PDF**: https://arxiv.org/pdf/2605.12997
- **Categories**: cs.LG
- **なぜHigh Priorityか**:
  - 可変係数波動方程式におけるFNOとDeepONetの周波数バイアスとOOD一般化を系統的に分析。本研究の「FNOベースラインの限界理解」に直接寄与。
  - FNOが未見の高周波入力で急激な誤差増加を示すことを実証。本研究で問題になっているsource-near領域（高周波成分が強い）の誤差の根本原因の理解に役立つ。
  - 係数滑らかさの変化（material propertyの変化）に対する一般化性能の評価。本研究で異なる室内環境（異なるeps_r分布）へのFNO一般化の参考になる。
- **本研究に使えそうな要素**:
  - 周波数シフト実験の設計: 本研究のFNOベースラインがどの周波数帯域で性能劣化するかを同定する実験プロトコルとして使える。
  - FNO vs DeepONetの比較結果: 本研究の「2D FNOを置き換えるのではなく改善する」という方針において、DeepONetの特定の強み（周波数ロバスト性）をFNOに組み込むヒントになる。
  - 分布シフトの構造化評価手法: 本研究で「訓練用室内環境→テスト用室内環境」への一般化を定量的に評価するフレームワークとして転用可能。
- **実装に使う場合の注意点**:
  - 1D波動方程式での分析であり、本研究の2D設定への拡張は自明ではない。
  - 診断的な分析論文であり、新しいアーキテクチャを提案するものではない。本研究のFNO改善に直接適用できる具体的な手法は限定的。
  - DeepONetの周波数ロバスト性のメカニズムが、本研究のFNO実装にどのように取り込めるかは要検討。
- **要確認事項**:
  - 周波数シフト実験の具体的な実装（入力周波数の変化をどのように制御しているか）
  - FNOの周波数カットオフパラメータと性能劣化の定量的関係
  - 2D/3D設定での再現性検証の有無

---

---

## 候補論文 4: Accelerated Time-Domain Simulation of Complex Photonic Structures with a Data-Aware Fourier Neural Operator

- **Title**: Accelerated Time-Domain Simulation of Complex Photonic Structures with a Data-Aware Fourier Neural Operator
- **Authors**: Zaifan Wu, Yue You, Xian Zhou, Fan Zhang
- **Year**: 2025
- **arXiv ID**: 2508.17238
- **URL**: https://arxiv.org/abs/2508.17238
- **使用手法**: Data-Aware Fourier Neural Operator (DA-FNO) — FDTDの代替として時間領域電磁界シミュレーションを加速。FNOにデータ認識性を組み込み、複雑な光子構造の電磁界進化を学習。
- **baseline**: 従来のFDTD (Finite-Difference Time-Domain) 数値解法
- **評価指標**: 電磁界シミュレーションの精度、計算加速比
- **精度向上**: 要本文確認 — FNOベースでFDTDを加速したと記載されているが、具体的な数値は本文確認が必要
- **本研究への使い道**:
  - FNOをFDTD代替として電磁界シミュレーションに適用した先行研究として最も直接的。WiFi伝播予測のFNOベースsurrogateモデル構築の参考になる。
  - Data-Awareな設計: 入力データ特性に応じてFNOの振る舞いを調整する手法は、本研究の「室内環境固有の伝播特性を学習する」という方針と一致。
  - 時間領域シミュレーションへのFNO適用: WiFi伝播を周波数領域だけでなく時間領域でもモデル化する際の参考になる。
- **現在データ形式で使えるか**: 要確認 — 光子構造のシミュレーションなので、WiFi伝播の2D/3D空間データへの直接適用は自明ではないが、FNOのアーキテクチャ設計の参考にはなる。
- **実装コスト**: 中程度 — FNOの基本的な実装は既存だが、Data-Awareな拡張部分の実装が必要。
- **注意点**:
  - 光子デバイス（光学）が対象であり、無線通信のWiFi伝播とは物理スケールが異なる。
  - 時間領域シミュレーションに焦点があり、本研究の周波数領域アプローチとの整合性を確認する必要がある。
- **要確認事項**:
  - Data-Awareな設計の具体的な実装方法
  - FDTDとの定量的比較結果（加速比、精度）
  - 複雑な幾何学形状への一般化性能
  - WiFi帯域（2.4GHz/5GHz）の電磁界シミュレーションへの適用可能性

---

## 候補論文 6: MscaleFNO: Multi-scale Fourier Neural Operator Learning for Oscillatory Function Spaces

- **Title**: MscaleFNO: Multi-scale Fourier Neural Operator Learning for Oscillatory Function Spaces
- **Authors**: Zhilin You, Zhenli Xu, Wei Cai
- **Year**: 2024
- **arXiv ID**: 2412.20183
- **URL**: https://arxiv.org/abs/2412.20183
- **使用手法**: Multi-scale Fourier Neural Operator (MscaleFNO) — 高周波数成分の学習におけるFNOのスペクトルバイアスを低減。並列のFNOに異なるスケールの入力を与え、その出力を組み合わせることで、ヘルムホルツ方程式の解の高周波数成分を捉える。
- **baseline**: 標準FNO、他のNeural Operator手法
- **評価指標**: ヘルムホルツ方程式の解の精度、高周波数成分の再現性
- **精度向上**: 要本文確認 — 標準FNOと比較して高周波数成分の学習精度が改善したと記載されているが、具体的な数値は本文確認が必要
- **本研究への使い道**:
  - ヘルムホルツ方程式を直接対象としたFNOの研究として極めて重要。WiFi伝播はヘルムホルツ方程式で記述されるため、FNOの周波数特性改善手法は直接的に参考になる。
  - スペクトルバイアスの低減: FNOが低周波数成分を優先して学習する問題は、WiFi伝播の近接領域効果やマルチパス干渉のような高周波数空間変動を捉える上でボトルネックになる。MscaleFNOのアプローチはこの問題に直接取り組んでいる。
  - 非線形マッピングの学習: ヘルムホルツ方程式の係数から解への非線形マッピングをFNOで学習する手法は、本研究の「環境配置→伝播場」マッピングと概念的に一致。
- **現在データ形式で使えるか**: 要確認 — ヘルムホルツ方程式の数値解が対象なので、WiFi伝播の測定データへの直接適用は自明ではないが、FNOのアーキテクチャ改善の参考にはなる。
- **実装コスト**: 中高 — 並列FNOのスケール設計と出力統合ロジックの実装が必要。本研究のFNO実装に統合するにはアーキテクチャの改変が必要。
- **注意点**:
  - ヘルムホルツ方程式の数値解が対象であり、実際のWiFi伝播測定データとの整合性を確認する必要がある。
  - 並列FNOの計算コストが増加するため、本研究のリアルタイム推論要件との兼ね合いを考慮する必要がある。
- **要確認事項**:
  - MscaleFNOの具体的なアーキテクチャ設計（スケール数、統合方法）
  - 標準FNOとの定量的比較結果
  - 計算コストの増加量
  - 2D/3D空間での検証結果
  - WiFi伝播のような実際の物理場への適用可能性

---

## Paper: Deep Neural Helmholtz Operators for 3D Elastic Wave Propagation and Inversion

| Field | Content |
|---|---|
| **Title** | Deep Neural Helmholtz Operators for 3D Elastic Wave Propagation and Inversion |
| **Authors** | Yuxuan Lin, Yihan Wang, Qiancheng Zhang, Shuhang Hu, Bin Wang |
| **Year** | 2023 |
| **arXiv ID** | 2311.09608 |
| **URL** | https://arxiv.org/abs/2311.09608 |
| **使用手法** | Deep Neural Helmholtz Operator (NHO) — 時間周波数領域のヘルムホルツ方程式を連続線形作用素として定式化し、深層ネットワークで学習。3D弾性波伝播の正解と逆問題の両方を処理。 |
| **baseline** | 従来の数値シミュレータ（有限要素法など） |
| **評価指標** | 波場予測の相対誤差、逆問題での媒質パラメータ復元精度 |
| **精度向上** | 要本文確認 — 従来の数値シミュレータと比較して1000倍以上の推論速度向上を主張しているが、精度の数値は本文確認が必要 |
| **本研究への使い道** |
  - ヘルムホルツ方程式をNeural Operatorで直接扱う最先端のアプローチ。WiFi伝播もヘルムホルツ方程式で記述されるため、概念的に極めて関連性が高い。
  - 3D波伝播の正解・逆問題の両方を統一フレームワークで処理する手法は、本研究の「環境→伝播場」マッピングの設計参考になる。
  - 連続線形作用素の定式化: 離散グリッドに依存しない汎用性の高い表現が可能。 |
| **現在データ形式で使えるか** | 要確認 — 弾性波が対象なので直接適用は難しいが、ヘルムホルツ方程式のNeural Operator定式化のアイデアは転用可能。 |
| **実装コスト** | 高 — 3D弾性波用のアーキテクチャをWiFi伝播用に再設計する必要あり。連続作用素の離散化方法の検討も必要。 |
| **注意点** |
  - 弾性波（固体中の波）が対象であり、電磁波（WiFi）との物理的差異を考慮する必要がある。
  - 逆問題も扱っているが、本研究の目的は主に正解（推論）なので、その部分に焦点を当てる。 |
| **要確認事項** |
  - NHOの具体的なアーキテクチャ（ネットワーク構造、損失関数）
  - 推論速度と精度のトレードオフ
  - 電磁波（スカラーヘルムホルツ）への適用可能性
  - 境界条件の扱い方
  - 本研究のデータ形式への適合性 |

---

## 10. A neural operator-based surrogate solver for free-form electromagnetic inverse design

| **Title** | A neural operator-based surrogate solver for free-form electromagnetic inverse design |
| **Authors** | Yannick Augenstein, Taavi Repan, Carsten Rockstuhl |
| **Year** | 2023 |
| **arXiv ID** | 2302.01934 |
| **URL** | https://arxiv.org/abs/2302.01934 |
| **使用手法** | modified Fourier Neural Operatorを3D electromagnetic scattering surrogate solverとして訓練し、free-form nanophotonic inverse designへ接続。 |
| **baseline** | 既存の電磁界サロゲート/数値ソルバとのデータ効率比較。詳細な比較対象と数値は要本文確認。 |
| **評価指標** | scattering field prediction error、data efficiency、inverse designでの最適化性能。詳細数値は要本文確認。 |
| **精度向上** | 要本文確認 — arXiv概要ではmodified FNOを電磁散乱問題のサロゲートとして実装し、既存手法とのデータ効率比較を行うとされている。 |
| **本研究への使い道** |
  - modified FNOを電磁散乱のfield solver surrogateに使う先行例として重要。
  - 入力材料分布から電磁場を予測する構造は、本研究のeps_r/sigma/source_mapからEzを予測する設定に近い。
  - ただし対象はnanophotonic inverse designなので、順伝播field prediction部分だけを参照する。 |
| **現在データ形式で使えるか** | 要確認 — 3D nanophotonic scattering向けなので、2D indoor WiFi Ez定常場へは入力チャネル、境界条件、source表現の再設計が必要。 |
| **実装コスト** | 中高 — modified FNOの構造と3D/2D差分を確認し、本研究の2D FNO baselineへ移植できる部分だけ検討する。 |
| **注意点** |
  - 逆設計応用が主目的であり、本研究の順伝播サロゲートとは評価目的が異なる。
  - ナノフォトニクスと室内WiFiではスケール、境界条件、材料分布が大きく異なる。 |
| **要確認事項** |
  - modified FNOの具体的な変更点
  - scattering field誤差とデータ効率の具体値
  - 本研究の2D Ez定常場設定との互換性
  - 実装コードの公開状況 |

---

## 11. PIC2O-Sim: A Physics-Inspired Causality-Aware Dynamic Convolutional Neural Operator for Ultra-Fast Photonic Device FDTD Simulation

| **Title** | PIC2O-Sim: A Physics-Inspired Causality-Aware Dynamic Convolutional Neural Operator for Ultra-Fast Photonic Device FDTD Simulation |
| **Authors** | Pingchuan Ma, Haoyu Yang, Zhengqi Gao, Duane S. Boning, Jiaqi Gu |
| **Year** | 2024 |
| **arXiv ID** | 2406.17810 |
| **URL** | https://arxiv.org/abs/2406.17810 |
| **使用手法** | 物理インスパイアされた因果性認識動的畳み込みニューラルオペレータ (PIC2O)。FDTDシミュレーションのサロゲートモデル。光デバイスの電磁場シミュレーションを高速化。動的畳み込み重みにより空間的に変化する媒質特性を適応的に処理。 |
| **baseline** | 従来のFDTDシミュレーション、標準的なニューラルオペレータ (FNOなど) |
| **評価指標** | 電磁場再構成精度、計算速度の改善率 (FDTD 대비何倍高速か)、メモリ使用量 |
| **精度向上** | arXiv概要では、state-of-the-art neural operatorsよりroll-out prediction errorを51.2%低減、パラメータ数を23.5倍削減、open-source FDTD solver比で300-600x高速化とされる。本文で条件と評価指標を要確認。 |
| **本研究への使い道** |
  - FDTDシミュレーションのサロゲートモデルとして直接関連。本研究の「Meep/FDTDで生成したデータをFNOで近似する」というアプローチと完全に一致。
  - 因果性認識の設計は、電磁波伝播の物理的制約をモデルに組み込む参考になる。
  - 動的畳み込みは、空間的に変化する媒質（壁/家具/空気）の適応的処理に使える可能性。 |
| **現在データ形式で使えるか** | 要確認 — 光デバイス（ナノスケール）の設定であり、本研究のWiFi伝播（メートルスケール）とのスケール差がある。ただし基本的な電磁場シミュレーションの枠組みは共通。 |
| **実装コスト** | 中高 — 動的畳み込みニューラルオペレータの実装が必要。因果性認識の設計も追加の実装コスト。 |
| **注意点** |
  - 光デバイスのナノスケール設定であり、本研究のWiFi伝播のメートルスケールとの適用性に注意が必要。
  - 動的畳み込みの計算コストが標準FNOより高くなる可能性。 |
| **要確認事項** |
  - PIC2Oの具体的なアーキテクチャと動的畳み込みの実装詳細
  - 因果性認識の具体的な方法（物理的制約の組み込み方）
  - FDTDペアデータセットの生成方法
  - 電磁場誤差の具体的な数値
  - 本研究の2D Ez定常場設定への適用可能性
  - 実装コードの公開状況 |


---

## 2026-03-26 | Physics-Informed Neural Operator for Electromagnetic Inverse Scattering Problems

- **Title**: Physics-Informed Neural Operator for Electromagnetic Inverse Scattering Problems
- **Authors**: Q. C. Dong, Zi-Xuan Su, Qing Huo Liu, Wen Chen, Zhizhang Chen
- **Year**: 2026
- **arXiv ID**: 2603.25404
- **URL**: https://arxiv.org/abs/2603.25404
- **Priority**: High
- **High判定理由**:
  - 対象物理: Electromagnetic inverse scattering (電磁波逆散乱問題)
  - 入力/出力field: 誘電率分布 → 誘導電流分布 (field prediction)
  - FNO/Neural Operatorとの関係: Physics-Informed Neural Operator (PINO) を用いた電磁波シミュレーション
  - 本研究のEz full-field予測への接続: 電磁場分布をneural operatorで予測するアプローチが直接関連。逆問題だが、forward mappingのアーキテクチャは参考になる
- **対象物理現象**: Electromagnetic inverse scattering (電磁波逆散乱)
- **入力/出力field**: 誘電率分布 (入力) → 誘導電流分布 (出力)
- **使用手法**: Physics-Informed Neural Operator (PINO), hybrid loss (state loss + data loss + TV regularization)
- **baseline**: 従来の逆散乱手法 (abstractから推測)
- **評価指標**: 再構成精度 (abstractから推測)
- **精度向上**: 要本文確認
- **本研究への使い道**: 電磁場予測におけるphysics-informed lossの設計参考。neural operatorのアーキテクチャ比較
- **現在のデータ形式で使えるか**: 要確認 (逆問題設定だが、forward mapping部分は転用可能か)
- **実装コスト**: Med (physics-informed lossの設計が必要)
- **要確認事項**:
  - forward mappingとしての性能
  - 2D/3D設定
  - sparse observation下での性能

---

## 2026-01-26 | Data-Efficient Electromagnetic Surrogate Solver Through Dissipative Relaxation Transfer Learning

- **Title**: Data-Efficient Electromagnetic Surrogate Solver Through Dissipative Relaxation Transfer Learning
- **Authors**: Sunghyun Nam, Chan Y. Park, Min Seok Jang
- **Year**: 2026
- **arXiv ID**: 2601.18235
- **URL**: https://arxiv.org/abs/2601.18235
- **Priority**: High
- **High判定理由**:
  - 対象物理: Electromagnetic simulations (電磁波シミュレーション)
  - 入力/出力field: 電磁場分布予測 (surrogate solver)
  - FNO/Neural Operatorとの関係: Neural network surrogate solver for electromagnetic simulations
  - 本研究のEz full-field予測への接続: 電磁波surrogate solverとして直接関連。resonant phenomenaの扱いがWiFi propagationで重要
- **対象物理現象**: Electromagnetic wave propagation with resonant phenomena (共振現象を含む電磁波伝搬)
- **入力/出力field**: 電磁場入力 → 電磁場分布 (surrogate solver)
- **使用手法**: Dissipative Relaxation Transfer Learning (DIRTL), transfer learning + data-efficient training
- **baseline**: 従来のneural network surrogate solvers
- **評価指標**: 予測精度、安定性 (abstractから推測)
- **精度向上**: 要本文確認
- **本研究への使い道**: 電磁波surrogate solverのdata-efficient training手法参考。resonant phenomenaの扱いがWiFi環境で重要
- **現在のデータ形式で使えるか**: 要確認 (surrogate solver設定だが、training手法は転用可能)
- **実装コスト**: Med (transfer learningの設定が必要)
- **要確認事項**:
  - 具体的なアーキテクチャ (FNOベースか)
  - 2D/3D設定
  - WiFi propagationへの適用可能性


---

## 2026 | Fast Reconstruction of Exact Maxwell Dynamics from Sparse Data

- **Title**: Fast Reconstruction of Exact Maxwell Dynamics from Sparse Data
- **Authors**: Dan DeGenaro, Xin Li, Obed Amo, Michael Pokojovy, Sarah Adel Bargal, Markus Lange‐Hegermann, Bogdan Raiţă
- **Year**: 2026
- **arXiv ID**: 2605.20514
- **DOI**: なし
- **URL**: https://arxiv.org/abs/2605.20514
- **Source API**: openalex
- **Priority**: High
- **High判定理由**:
  - 対象物理: title/abstract に electromagnetic / Maxwell / photonic / wireless / scattering 等の関連語が含まれる。
  - 入力/出力field: abstract上は field prediction / reconstruction / surrogate の文脈。詳細は要本文確認。
  - FNO/Neural Operatorとの関係: neural operator / surrogate / reconstruction 系候補として検索スコア条件を満たす。
  - 本研究のEz full-field予測への接続: 電磁場・伝搬場の高速推定または疎観測再構成の設計比較対象になりうる。
- **対象物理現象**: 要本文確認
- **入力/出力field**: 要本文確認
- **使用手法**: 要本文確認
- **baseline**: 要本文確認
- **評価指標**: 要本文確認
- **精度向上**: 要本文確認
- **本研究への使い道**: WiFi電波伝搬サロゲートの候補手法・比較対象・補助損失設計の参考。
- **現在のデータ形式で使えるか**: 要確認
- **実装コスト**: 要確認
- **要確認事項**:
  - 本文で手法がFNO/Neural Operator系か、または比較対象として妥当か確認
  - 2D Ez定常場への適用可能性
  - 入出力テンソル、境界条件、source表現
  - コード・データ公開状況
- **Abstract memo**: We introduce FLASH-MAX, a shallow, exact-by-construction neural network architecture for predicting homogeneous electromagnetic fields from sparse pointwise observations. Each hidden neuron represents a separate exact solution to Maxwell's equations, so that the network satisfies the governing equations symbolically by construction and can be trained end-to-end from sparse data within seconds. We prove a universal approximation result showing that this exact model class remains universal on arbitrary domains. FLASH-MAX reaches sub-1% relative validation error from about 1K sparse pointwise observations in seconds, all while maintaining a zero PDE residual, and keeps single-digit errors even ...


---

## 2026 | PointNeRT: A Physics Aware Neural Ray Tracing Surrogate for Propagation Channel Modeling

- **Title**: PointNeRT: A Physics Aware Neural Ray Tracing Surrogate for Propagation Channel Modeling
- **Authors**: Zhuoyin Li, Ruisi He, Mi Yang, Ziyi Qi, Zhengyu Zhang, Jiahui Han, Haoxiang Zhang, Bingcheng Liu
- **Year**: 2026
- **arXiv ID**: 2605.11828
- **DOI**: なし
- **URL**: https://arxiv.org/abs/2605.11828
- **Source API**: openalex
- **Priority**: High
- **High判定理由**:
  - 対象物理: title/abstract に electromagnetic / Maxwell / photonic / wireless / scattering 等の関連語が含まれる。
  - 入力/出力field: abstract上は field prediction / reconstruction / surrogate の文脈。詳細は要本文確認。
  - FNO/Neural Operatorとの関係: neural operator / surrogate / reconstruction 系候補として検索スコア条件を満たす。
  - 本研究のEz full-field予測への接続: 電磁場・伝搬場の高速推定または疎観測再構成の設計比較対象になりうる。
- **対象物理現象**: 要本文確認
- **入力/出力field**: 要本文確認
- **使用手法**: 要本文確認
- **baseline**: 要本文確認
- **評価指標**: 要本文確認
- **精度向上**: 要本文確認
- **本研究への使い道**: WiFi電波伝搬サロゲートの候補手法・比較対象・補助損失設計の参考。
- **現在のデータ形式で使えるか**: 要確認
- **実装コスト**: 要確認
- **要確認事項**:
  - 本文で手法がFNO/Neural Operator系か、または比較対象として妥当か確認
  - 2D Ez定常場への適用可能性
  - 入出力テンソル、境界条件、source表現
  - コード・データ公開状況
- **Abstract memo**: Ray tracing (RT) has emerged as a key tool for propagation channel modeling and network planning. Conventional RT is based on electromagnetic (EM) wave theory and its application relies on detailed mesh-based environment representations and material properties. In realistic environments, limited environmental geometry and material uncertainties hinder its scalability to complex scenarios. In this paper, we propose a novel physics aware neural RT surrogate named PointNeRT to address these limitations. The proposed model directly takes point clouds as environmental input, and efficiently reconstruct multipath without explicitly constructing mesh models or manually defining EM interaction rules...


---

## 2026-05-28 | Codex Research-Container Candidate Batch

研究用コンテナの進捗文脈から選定された 2024-2026 年の候補論文一覧。Hermes Reader/Verifier が順次読むためのキュー。

---

## 2025 | A Data-Aware Fourier Neural Operator for Modeling Spatiotemporal Electromagnetic Fields

- **Title**: A Data-Aware Fourier Neural Operator for Modeling Spatiotemporal Electromagnetic Fields
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2508.17238
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2508.17238
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: FDTD-like EM field evolution, adaptive spectral/mode design, Ez/Hx/Hy系に近く current FNO の直接改善候補。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | Physic-informed deep operator networks for modeling 2D time-domain electromagnetic wave propagation in various media

- **Title**: Physic-informed deep operator networks for modeling 2D time-domain electromagnetic wave propagation in various media
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/pii/S2589004226004517
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: 2D EM、source/material generalization、FDTD比較があり、source-conditioned operator の設計参考になる。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | Fourier Neural Operator as a Surrogate Model for 2D Electromagnetic FDTD Simulation

- **Title**: Fourier Neural Operator as a Surrogate Model for 2D Electromagnetic FDTD Simulation
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.clawrxiv.io/abs/2603.00351
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: 2D TM steady EM fieldを permittivity/source から予測するため、現行課題に最も形が近い。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | PIC2O-Sim: A Physics-Inspired Causality-Aware Dynamic Convolutional Neural Operator for Ultra-Fast Photonic Device FDTD Simulation

- **Title**: PIC2O-Sim: A Physics-Inspired Causality-Aware Dynamic Convolutional Neural Operator for Ultra-Fast Photonic Device FDTD Simulation
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2406.17810
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2406.17810
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: Maxwell/FDTD surrogateで、local causality と permittivity-dependent propagation が壁近傍誤差に効く可能性。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | NSNO: Neumann Series Neural Operator for Solving Helmholtz Equations in Inhomogeneous Medium

- **Title**: NSNO: Neumann Series Neural Operator for Solving Helmholtz Equations in Inhomogeneous Medium
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2401.13494
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2401.13494
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: inhomogeneity + source term -> solution の Helmholtz operator で、2.4GHz steady field に理論的に近い。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | MscaleFNO: Multi-scale Fourier Neural Operator Learning for Oscillatory Function Spaces

- **Title**: MscaleFNO: Multi-scale Fourier Neural Operator Learning for Oscillatory Function Spaces
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2412.20183
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2412.20183
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: FNO の高周波・oscillatory field 弱点を狙っており、干渉縞と max error 改善候補。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Learned frequency-domain scattered wavefield solutions using neural operators

- **Title**: Learned frequency-domain scattered wavefield solutions using neural operators
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2405.01272
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2405.01272
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: source/frequencyを埋め込む scattered wavefield NO で、source_map 条件付け改善に近い。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Multiple-input Fourier Neural Operator for source-dependent 3D elastodynamics

- **Title**: Multiple-input Fourier Neural Operator for source-dependent 3D elastodynamics
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/pii/S0021999125000968
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: source位置・向きを明示入力する FNO 系で、WiFi source_map 条件付けの公平な拡張に使える。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Enhancing Fourier Neural Operators with Local Spatial Features

- **Title**: Enhancing Fourier Neural Operators with Local Spatial Features
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2503.17797
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2503.17797
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: Conv-FNO型で局所特徴を足すだけなので、source-near/wall boundary の局所誤差に実装しやすい。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Neural Operators with Localized Integral and Differential Kernels

- **Title**: Neural Operators with Localized Integral and Differential Kernels
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2402.16845
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2402.16845
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: FNOの過平滑化を local kernel で補う設計で、壁影・source近傍の sharp error に直結する。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | LOGLO-FNO: Efficient Learning of Local and Global Features in Fourier Neural Operators

- **Title**: LOGLO-FNO: Efficient Learning of Local and Global Features in Fourier Neural Operators
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2504.04260
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2504.04260
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: local/global 分岐で FNO の高周波欠落を補うため、既存FNOの差分ablationがしやすい。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | FreqMoE: Dynamic Frequency Enhancement for Neural PDE Solvers

- **Title**: FreqMoE: Dynamic Frequency Enhancement for Neural PDE Solvers
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2505.06858
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2505.06858
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: low-frequency pretrain/high-frequency fine-tune は interference と max error 改善の候補。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | RecFNO: A resolution-invariant flow and heat field reconstruction method from sparse observations via Fourier neural operator

- **Title**: RecFNO: A resolution-invariant flow and heat field reconstruction method from sparse observations via Fourier neural operator
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/abs/pii/S1290072923004805
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: sparse/line observationを full-field に戻す構成で、router line/patch auxiliary の次の形に近い。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Active operator learning with predictive uncertainty quantification for partial differential equations

- **Title**: Active operator learning with predictive uncertainty quantification for partial differential equations
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2503.03178
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2503.03178
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: error/uncertaintyで追加Meep caseやregion weightを選ぶ設計に使える。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Data-Efficient Operator Learning via Unsupervised Pretraining and In-Context Learning

- **Title**: Data-Efficient Operator Learning via Unsupervised Pretraining and In-Context Learning
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2402.15734
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2402.15734
- **Source API**: research-container-codex
- **Priority**: High
- **Why candidate**: unlabeled geometry/source map の mask/super-resolution pretrain が高価なMeep教師節約に合う。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Amortized Fourier Neural Operators

- **Title**: Amortized Fourier Neural Operators
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://proceedings.neurips.cc/paper_files/paper/2024/hash/d06a797c436cd5136a6f45b063316278-Abstract-Conference.html
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: 周波数mode表現を改善し、現行 fno_modes=256 の高周波表現不足に効く可能性。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | MgFNO: Multi-grid Architecture Fourier Neural Operator for Parametric Partial Differential Equations

- **Title**: MgFNO: Multi-grid Architecture Fourier Neural Operator for Parametric Partial Differential Equations
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2407.08615
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2407.08615
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: 800x800高解像度で multi-grid/local-to-global 表現を試せる。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Gabor-Filtered Fourier Neural Operator for solving Partial Differential Equations

- **Title**: Gabor-Filtered Fourier Neural Operator for solving Partial Differential Equations
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/pii/S0045793024000719
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: Gabor/Fourier filtering は局所波束・高周波干渉を扱いやすい可能性。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Physics-constrained machine learning for electrodynamics without gauge ambiguity based on Fourier transformed Maxwell's equations

- **Title**: Physics-constrained machine learning for electrodynamics without gauge ambiguity based on Fourier transformed Maxwell's equations
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.nature.com/articles/s41598-024-65650-9
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: Maxwell制約を Fourier 表現で入れる方向で、完全PDE lossでなく補助制約として検討できる。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Physics-informed Neural Operators for Predicting 3D Electromagnetic Fields Transformed by Metasurfaces

- **Title**: Physics-informed Neural Operators for Predicting 3D Electromagnetic Fields Transformed by Metasurfaces
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2512.15694
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2512.15694
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: Maxwell系NOの物理制約付き設計で、EM field surrogate の近縁事例。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Physical knowledge improves prediction of EM Fields

- **Title**: Physical knowledge improves prediction of EM Fields
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2503.11703
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2503.11703
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: permittivity/conductivity/sourceに近い入力でEM場予測し、物理特徴追加の参考になる。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Wave Interpolation Neural Operator

- **Title**: Wave Interpolation Neural Operator
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2408.02971
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2408.02971
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: wavelength/frequency条件付けのNOで、将来2.4GHz以外へ拡張するなら有用。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | A boundary-based fourier neural operator method for efficient parametric acoustic wave analysis

- **Title**: A boundary-based fourier neural operator method for efficient parametric acoustic wave analysis
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://link.springer.com/article/10.1007/s00366-024-02103-x
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: boundary/wave問題向けFNOで、壁境界の扱いを強化する設計ヒントになる。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Deep neural Helmholtz operators for 3-D elastic wave propagation and inversion

- **Title**: Deep neural Helmholtz operators for 3-D elastic wave propagation and inversion
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://academic.oup.com/gji/article/239/3/1469/7760394
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: frequency-domain wave operatorの高速化で、Helmholtz的 steady WiFi場に近い。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Reducing Frequency Bias of Fourier Neural Operators in 3D Seismic Wavefield Simulations Through Multi-Stage Training

- **Title**: Reducing Frequency Bias of Fourier Neural Operators in 3D Seismic Wavefield Simulations Through Multi-Stage Training
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2503.02023
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2503.02023
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: FNOの低周波先行・高周波苦手問題を明示的に扱う。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Physics field super-resolution reconstruction via enhanced diffusion model and fourier neural operator

- **Title**: Physics field super-resolution reconstruction via enhanced diffusion model and fourier neural operator
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/pii/S2095034925000364
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: coarse-to-fine field reconstructionの候補で、低解像度Meep pretrain案に合う。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | 3-D full-field reconstruction of chemically reacting flow towards high-dimension conditions through machine learning

- **Title**: 3-D full-field reconstruction of chemically reacting flow towards high-dimension conditions through machine learning
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/pii/S1385894724079269
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: RSRNetの元論文で、feature loss/低次元補助の既存実験の妥当化に使える。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | Building Digital Twin for 3D Multi-Field Reconstruction and Optimization of Industrial-Scale Combustion Systems

- **Title**: Building Digital Twin for 3D Multi-Field Reconstruction and Optimization of Industrial-Scale Combustion Systems
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/pii/S2095809925004874
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: MFRNetの multi-fidelity/feature fusion が低解像度Meep→高解像度Ez残差学習に合う。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | Learning multi-physical system on a unified manifold by collaboratively fused features

- **Title**: Learning multi-physical system on a unified manifold by collaboratively fused features
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://www.sciencedirect.com/science/article/pii/S0952197626004598
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: CoFFe型の sparse/manifold pretraining は将来の少数観測・複数物理量化に使える。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | GLU: Global-Local-Uncertainty Fusion for Scalable Spatiotemporal Reconstruction and Forecasting

- **Title**: GLU: Global-Local-Uncertainty Fusion for Scalable Spatiotemporal Reconstruction and Forecasting
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: 2603.26023
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2603.26023
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: global/local/uncertainty weighting は現行 region-weighted loss の発展形として使える。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | MTL-FNO: A Lightweight Multi-Task Fourier Neural Operator for Sparse Field Reconstruction

- **Title**: MTL-FNO: A Lightweight Multi-Task Fourier Neural Operator for Sparse Field Reconstruction
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: 2605.26718
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2605.26718
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: sparse field reconstruction と multi-task shared FNO が、region別 auxiliary head に近い。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | iRadioDiff: Physics-Informed Diffusion Model for Indoor Radio Map Construction and Localization

- **Title**: iRadioDiff: Physics-Informed Diffusion Model for Indoor Radio Map Construction and Localization
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2511.20015
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2511.20015
- **Source API**: research-container-codex
- **Priority**: Medium
- **Why candidate**: indoor RF、material係数、diffraction/LoS/boundary-weighted objective が壁影誤差に参考。ただしpathloss寄り。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | Physically Accurate Differentiable Inverse Rendering for Radio Frequency Digital Twin

- **Title**: Physically Accurate Differentiable Inverse Rendering for Radio Frequency Digital Twin
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: 2603.18026
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2603.18026
- **Source API**: research-container-codex
- **Priority**: Low
- **Why candidate**: RF反射/回折境界を連続化する考えは、wall-shadow region設計の参考になる。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2026 | oneTwin: Online Digital Network Twin via Neural Radio Radiance Field

- **Title**: oneTwin: Online Digital Network Twin via Neural Radio Radiance Field
- **Authors**: 要確認
- **Year**: 2026
- **arXiv ID**: 2601.03216
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2601.03216
- **Source API**: research-container-codex
- **Priority**: Low
- **Why candidate**: radio digital twin と material tuning は近いが、出力は物理層metric寄りで直接比較は弱い。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2024 | Physics-informed generative neural networks for RF propagation prediction with application to indoor body perception

- **Title**: Physics-informed generative neural networks for RF propagation prediction with application to indoor body perception
- **Authors**: 要確認
- **Year**: 2024
- **arXiv ID**: 2405.02131
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2405.02131
- **Source API**: research-container-codex
- **Priority**: Low
- **Why candidate**: indoor RF/EM diffractionを生成モデルに入れる例だが、FNO/全場Ez教師とは距離がある。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | TransPathNet: A Novel Two-Stage Framework for Indoor Radio Map Prediction

- **Title**: TransPathNet: A Novel Two-Stage Framework for Indoor Radio Map Prediction
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2501.16023
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2501.16023
- **Source API**: research-container-codex
- **Priority**: Low
- **Why candidate**: indoor geometry/source conditioned map refinementは参考になるが、pathloss-onlyなので低優先。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | IPP-Net: A Generalizable Deep Neural Network Model for Indoor Pathloss Radio Map Prediction

- **Title**: IPP-Net: A Generalizable Deep Neural Network Model for Indoor Pathloss Radio Map Prediction
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: 2501.06414
- **DOI**: 要確認
- **URL**: https://arxiv.org/abs/2501.06414
- **Source API**: research-container-codex
- **Priority**: Low
- **Why candidate**: indoor radio mapの公平比較設計・augmentation参考用、ただしEz/FDTD surrogateではない。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics

---

## 2025 | Sparse-Guided RadioUNet with Adaptive Sampling for the MLSP 2025 Sampling-Assisted Pathloss Radio Map Prediction Data Competition

- **Title**: Sparse-Guided RadioUNet with Adaptive Sampling for the MLSP 2025 Sampling-Assisted Pathloss Radio Map Prediction Data Competition
- **Authors**: 要確認
- **Year**: 2025
- **arXiv ID**: なし
- **DOI**: 要確認
- **URL**: https://openreview.net/forum?id=bsK6nGqzsW
- **Source API**: research-container-codex
- **Priority**: Low
- **Why candidate**: sparse sample + map conditioningは router patch/line 拡張の参考になるが pathloss対象。
- **要確認事項**: 概要・本文・コード・入力出力・baseline・metrics
