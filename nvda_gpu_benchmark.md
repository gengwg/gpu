# Using NVIDIA GPU P2P Benchmark tool

## Build the tools

Download repo git clone https://github.com/NVIDIA/cuda-samples.git

Checkout the tag that corresponds with the right CUDA version, and build the tools.

On x84_64:

```
# git checkout v11.4
# make
```

[Example](https://gist.github.com/gengwg/a3f68aa6ecb18833b1fb8f288bebd38a)

## Run the tests

Work dir:

```
cd cuda-samples/bin/x86_64/linux/release
```

### Host-to-Device Transfer Speeds Test

#### DGX A100:

Pinned memory:

```
# ./bandwidthTest --memory=pinned --mode=quick --start=1024 --end=102400 --increment=1024 --dtoh -htod
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 0: NVIDIA A100-SXM4-80GB
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)>---Bandwidth(GB/s)
   32000000>>--->---25.7

 Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)>---Bandwidth(GB/s)
   32000000>>--->---25.8

Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.
```

No pin memory:

```
# ./bandwidthTest --memory=pageable --mode=quick --start=1024 --end=102400 --increment=1024 --dtoh -htod
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 0: NVIDIA A100-SXM4-80GB
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PAGEABLE Memory Transfers
   Transfer Size (Bytes)>---Bandwidth(GB/s)
   32000000>>--->---10.0

 Device to Host Bandwidth, 1 Device(s)
 PAGEABLE Memory Transfers
   Transfer Size (Bytes)>---Bandwidth(GB/s)
   32000000>>--->---10.8

Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.
```

#### Dell A100

```
# ./bandwidthTest --memory=pinned --mode=quick --start=1024 --end=102400 --increment=1024 --dtoh -htod
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 0: NVIDIA A100 80GB PCIe
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			17.3

 Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			24.6

Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.
[root@pit101-xea1002 release]# ./bandwidthTest --memory=pageable --mode=quick --start=1024 --end=102400 --increment=1024 --dtoh -htod
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 0: NVIDIA A100 80GB PCIe
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PAGEABLE Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			3.3

 Device to Host Bandwidth, 1 Device(s)
 PAGEABLE Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			8.5

Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.
```

### GPU-to-GPU Transfers with and without NVSwitch and NVLink

```
# ./p2pBandwidthLatencyTest
```

Examples:

- [Nvidia DGX A100](https://gist.github.com/gengwg/29302ceff3aa2519d1619dd35b95ae7b)
- [Dell XEA A100](https://gist.github.com/gengwg/8d8a162f82b2ee8e391e39f5d77b34de)

## CUDA Device Query

```
# ./deviceQuery
```

Examples:

- [Nvidia DGX A100](https://gist.github.com/gengwg/3520df2ebf8872e5ee4818367e3a4122)
- [Dell XEA A100](https://gist.github.com/gengwg/00f43c712c8da0c0c0b11f4d7bd6f74d)

We can notice Dell A100 is PCIe based and DGX A100 is SXM based with NVSwitch.
