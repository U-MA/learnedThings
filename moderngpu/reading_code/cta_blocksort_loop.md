## CTABlocksortLoop: include/device/ctamerge.cuh

```C++
template<int NT, int VT, bool HasValue, typename KeyType,
         typename ValType, typename Comp>
MGPU_DEVICE void
CTABlocksortLoop(ValType threadValues[VT], KeyType* keys_shared, ValType* values_shared, 
                 int tid, int count, Comp comp);
```

keys_sharedはthread orderでソート済み  
すなわち, keys_shared[0] ~ keys_shared[VT-1], keys_shared[VT] ~ ...はそれぞれソート済み

内容としては次の通り  

for (coop = 2; coop <= NT; coop *=2)  
  1. CTABlocksortPassを呼び出し, keys_sharedに入っている要素をマージソートし, keysにその結果を格納  
  2. HasValueがtrueの場合の処理(省略)  
  3. keysの内容をkeys_sharedに格納  

for文に入る前は, CTAの各行がソートされている状態  
for文の処理を１回終えると, CTAの2行ごとにソートされている状態になる  
for文の処理を２回終えると, CTAの4行ごとにソートされている状態になる  
これをlog(NT)回繰り返す
