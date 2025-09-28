# OpenDwarfs Capstone

## Overview
This repository is a **subset** of the [OpenDwarfs benchmark suite](https://github.com/opendwarfs/opendwarfs).  

OpenDwarfs is organized around the Berkeley Dwarfs taxonomy, which classifies computational patterns common in scientific and engineering applications.  

# Project Background

In the last two decades, computing has shifted strongly toward parallelism, bringing parallel computing to the mainstream. Programmers now face a wide range of platforms, each with its own performance opportunities and challenges. This creates two main problems:  
1. the need for a common programming model, and  
2. the need for a common way to evaluate architectures.  

The first problem has been largely solved by frameworks like **OpenMP**, **OpenCL**, and **SYCL**, which allow programmers to write portable parallel code for CPUs, GPUs, and accelerators. These frameworks simplify programming across diverse hardware while still exposing parallelism to improve performance.  

The second problem is harder. Traditional benchmark suites such as SPEC CPU or PARSEC are tied to specific programming languages and architectures. They often represent narrow applications rather than general computational patterns, which limits their usefulness for evaluating and designing parallel systems.  

**OpenDwarfs** addresses this by organizing workloads around the *Berkeley Dwarfs* — a taxonomy of fundamental computational motifs such as dense linear algebra, sparse linear algebra, graph traversal, spectral methods, and structured grids. Instead of focusing on single applications, OpenDwarfs captures general computation and communication patterns that appear across many scientific and engineering problems. This makes it possible to study parallelism more systematically and compare performance across CPUs, GPUs, and other devices.  

This repository provides a **truncated version of OpenDwarfs** (full version: [OpenDwarfs GitHub](https://github.com/opendwarfs/opendwarfs)) for the Capstone project. The included dwarfs/motifs are:  
- **Branch and Bound** → `nqueens` (N-Queens search)  
- **Dense Linear Algebra** → `lud` (LU decomposition)  
- **Graph Traversal** → `bfs` (Breadth-First Search)  
- **Sparse Linear Algebra** → `spmv` (Sparse Matrix-Vector Multiplication)  
- **Spectral Methods** → `fft` (Fast Fourier Transform)  
- **Structured Grids** → `gol` (Game of Life simulation)  

# Objectives

**Required objectives:**  
For each dwarf, the code to be parallelized is located in `<programname>_parallel.cpp` inside the directory for that dwarf.  
- If working in groups of three, parallelize **all six workloads** (two dwarfs to be parallelized by each team member).  
- Use OpenMP to parallelize the workload in `<programname>_parallel.cpp` adding appropriate OpenMP pragma directives.  

**Additional objectives** (not necessarily in order of relevance; you may pick any one or more that you find interesting):  
1. Pick one dwarf out of the six and attempt to optimize the code beyond what is achieved by the initial placement of OpenMP directives. Your approach may include modifications to the code, algorithm, data structure, the way the workload is parallelized, or any combination of these changes. (Example: assign threads to nonzero elements in SPMV using a COO data structure instead of row-centric parallelism with CSR.)  
2. Use **OpenACC** to offload the computation to a GPU. Parallelize `<dwarf>_parallel.cpp` using OpenACC and evaluate runtimes on `glogin` GPU nodes. Compare CPU (OpenMP) vs GPU (OpenACC) performance and analyze the results.  
3. Use **OpenMP 4.5** to offload computation to a GPU. Compare performance between CPU (OpenMP) and GPU (OpenMP offload), and optionally vs GPU (OpenACC). Comment on the relative difficulty/ease of programming with the different frameworks.  

Refer to the README in each dwarf’s directory for compilation and execution details.  

## Repository Structure
```
branch-and-bound/       # Contains nqueens benchmark  
dense-linear-algebra/   # Contains lud benchmark  
graph-traversal/        # Contains bfs benchmark  
sparse-linear-algebra/  # Contains spmv benchmark  
spectral-methods/       # Contains fft benchmark  
structured_grids/       # Contains gol benchmark  
```

Each directory contains its own source files and a `README.md` with build and run instructions.  

# Infrastructure Requirements

Performance should be measured on the **Virginia Tech rlogin/glogin clusters**:  
- Use `rlogin.cs.vt.edu` for CPU testing (OpenMP).  
- Use `glogin.cs.vt.edu` for GPU testing (OpenACC or OpenMP offload).  

Log in with `ssh <your_pid>@rlogin.cs.vt.edu` (or `glogin` for GPUs) and clone this repository before starting.  

# Compilation

See the `README.md` inside each dwarf’s directory for detailed build/run instructions.  

# Parallelization
The **workload to be parallelized** for this capstone is primarily located in the files named:  

```
<program_name>_parallel.cpp
```

These files contain the parallel implementations.  
Your task is to analyze, parallelize, and optimize these implementations, then compare them to the serial versions.  

# Example: BFS

### How to compile
Using the provided Makefile:  
```bash
make
```
This builds the `bfs` executable.  

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
- `OK` implies output of parallel code matches serial output  
