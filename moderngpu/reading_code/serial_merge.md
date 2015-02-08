## SerialMerge: device/ctamerge.cuh  

```C++
template<int VT, bool RangeCheck, typename T, typename Comp>
MGPU_DEVICE void
SerialMerge(const T* keys_shared, int aBegin, int aEnd,
            int bBegin, int bEnd, T* result, int* indices, Comp comp);
```

普通のマージソート  

keys_sharedに２つのソート済みの配列が格納されていることを仮定  
それぞれのソート済み配列はkeys_sharedの[aBegin, aEnd), [bBegin, bEnd)に  
格納されている  

２つの配列をマージした結果はresultに格納される 
indicesは, sharedメモリ上のキーのindexを格納する  
