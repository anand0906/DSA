# Disjoint Set Union (DSU) - Revision Notes

## Table of Contents
1. [Number of Operations to Make Network Connected](#1-number-of-operations-to-make-network-connected)
2. [Accounts Merge](#2-accounts-merge)
3. [Number of Islands II](#3-number-of-islands-ii)
4. [Making a Large Island](#4-making-a-large-island)
5. [Most Stones Removed with Same Row or Column](#5-most-stones-removed-with-same-row-or-column)
6. [Problems Comparison](#problems-comparison)

---

## 1. Number of Operations to Make Network Connected

### Problem Description
Given a network with `n` computers and cables (edges), find the minimum operations needed to make all computers connected. You can move cables between computers. Return -1 if impossible.

### Sample Test Cases

**Example 1:**
```
Input: n = 4, edges = [[0,1], [0,2], [1,2]]
Output: 1
Explanation: Remove extra cable (1,2) and use it to connect computer 3
```

**Example 2:**
```
Input: n = 9, edges = [[0,1],[0,2],[0,3],[1,2],[2,3],[4,5],[5,6],[7,8]]
Output: 2
Explanation: 3 separate groups need 2 bridges to connect all
```

### Approach: Union-Find

**Intuition:**
- To connect k separate components, you need exactly (k-1) bridges
- A connected graph with n nodes needs at least (n-1) edges
- If total edges < (n-1), it's impossible
- Count connected components using DSU, answer = components - 1

**Code:**
```python
class DisjointSet:
    def __init__(self, n: int):
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return
        
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]

def solution(n, edges):
    # Check if enough edges exist
    if len(edges) < n - 1:
        return -1
    
    ds = DisjointSet(n)
    
    # Union all edges
    for u, v in edges:
        ds.union_by_size(u, v)
    
    # Count components (nodes that are their own parent)
    connected_components = 0
    for node in range(n):
        if ds.find(node) == node:
            connected_components += 1
    
    return connected_components - 1
```

**Complexity:**
- **Time:** O(n + m × α(n)) ≈ O(n + m) where α is inverse Ackermann function
- **Space:** O(n) for DSU arrays

---

## 2. Accounts Merge

### Problem Description
Merge accounts belonging to the same person. Two accounts belong to the same person if they share at least one common email. People can have the same name but different accounts.

### Sample Test Cases

**Example 1:**
```
Input: accounts = [
    ["John", "johnsmith@mail.com", "john_newyork@mail.com"],
    ["John", "johnsmith@mail.com", "john00@mail.com"], 
    ["Mary", "mary@mail.com"],
    ["John", "johnnybravo@mail.com"]
]

Output: [
    ["John", "john00@mail.com", "john_newyork@mail.com", "johnsmith@mail.com"],
    ["Mary", "mary@mail.com"],
    ["John", "johnnybravo@mail.com"]
]
```

### Approach: Union-Find with Email Mapping

**Intuition:**
- Map each email to the account index where it first appears
- When same email appears again, union those account indices
- After processing, group emails by their ultimate parent account
- Shared emails create bridges between accounts

**Code:**
```python
class DisjointSet:
    def __init__(self, n: int):
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return
        
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]

class Solution:
    def accountsMerge(self, accounts):
        n = len(accounts)
        email_to_account = {}  # email -> (account_index, name)
        ds = DisjointSet(n)
        
        # Map emails and union accounts with common emails
        for i in range(n):
            name = accounts[i][0]
            for j in range(1, len(accounts[i])):
                email = accounts[i][j]
                
                if email in email_to_account:
                    existing_account = email_to_account[email][0]
                    ds.union_by_size(existing_account, i)
                else:
                    email_to_account[email] = (i, name)
        
        # Group emails by parent account
        merged_accounts = {}
        account_names = {}
        
        for email, (account_index, name) in email_to_account.items():
            parent = ds.find(account_index)
            
            if parent not in merged_accounts:
                merged_accounts[parent] = []
                account_names[parent] = name
            
            merged_accounts[parent].append(email)
        
        # Format result
        result = []
        for parent in merged_accounts:
            email_list = sorted(merged_accounts[parent])
            result.append([account_names[parent]] + email_list)
        
        return result
```

**Complexity:**
- **Time:** O(N × E × log(E)) where N = accounts, E = average emails per account
- **Space:** O(N + E) for DSU and email mappings

---

## 3. Number of Islands II

### Problem Description
Given an n×m grid (initially all water), perform k operations that convert water to land. After each operation, count the number of islands. Return array of island counts.

### Sample Test Cases

**Example 1:**
```
Input: n = 4, m = 5, k = 4
       operations = [[1,1], [0,1], [3,3], [3,4]]
Output: [1, 1, 2, 2]

Explanation:
- After [1,1]: 1 island
- After [0,1]: connects to existing, still 1 island
- After [3,3]: new separate island, 2 islands
- After [3,4]: connects to [3,3], still 2 islands
```

### Approach: Dynamic DSU

**Intuition:**
- Start with island count = 0
- Each new land cell initially adds 1 to count
- Check 4 neighbors; if neighbor is land and not already connected, merge and decrement count
- Formula: island_count = total_land_cells - number_of_successful_unions

**Code:**
```python
class DisjointSet:
    def __init__(self, n: int):
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int) -> bool:
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return False  # already connected
        
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
        
        return True  # successful union

class Solution:
    def numOfIslands(self, n, m, operations):
        ds = DisjointSet(n * m)
        visited = [[False] * m for _ in range(n)]
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        
        island_count = 0
        result = []
        
        for row, col in operations:
            if visited[row][col]:
                result.append(island_count)
                continue
            
            visited[row][col] = True
            island_count += 1  # new land cell
            
            current_node = row * m + col
            
            # Check 4 neighbors
            for dr, dc in directions:
                new_row, new_col = row + dr, col + dc
                
                if (0 <= new_row < n and 0 <= new_col < m and 
                    visited[new_row][new_col]):
                    
                    neighbor_node = new_row * m + new_col
                    
                    # Union and decrement if successful merge
                    if ds.union_by_size(current_node, neighbor_node):
                        island_count -= 1
            
            result.append(island_count)
        
        return result
```

**Complexity:**
- **Time:** O(K × α(N×M)) ≈ O(K) where K = operations
- **Space:** O(N×M + K) for DSU, visited matrix, and result

---

## 4. Making a Large Island

### Problem Description
Given an n×n binary grid, change at most one 0 to 1. Find the size of the largest possible island.

### Sample Test Cases

**Example 1:**
```
Input: grid = [[1,0],[0,1]]
Output: 3
Explanation: Change any 0 to connect both 1s into one island of size 3
```

**Example 2:**
```
Input: grid = [[1,1],[1,1]]
Output: 4
Explanation: Already all land, maximum possible
```

### Approach: Pre-compute Island Sizes

**Intuition:**
- Build all existing islands using DSU
- For each 0, calculate potential island size = 1 + sum of unique neighboring island sizes
- Use set to track unique parent islands (avoid double counting)
- Handle edge case: if no 0s exist, return n×n

**Code:**
```python
class DisjointSet:
    def __init__(self, n: int):
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return False
        
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
        
        return True

class Solution:
    def largestIsland(self, grid):
        n = len(grid)
        ds = DisjointSet(n * n)
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        
        # Build initial islands
        for row in range(n):
            for col in range(n):
                if grid[row][col] == 1:
                    current_node = row * n + col
                    
                    for dr, dc in directions:
                        new_row, new_col = row + dr, col + dc
                        
                        if (0 <= new_row < n and 0 <= new_col < n and 
                            grid[new_row][new_col] == 1):
                            neighbor_node = new_row * n + new_col
                            ds.union_by_size(current_node, neighbor_node)
        
        # Try converting each 0 to 1
        max_island_size = 0
        has_water = False
        
        for row in range(n):
            for col in range(n):
                if grid[row][col] == 0:
                    has_water = True
                    unique_parents = set()
                    potential_size = 1
                    
                    for dr, dc in directions:
                        new_row, new_col = row + dr, col + dc
                        
                        if (0 <= new_row < n and 0 <= new_col < n and 
                            grid[new_row][new_col] == 1):
                            neighbor_node = new_row * n + new_col
                            parent = ds.find(neighbor_node)
                            unique_parents.add(parent)
                    
                    for parent in unique_parents:
                        potential_size += ds.size[parent]
                    
                    max_island_size = max(max_island_size, potential_size)
        
        return n * n if not has_water else max_island_size
```

**Complexity:**
- **Time:** O(N² × α(N²)) ≈ O(N²)
- **Space:** O(N²) for DSU arrays

---

## 5. Most Stones Removed with Same Row or Column

### Problem Description
Given n stones on a 2D plane, remove maximum stones possible. A stone can be removed if it shares the same row OR column with another stone that hasn't been removed.

### Sample Test Cases

**Example 1:**
```
Input: n = 6, stones = [[0,0], [0,1], [1,0], [1,2], [2,1], [2,2]]
Output: 5
Explanation: Can remove 5 stones, must leave 1 per connected component
```

**Example 2:**
```
Input: n = 6, stones = [[0,0], [0,2], [1,3], [3,1], [3,2], [4,3]]
Output: 4
Explanation: 2 separate groups, leave 1 stone per group
```

### Approach: Row-Column Union

**Intuition:**
- Key formula: Max removable = Total stones - Number of components
- Instead of connecting stones directly, connect rows to columns
- Map: Row i → Node i, Column j → Node (maxRow + 1 + j)
- For stone at (r,c), union row node with column node
- This creates transitive connections between all stones in same component

**Code:**
```python
class DisjointSet:
    def __init__(self, n: int):
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return False
        
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
        
        return True

class Solution:
    def maxRemove(self, stones, n):
        max_row = max_col = 0
        for row, col in stones:
            max_row = max(max_row, row)
            max_col = max(max_col, col)
        
        ds = DisjointSet(max_row + max_col + 2)
        stone_nodes = set()
        
        # Union rows with columns
        for row, col in stones:
            row_node = row
            col_node = max_row + 1 + col
            ds.union_by_size(row_node, col_node)
            stone_nodes.add(row_node)
            stone_nodes.add(col_node)
        
        # Count unique components
        unique_components = len(set(ds.find(node) for node in stone_nodes))
        
        return n - unique_components
```

**Complexity:**
- **Time:** O(N × α(R+C)) ≈ O(N) where R = max row, C = max column
- **Space:** O(R + C + N) for DSU and tracking sets

---

## Problems Comparison

| Problem | Key Formula | Unique Technique | Time | Space |
|---------|-------------|------------------|------|-------|
| **Network Connected** | Operations = Components - 1 | Count root nodes | O(n+m) | O(n) |
| **Accounts Merge** | Email → Account mapping | Hash map + Union | O(N×E×log E) | O(N+E) |
| **Islands II** | Count = Land - Unions | Dynamic island tracking | O(K) | O(N×M) |
| **Large Island** | Size = 1 + Σ neighbors | Pre-compute + Simulate | O(N²) | O(N²) |
| **Stones Removed** | Remove = Total - Components | Row-Column union trick | O(N) | O(R+C) |

### Common Patterns
- **All use DSU/Union-Find** for grouping elements efficiently
- **Component counting** is central: answer often = total - components
- **Union optimization**: union_by_size with path compression
- **Coordinate mapping**: Convert 2D grid to 1D nodes using `row * cols + col`
- **Neighbor checking**: 4-directional traversal for grid problems
