## FindMergesortFrame: include/device/ctamerge.cuh

```C++
// Returns (offset of a, offset of b, length of list)
MGPU_HOST_DEVICE int3
FindMergesortFrame(int coop, int block, int nv) {
  // coop is the number of CTAs or threads cooperating to merge two lists into
  // one. We round block down to the first CTA's ID that is working on this
  // merge.
  int start = ~(coop - 1) & block;
  int size  = nv * (coop >> 1);
  return make_int3(nv * start, nv * start + size, size);
}
```

startについてはコメントに書いている通り  
例えば, blockIDが0, 1の二つのCTAが協調してマージするのであれば  
startはともに0になる  

sizeは一つのCTAが責任を持つ要素数  
コメントの力を借りれば, リストの長さ
