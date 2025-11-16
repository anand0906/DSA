# Graph Problems Collection

## Problems On Traversal
1. [Connected Components](#1-connected-components)
2. [Number of Provinces](#2-number-of-provinces)
3. [Number of Islands](#3-number-of-islands)
4. [Number of Enclaves](#4-number-of-enclaves)
5. [Surrounded Regions](#5-surrounded-regions)
6. [Flood Fill](#6-flood-fill)
7. [Rotting Oranges](#7-rotting-oranges)
8. [01 Matrix (Distance of Nearest Cell Having 0)](#8-01-matrix-distance-of-nearest-cell-having-0)
9. [Detect Cycle in an Undirected Graph](#9-detect-cycle-in-an-undirected-graph)
10. [Is Graph Bipartite? (Bipartite Graph Detection)](#10-is-graph-bipartite-bipartite-graph-detection)

---

## 1. Connected Components

**Problem Description:**

You are given an undirected graph consisting of `n` nodes numbered from `0` to `n - 1`. You are given a 2D array `edges` where `edges[i] = [ai, bi]` denotes that there exists an undirected edge connecting nodes `ai` and `bi`.

Return the number of connected components in the graph.

**Sample Test Cases:**

**Example 1:**
```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2

Visual Explanation:
    0---1---2        3---4
   (Component 1)   (Component 2)

The graph has 2 connected components.
```

**Example 2:**
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1

Visual Explanation:
    0---1---2---3---4
    (One Component)

All nodes are connected, forming 1 component.
```

**Constraints:**
- `1 <= n <= 2000`
- `0 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= ai <= bi < n`
- `ai != bi`
- There are no repeated edges

---

## 2. Number of Provinces

**Problem Description:**

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return the total number of provinces.

**Sample Test Cases:**

**Example 1:**
```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2

Visual Explanation:
    0---1        2
   (Province 1) (Province 2)

Cities 0 and 1 are connected (Province 1).
City 2 is isolated (Province 2).
Total: 2 provinces
```

**Example 2:**
```
Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3

Visual Explanation:
    0        1        2
(Province 1) (Province 2) (Province 3)

Each city is isolated.
Total: 3 provinces
```

**Constraints:**
- `1 <= n <= 200`
- `n == isConnected.length`
- `n == isConnected[i].length`
- `isConnected[i][j]` is `1` or `0`
- `isConnected[i][i] == 1`
- `isConnected[i][j] == isConnected[j][i]`

---

## 3. Number of Islands

**Problem Description:**

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Sample Test Cases:**

**Example 1:**
```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

Visual Explanation:
    1  1  1  1  0
    1  1  0  1  0
    1  1  0  0  0
    0  0  0  0  0

All the '1's are connected (horizontally or vertically), forming 1 island.
```

**Example 2:**
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3

Visual Explanation:
    [1  1] 0  0  0     ← Island 1
    [1  1] 0  0  0     ← Island 1
     0  0 [1] 0  0     ← Island 2
     0  0  0 [1  1]    ← Island 3

There are 3 separate islands.
```

**Constraints:**
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`

---

## 4. Number of Enclaves

**Problem Description:**

You are given an `m x n` binary matrix `grid`, where `0` represents a sea cell and `1` represents a land cell.

A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid.

Return the number of land cells in `grid` for which we cannot walk off the boundary of the grid in any number of moves.

**Sample Test Cases:**

**Example 1:**
```
Input: grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
Output: 3

Visual Explanation:
    0  0  0  0
    1  0 [1] 0
    0 [1][1] 0
    0  0  0  0

The land cell at (1,0) can walk off the boundary.
The 3 land cells marked with [] cannot walk off the boundary.
Answer: 3
```

**Example 2:**
```
Input: grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
Output: 0

Visual Explanation:
    0  1  1  0
    0  0  1  0
    0  0  1  0
    0  0  0  0

All land cells are connected to the boundary.
Answer: 0
```

**Constraints:**
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 500`
- `grid[i][j]` is either `0` or `1`

---

## Problem #5
**Title:** Surrounded Regions

**Problem Description:**

Given an `m x n` matrix `board` containing `'X'` and `'O'`, capture all regions that are 4-directionally surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

**Sample Test Cases:**

**Example 1:**
```
Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

Visual Explanation:
Before:                After:
X  X  X  X            X  X  X  X
X  O  O  X    →       X  X  X  X
X  X  O  X            X  X  X  X
X  O  X  X            X  O  X  X

The 'O' at (3,1) is on the border, so it's not captured.
The other 'O's are surrounded and get captured (flipped to 'X').
```

**Example 2:**
```
Input: board = [["X"]]
Output: [["X"]]

Visual Explanation:
Only one cell, no change needed.
```

**Constraints:**
- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is `'X'` or `'O'`

---

## Problem #6
**Title:** Flood Fill

**Problem Description:**

An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.

You are also given three integers `sr`, `sc`, and `color`. You should perform a flood fill on the image starting from the pixel `image[sr][sc]`.

To perform a flood fill, consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with `color`.

Return the modified image after performing the flood fill.

**Sample Test Cases:**

**Example 1:**
```
Input: image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2
Output: [[2,2,2],[2,2,0],[2,0,1]]

Visual Explanation:
Before:              After:
1  1  1              2  2  2
1 [1] 0      →       2 [2] 0
1  0  1              2  0  1

Starting from (1,1) with value 1, all connected 1's are changed to 2.
```

**Example 2:**
```
Input: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0
Output: [[0,0,0],[0,0,0]]

Visual Explanation:
The starting pixel is already the target color (0), so no change occurs.
```

**Constraints:**
- `m == image.length`
- `n == image[i].length`
- `1 <= m, n <= 50`
- `0 <= image[i][j], color < 2^16`
- `0 <= sr < m`
- `0 <= sc < n`

---

## Problem #7
**Title:** Rotting Oranges

**Problem Description:**

You are given an `m x n` grid where each cell can have one of three values:
- `0` representing an empty cell
- `1` representing a fresh orange
- `2` representing a rotten orange

Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.

**Sample Test Cases:**

**Example 1:**
```
Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4

Visual Explanation:
Minute 0:        Minute 1:        Minute 2:        Minute 3:        Minute 4:
2  1  1          2  2  1          2  2  2          2  2  2          2  2  2
1  1  0    →     2  1  0    →     2  2  0    →     2  2  0    →     2  2  0
0  1  1          0  1  1          0  2  1          0  2  2          0  2  2

After 4 minutes, all oranges are rotten.
```

**Example 2:**
```
Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1

Visual Explanation:
2  1  1
0  1  1
1  0  1

The orange at (2,0) can never rot because it's separated by empty cells.
Answer: -1
```

**Example 3:**
```
Input: grid = [[0,2]]
Output: 0

Visual Explanation:
There are no fresh oranges, so the answer is 0.
```

**Constraints:**
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` is `0`, `1`, or `2`

---

## Problem #8
**Title:** 01 Matrix (Distance of Nearest Cell Having 0)

**Problem Description:**

Given an `m x n` binary matrix `mat`, return the distance of the nearest `0` for each cell.

The distance between two adjacent cells is `1`.

**Sample Test Cases:**

**Example 1:**
```
Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
Output: [[0,0,0],[0,1,0],[0,0,0]]

Visual Explanation:
Input:               Output:
0  0  0              0  0  0
0  1  0      →       0  1  0
0  0  0              0  0  0

The cell at (1,1) has distance 1 to the nearest 0.
All cells with 0 have distance 0.
```

**Example 2:**
```
Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
Output: [[0,0,0],[0,1,0],[1,2,1]]

Visual Explanation:
Input:               Output:
0  0  0              0  0  0
0  1  0      →       0  1  0
1  1  1              1  2  1

Cell (2,0): distance 1 to (1,0) which is 0
Cell (2,1): distance 2 to nearest 0
Cell (2,2): distance 1 to (1,2) which is 0
```

**Constraints:**
- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 10^4`
- `1 <= m * n <= 10^4`
- `mat[i][j]` is either `0` or `1`
- There is at least one `0` in `mat`

---

## Problem #9
**Title:** Detect Cycle in an Undirected Graph

**Problem Description:**

Given an undirected graph with `V` vertices and `E` edges, check whether it contains any cycle or not. The graph is represented as an adjacency list where `adj[i]` contains all the vertices that are directly connected to vertex `i`.

**Sample Test Cases:**

**Example 1:**
```
Input: V = 5, adj = [[1],[0,2,4],[1,3],[2,4],[1,3]]
Output: true

Visual Explanation:
    0---1---2
        |   |
        4---3

There is a cycle: 1 → 2 → 3 → 4 → 1
```

**Example 2:**
```
Input: V = 4, adj = [[1],[0,2],[1,3],[2]]
Output: false

Visual Explanation:
    0---1---2---3

This is a tree (no cycles).
```

**Example 3:**
```
Input: V = 5, adj = [[],[2],[1,3],[2],[]]
Output: false

Visual Explanation:
    0    1---2---3    4

No cycles exist. Node 0 and 4 are isolated.
```

**Constraints:**
- `1 <= V, E <= 10^5`
- The graph has no self-loops or multiple edges

---

## Problem #10
**Title:** Is Graph Bipartite? (Bipartite Graph Detection)

**Problem Description:**

There is an undirected graph with `n` nodes, where each node is numbered between `0` and `n - 1`. You are given a 2D array `graph`, where `graph[u]` is an array of nodes that node `u` is adjacent to.

A graph is bipartite if the nodes can be partitioned into two independent sets `A` and `B` such that every edge in the graph connects a node in set `A` and a node in set `B`.

Return `true` if and only if it is bipartite.

**Sample Test Cases:**

**Example 1:**
```
Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
Output: false

Visual Explanation:
    0---1
    |\ /|
    | X |
    |/ \|
    3---2

Cannot partition into two sets.
Trying: A={0,2}, B={1,3} fails because 1-2 are both edges and they're in the same set.
```

**Example 2:**
```
Input: graph = [[1,3],[0,2],[1,3],[0,2]]
Output: true

Visual Explanation:
    0---1
    |   |
    3---2

Can partition into: A={0,2} and B={1,3}
All edges connect nodes from different sets.
```

**Example 3:**
```
Input: graph = [[1],[0,3],[3],[1,2]]
Output: true

Visual Explanation:
    0---1---3
            |
            2

Can partition into: A={0,3} and B={1,2}
```

**Constraints:**
- `graph.length == n`
- `1 <= n <= 100`
- `0 <= graph[u].length < n`
- `0 <= graph[u][i] <= n - 1`
- `graph[u]` does not contain `u`
- All the values of `graph[u]` are unique
- If `graph[u]` contains `v`, then `graph[v]` contains `u`
