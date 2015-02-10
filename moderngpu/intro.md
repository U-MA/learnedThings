## Introduction

MGPUは問題を二つの部分に分割する
  1. 各スレッドに正確に負荷分散するように問題の粗粒度分割を見つける  
     スケジューリング - 作業項目をGPU上のCTAやスレッドに割り当てる処理 - はこのステップで扱われる  
  2. 問題を解くwork-efficientなsequential logicを実行する  
     スケジューリングはステップ1の部分なので, このステップは並列に実行される  

このライブラリのパフォーマンスチューニングは単純で理解しやすい 
各スレッドはVT(values per thread)個の要素しか処理しない  
この値を増やすと, 分割のコストが償却される  
これはwork-efficiencyを向上させるが, 並列性とレイテンシ隠蔽の性能を減らす  

MGPUライブラリで使われている分割関数の例は, `Merge Path`, `Balanced Path`, そして`load-balancing search`  

