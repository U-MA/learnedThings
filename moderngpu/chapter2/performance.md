# Chapter 2 Performance

## Ocupancy and latency

レイテンシ志向のシステム(CPU)は大きいキャッシュ, 分岐予測, 推測的なフェッチを使用  
一方GPUはレイテンシを隠蔽するために大規模な並列性を使用する処理能力志向  
  
*Occupancy*はCUDAプログラムにおいてスレッド並列の一つの指標  
*Instruction-level Parallelism*はスレッド内の並列の一つの指標  
  
occupancyやILPが高ければ高いほど, SMは演算ユニットやload/storeユニットを置かななければならない可能性が増える  
データ依存を待つスレッドやバリアは, それらの危険が解決するまで考慮される  
  
latency-limitedのカーネルのパフォーマンスは推測するのが難しい
  
occupancyの上限を設けるリソース制限が5つある
* Max Threads
* Max CTAs
* Shared Memory Capacity
* Register File Capacity
* Max Registers

##Getting more performance from MGPU

ほとんどのMGPUカーネルはNT(CTA当たりのスレッド数)とVT(スレッド当たりの値)でパラメータ化されている  
これら二つの積, NV(CTA当たりの値数)はタイルのサイズである  
タイルサイズを増やすことはスレッド当たりの１回の操作とCTA当たりの一回の操作のコストを返済し  
仕事効率を向上する  
一方, VTを増やすことはsharedメモリやレジスタをより消費し, occupancyやレイテンシ隠蔽の能力を下げる  
  
定数NT, VT, __launch_bounds__引数のOCC(SM当たりのCTAの最小値)はチューニングパラメータである  
様々なハードウェア, データ型, 入力サイズ, ディストリビューション, コンパイラバージョン, これら全てが  
パラメータの選択に影響する  
MGPUライブラリ関数のユーザーチューニングはライブラリのハードコードされた標準に対して  
50%くらい処理能力を改良するかもしれない  
  
VTが大きくなるにつれて並列性(occupancy)は下がり, 仕事効率は上がる  
