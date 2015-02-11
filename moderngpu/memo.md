## Memo

ModernGPUは, work-efficiencyを改善し同期を減らすためにregister blockingを使用している  
1スレッドに割り当てられる要素数はVTとして呼ばれるgrain sizeである  

この値を増やすと, 1スレッド当たりの処理がより逐次的になり, work-efficiencyを向上させるが  
1スレッド当たりの状態が増え, occupancyとレイテンシ隠蔽の能力を減らす  

この値を減らすと, 1スレッド当たりの処理の逐次性が減り, work-efficiencyを減らすが  
1スレッド当たりの状態が減り, occupancyとレイテンシ隠蔽の能力が増える
