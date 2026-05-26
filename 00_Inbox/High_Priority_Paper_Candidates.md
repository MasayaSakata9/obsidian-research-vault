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
