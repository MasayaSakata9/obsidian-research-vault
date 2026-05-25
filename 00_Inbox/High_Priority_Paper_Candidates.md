# High Priority Paper Candidates

> 最終更新: 2026-05-25
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

## 候補論文 5: Learning Function-to-Function Mappings: A Fourier Neural Operator for Next-Generation MIMO Systems

- **Title**: Learning Function-to-Function Mappings: A Fourier Neural Operator for Next-Generation MIMO Systems
- **Authors**: Jian Xiao, Ji Wang, Qi Sun, Qimei Cui, Xingwang Li
- **Year**: 2025
- **arXiv ID**: 2510.04664
- **URL**: https://arxiv.org/abs/2510.04664
- **使用手法**: Fourier Neural Operator (FNO) を次世代MIMOシステムに適用。超大規模アンテナ配列、ホログラフィックサーフェス、近接領域伝播の物理層信号処理をFNOでモデル化。
- **baseline**: 従来のMIMO信号処理手法、物理ベースの伝播モデル
- **評価指標**: 信号処理精度、スペクトル効率、計算効率
- **精度向上**: 要本文確認 — FNOが従来のMIMO信号処理を改善したと記載されているが、具体的な数値は本文確認が必要
- **本研究への使い道**:
  - FNOを無線通信システムに適用した研究として直接的。WiFi伝播予測のFNOベースsurrogateモデルの設計参考になる。
  - 近接領域伝播（near-field propagation）のモデル化: 室内WiFi環境でアンテナが近い場合の伝播特性をFNOで捉える手法の参考になる。
  - 連続アパーチャモデル化: 空間的に連続的な伝播場をFNOで表現するアプローチは、本研究の2D空間データへの適用と一致。
- **現在データ形式で使えるか**: 要確認 — MIMOシステムが対象なので、WiFi伝播のRSSI/電界強度データへの直接適用は自明ではないが、FNOのアーキテクチャ設計の参考にはなる。
- **実装コスト**: 中程度 — FNOの基本的な実装は既存だが、MIMO特有の入力表現（アンテナ配列、チャネル状態）をWiFi伝播データにどのように対応付けるかは要検討。
- **注意点**:
  - MIMO信号処理が対象であり、WiFi伝播予測とは目的が異なる。
  - 超大規模アンテナ配列の設定なので、本研究の室内WiFi環境とのスケール違いを考慮する必要がある。
- **要確認事項**:
  - FNOの入力表現（どのように伝播場を表現しているか）
  - 近接領域伝播モデルの具体的な実装
  - 従来のMIMO手法との定量的比較結果
  - WiFi帯域での検証の有無

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
