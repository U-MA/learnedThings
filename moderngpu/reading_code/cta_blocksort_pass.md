## CTABlockSortPass: device/ctamerge.cuh

```C++
template<int NT, int VT, typename T, typename Comp>
MGPU_DEVICE void
CTABlocksortPass(T* keys_shared, int tid, int count,
                 int coop, T* keys, int* indices, Comp comp);
```

マージされたキーとindicesを返す  

(coop - 1) & tid  == tid % coop  
~(coop - 1) & tid == coop * (tid / coop)

MergePathを計算し, それを利用し二つの配列をマージ  
その前にいくつかの変数を計算する  
その部分がわかりづらいので, ModernGPUにコメントされている例を使用して, 考える  

NT = 8, VT = 7, count = 49でcoop = 2のときを考える  
このとき, tid = 0とすると   
```C++
list = ~(coop - 1) & tid  
     = ~(2 - 1) & 0  
     = ~1 & 0
     = 0

diag = min(count, VT * ((coop - 1) & tid))  
     = min(49, 7 * ((2 - 1) & 0))  
     = min(49, 7 * (1 & 0))  
     = min(49, 7 * 0)  
     = min(49, 0)  
     = 0  

start = VT * list  
      = 7 * 0
      = 0  

a0 = min(count, start)  
   = min(49, 0)  
   = 0  

b0 = min(count, start + VT * (coop / 2))  
   = min(49, 0 + 7 * (2 / 2))  
   = min(49, 7)  
   = 7  

b1 = min(count, start + VT * coop)  
   = min(49, 0 + 7 * 2)  
   = min(49, 14)  
   = 14  
```

となる  
同様の計算を続けていくと次の表のようになる

tid | coop = 2                           |
--- | ---------------------------------- |
0   | A: [ 0,  7)   B: [ 7, 14) diag = 0 | 
1   | A: [ 0,  7)   B: [ 7, 14) diag = 7 |
2   | A: [14, 21)   B: [21, 28) diag = 0 |
3   | 以下略                             |

これら計算した値(a0, b0, b1, diag)を用いて, MergePathを求め, SerialMergeを行う
