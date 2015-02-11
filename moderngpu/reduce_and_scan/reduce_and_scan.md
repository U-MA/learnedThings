# Reduce and Scan

## Reduce

Reduceは入力列の要素の和  

```
Input: [ 1, 7, 4, 0, 9, 4 ] => Output: 25
```

## Scan

Scanは入力列の初めから現在の要素までをReduce処理する  
Scanには`Exclusive Scan`と`Inclusive Scan`の二種類ある  
`Exclusive Scan`は, 現在の要素より前までの和を求める  
`Inclusive Scan`は, `Exclusive Scan`に現在の要素を足したもの  


```
Input:     [ 1, 7,  4,  0,  9, 4  ]
Exclusive: [ 0, 1,  8, 12, 12, 21 ]
Inclusive: [ 1, 8, 12, 12, 21, 25 ]
```
