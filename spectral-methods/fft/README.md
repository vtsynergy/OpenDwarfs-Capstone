# Fast Fourier Transform (FFT)

This code implements a **radix-2 FFT** with bit-reversal

- **What it does:**  
  Performs an in-place FFT on a synthetic test signal (sum of three sine waves), stage by stage using butterfly operations and precomputed twiddle factors.  
  Provides both a serial implementation and a placeholder "parallel" version (currently identical) for students to parallelize.

---

## Input Signal

For size *N*, the input is:  

$$x[n] = \sin\left(\tfrac{100\pi}{N}n\right) + \sin\left(\tfrac{1000\pi}{N}n\right) + \sin\left(\tfrac{2000\pi}{N}n\right), \quad n = 0,1,\dots,N-1$$  



- Real part = `x[n]`  
- Imaginary part = `0`  

Before the FFT stages, the input undergoes **bit-reversal reordering**.

---

## Files

- `fft_serial.cpp` — Serial FFT implementation (butterflies as in kernel).  
- `fft_parallel.cpp` — Parallel FFT (currently same as serial; to be parallelized).  
- `main.cpp` — Driver: generates input + twiddles, applies bit-reversal, runs correctness & performance.  
- `Makefile` — Used to compile the code.

---

## Build

```bash
make
```

Produces the executable `fft_app`.

---

## Run

### Correctness checks
```bash
./fft_app -c
```
- Runs FFT for `N=4096,8192,16384`.  
- Compares serial and parallel results (max abs error).  

### Performance evaluation
```bash
./fft_app -p
```
- Runs FFT for `N=4096,8192,16384,32768,65536 ....`.  
- Tests placeholder parallel across thread counts `2..64`.  
- Outputs **fft_perf.csv** with runtime and speedup (parallel = same as serial for now).

---

## Clean

```bash
make clean
```

Removes objects, binary, and `fft_perf.csv`.

