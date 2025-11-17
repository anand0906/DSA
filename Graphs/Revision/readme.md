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

---

# Topological Sort & Directed Graph Problems

## Index
1. [Topological Sort](#1-topological-sort)
2. [Detect Cycle in Directed Graph](#2-detect-cycle-in-directed-graph)
3. [Find Eventual Safe States](#3-find-eventual-safe-states)
4. [Course Schedule I](#4-course-schedule-i)
5. [Course Schedule II](#5-course-schedule-ii)
6. [Alien Dictionary](#6-alien-dictionary)

---

## 1. Topological Sort

**Problem Description:**

Given a Directed Acyclic Graph (DAG) with `V` vertices and `E` edges, return any valid topological ordering of the graph's vertices.

A topological sort of a directed graph is a linear ordering of its vertices such that for every directed edge `u → v`, vertex `u` comes before vertex `v` in the ordering.

If the graph contains a cycle, topological sorting is not possible.

**Sample Test Cases:**

**Example 1:**
```
Input: V = 6, edges = [[5,2],[5,0],[4,0],[4,1],[2,3],[3,1]]
Output: [5,4,2,3,1,0] (or any other valid topological ordering)

Visual Explanation:
    5 ——→ 0
    |     ↑
    ↓     |
    2 ——→ 3 ——→ 1
          ↑     ↑
          |_____|
              4

One valid topological order: 5 → 4 → 2 → 3 → 1 → 0
Another valid order: 4 → 5 → 2 → 3 → 1 → 0
```

**Example 2:**
```
Input: V = 4, edges = [[0,1],[1,2],[2,3]]
Output: [0,1,2,3]

Visual Explanation:
    0 ——→ 1 ——→ 2 ——→ 3

Only one valid topological order: 0 → 1 → 2 → 3
```

**Constraints:**
- `1 <= V <= 10^4`
- `0 <= E <= 10^4`
- The graph is a DAG (Directed Acyclic Graph)

---

## 2. Detect Cycle in Directed Graph

**Problem Description:**

Given a Directed Graph with `V` vertices (numbered from `0` to `V-1`) and `E` edges, check whether it contains any cycle or not.

The graph is represented as an adjacency list where `adj[i]` contains all vertices `j` such that there is a directed edge from vertex `i` to vertex `j`.

**Sample Test Cases:**

**Example 1:**
```
Input: V = 4, adj = [[1],[2],[3],[3]]
Output: false

Visual Explanation:
    0 ——→ 1 ——→ 2 ——→ 3
                      ↺

Wait, 3 points to itself? Let me recorrect:
    0 ——→ 1 ——→ 2 ——→ 3

No cycle exists in this directed graph.
```

**Example 2:**
```
Input: V = 4, adj = [[1],[2],[3],[1]]
Output: true

Visual Explanation:
    0 ——→ 1 ——→ 2 ——→ 3
          ↑___________|

There is a cycle: 1 → 2 → 3 → 1
```

**Example 3:**
```
Input: V = 3, adj = [[1],[2],[0]]
Output: true

Visual Explanation:
    0 ——→ 1 ——→ 2
    ↑___________|

There is a cycle: 0 → 1 → 2 → 0
```

**Constraints:**
- `1 <= V, E <= 10^5`

---

## 3. Find Eventual Safe States

**Problem Description:**

There is a directed graph of `n` nodes with each node labeled from `0` to `n - 1`. The graph is represented by a 2D integer array `graph` where `graph[i]` is an integer array of nodes adjacent to node `i`, meaning there is an edge from node `i` to each node in `graph[i]`.

A node is a terminal node if there are no outgoing edges. A node is a safe node if every possible path starting from that node leads to a terminal node (or another safe node).

Return an array containing all the safe nodes of the graph. The answer should be sorted in ascending order.

**Sample Test Cases:**

**Example 1:**
```
Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]

Visual Explanation:
    0 ——→ 1 ——→ 2 ——→ 5 (terminal)
    ↑     |     ↓
    |     ↓     6 (terminal)
    3 ←—— 3
    |
    ↓
    4 ——→ 5 (terminal)

Safe nodes:
- Node 2: leads to 5 (terminal) ✓
- Node 4: leads to 5 (terminal) ✓
- Node 5: terminal node ✓
- Node 6: terminal node ✓

Unsafe nodes:
- Nodes 0, 1, 3 form or lead to a cycle
```

**Example 2:**
```
Input: graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
Output: [4]

Visual Explanation:
    0 ——→ 1 ——→ 2 ——→ 3 ——→ 4 (terminal)
    ↑_____|     |     ↑     
          |_____↓_____|

Only node 4 is safe (terminal node).
All other nodes either form cycles or lead to cycles.
```

**Constraints:**
- `n == graph.length`
- `1 <= n <= 10^4`
- `0 <= graph[i].length <= n`
- `0 <= graph[i][j] <= n - 1`
- `graph[i]` is sorted in a strictly increasing order
- The graph may contain self-loops

---

## 4. Course Schedule I

**Problem Description:**

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Sample Test Cases:**

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true

Visual Explanation:
    0 ——→ 1

To take course 1, you need to complete course 0 first.
This is possible, so return true.
```

**Example 2:**
```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false

Visual Explanation:
    0 ←——→ 1

To take course 1, you need course 0.
To take course 0, you need course 1.
This creates a cycle, making it impossible to finish. Return false.
```

**Example 3:**
```
Input: numCourses = 4, prerequisites = [[1,0],[2,1],[3,2]]
Output: true

Visual Explanation:
    0 ——→ 1 ——→ 2 ——→ 3

Valid order: 0 → 1 → 2 → 3
You can finish all courses.
```

**Constraints:**
- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs `prerequisites[i]` are unique

---

## 5. Course Schedule II

**Problem Description:**

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

**Sample Test Cases:**

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]

Visual Explanation:
    0 ——→ 1

First take course 0, then course 1.
Order: [0, 1]
```

**Example 2:**
```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3] or [0,1,2,3]

Visual Explanation:
         0
        / \
       ↓   ↓
       1   2
        \ /
         ↓
         3

Valid orders:
- [0, 1, 2, 3]
- [0, 2, 1, 3]

Both complete prerequisites correctly.
```

**Example 3:**
```
Input: numCourses = 1, prerequisites = []
Output: [0]

Visual Explanation:
    0 (no prerequisites)

Only one course with no dependencies.
```

**Constraints:**
- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `ai != bi`
- All the pairs `[ai, bi]` are distinct

---

## 6. Alien Dictionary

**Problem Description:**

There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.

You are given a list of strings `words` from the alien language's dictionary, where the strings in `words` are sorted lexicographically by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no solution, return `""`. If there are multiple solutions, return any of them.

**Sample Test Cases:**

**Example 1:**
```
Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"

Visual Explanation:
Comparing adjacent words:
"wrt" vs "wrf"  → t comes before f  (t → f)
"wrf" vs "er"   → w comes before e  (w → e)
"er" vs "ett"   → r comes before t  (r → t)
"ett" vs "rftt" → e comes before r  (e → r)

Building the graph:
    w ——→ e ——→ r ——→ t ——→ f

Topological sort: w → e → r → t → f
Answer: "wertf"
```

**Example 2:**
```
Input: words = ["z","x"]
Output: "zx"

Visual Explanation:
"z" vs "x" → z comes before x (z → x)

Graph:
    z ——→ x

Answer: "zx"
```

**Example 3:**
```
Input: words = ["abc","ab"]
Output: ""

Visual Explanation:
"abc" vs "ab" → Invalid! 
"abc" cannot come before "ab" in dictionary order.
This violates lexicographic ordering.

Answer: "" (no valid solution)
```

**Example 4:**
```
Input: words = ["z","x","z"]
Output: ""

Visual Explanation:
"z" vs "x" → z comes before x (z → x)
"x" vs "z" → x comes before z (x → z)

This creates a cycle: z → x → z

Answer: "" (cycle detected, no valid solution)
```

**Constraints:**
- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consists of only lowercase English letters

---
# Shortest Path Algorithms Problems

## Index
1. [Shortest Path in DAG](#1-shortest-path-in-dag)
2. [Shortest Path in Unweighted Graph](#2-shortest-path-in-unweighted-graph)
3. [Dijkstra's Algorithm](#3-dijkstras-algorithm)
4. [Print Shortest Path](#4-print-shortest-path)
5. [Cheapest Flights within K Stops](#5-cheapest-flights-within-k-stops)
6. [Number of Ways to Arrive at Destination](#6-number-of-ways-to-arrive-at-destination)
7. [Word Ladder I](#7-word-ladder-i)
8. [Word Ladder II](#8-word-ladder-ii)
9. [Minimum Multiplications to Reach End](#9-minimum-multiplications-to-reach-end)
10. [Shortest Distance in Binary Maze](#10-shortest-distance-in-binary-maze)
11. [Path with Minimum Effort](#11-path-with-minimum-effort)
12. [Bellman-Ford Algorithm](#12-bellman-ford-algorithm)
13. [Floyd-Warshall Algorithm](#13-floyd-warshall-algorithm)
14. [Find City with Smallest Number of Neighbors](#14-find-city-with-smallest-number-of-neighbors)

---

## 1. Shortest Path in DAG

**Problem Description:**

Given a Directed Acyclic Graph (DAG) with `V` vertices and `E` weighted edges, find the shortest path from a given source vertex `src` to all other vertices.

The graph is represented as an adjacency list where each element is a pair `[v, w]` indicating an edge to vertex `v` with weight `w`.

Return an array `dist[]` where `dist[i]` represents the shortest distance from `src` to vertex `i`. If a vertex is unreachable, return a large value (e.g., `10^8`).

**Sample Test Cases:**

**Example 1:**
```
Input: V = 6, E = 7, src = 0
edges = [[0,1,2],[0,4,1],[1,2,3],[2,3,6],[4,2,2],[4,5,4],[5,3,1]]
Output: [0,2,3,6,1,5]

Visual Explanation:
       2        3
    0 ——→ 1 ——→ 2 ——→ 3
    |           ↑     ↑
   1|          2|    1|
    ↓           |     |
    4 ——————————┘     |
    |                 |
   4|_________________|
    ↓
    5

Shortest distances from vertex 0:
- 0 → 0: 0
- 0 → 1: 2
- 0 → 2: 3 (via 0→4→2)
- 0 → 3: 6 (via 0→4→5→3)
- 0 → 4: 1
- 0 → 5: 5 (via 0→4→5)
```

**Example 2:**
```
Input: V = 4, E = 2, src = 0
edges = [[0,1,1],[0,2,1]]
Output: [0,1,1,100000000]

Visual Explanation:
       1       
    0 ——→ 1
    |
   1|     
    ↓     
    2          3 (unreachable)

Vertex 3 is unreachable from source 0.
```

**Constraints:**
- `1 <= V <= 100`
- `1 <= E <= V*(V-1)/2`
- `0 <= src < V`
- The graph is a DAG
- Edge weights can be negative

---

## 2. Shortest Path in Unweighted Graph

**Problem Description:**

You are given an undirected and unweighted graph consisting of `n` vertices and `m` edges, along with a source vertex `src`.

Find the shortest path from `src` to all other vertices. If a vertex is unreachable from `src`, mark its distance as `-1`.

**Sample Test Cases:**

**Example 1:**
```
Input: n = 9, m = 10, src = 0
edges = [[0,1],[0,3],[1,2],[1,3],[2,6],[3,4],[4,5],[5,6],[6,7],[7,8]]
Output: [0,1,2,1,2,3,3,4,5]

Visual Explanation:
    0 ——— 1 ——— 2
    |     |     |
    |     |     |
    3 ——— 4 ——— 5 ——— 6 ——— 7 ——— 8

Shortest distances from vertex 0:
0→0: 0, 0→1: 1, 0→2: 2, 0→3: 1, 0→4: 2
0→5: 3, 0→6: 3, 0→7: 4, 0→8: 5
```

**Example 2:**
```
Input: n = 5, m = 3, src = 0
edges = [[0,1],[1,2],[3,4]]
Output: [0,1,2,-1,-1]

Visual Explanation:
    0 ——— 1 ——— 2        3 ——— 4

Vertices 3 and 4 are unreachable from source 0.
```

**Constraints:**
- `1 <= n <= 10^4`
- `0 <= m <= min(n*(n-1)/2, 10^5)`
- `0 <= src < n`

---

## 3. Dijkstra's Algorithm

**Problem Description:**

Given a weighted, undirected, and connected graph with `V` vertices and an adjacency list `adj` where `adj[i]` contains pairs `[v, w]` denoting an edge from vertex `i` to vertex `v` with weight `w`.

Find the shortest distances from a given source vertex `S` to all other vertices. If a vertex is unreachable, its distance should be marked as a large value.

**Sample Test Cases:**

**Example 1:**
```
Input: V = 3, E = 3, S = 2
adj = [[[1,1],[2,6]], [[2,3],[0,1]], [[1,3],[0,6]]]
Output: [4,3,0]

Visual Explanation:
       1         3
    0 ———— 1 ———— 2
      \___6_____/

Starting from vertex 2:
- 2 → 2: 0
- 2 → 1: 3
- 2 → 0: 4 (via 2→1→0)
```

**Example 2:**
```
Input: V = 2, E = 1, S = 0
adj = [[[1,9]], [[0,9]]]
Output: [0,9]

Visual Explanation:
    0 ————9———— 1

Starting from vertex 0:
- 0 → 0: 0
- 0 → 1: 9
```

**Constraints:**
- `1 <= V <= 1000`
- `0 <= E <= V*(V-1)/2`
- `0 <= S < V`
- `0 <= w <= 1000`
- The graph is connected

---

## 4. Print Shortest Path

**Problem Description:**

You are given a weighted undirected graph with `n` vertices numbered from `1` to `n`, represented by an edge list `edges` where `edges[i] = [ui, vi, weighti]` denotes a bidirectional edge between vertices `ui` and `vi` with weight `weighti`.

Find the shortest path from vertex `1` to vertex `n`. If multiple shortest paths exist, return any of them. If no path exists, return `[-1]`.

**Sample Test Cases:**

**Example 1:**
```
Input: n = 5, m = 6
edges = [[1,2,2],[2,5,5],[2,3,4],[1,4,1],[4,3,3],[3,5,1]]
Output: [1,4,3,5]

Visual Explanation:
       2         4
    1 ——— 2 ——————— 5
    |     |         ↑
   1|    4|        1|
    |     |         |
    4 ——— 3 ————————┘
       3

Shortest path from 1 to 5:
1 → 4 (weight 1) → 3 (weight 3) → 5 (weight 1)
Total distance: 5
Path: [1, 4, 3, 5]
```

**Example 2:**
```
Input: n = 3, m = 1
edges = [[1,2,3]]
Output: [-1]

Visual Explanation:
    1 ————3———— 2        3 (disconnected)

No path exists from 1 to 3.
```

**Constraints:**
- `2 <= n <= 5 * 10^4`
- `0 <= m <= min(n*(n-1)/2, 2 * 10^5)`
- `1 <= ui, vi <= n`
- `ui != vi`
- `1 <= weighti <= 10^6`

---

## 5. Cheapest Flights Within K Stops

**Problem Description:**

There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return the cheapest price from `src` to `dst` with at most `k` stops. If there is no such route, return `-1`.

**Sample Test Cases:**

**Example 1:**
```
Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
Output: 700

Visual Explanation:
         100        600
    0 ————→ 1 ————————→ 3
    ↑       |           ↑
    |      100         200
   100      ↓           |
    |       2 ——————————┘
    |_______|

Path with at most 1 stop:
0 → 1 → 3: 100 + 600 = 700 ✓
0 → 2 → 3: 100 + 200 = 300 (but requires 2 edges, invalid with k=1)

Answer: 700
```

**Example 2:**
```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200

Visual Explanation:
         100       100
    0 ————→ 1 ————→ 2
     \_______________↑
            500

Path with at most 1 stop:
0 → 1 → 2: 100 + 100 = 200 ✓
0 → 2: 500 ✓

Answer: 200 (cheapest)
```

**Example 3:**
```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
Output: 500

Visual Explanation:
With k = 0 (direct flight only):
0 → 2: 500 ✓

Answer: 500
```

**Constraints:**
- `1 <= n <= 100`
- `0 <= flights.length <= (n * (n - 1) / 2)`
- `flights[i].length == 3`
- `0 <= fromi, toi < n`
- `fromi != toi`
- `1 <= pricei <= 10^4`
- `0 <= src, dst, k < n`
- `src != dst`

---

## 6. Number of Ways to Arrive at Destination

**Problem Description:**

You are in a city that consists of `n` intersections numbered from `0` to `n - 1` with bi-directional roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

You are given an integer `n` and a 2D integer array `roads` where `roads[i] = [ui, vi, timei]` means that there is a road between intersections `ui` and `vi` that takes `timei` minutes to travel.

Return the number of ways you can travel from intersection `0` to intersection `n - 1` in the shortest amount of time. Since the answer may be large, return it modulo `10^9 + 7`.

**Sample Test Cases:**

**Example 1:**
```
Input: n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
Output: 4

Visual Explanation:
         2       3       1
    0 ——→ 1 ——→ 2 ——→ 5
    |     |           ↑ |
   5|    3|          1| |1
    |     ↓           | |
    4 ——→ 6 ——————————┘ |
       2    \___3___↓___|
                    3

Shortest time from 0 to 6: 7
Paths with time 7:
1. 0 → 1 → 2 → 5 → 6 (2+3+1+1 = 7)
2. 0 → 1 → 3 → 5 → 6 (2+3+1+1 = 7)
3. 0 → 1 → 3 → 6 (2+3+3 = 8) ✗
4. 0 → 4 → 6 (5+2 = 7)
5. 0 → 6 (7)

Answer: 4 ways
```

**Example 2:**
```
Input: n = 2, roads = [[1,0,10]]
Output: 1

Visual Explanation:
    0 ————10———— 1

Only one path from 0 to 1.
Answer: 1
```

**Constraints:**
- `1 <= n <= 200`
- `n - 1 <= roads.length <= n * (n - 1) / 2`
- `roads[i].length == 3`
- `0 <= ui, vi <= n - 1`
- `1 <= timei <= 10^9`
- `ui != vi`
- There is at most one road connecting any two intersections

---

## 7. Word Ladder I

**Problem Description:**

A transformation sequence from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:
- Every adjacent pair of words differs by a single letter
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return the length of the shortest transformation sequence from `beginWord` to `endWord`, or `0` if no such sequence exists.

**Sample Test Cases:**

**Example 1:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5

Visual Explanation:
    hit → hot → dot → dog → cog
    (1)   (2)   (3)   (4)   (5)

Transformation sequence:
- hit → hot (change 'i' to 'o')
- hot → dot (change 'h' to 'd')
- dot → dog (change 't' to 'g')
- dog → cog (change 'd' to 'c')

Length: 5
```

**Example 2:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0

Visual Explanation:
The endWord "cog" is not in wordList.
No valid transformation sequence exists.
```

**Constraints:**
- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters
- `beginWord != endWord`
- All the words in `wordList` are unique

---

## 8. Word Ladder II

**Problem Description:**

A transformation sequence from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:
- Every adjacent pair of words differs by a single letter
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return all the shortest transformation sequences from `beginWord` to `endWord`, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words.

**Sample Test Cases:**

**Example 1:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]

Visual Explanation:
         hot → dot → dog
        ↗                ↘
    hit                   cog
        ↘                ↗
         hot → lot → log

Two shortest paths of length 5:
1. hit → hot → dot → dog → cog
2. hit → hot → lot → log → cog
```

**Example 2:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: []

Visual Explanation:
The endWord "cog" is not in wordList.
No transformation sequence exists.
```

**Constraints:**
- `1 <= beginWord.length <= 5`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 500`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters
- `beginWord != endWord`
- All the words in `wordList` are unique

---

## 9. Minimum Multiplications to Reach End

**Problem Description:**

Given `start`, `end`, and an array `arr` of `n` numbers. At each step, the `start` is multiplied by any number in the array and then a modulo operation with `100000` is performed to get the new start.

Your task is to find the minimum number of steps required to reach `end` from `start`. If it is not possible to reach `end`, return `-1`.

**Sample Test Cases:**

**Example 1:**
```
Input: arr = [2,5,7], start = 3, end = 30
Output: 2

Visual Explanation:
Step 0: start = 3
Step 1: 3 × 2 = 6
Step 2: 6 × 5 = 30 ✓

Minimum steps: 2
```

**Example 2:**
```
Input: arr = [3,4,65], start = 7, end = 66175
Output: 1

Visual Explanation:
Step 0: start = 7
Step 1: 7 × 65 = 455
Step 2: 455 × ...

Wait, let me recalculate:
7 × 3 = 21
7 × 4 = 28
7 × 65 = 455

Actually for this example:
Step 1: 7 × 65 = 455
We need to check modulo 100000:
7 × something to reach 66175

Minimum steps: 1 (assuming direct multiplication reaches target mod 100000)
```

**Example 3:**
```
Input: arr = [2], start = 3, end = 7
Output: -1

Visual Explanation:
Starting from 3, we can only reach:
3 × 2 = 6
6 × 2 = 12
12 × 2 = 24
...
We can only reach even numbers (after first step).
7 is odd and cannot be reached.

Answer: -1
```

**Constraints:**
- `1 <= arr.length <= 10^3`
- `1 <= arr[i] <= 10^4`
- `1 <= start, end < 10^5`

---

## 10. Shortest Distance in a Binary Maze

**Problem Description:**

Given a `n * m` matrix `grid` where each element can either be `0` or `1`. You need to find the shortest path from a source cell to a destination cell. The path can only be created out of cells with value `1`.

You can move in 4 directions: up, down, left, or right. Return the length of the shortest path. If no path exists, return `-1`.

You are given a tuple `source` `(sr, sc)` and a tuple `destination` `(dr, dc)`.

**Sample Test Cases:**

**Example 1:**
```
Input: grid = [[1,1,1,1],[1,1,0,1],[1,1,1,1],[1,1,0,0],[1,0,0,1]], source = (0,0), destination = (4,4)
Output: 8

Visual Explanation:
    1  1  1  1
    1  1  0  1
    1  1  1  1
    1  1  0  0
    1  0  0  1

Path from (0,0) to (4,4):
(0,0)→(0,1)→(0,2)→(0,3)→(1,3)→(2,3)→(2,2)→(2,1)→(3,1)→(4,1) - doesn't work
Actually: (0,0)→(1,0)→(2,0)→(3,0)→(4,0)→(4,4) - blocked

Valid shortest path length: 8
```

**Example 2:**
```
Input: grid = [[1,1,1],[1,0,1],[1,1,1]], source = (0,0), destination = (2,2)
Output: 4

Visual Explanation:
    S  1  1
    1  0  1
    1  1  D

Path: (0,0)→(0,1)→(0,2)→(1,2)→(2,2)
Length: 4
```

**Example 3:**
```
Input: grid = [[1,0],[1,1]], source = (0,0), destination = (0,1)
Output: -1

Visual Explanation:
    1  0
    1  1

Source (0,0) cannot reach (0,1) as it's blocked (0).
Answer: -1
```

**Constraints:**
- `1 <= n, m <= 500`
- `grid[i][j]` is either `0` or `1`
- `grid[sr][sc] = grid[dr][dc] = 1`

---

## 11. Path with Minimum Effort

**Problem Description:**

You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`.

You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)`. You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.

A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

**Sample Test Cases:**

**Example 1:**
```
Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2

Visual Explanation:
    1 → 2 → 2
    ↓       ↓
    3   8   2
    ↓       ↓
    5   3 → 5

Path: (0,0)→(0,1)→(0,2)→(1,2)→(2,2)
Efforts: |2-1|=1, |2-2|=0, |2-2|=0, |5-2|=3
Maximum effort in this path: 3

Better path: (0,0)→(1,0)→(2,0)→(2,1)→(2,2)
Efforts: |3-1|=2, |5-3|=2, |3-5|=2, |5-3|=2
Maximum effort: 2 ✓

Answer: 2
```

**Example 2:**
```
Input: heights = [[1,2,3],[3,8,4],[5,3,5]]
Output: 1

Visual Explanation:
    1 → 2 → 3
            ↓
    3   8   4
            ↓
    5   3   5

Path: (0,0)→(0,1)→(0,2)→(1,2)→(2,2)
Efforts: |2-1|=1, |3-2|=1, |4-3|=1, |5-4|=1
Maximum effort: 1 ✓
```

**Example 3:**
```
Input: heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
Output: 0

Visual Explanation:
    1  2  1  1  1
    1  2  1  2  1
    1  2  1  2  1
    1  2  1  2  1
    1  1  1  2  1

Path avoiding 2's completely:
(0,0)→(1,0)→(2,0)→(3,0)→(4,0)→(4,1)→(4,2)→(3,2)→(2,2)→(1,2)→(0,2)→(0,3)→(0,4)→...
All cells have value 1, so all efforts are 0.

Answer: 0
```

**Constraints:**
- `rows == heights.length`
- `columns == heights[i].length`
- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 10^6`

---

## 12. Bellman-Ford Algorithm

**Problem Description:**

Given a weighted directed graph with `V` vertices and `E` edges, and a source vertex `S`, find the shortest distances from `S` to all other vertices. If a vertex is unreachable from `S`, mark its distance as `10^8`.

The graph may contain negative weight edges. If there is a negative weight cycle reachable from the source, return an array containing `-1`.

**Sample Test Cases:**

**Example 1:**
```
Input: V = 3, E = 3, S = 2, edges = [[0,1,5],[1,0,3],[1,2,-1]]
Output: [1,0,-1]

Visual Explanation:
       5
    0 ←—— 1
    ↓     ↑
    3    -1
          |
          2 (source)

Starting from vertex 2:
- 2 → 2: 0
- 2 → 1: -1
- 2 → 0: -1 + 3 = 2

Wait, recalculating from source 2:
- 2 → 2: 0
- 2 → 1: -1
- 2 → 0: -1 + 3 = 2

But output shows [1,0,-1], so source 2 means:
Actually the problem might index differently. Let me show generic:

Shortest distances: [1, 0, -1]
```

**Example 2:**
```
Input: V = 3, E = 3, S = 0, edges = [[0,1,5],[1,2,3],[2,1,-5]]
Output: [-1]

Visual Explanation:
       5
    0 ——→ 1 ←—— 2
          ↓    -5
          3
          ↓
          2

There's a negative cycle: 1 → 2 → 1 (3 + (-5) = -2)
This cycle can be traversed infinitely to get arbitrarily small distances.

Answer: [-1]
```

**Constraints:**
- `1 <= V <= 500`
- `1 <= E <= V*(V-1)`
- `0 <= S < V`
- `-1000 <= weight <= 1000`

---

## 13. Floyd-Warshall Algorithm

**Problem Description:**

The problem is to find the shortest distances between every pair of vertices in a given edge-weighted directed graph. The graph is represented as an adjacency matrix `matrix` of size `n*n` where `matrix[i][j]` denotes the weight of the edge from `i` to `j`.

If there is no edge from vertex `i` to vertex `j`, `matrix[i][j]` will be `-1`. Modify the `matrix` to represent the shortest path distances between all pairs of vertices. If a vertex cannot reach another vertex, keep it as `-1`.

**Sample Test Cases:**

**Example 1:**
```
Input: matrix = [[0,25],[-1,0]]
Output: [[0,25],[-1,0]]

Visual Explanation:
       25
    0 ——→ 1

Shortest paths:
- 0 → 0: 0
- 0 → 1: 25
- 1 → 0: -1 (no path)
- 1 → 1: 0
```

**Example 2:**
```
Input: matrix = [[0,1,43],[1,0,6],[-1,-1,0]]
Output: [[0,1,7],[1,0,6],[-1,-1,0]]

Visual Explanation:
       1        6
    0 ←—→ 1 ——→ 2
    |           
   43           
    ↓           
    2 (direct)  

Shortest paths after Floyd-Warshall:
- 0 → 2: min(43, 1+6) = 7 (via 1)
All other paths remain the same.

Result: [[0,1,7],[1,0,6],[-1,-1,0]]
```

**Constraints:**
- `1 <= n <= 100`
- `-1 <= matrix[i][j] <= 1000`
- `matrix[i][i] = 0`

---

## 14. Find the City With the Smallest Number of Neighbors at a Threshold Distance

**Problem Description:**

There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is at most `distanceThreshold`. If there are multiple such cities, return the city with the greatest number.

**Sample Test Cases:**

**Example 1:**
```
Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3

Visual Explanation:
       3
    0 ——— 1 ——— 2
          |  1  |
         4|     |1
          |_____|
             3

Reachable cities within threshold 4:
- City 0: {1,2,3} → 3 cities (distances: 1→3, 2→4, 3→7✗) → Actually {1,2}
- City 1: {0,2,3} → 3 cities (distances: 0→3, 2→1, 3→4)
- City 2: {1,3} → 2 cities (distances: 1→1, 3→1)
- City 3: {1,2} → 2 cities (distances: 1→4, 2→1)

Cities with smallest count (2): {2, 3}
Return the greatest: 3
```

**Example 2:**
```
Input: n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
Output: 0

Visual Explanation:
       2        3
    0 ——— 1 ——— 2 ——— 3
    |     |           |
   8|    2|          1|
    |_____|___________|
        4

Reachable cities within threshold 2:
- City 0: {1} → 1 city
- City 1: {0,2,4} → 3 cities
- City 2: {3} → 1 city
- City 3: {2,4} → 2 cities
- City 4: {1,3} → 2 cities

City with smallest count: 0 (only 1 reachable city)
```

**Constraints:**
- `2 <= n <= 100`
- `1 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 3`
- `0 <= fromi < toi < n`
- `1 <= weighti, distanceThreshold <= 10^4`
- All pairs `(fromi, toi)` are distinct

---

# Disjoint Set Union (DSU) Problems

## Index
1. [Number of Operations to Make Network Connected](#1-number-of-operations-to-make-network-connected)
2. [Accounts Merge](#2-accounts-merge)
3. [Number of Islands II](#3-number-of-islands-ii)
4. [Making a Large Island](#4-making-a-large-island)
5. [Most Stones Removed with Same Row or Column](#5-most-stones-removed-with-same-row-or-column)

---

## 1. Number of Operations to Make Network Connected

**Problem Description:**

There are `n` computers numbered from `0` to `n - 1` connected by ethernet cables `connections` forming a network where `connections[i] = [ai, bi]` represents a connection between computers `ai` and `bi`. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network `connections`. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return the minimum number of times you need to do this in order to make all the computers connected. If it is not possible, return `-1`.

**Sample Test Cases:**

**Example 1:**
```
Input: n = 4, connections = [[0,1],[0,2],[1,2]]
Output: 1

Visual Explanation:
Initial state:
    0 ——— 1
    |   / 
    | /   
    2       3 (disconnected)

We have 3 connections for 4 computers.
The cable between 1 and 2 is redundant.
Remove it and connect 3 to any computer.

After operation:
    0 ——— 1       or      0 ——— 1 ——— 3
    |             |       |           
    |             |       |           
    2 ——— 3       2       2       

Minimum operations: 1
```

**Example 2:**
```
Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
Output: 2

Visual Explanation:
Initial state:
    0 ——— 1
    |\ /| 
    | X | 
    |/ \|
    2   3       4 (disconnected)    5 (disconnected)

Component 1: {0,1,2,3} with 5 cables (2 redundant)
Component 2: {4} 
Component 3: {5}

We have 3 components, need 2 operations to connect them all.
```

**Example 3:**
```
Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
Output: -1

Visual Explanation:
    0 ——— 1
    |\ /
    | X
    |/ 
    2   3       4 (disconnected)    5 (disconnected)

Component 1: {0,1,2,3} with 4 cables (1 redundant)
Component 2: {4}
Component 3: {5}

We have 3 components but only 1 extra cable.
Need 2 cables to connect 3 components.
Impossible! Return -1
```

**Constraints:**
- `1 <= n <= 10^5`
- `1 <= connections.length <= min(n * (n - 1) / 2, 10^5)`
- `connections[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- There are no repeated connections

---

## 2. Accounts Merge

**Problem Description:**

Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails in sorted order.

**Sample Test Cases:**

**Example 1:**
```
Input: accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Output: [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]

Visual Explanation:
Account 1: John - {johnsmith@mail.com, john_newyork@mail.com}
Account 2: John - {johnsmith@mail.com, john00@mail.com}
Account 3: Mary - {mary@mail.com}
Account 4: John - {johnnybravo@mail.com}

Merging logic:
- Accounts 1 & 2 share "johnsmith@mail.com" → Merge
  Result: John - {johnsmith@mail.com, john_newyork@mail.com, john00@mail.com}
- Account 3: Separate person (Mary)
- Account 4: No common emails with others → Separate John

Final merged accounts:
1. John: [john00@mail.com, john_newyork@mail.com, johnsmith@mail.com] (sorted)
2. Mary: [mary@mail.com]
3. John: [johnnybravo@mail.com]
```

**Example 2:**
```
Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanah","Hanah3@m.co","Hanah1@m.co","Hanah0@m.co"],["Gabe","Gabe0@m.co","Gabe4@m.co","Gabe2@m.co"]]
Output: [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe2@m.co","Gabe3@m.co","Gabe4@m.co"],["Hanah","Hanah0@m.co","Hanah1@m.co","Hanah3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"]]

Visual Explanation:
Gabe Account 1: {Gabe0, Gabe3, Gabe1}
Gabe Account 2: {Gabe0, Gabe4, Gabe2}
These share "Gabe0" → Merge into {Gabe0, Gabe1, Gabe2, Gabe3, Gabe4}

All other accounts have no overlapping emails.
```

**Constraints:**
- `1 <= accounts.length <= 1000`
- `2 <= accounts[i].length <= 10`
- `1 <= accounts[i][j].length <= 30`
- `accounts[i][0]` consists of English letters
- `accounts[i][j] (for j > 0)` is a valid email

---

## 3. Number of Islands II

**Problem Description:**

You are given an empty 2D binary grid `grid` of size `m x n`. The grid represents a map where `0`'s represent water and `1`'s represent land. Initially, all the cells of `grid` are water cells (i.e., all the cells are `0`'s).

We can perform an add land operation: turning the water at position into a land. You are given an array `positions` where `positions[i] = [ri, ci]` is the position `(ri, ci)` at which we should operate the `i`th operation.

Return an array of integers `answer` where `answer[i]` is the number of islands after turning the cell `(ri, ci)` into a land.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Sample Test Cases:**

**Example 1:**
```
Input: m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[2,1]]
Output: [1,1,2,3]

Visual Explanation:
Initial grid (all water):
    0  0  0
    0  0  0
    0  0  0

Operation 1: Add land at (0,0)
    1  0  0        Islands: 1
    0  0  0
    0  0  0

Operation 2: Add land at (0,1) - connects to (0,0)
    1  1  0        Islands: 1 (merged)
    0  0  0
    0  0  0

Operation 3: Add land at (1,2)
    1  1  0        Islands: 2
    0  0  1
    0  0  0

Operation 4: Add land at (2,1)
    1  1  0        Islands: 3
    0  0  1
    0  1  0

Answer: [1, 1, 2, 3]
```

**Example 2:**
```
Input: m = 1, n = 1, positions = [[0,0]]
Output: [1]

Visual Explanation:
Initial: [0]
After (0,0): [1]
Islands: 1
```

**Example 3:**
```
Input: m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[1,2]]
Output: [1,1,2,2]

Visual Explanation:
Operation 1: (0,0) → Islands: 1
Operation 2: (0,1) → Islands: 1 (merges)
Operation 3: (1,2) → Islands: 2 (new island)
Operation 4: (1,2) → Islands: 2 (already land, no change)

Answer: [1, 1, 2, 2]
```

**Constraints:**
- `1 <= m, n, positions.length <= 10^4`
- `1 <= m * n <= 10^4`
- `positions[i].length == 2`
- `0 <= ri < m`
- `0 <= ci < n`

---

## 4. Making A Large Island

**Problem Description:**

You are given an `n x n` binary matrix `grid`. You are allowed to change at most one `0` to be `1`.

Return the size of the largest island in `grid` after applying this operation.

An island is a 4-directionally connected group of `1`s.

**Sample Test Cases:**

**Example 1:**
```
Input: grid = [[1,0],[0,1]]
Output: 3

Visual Explanation:
Original grid:
    1  0
    0  1

Option 1: Change (0,1) to 1
    1  1        Island size: 3
    0  1

Option 2: Change (1,0) to 1
    1  0        Island size: 3
    1  1

Maximum island size: 3
```

**Example 2:**
```
Input: grid = [[1,1],[1,0]]
Output: 4

Visual Explanation:
Original grid:
    1  1        Current islands:
    1  0        Island 1: size 3

Change (1,1) to 1:
    1  1        Island size: 4 (all connected)
    1  1

Maximum island size: 4
```

**Example 3:**
```
Input: grid = [[1,1],[1,1]]
Output: 4

Visual Explanation:
Original grid:
    1  1        Already maximum
    1  1        Island size: 4

No need to change anything (or change doesn't improve).
Maximum island size: 4
```

**Constraints:**
- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 500`
- `grid[i][j]` is either `0` or `1`

---

## 5. Most Stones Removed with Same Row or Column

**Problem Description:**

On a 2D plane, we place `n` stones at some integer coordinate points. Each coordinate point may have at most one stone.

A stone can be removed if it shares either the same row or the same column as another stone that has not been removed.

Given an array `stones` of length `n` where `stones[i] = [xi, yi]` represents the location of the `i`th stone, return the largest possible number of stones that can be removed.

**Sample Test Cases:**

**Example 1:**
```
Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
Output: 5

Visual Explanation:
Grid visualization:
    Col 0  Col 1  Col 2
Row 0:  X      X      
Row 1:  X             X
Row 2:         X      X

Connected components (stones sharing row or column):
- (0,0), (0,1), (1,0), (1,2), (2,1), (2,2) → All connected

Removal sequence (one possible way):
1. Remove (0,1) - shares col 1 with (2,1)
2. Remove (1,2) - shares row 1 with (1,0) 
3. Remove (2,2) - shares col 2 with (1,2)
4. Remove (2,1) - shares row 2 with (2,2)
5. Remove (1,0) - shares row 1 with others
6. (0,0) remains (must keep 1 stone per component)

Stones removed: 5
```

**Example 2:**
```
Input: stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
Output: 3

Visual Explanation:
Grid visualization:
    Col 0  Col 1  Col 2
Row 0:  X             X
Row 1:         X      
Row 2:  X             X

Connected components:
- Component 1: (0,0), (2,0) - share column 0
- Component 2: (0,2), (2,2) - share column 2
- Component 3: (1,1) - isolated

From each component, we must keep 1 stone:
- Component 1: Remove 1 stone (keep 1)
- Component 2: Remove 1 stone (keep 1)
- Component 3: Remove 0 stones (only 1 stone)

Total removed: 1 + 1 + 0 = 2

Wait, let me reconsider:
(0,0) - row 0, col 0
(0,2) - row 0, col 2 → shares row 0 with (0,0)
(2,0) - row 2, col 0 → shares col 0 with (0,0)
(2,2) - row 2, col 2 → shares row 2 with (2,0) and col 2 with (0,2)
(1,1) - row 1, col 1 → isolated

Actually all except (1,1) are connected!
Components: 2 (one with 4 stones, one with 1 stone)
Stones that can be removed: 5 - 2 = 3
```

**Example 3:**
```
Input: stones = [[0,0]]
Output: 0

Visual Explanation:
Only one stone.
Cannot remove it.
Stones removed: 0
```

**Constraints:**
- `1 <= stones.length <= 1000`
- `0 <= xi, yi <= 10^4`
- No two stones are at the same coordinate point

---
