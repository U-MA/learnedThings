## CTAMergesort: include/device/ctamerge.cuh

```C++
template<int NT, int VT, bool HasValue, typename KeyType,
         typename ValType, typename Comp>
MGPU_DEVICE void
CTAMergesort(KeyType threadKeys[VT], ValType threadValues[VT],
             KeyType* keys_shared, ValType* values_shared, int count,
             int tid, Comp comp);
```

ブロックレベルのマージソート  
キーだけをソートしたい場合は, HasValueをfalseに, ValTypeをintに設定しておく  

この関数の内容は次の通り  
  1. スレッド番号tidのレジスタに格納されている要素をOddEvenTransposeSortでソートしておく  
  2. ソートしたキーをレジスタからsharedメモリに移す  
    * NT個ある各スレッドが責任を持つVT個の要素がソート済みにされてkeys_sharedに格納される  
  3. DeviceBlocksortLoopでCTA内の要素をソートする  
