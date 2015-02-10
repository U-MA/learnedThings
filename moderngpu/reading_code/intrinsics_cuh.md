## include/device/intrinsics.cuh

```C++
// Find log2(x) and optionally round up to the next integer logarithm.
MGPU_HOST_DEVICE int
FindLog2(int x, bool roundUp = false);
```

コメントの通り, log2(x)を求める関数  
roundUp = trueのとき, 整数にまで丸めてくれる  
すなわち, log2(10) = 3.321...なので, FindLog2(10, true) = 4ということだと思われる
