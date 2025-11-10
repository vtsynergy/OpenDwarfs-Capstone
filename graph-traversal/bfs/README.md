# OpenDwarfs Capstone

## Overview
This repository is a **subset** of the [OpenDwarfs benchmark suite](https://github.com/opendwarfs/opendwarfs).  
 
OpenDwarfs is organized around the Berkeley Dwarfs taxonomy, which classifies computational patterns common in scientific and engineering applications.  
This code includes the serial implementation of breadth-first search algorithm


## Parallelization
The **workload to be parallelized** for this capstone is primarily located in the files named:

```
<program_name>_parallel.cpp
```

These files contain the parallel implementations.  
Your task is to analyze, parallelize, and optimize these implementations, then compare them to the serial versions.

---
## Get the data
Use the provided script to download and extract matrices into `../data`:
```bash
chmod +x get_data.sh
./get_data.sh
```
This pulls SuiteSparse tarballs and extracts them so each matrix appears under `../data/<name>/` (for example, `../data/hollywood-2009/hollywood-2009.mtx`).
Feel free to update get_data.sh to add more matrices from https://sparse.tamu.edu/

## 3) Matrix representation (how the structs are used)
We store matrices in **CSR (Compressed Sparse Row)** 

### How to compile
Using the provided Makefile:  
```bash
make
```
This builds the `bfs` executable.

---

### How to run with different options

**Correctness mode**  
```bash
./bfs -c
```
- Compares `bfs_parallel` against `bfs_serial` on a sample graph.  
- Also runs `bfs_parallel` with 1, 2, 8, 16 threads and prints time + OK/MISMATCH.  

**Performance evaluation**  
```bash
./bfs -p
```
- Runs `bfs_parallel` on every `../data/<graph>/<graph>.mtx` with threads **1, 2, 4, 8, 16, 32**.  
- Appends results to `../output/bfs_profile.csv`.  

**Both**  
```bash
./bfs -c -p
```

**Expected output for `-c` mode**  
```
[-c] serial   : ../data/roadNet-CA/roadNet-CA.mtx  n=1971281  m=5533214  ms=1121.379
[-c] parallel : threads=1  ms=1128.144  OK
[-c] parallel : threads=2  ms=1129.345  OK
[-c] parallel : threads=8  ms=1131.023  OK
[-c] parallel : threads=16 ms=1134.328  OK
```
- `n` = number of vertices  
- `m` = number of directed edges in CSR  
- `ms` = BFS runtime in milliseconds  
- `OK` means parallel matches serial  
