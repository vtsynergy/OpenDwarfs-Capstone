# N-Queens (Iterative Bitmask) — Serial & Parallel

## Overview
Counts solutions to the N-Queens problem using a bitmask-based backtracking algorithm.
- `count_nqueens_serial(n)` — single threaded.
- `count_nqueens_parallel(n)` — same logic, to be parallelized using OpenMP directives 


The program can:
- Check correctness (expected value for  `n=10` : `724`).
- Benchmark the code for `n ∈ {8,10,12,14,15}` with threads `{1,2,4,8,16,32}`.
- Write results to `../output/results.csv` (directory auto-created).

## Build
```bash
make 
```

## Run
**Correctness check**
```bash
./nqueens -c
```

**Benchmark and write CSV**
```bash
./nqueens -p
```
**CSV path**
```
../output/results.csv
```

## CSV Format
**Header**
```
n,count,serial_ms,t1_ms,t2_ms,t4_ms,t8_ms,t16_ms,t32_ms,speedup_x2,speedup_x4,speedup_x8,speedup_x16,speedup_x32
```
- `count` — solutions from the serial solver (ground truth).
- `tx_ms` — parallel runtime at `x` threads.
- `speedup_xk` — `serial_ms / tk_ms`.

**Example row**
```
10,724,12.345,12.100,6.200,3.200,1.800,1.300,0.950,1.99,3.86,6.86,9.50,12.99
```

## What the program does
- Places queens **column by column** from left to right.
- Uses **bit masks** to track rows already used and rows blocked by diagonals.
- Picks the next available row, pushes constraints to the next column, and **backtracks** when a column has no candidates.
- Parallel version simply distributes independent **first-column choices** across threads and sums the counts.

## Known correct counts (for sanity checks)
| n  | solutions |
|----|-----------|
| 8  | 92        |
| 10 | 724       |
| 12 | 14200     |
| 14 | 365596    |
| 15 | 2279184   |

## Notes
- Requires C++17 (`<filesystem>` used to create `../output`).

