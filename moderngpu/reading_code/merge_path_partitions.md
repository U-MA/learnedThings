## MergePathPartitions: include/kernels/search.cuh

```C++
template<MgpuBounds Bounds, typename It1, typename It2, typename Comp>
MGPU_MEM(int)
MergePathPartitions(It1 a_global, int aCount, It2 b_global, int bCount,
                    int nv, int coop, Comp comp, CudaContext& context);
```

いくつかの変数を設定した後で, 処理を`KernelMergePartition`に移している  
処理が戻ってきたら`partitionDevice`を返している  
