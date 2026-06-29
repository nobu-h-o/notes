# [Pearl: Automatic Code Optimization Using Deep Reinforcement Learning](https://hpcrl.github.io/ICS2025-webpage/program/Proceedings_ICS25/ics25-27.pdf)メモ

## 問題設定
- コンパイラの auto-scheduling（どのループ変換を・どの順で・どのループに・どのパラメータで適用するか）の探索空間が巨大（約 10^170 候補）
  - 6種の変換とそのパラメータ（tile size など）の組み合わせ＋適用するループの選択を掛け合わせた、loop-optimization sequence の候補数の見積もり
- 従来の tree-search（Tiramisu autoscheduler の beam search + cost model）は、巨大な探索空間に対し全探索が非現実的なため、探索順序を固定したり変換を一度しか試さなかったりして空間を制限 → その制限ゆえに最適ではない
- 既存の RL 手法にはそれぞれ穴がある：
  - HalideRL — semi-automatic で未知プログラムに汎化しない
  - PolyGym / CompilerGym — environment だけで agent なし
  - AutoPhase — phase ordering（部分問題）のみ
## 貢献
- Pearl は「一般のループネストを最適化でき」「未知プログラムに汎化し」「polyhedral（affine）変換をサポートする」初の RL agent だと主張
- Action Spaceの表現：
  - AST を branch ごとに左→右で辿り、「Next」アクションでループネストの「どの部分」を変換するか選べる
  - これで一般ループネストに対応（計 56 アクション、変換は6種：parallelization, unrolling, tiling, skewing, interchange, reversal）
## 手法
- State = ASTをグラフ化
- Reward = speedupのlog
- Tiramisu Compiler上で実装
## 結果
- 未最適化比で geo-mean 3.16×、Tiramisu autoscheduler 比 2.02×、Pluto 比 3.36×
- slowdown を一度も出さなかった（全 speedup ≥ 1）
- 推論は平均 33.36ms = tree search より 563× 速い（policy network が直接予測するため）

## 今週
- Polyhedral model、強化学習について勉強
- LOOPer: A Learned Automatic Code Optimizer For Polyhedral Compilersを読む
