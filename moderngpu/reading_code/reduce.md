# Reduce

Reduce関数を見ていく  

## Reduce: include/kernel/reduce.cuh

```C++
template<typename InputIt>
MGPU_HOST typename std::iterator_traits<InputIt>::value_type
Reduce(InputIt data_global, int count, CudaContext& context) { 
	typedef typename std::iterator_traits<InputIt>::value_type T;
	T result;
	Reduce(data_global, count, (T)0, mgpu::plus<T>(), (T*)0, &result, context);
	return result;
}
```

関数の引数はそれぞれ  
* Reduce処理をするデバイスメモリ上のデータ  
* データの要素数  
* コンテキスト  

処理は異なるホスト関数のReduceに移している  

```C++
template<typename InputIt, typename T, typename Op>
MGPU_HOST void Reduce(InputIt data_global, int count, T identity, Op op,
	T* reduce_global, T* reduce_host, CudaContext& context) {

	MGPU_MEM(T) totalDevice;
	if(!reduce_global) {
		totalDevice = context.Malloc<T>(1);
		reduce_global = totalDevice->get();
	}

	if(count <= 256) {
		typedef LaunchBoxVT<256, 1> Tuning;
		KernelReduce<Tuning><<<1, 256, 0, context.Stream()>>>(
			data_global, count, identity, op, reduce_global);
		MGPU_SYNC_CHECK("KernelReduce");

	} else if(count <= 768) {
		typedef LaunchBoxVT<256, 3> Tuning;
		KernelReduce<Tuning><<<1, 256, 0, context.Stream()>>>(
			data_global, count, identity, op, reduce_global);
		MGPU_SYNC_CHECK("KernelReduce");

	} else if(count <= 512 * ((sizeof(T) > 4) ? 4 : 8)) {
		typedef LaunchBoxVT<512, (sizeof(T) > 4) ? 4 : 8> Tuning;
		KernelReduce<Tuning><<<1, 512, 0, context.Stream()>>>(
			data_global, count, identity, op, reduce_global);
		MGPU_SYNC_CHECK("KernelReduce");

	} else {
		// Launch a grid and reduce tiles to temporary storage.
		typedef LaunchBoxVT<256, (sizeof(T) > 4) ? 8 : 16> Tuning;
		int2 launch = Tuning::GetLaunchParams(context);
		int NV = launch.x * launch.y;
		int numBlocks = MGPU_DIV_UP(count, NV);

		MGPU_MEM(T) reduceDevice = context.Malloc<T>(numBlocks);
		KernelReduce<Tuning><<<numBlocks, launch.x, 0, context.Stream()>>>(
			data_global, count, identity, op, reduceDevice->get());
		MGPU_SYNC_CHECK("KernelReduce");

		Reduce(reduceDevice->get(), numBlocks, identity, op, reduce_global,
			(T*)0, context);
	}

	if(reduce_host)
		copyDtoH(reduce_host, reduce_global, 1);
}
```

この関数の引数はそれぞれ  
* Reduce処理をするデバイスメモリ上のデータ  
* データの要素数  
* (おそらく) 単位元. 和の場合, 0であり, 積の場合, 1    
* Reduceする際の演算子. `mgpu::plus<type>`など   
* Reduceした結果を格納するデバイスメモリ上のアドレス
* Reduceした結果を格納するホストメモリ上のアドレス
* コンテキスト

関数の処理の内容を大雑把に見ていくと  
  1. `reduce_global`が設定されていない時の処理.  
     `reduce_global`にデバイスメモリの領域を割り当てている  
  2. `KernelReduce`関数の呼び出し.  

