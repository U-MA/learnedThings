## include/util/static.h

```C++
#define MGPU_DIV_UP(x, y) (((x) + (y) - 1) / (y))
```

マクロ名から推測できるように, x / y の切り上げ  
