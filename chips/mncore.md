# MN-Core

MN-Coreは、Preferred Networksが開発している、ニューラルネット向けの半導体チップ。

## 技術的特徴

- 行列演算に特化: ディープラーニングに不可欠な行列演算を高速かつ効率的に処理するよう設計されています。
- 高効率な計算能力: 従来のGPUやCPUを上回る計算効率を実現し、AIモデルのトレーニング時間を大幅に短縮します。
- エネルギー効率: 低消費電力での運用が可能で、環境負荷の低減に貢献します。
- Fully-deterministic Architecture: ソフトウェアによるハードウェア資源の細粒度な制御を実現し、高いピーク性能と電力あたりの性能を達成します。

## アーキテクチャの特徴

- 同期的動作: すべてのProcessing Element（PE）が完全に同期して動作し、ホストCPUから直接命令を受け取ります。
- 高密度MAU: Matrix Arithmetic Unit（MAU）と呼ばれる行列演算器を高密度に実装しています。
- 64bitx4 SIMD: PE命令、MAU命令は、４サイクルの間、同一命令を実行(アドレスはインクリメント可)。実質的に256bit SIMD。半精度（16ビット）の浮動小数点演算であれば、１命令で16個の計算を並列して行える（4クロック）。
- 独立したメモリ空間：それぞれのPEが独立したメモリ（Static Memory）を持つ（グローバルメモリへのアクセスは出来ない）
- MAB: PE(18KB SRAM) x 4 + MAU
- L1B: MAB x 16 + L1B Memory (64KB) + L1B Reduction Unit
- L2B: L1B x 8 + L2B Memory (1MB) + L2B Reduction Unit
- Chip: L2B x 8 + DRAM (16GB)
- 階層的メモリ構造: PE(SRAM:144MB, 18KB x 8,048)、L1(SRAM:4MB, 64KB x 64)、L2(SRAM:8MB, 1MB x 8)、グローバルメモリ(DRAM:16GB)という階層的なメモリ構造を採用し、その間の移動は、ソフトウェアで明示的に指示を出して行う（キャッシュの代わり。Deterministic）。
- PEでは、2長語（128bits、8bytes）でのアクセスが可能
- GRF0/GRF1: 2KB(256 x 8B), 32B/cycle, 1 read and 1 write / cycle、書き込み後に同一アドレス読めるようになるまで２サイクル必要
- LM0/LM1: 16KB(2 x 8B), 16B/clcle, read or write / cycle、書き込み後に同一アドレス読めるようになるまで２サイクル必要
- T register: 書き込み後に読めるようになるまで１サイクル必要
- Forwarding Path: 前の命令の結果をそのまま使える機能
- Near memory computing: 
- 演算とデータ転送のオーバーラップ
- VLIW: PE命令/MAU命令/L1B転送命令/L2B転送命令/DRAM転送命令

- マスクレジスタ($omr...): 出力専用オペラんど
(MN-Core勉強会 - Preferred Networks)[https://www.youtube.com/live/U4HjE2S8wAY?si=NZSawy77L-WFhr1A]

[MN-CoreTM 2 ソフトウェア開発者マニュアル](https://projects.preferred.jp/mn-core/assets/mncore2_dev_manual_ja.pdf)
[MN-Core Challenge #1 で優勝しました 【問題解説付】](https://primenumber.hatenadiary.jp/entry/2024/09/26/234205)
[MN-Core Challenge #1 Writeup](https://qiita.com/logicmachine/items/3028ac2bcac03f31c166)
[MN-Core のデータ転送系命令の可視化に関する取り組み](https://tech.preferred.jp/ja/blog/mn-core-visualization/)


- L1BM → L2BM 転送命令から、重複する L2BM の領域を読み出す MV 命令まではデータ競合のために 1
ステップ空ける必要がある。
- L1BM → L2BM 転送命令から L2BM → L1BM 転送命令まではポート衝突のため 3 ステップ空ける必要
がある。

