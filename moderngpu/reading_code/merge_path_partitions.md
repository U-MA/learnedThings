## MergePathPartitions: include/kernels/search.cuh

```C++
template<MgpuBounds Bounds, typename It1, typename It2, typename Comp>
MGPU_MEM(int)
MergePathPartitions(It1 a_global, int aCount, It2 b_global, int bCount,
                    int nv, int coop, Comp comp, CudaContext& context);
```

いくつかの変数を設定した後で, 処理を`KernelMergePartition`に移している  
処理が戻ってきたら`partitionDevice`を返している  

## KernelMergePartition: include/kernels/search.cuh

```C++
template<int NT, MgpuBounds Bounds, typename It1, typename It2, typename Comp>
__global__ void
KernelMergePartition(It1 a_global, int aCount, It2 b_global, int bCount,
                     int nv, int coop, int* mp_global, int numSearches, Comp comp);
```

MergePathを計算する関数  
結果はmp_globalで返される
