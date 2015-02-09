## CTABlockSortPass: device/ctamerge.cuh

```C++
template<int NT, int VT, typename T, typename Comp>
MGPU_DEVICE void
CTABlocksortPass(T* keys_shared, int tid, int count,
                 int coop, T* keys, int* indices, Comp comp);
```

マージされたキーとindicesを返す  

MergePathを計算し, それを利用し二つの配列をマージ  
その前にいくつかの変数を計算する  
その部分がわかりづらいので, ModernGPUにコメントされている例を使用して, 考える  

NT = 8, VT = 7, count = 49

tid | coop = 2 |
--- | -------- |
0 | A: [0, 7) B: [7, 14) diag = 0| 
