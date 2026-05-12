# Grid DP and Path Counting

**Grid DP** applies when state is **`(row, col)`** with transitions from **limited neighbors**—classic for **unique paths**, **min path sum**, **dungeon** problems.

---

## Unique paths with obstacles

`dp[r][c] = 0` if blocked, else **`dp[r-1][c] + dp[r][c-1]`** with base row/column initialization.

**Space optimization**: rolling **1D array** over columns because only **previous row** needed.

---

## Min path sum

`dp[r][c] = cost[r][c] + min(dp[r-1][c], dp[r][c-1])`.

---

## Pitfalls

- **Modulo** combinatorics problems need **multiplicative** DP or **math** (choose) not naive `int` overflow.
- **Cycles** invalidates simple DP—need **graph** algorithms.

---

## Related

- [Dynamic Programming](15_Dynamic_Programming.md)
- [Advanced DP Patterns](30_Advanced_DP_Patterns.md)
