# LUD of Dense 3D Laplacian

A C++ program that builds a dense matrix from a 3D 7‑point Laplacian discretization with Dirichlet boundaries, then computes an in‑place LU factorization without pivoting. The project has three files: `main.cpp`, `lud_serial.cpp`, and `lud_parallel.cpp`. The serial and parallel entry points are intentionally identical, your objective is to parallelize the function in `lud_parallel.cpp`.

## What it does

- Builds an N×N dense matrix `A` for a 3D grid of size `nx × ny × nz` where `N = nx*ny*nz`.
- Runs LU decomposition in place with no pivoting, stored as packed `[L|U]` inside `A`:
  - `L` is the lower triangular matrix.
  - `U` is the upper triangular matrix.
- `-c` compares the LU decomposition from the serial and the parallel implementations, and reports reconstruction error.
- `-p` or `-P` times serial and parallel implementations, and writes runtimes and speedups to `lud_perf.csv` with.

## Build

```bash
make            # optimized default build
make release    # force -O3 -march=native
make debug      # -O0 with debug symbols
make clean
```

The Makefile builds `lud` from `main.cpp`, `lud_serial.cpp`, and `lud_parallel.cpp`.

## Run

Correctness checks:
```bash
./lud -c
```

Performance runs :
```bash
./lud -p     # or ./lud -P
# Produces lud_perf.csv with columns:
# nx,ny,nz,N,serial_ms,parallel_ms,speedup
```

The performance mode evaluates these sizes:
1. `nx=ny=nz=8`   which discretizes to a 2D square matrix with rows `N=512`
2. `nx=8, ny=16, nz=8`   matrix with `N=1024`
3. `nx=8, ny=16, nz=16`  matrix with `N=2048`
4. `nx=ny=nz=16`  matrix with `N=4096`

## Notes

- LU uses no pivoting. The Laplacian matrix is strictly diagonally dominant and zero pivoting is not required
- Matrices are stored as `std::vector<std::vector<double>>` for clarity. For higher performance, consider a flat `std::vector<double>` with manual indexing.
- Complexity is `O(N^3)` and memory is `O(N^2)`. The `N=4096` case is compute and memory heavy.

