# 🧠 CRR Template for Recursion Problems

This document breaks down recursion into a simple, repeatable 3-step mindset: **CRR – Choice, Result, Return**. If you're struggling with recursion or Dynamic Programming, this structure will help you think clearly.

---

## ✅ Fix: Use the “Choice-Result-Return” Template

This 3-step mindset works in all recursive problems, especially:

* Finding max/min values
* Counting total ways
* Exploring all paths

---

## 🔁 Step-by-step Recursion Mindset (CRR)

**1. Choice**

> At this step, what options do I have?

**2. Result**

> What will I get by picking each option?

**3. Return**

> From those results, what do I return (min/max/sum/etc.)?

---

## 🧠 Let's Take a Real Example: House Robber

### Problem:

Given `nums = [2,7,9,3,1]`, return the maximum amount you can rob without robbing two adjacent houses.

### CRR Breakdown:

**1. Choices:**

* Rob house `i` → skip `i+1`, go to `i+2`
* Don’t rob house `i` → go to `i+1`

**2. Result:**

```cpp
if I rob → nums[i] + solve(i + 2)
if I skip → solve(i + 1)
```

**3. Return:**

```cpp
return max(rob, skip);
```

### Final Recursive Code:

```cpp
int solve(int i) {
    if (i >= nums.size()) return 0;

    int rob = nums[i] + solve(i + 2);
    int skip = solve(i + 1);

    return max(rob, skip);
}
```

Just plug in the CRR mindset. It works.

---

## 👊 Apply CRR to Any Problem:

* **Climbing stairs** → Choices: 1 step or 2 steps
* **Coin change** → Choices: pick coin or skip
* **Min path sum** → Choices: go down or right
* **Max profit** → Choices: buy/sell/skip

Same recursive structure every time.

---

## 🧩 Problem: Minimum Path Sum

**Problem:**
Given a grid `grid[m][n]` where you can only move right or down, find the minimum path sum from the top-left `(0,0)` to bottom-right `(m-1,n-1)`.

---

## ✅ CRR Breakdown

### ✅ C → Choices

From cell `(i, j)`, two valid moves:

* Move Down → `(i + 1, j)`
* Move Right → `(i, j + 1)`

### ✅ R → Result

```cpp
int down = solve(i + 1, j);
int right = solve(i, j + 1);
```

### ✅ R → Return

```cpp
return grid[i][j] + min(down, right);
```

### ✅ Base Cases

```cpp
// Reached destination:
if (i == m - 1 && j == n - 1) return grid[i][j];

// Out of bounds:
if (i >= m || j >= n) return INT_MAX;
```

### Full Recursive Code:

```cpp
int solve(int i, int j) {
    if (i >= m || j >= n) return INT_MAX;
    if (i == m - 1 && j == n - 1) return grid[i][j];

    int down = solve(i + 1, j);
    int right = solve(i, j + 1);

    return grid[i][j] + min(down, right);
}
```

---

## ✅ CRMA Breakdown for Optimized Recursion (Memoized)

### ✅ C – Choices

From `(i, j)`:

* Move Down → `(i+1, j)`
* Move Right → `(i, j+1)`

### ✅ R – Result

```cpp
down = solve(i + 1, j)
right = solve(i, j + 1)
return grid[i][j] + min(down, right);
```

### ✅ M – Memoization

Use 2D dp table:

```cpp
if (dp[i][j] != -1) return dp[i][j];
vector<vector<int>> dp(m, vector<int>(n, -1));
```

### ✅ A – Answer

Call from starting point:

```cpp
solve(0, 0)
```

---

### Final Recursive Code with CRMA:

```cpp
int solve(int i, int j, vector<vector<int>>& grid, vector<vector<int>>& dp) {
    int m = grid.size(), n = grid[0].size();

    if (i >= m || j >= n) return INT_MAX;
    if (i == m - 1 && j == n - 1) return grid[i][j];

    if (dp[i][j] != -1) return dp[i][j];

    int down = solve(i + 1, j, grid, dp);
    int right = solve(i, j + 1, grid, dp);

    return dp[i][j] = grid[i][j] + min(down, right);
}
```

---

### 🟢 Intuition Behind INT\_MAX

We return `INT_MAX` for out-of-bound cells because:

* We're using `min(down, right)`
* If we go out of bounds, that path shouldn't be considered
* `INT_MAX` ensures it never gets picked in the minimum
