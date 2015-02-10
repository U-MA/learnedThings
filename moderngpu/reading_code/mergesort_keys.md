## MergesortKeys: include/kernels/mergesort.cuh

```C++
// MergesortKeys sorts data_global using comparator Comp.
// It !comp(b, a), then a comes before b in the output. The data is sorted
// in-place.
template<typename T, typename Comp>
MGPU_HOST void
MergesortKeys(T* data_global, int count, Comp comp, CudaContext& context);
```

動作としては  
  1. KernelBlocksortで各CTAをソート済みにする  
  2. グローバルメモリ全体の要素がソート済みになるまでマージソートを繰り返す 
