## KernelMerge: include/kernels/merge.cuh

```C++
template<typename Tuning, bool HasValues, bool LoadExtended,
         typename KeysIt1, typename KeysIt2, typename KeysIt3,
         typename ValsIt1, typename ValsIt2, typename ValsIt3, typename Comp>
MGPU_LAUNCH_BOUNDS void
KernelMerge(KeysIt1 aKeys_global, ValsIt1 aVals_global, int aCount,
            KeysIt2 bKeys_global, ValsIt2 bVals_global, int bCount,
	          const int* mp_global, int coop,
            KeysIt3 keys_global, ValsIt3 vals_global, Comp comp);
```

この関数の処理は主に２つ  
  1. ComputeMergeRangeの呼び出し  
  2. DeviceMergeの呼び出し  

それぞれの関数の動きを見ていく  

## ComputeMergeRange: include/device/ctamerge.cuh

```C++
MGPU_HOST_DEVICE int4
ComputeMergeRange(int aCount, int bCount, int block,
                  int coop, int NV, const int* mp_global);
```

## FindMergesortInterval: include/

```C++
// Returns (a0, a1, b0, b1) into mergesort input lists between mp0 and mp1.
MGPU_HOST_DEVICE int4
FindMergesortInterval(int3 frame, int coop, int block,
	                    int nv, int count, int mp0, int mp1);
```

mp0とmp1が作る矩形のインターバルを計算してくれる関数  

## DeviceMerge: include/device/ctamerge.cuh

```C++
// Merge pairs from global memory into global memory. Useful factorization to
// enable calling from merge, mergesort, and locality sort.

template<int NT, int VT, bool HasValues, bool LoadExtended,
         typename KeysIt1, typename KeysIt2, typename KeysIt3,
         typename ValsIt1, typename ValsIt2, typename KeyType,
         typename ValsIt3, typename Comp>
MGPU_DEVICE void
DeviceMerge(KeysIt1 aKeys_global, ValsIt1 aVals_global, int aCount,
            KeysIt2 bKeys_global, ValsIt2 bVals_global, int bCount, 
	          int tid, int block, int4 range, KeyType* keys_shared, int* indices_shared,
	          KeysIt3 keys_global, ValsIt3 vals_global, Comp comp);
```
