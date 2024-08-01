## HPC Infrastructure Testing Tradecraft

This guide provides a comprehensive list of tools and commands for testing various subsystems of high-performance computing (HPC) infrastructure. The structure and style are inspired by the "ROCm GPU Tradecrafts" repository here: [ROCm GPU Tradecrafts](https://github.com/kennethdsheridan/rocm_gpu_tradecrafts).

### Table of Contents
- [Introduction](#introduction)
- [Installation](#installation)
- [Stress-ng](#stress-ng)
  - [General Options](#general-options)
  - [CPU Stressors](#cpu-stressors)
  - [Memory Stressors](#memory-stressors)
  - [I/O Stressors](#io-stressors)
  - [Filesystem Stressors](#filesystem-stressors)
  - [Network Stressors](#network-stressors)
  - [Miscellaneous Stressors](#miscellaneous-stressors)
  - [Examples](#examples)
- [Additional Tools](#additional-tools)
  - [Linpack](#linpack)
  - [Iperf](#iperf)
  - [FIO (Flexible I/O Tester)](#fio-flexible-io-tester)
  - [Memtester](#memtester)
  - [Examples](#examples-1)

## Introduction


This guide is designed to help you stress test and benchmark various components of HPC infrastructure. It covers a variety of tools, with a focus on `stress-ng` for system stress testing.

</details>



## Stress-ng
<details>
<summary>Click to expand</summary>

`stress-ng` is a tool designed to stress test a computer system in various selectable ways. It was created by Colin King, a Principal Engineer at Intel. The tool can exercise different physical subsystems of a computer as well as various operating system kernel interfaces. For more information, visit the [stress-ng GitHub repository](https://github.com/ColinIanKing/stress-ng).

## Installation
<details>
<summary>Click to expand</summary>

To install `stress-ng` on a Linux system, use the following commands:

### Ubuntu/Debian
```sh
sudo apt update
sudo apt install stress-ng
```

### Fedora/CentOS/RHEL
```sh
sudo yum install stress-ng
```

### Arch Linux
```sh
sudo pacman -S stress-ng
```

### FreeBSD
```sh
sudo pkg install stress-ng
```

</details>

### General Options
<details>
<summary>Click to expand</summary>

- `--abort`: Force all running stressors to abort if any stressor terminates prematurely.
- `--aggressive`: Enable more file, cache, and memory aggressive options.
- `--all N`: Start N instances of all stressors in parallel.
- `--backoff N`: Wait N microseconds between the start of each stress worker process.
- `--class name`: Specify the class of stressors to run (e.g., cpu, io, memory).
- `--dry-run`: Parse options but do not run stress tests.
- `--help`: Show help.
- `--timeout T`: Run each stress test for at least T seconds.
- `--verbose`: Show all debug, warnings, and normal information output.
- `--version`: Show version of stress-ng, version of toolchain used to build stress-ng, and system information.

</details>

### CPU Stressors
<details>
<summary>Click to expand</summary>

- `--cpu N`: Start N workers exercising the CPU.
- `--cpu-method method`: Specify a CPU stress method (e.g., fft, matrix, all).
- `--cpu-load P`: Load CPUs to P% utilization.
- `--cpu-ops N`: Stop CPU stressors after N bogo operations.

</details>

### Memory Stressors
<details>
<summary>Click to expand</summary>

- `--vm N`: Start N workers exercising virtual memory.
- `--vm-bytes B`: Allocate B bytes per vm worker.
- `--vm-ops N`: Stop VM stressors after N bogo operations.
- `--page-in`: Force allocated pages to be paged back in.

</details>

### I/O Stressors
<details>
<summary>Click to expand</summary>

- `--io N`: Start N workers exercising I/O.
- `--io-ops N`: Stop I/O stressors after N bogo operations.
- `--hdd N`: Start N workers continually writing, reading, and removing temporary files.
- `--hdd-ops N`: Stop HDD stressors after N bogo operations.

</details>

### Filesystem Stressors
<details>
<summary>Click to expand</summary>

- `--fallocate N`: Start N workers exercising file allocation.
- `--file N`: Start N workers creating and deleting files.
- `--file-ops N`: Stop file stressors after N bogo operations.

</details>

### Network Stressors
<details>
<summary>Click to expand</summary>

- `--udp N`: Start N workers exercising UDP sockets.
- `--tcp N`: Start N workers exercising TCP sockets.
- `--socket N`: Start N workers exercising UNIX domain sockets.

</details>

### Miscellaneous Stressors
<details>
<summary>Click to expand</summary>

- `--timer N`: Start N workers exercising high-resolution timers.
- `--fork N`: Start N workers continually forking processes.
- `--switch N`: Start N workers forcing context switching between tied processes.

</details>

### Examples
<details>
<summary>Click to expand</summary>

#### Basic CPU Stress Test
```sh
stress-ng --cpu 4 --timeout 60s --metrics-brief
```

#### Memory Stress Test
```sh
stress-ng --vm 2 --vm-bytes 1G --timeout 60s
```

#### I/O Stress Test
```sh
stress-ng --io 2 --timeout 60s --metrics-brief
```

#### Combined Stress Test
```sh
stress-ng --cpu 4 --io 2 --vm 1 --vm-bytes 1G --timeout 60s --metrics-brief
```

#### Sequential Stress Test
```sh
stress-ng --sequential 4 --timeout 2m --metrics
```

#### Random Stress Test
```sh
stress-ng --random 64 --timeout 60s
```

#### Stress Test with Specific CPU Method
```sh
stress-ng --cpu 4 --cpu-method fft --timeout 2m
```

#### Stress Test with Verification
```sh
stress-ng --cpu 64 --cpu-method all --verify -t 10m --metrics-brief
```

</details>

</details>

## Additional Tools
<details>
<summary>Click to expand</summary>

In addition to `stress-ng`, there are other tools that can be used for comprehensive HPC infrastructure testing:

### Linpack
<details>
<summary>Click to expand</summary>

Linpack is a benchmark that measures a system's floating-point computing power. It is commonly used to rank supercomputers.

</details>

### Iperf
<details>
<summary>Click to expand</summary>

Iperf is a network testing tool that can create TCP and UDP data streams and measure the throughput of a network.

</details>

### FIO (Flexible I/O Tester)
<details>
<summary>Click to expand</summary>

FIO is a versatile I/O workload generator that can be used to test the performance of storage systems.

</details>

### Memtester
<details>
<summary>Click to expand</summary>

Memtester is a utility for testing the memory (RAM) in a computer to ensure that it is functioning correctly.

</details>

### Examples
<details>
<summary>Click to expand</summary>

#### Linpack Benchmark
```sh
./run_linpack.sh
```

#### Iperf Network Test
```sh
iperf -s  # Run on the server
iperf -c <server_ip>  # Run on the client
```

#### FIO Storage Test
```sh
fio --name=randwrite --ioengine=libaio --iodepth=4 --rw=randwrite --bs=4k --direct=1 --size=1G --numjobs=4 --runtime=60 --group_reporting
```

#### Memtester Memory Test
```sh
memtester 1024 5
```

</details>

</details>

This guide provides a quick reference to the most commonly used tools and commands for HPC infrastructure testing. Use these commands judiciously, especially on production systems, as they can significantly impact system performance.
