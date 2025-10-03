# Number of Operations to Make Network Connected

## Problem Description

You have a network with `n` computers (vertices) and some cables (edges) connecting them. You can remove any cable and place it between any two computers in one operation. Find the minimum number of operations needed to make all computers connected to each other. If it's impossible, return -1.

Think of it like having several groups of computers that can talk to each other within their group, but different groups can't communicate. You need to "bridge" these groups by moving cables around.

## Examples

### Input
```
n = 4, edges = [[0,1], [0,2], [1,2]]
```
<img src="https://static.takeuforward.org/content/ProblemSetter-c_c-xpxh" />

### Output
```
1
```

### Explanation
Initially, we have computers 0, 1, 2 connected to each other, but computer 3 is isolated. We have 3 cables but only need 2 to keep computers 0, 1, 2 connected. So we can remove the "extra" cable (1,2) and use it to connect computer 3 to the main group.

**Visual representation:**
```
Before:     After 1 operation:
0---1       0---1
|\ /|       |   |
| X |  →    |   |
|/ \|       |   |
2   3       2---3
(isolated)  (all connected)
```

### Input
```
n = 9, edges = [[0,1],[0,2],[0,3],[1,2],[2,3],[4,5],[5,6],[7,8]]
```
<img src="https://static.takeuforward.org/content/ProblemSetter-MYcPKA8W" />
### Output
```
2
```

### Explanation
We have 3 separate groups: {0,1,2,3}, {4,5,6}, and {7,8}. We need 2 operations to connect all 3 groups together.

## Solution

Use Union-Find (Disjoint Set) to identify connected components, then calculate how many "bridges" we need to connect all components. We need exactly `(number of components - 1)` operations.

## Intuition

**Key Insight:** To connect `k` separate islands, you need exactly `k-1` bridges.

### Detailed Diagram - Why This Works

```
Case 1: Sufficient edges available
Components: A, B, C (3 components)
Need: 3-1 = 2 bridges

A: 0---1---2    B: 3---4    C: 5
   (3 nodes,       (2 nodes,   (1 node,
    2 edges)       1 edge)     0 edges)

Total edges: 2 + 1 + 0 = 3 edges
Minimum needed for connectivity: 3 components - 1 = 2 bridges
Extra edges available: 3 - 2 = 1 extra edge

Since we have at least 2 total edges, we can:
1. Remove 1 extra edge from component A: 0---2
2. Use it to connect A to B: A---B
3. Remove 1 edge from component B: 3---4  
4. Use it to connect B to C: B---C

Result: A---B---C (all connected with 2 operations)
```

```
Case 2: Insufficient edges
Components: A, B, C, D (4 components)  
Each component has only 1 node: 0, 1, 2, 3
Total edges: 0
Need: 4-1 = 3 bridges
Available: 0 edges

Since 0 < 3, it's impossible → return -1
```

### Union-Find Visualization

```
Initial state (n=6, edges=[[0,1],[2,3],[4,5]]):
Parent: [0, 1, 2, 3, 4, 5]
         0  1  2  3  4  5

After processing edge [0,1]:
Parent: [0, 0, 2, 3, 4, 5]
         0  1  2  3  4  5
Component 1: {0,1}

After processing edge [2,3]:
Parent: [0, 0, 2, 2, 4, 5]
         0  1  2  3  4  5
Component 1: {0,1}, Component 2: {2,3}

After processing edge [4,5]:
Parent: [0, 0, 2, 2, 4, 4]
         0  1  2  3  4  5
Component 1: {0,1}, Component 2: {2,3}, Component 3: {4,5}

Counting components (nodes that are their own parent):
- Node 0: parent[0] = 0 ✓ (component root)
- Node 2: parent[2] = 2 ✓ (component root)  
- Node 4: parent[4] = 4 ✓ (component root)

Total components: 3
Operations needed: 3 - 1 = 2
```

## Approach Steps

1. **Feasibility Check**: If total edges < (n-1), return -1
   - A connected graph with n nodes needs at least n-1 edges
   
2. **Initialize Union-Find**: Create disjoint set for n vertices

3. **Process All Edges**: For each edge [u,v], union u and v
   - This groups vertices into connected components
   
4. **Count Components**: Count vertices that are their own parent
   - These represent the "root" of each connected component
   
5. **Calculate Operations**: Return (number of components - 1)
   - This is the minimum bridges needed to connect all components

## Code

```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set (Union-Find) for n elements.
        Each node is its own parent initially, with size = 1.
        """
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        """
        Find the ultimate parent (representative) of a node.
        Uses path compression optimization.
        """
        if node == self.parent[node]:
            return node
        # Path compression: directly connect node to its ultimate parent
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        """
        Union two sets by size.
        Attach the smaller tree under the larger one.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return  # already in the same set
        
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]

def solution(n, edges):
    """
    Find minimum operations to make network connected.
    
    Args:
        n: number of vertices
        edges: list of edges [u, v]
    
    Returns:
        minimum operations needed, or -1 if impossible
    """
    # Check if we have enough edges to potentially connect the graph
    if len(edges) < n - 1:
        return -1
    
    # Initialize Union-Find data structure
    ds = DisjointSet(n)
    
    # Process all edges to form connected components
    for u, v in edges:
        ds.union_by_size(u, v)
    
    # Count number of connected components
    connected_components = 0
    for node in range(n):
        if ds.find(node) == node:  # node is root of its component
            connected_components += 1
    
    # Operations needed = components - 1
    return connected_components - 1

class Solution:
    def solve(self, n, edges):
        return solution(n, edges)
```

## Time and Space Complexity

**Time Complexity: O(n + m × α(n))**
- Processing m edges with Union-Find operations: O(m × α(n))
- Counting components by checking n vertices: O(n × α(n))
- α(n) is the inverse Ackermann function, practically constant
- **Simplified: O(n + m)** for practical purposes

**Space Complexity: O(n)**
- Union-Find uses parent array and size array, each of size n
- No additional space proportional to number of edges

### Complexity Explanation in Simple Terms

- **Time**: We look at each edge once and each vertex once. Union-Find operations are nearly instant due to optimizations.
- **Space**: We only store information about each vertex (its parent and component size), so space grows linearly with the number of vertices.

---

### Key Insights

- **Minimum edges for connectivity**: Any connected graph with n vertices needs at least n-1 edges
- **Component bridging**: To connect k components, you need exactly k-1 "bridge" edges
- **Edge redistribution**: We can always rearrange existing edges as long as we have enough total edges

---

# Accounts Merge

## Problem Description

Given a list of accounts where each account contains a name followed by emails, merge accounts that belong to the same person. Two accounts belong to the same person if they share at least one common email address. People can have the same name but be different individuals, so we only merge based on shared emails.

## Examples

### Input
```
N = 4
accounts = [
    ["John", "johnsmith@mail.com", "john_newyork@mail.com"],
    ["John", "johnsmith@mail.com", "john00@mail.com"], 
    ["Mary", "mary@mail.com"],
    ["John", "johnnybravo@mail.com"]
]
```

### Output
```
[
    ["John", "john00@mail.com", "john_newyork@mail.com", "johnsmith@mail.com"],
    ["Mary", "mary@mail.com"],
    ["John", "johnnybravo@mail.com"]
]
```

### Explanation
Think of this like organizing scattered business cards. The first two John accounts share "johnsmith@mail.com", so they belong to the same person and get merged. Mary's account stands alone. The third John account has no common emails with others, so it remains separate. It's like finding that two business cards have the same phone number - they must belong to the same person!

## Solution

Use **Disjoint Set Union (DSU)** to efficiently group accounts that share emails. Map each email to its first occurrence, and when we see the same email again, union those accounts together.

## Intuition

### Visual Representation

```
Initial State:
Account 0: John → [johnsmith@mail.com, john_newyork@mail.com]
Account 1: John → [johnsmith@mail.com, john00@mail.com]
Account 2: Mary → [mary@mail.com]
Account 3: John → [johnnybravo@mail.com]

Email Mapping Process:
┌─────────────────────────────────────────────────────────────┐
│ Step 1: Process Account 0                                   │
│ johnsmith@mail.com     → maps to Account 0                 │
│ john_newyork@mail.com  → maps to Account 0                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ Step 2: Process Account 1                                   │
│ johnsmith@mail.com     → ALREADY SEEN! Union(0, 1)         │
│ john00@mail.com        → maps to Account 1                 │
│                                                             │
│ DSU State: Account 0 and 1 are now connected               │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ Step 3: Process Account 2                                   │
│ mary@mail.com          → maps to Account 2                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ Step 4: Process Account 3                                   │
│ johnnybravo@mail.com   → maps to Account 3                 │
└─────────────────────────────────────────────────────────────┘

Final Connected Components:
Component 1: {Account 0, Account 1} → Same person
Component 2: {Account 2} → Different person  
Component 3: {Account 3} → Different person
```

### DSU Tree Visualization

```
After Union Operations:
    0 (John)
    │
    └── 1 (John)  [connected via johnsmith@mail.com]

    2 (Mary)      [standalone]

    3 (John)      [standalone]

Final Groups:
Group 1: All emails from accounts 0 & 1
Group 2: All emails from account 2
Group 3: All emails from account 3
```

The key insight is that **shared emails create bridges between accounts**. Like connecting islands with bridges - if Island A connects to Island B, and Island B connects to Island C, then all three islands form one connected landmass!

## Approach Steps

1. **Initialize DSU**: Create a Disjoint Set Union structure for all accounts
2. **Map emails to accounts**: For each account, process all its emails:
   - If email seen for first time → map it to current account
   - If email already exists → union current account with the account that has this email
3. **Group emails by parent**: After all unions, group emails by their ultimate parent account
4. **Format result**: For each group, sort emails and prepend the person's name

## Code

```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set (Union-Find) for n elements.
        Each node is its own parent initially, with size = 1.
        """
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        """
        Find the ultimate parent (representative) of a node.
        Uses path compression optimization.
        """
        if node == self.parent[node]:
            return node
        # Path compression: directly connect node to its ultimate parent
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        """
        Union two sets by size.
        Attach the smaller tree under the larger one.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return  # already in the same set
        
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
        
        # Step 1: Process each account and union accounts with common emails
        for i in range(n):
            name = accounts[i][0]
            for j in range(1, len(accounts[i])):
                email = accounts[i][j]
                
                if email in email_to_account:
                    # Found common email - union current account with existing one
                    existing_account = email_to_account[email][0]
                    ds.union_by_size(existing_account, i)
                else:
                    # First time seeing this email
                    email_to_account[email] = (i, name)
        
        # Step 2: Group emails by their ultimate parent account
        merged_accounts = {}  # parent_account -> list of emails
        account_names = {}    # parent_account -> name
        
        for email, (account_index, name) in email_to_account.items():
            parent = ds.find(account_index)
            
            if parent not in merged_accounts:
                merged_accounts[parent] = []
                account_names[parent] = name
            
            merged_accounts[parent].append(email)
        
        # Step 3: Format the result - sort emails and prepend name
        result = []
        for parent in merged_accounts:
            email_list = sorted(merged_accounts[parent])
            result.append([account_names[parent]] + email_list)
        
        return result
```

## Time and Space Complexity

### Time Complexity: **O(N × E × α(N) + N × E × log(E))**
- **N** = number of accounts, **E** = average emails per account
- **O(N × E × α(N))**: Processing all emails and performing union operations (α is inverse Ackermann function, practically constant)
- **O(N × E × log(E))**: Sorting emails within each merged account
- **Overall**: Effectively **O(N × E × log(E))** since sorting dominates

### Space Complexity: **O(N + E)**
- **O(N)**: DSU parent and size arrays
- **O(E)**: Hash map storing email-to-account mappings
- **O(E)**: Result storage for merged accounts

### Simplified Explanation
- **Time**: We visit each email once to build connections, then sort emails within each group. Like organizing mail - first we group letters by recipient, then sort each person's mail alphabetically.
- **Space**: We need memory to track which emails belong to whom and the family tree structure of merged accounts.

---
# Number of islands II

## Problem Description

Given an n×m grid initially filled with water (all 0s), you'll receive k operations. Each operation converts a water cell to land (changes 0 to 1) at position [row, col]. After each operation, count how many separate islands exist. An island is a group of connected land cells (connected horizontally or vertically, not diagonally).

## Examples

### Input
```
n = 4, m = 5, k = 4
A = [[1,1], [0,1], [3,3], [3,4]]
```
<img src="https://static.takeuforward.org/content/ProblemSetter-LfUoJCTR" />

### Output
```
[1, 1, 2, 2]
```

### Explanation
Think of this like dropping stones into a pond to create islands:

**Operation 1**: Drop stone at (1,1) → Creates 1st island → Count = 1
**Operation 2**: Drop stone at (0,1) → Connects to existing island at (1,1) → Count = 1  
**Operation 3**: Drop stone at (3,3) → Creates separate 2nd island → Count = 2
**Operation 4**: Drop stone at (3,4) → Connects to island at (3,3) → Count = 2

It's like building with LEGO blocks - when you place a new block next to existing ones, they connect to form a larger structure!

## Solution

Use **Disjoint Set Union (DSU)** to dynamically track connected components as we add land cells. Each time we add land, we check if it connects to existing neighboring islands and merge them accordingly.

## Intuition

### Visual Step-by-Step Process

```
Initial Grid (4×5): All water
┌─────┬─────┬─────┬─────┬─────┐
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
└─────┴─────┴─────┴─────┴─────┘

Operation 1: Add land at (1,1)
┌─────┬─────┬─────┬─────┬─────┐
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  1  │  0  │  0  │  0  │ ← New island
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
└─────┴─────┴─────┴─────┴─────┘
Islands: 1

Operation 2: Add land at (0,1) 
┌─────┬─────┬─────┬─────┬─────┐
│  0  │  1  │  0  │  0  │  0  │ ← New land connects to existing
├─────┼─────┼─────┼─────┼─────┤
│  0  │  1  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
└─────┴─────┴─────┴─────┴─────┘
Islands: 1 (merged with existing)

Operation 3: Add land at (3,3)
┌─────┬─────┬─────┬─────┬─────┐
│  0  │  1  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  1  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  1  │  0  │ ← New separate island
└─────┴─────┴─────┴─────┴─────┘
Islands: 2

Operation 4: Add land at (3,4)
┌─────┬─────┬─────┬─────┬─────┐
│  0  │  1  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  1  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  0  │  0  │
├─────┼─────┼─────┼─────┼─────┤
│  0  │  0  │  0  │  1  │  1  │ ← Extends existing island
└─────┴─────┴─────┴─────┴─────┘
Islands: 2 (connected to existing)
```

### Cell-to-Node Mapping System
```
Grid Coordinates → Node Numbers (for DSU)
┌─────┬─────┬─────┬─────┬─────┐
│  0  │  1  │  2  │  3  │  4  │
├─────┼─────┼─────┼─────┼─────┤
│  5  │  6  │  7  │  8  │  9  │
├─────┼─────┼─────┼─────┼─────┤
│ 10  │ 11  │ 12  │ 13  │ 14  │
├─────┼─────┼─────┼─────┼─────┤
│ 15  │ 16  │ 17  │ 18  │ 19  │
└─────┴─────┴─────┴─────┴─────┘

Formula: Node = row × m + col
Example: Cell (1,1) → Node = 1×5 + 1 = 6
```

### DSU Connection Process
```
When adding land at (1,1):
1. Create new island → Count = 1
2. Check neighbors: up(0,1), down(2,1), left(1,0), right(1,2)
3. No land neighbors found → Island count remains 1

When adding land at (0,1):
1. Create new island → Count = 2
2. Check neighbors: up(-1,1)❌, down(1,1)✅, left(0,0)❌, right(0,2)❌
3. Found land neighbor at (1,1) → Union nodes 1 and 6 → Count = 1

Connection Visualization:
Before Union:    After Union:
Node 1: [1]      Node 6: [1, 6]  ← 6 becomes parent
Node 6: [6]      Node 1: points to 6
Count: 2         Count: 1
```

The key insight is that **each new land cell starts as its own island, then merges with neighboring islands**. It's like dropping puzzle pieces - each piece starts separate, but connects to adjacent pieces to form larger pictures!

## Approach Steps

1. **Initialize DSU**: Create Disjoint Set for all n×m cells and a visited matrix
2. **For each operation**:
   - Convert water cell to land at given position
   - If already land, skip (add current count to result)
   - If new land, increment island count by 1
   - Check all 4 neighbors (up, down, left, right)
   - For each land neighbor, try to union with current cell
   - If union successful (weren't already connected), decrement island count
   - Add current island count to result
3. **Return result array**

## Code

```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set (Union-Find) for n elements.
        Each node is its own parent initially, with size = 1.
        """
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        """
        Find the ultimate parent (representative) of a node.
        Uses path compression optimization.
        """
        if node == self.parent[node]:
            return node
        # Path compression: directly connect node to its ultimate parent
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int) -> bool:
        """
        Union two sets by size.
        Returns True if union happened, False if already connected.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return False  # already in the same set
        
        # Union by size: attach smaller tree under larger one
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
        
        return True  # successful union

class Solution:
    def numOfIslands(self, n, m, operations):
        # Initialize DSU for all cells and visited matrix
        ds = DisjointSet(n * m)
        visited = [[False] * m for _ in range(n)]
        
        # Direction vectors for 4-directional movement (up, down, left, right)
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        
        island_count = 0
        result = []
        
        for row, col in operations:
            # Skip if cell is already land
            if visited[row][col]:
                result.append(island_count)
                continue
            
            # Convert water to land
            visited[row][col] = True
            island_count += 1  # Start as new island
            
            # Convert 2D coordinates to 1D node number
            current_node = row * m + col
            
            # Check all 4 neighbors
            for dr, dc in directions:
                new_row, new_col = row + dr, col + dc
                
                # Check if neighbor is within bounds and is land
                if (0 <= new_row < n and 0 <= new_col < m and 
                    visited[new_row][new_col]):
                    
                    neighbor_node = new_row * m + new_col
                    
                    # Try to union current cell with neighbor
                    if ds.union_by_size(current_node, neighbor_node):
                        island_count -= 1  # Successful merge reduces count
            
            result.append(island_count)
        
        return result
```

## Time and Space Complexity

### Time Complexity: **O(K × α(N×M))**
- **K** = number of operations
- **α(N×M)** = inverse Ackermann function (practically constant)
- For each operation, we check 4 neighbors and perform DSU operations
- **Overall**: Effectively **O(K)** since α is nearly constant

### Space Complexity: **O(N×M + K)**
- **O(N×M)**: DSU parent and size arrays for all cells
- **O(N×M)**: Visited matrix to track land cells  
- **O(K)**: Result array storing island count after each operation

### Simplified Explanation
- **Time**: For each stone we drop, we check its 4 neighbors and possibly merge islands. Like checking if a new LEGO piece connects to existing structures - very fast per operation.
- **Space**: We need to remember which cells have land and track how islands are connected, plus store the answer for each operation.

---

# Making a large island

## Problem Description

Given an n×n binary grid, you can change **at most one** 0 to 1. Find the size of the largest possible island after this operation. An island is a group of connected 1s (connected horizontally or vertically). If the grid is already all 1s, return the total grid size.

## Examples

### Input
```
grid = [[1,0],[0,1]]
```

### Output
```
3
```

### Explanation
We can change either 0 to 1. Changing any 0 connects the two separate 1s into one island of size 3. It's like building a bridge between two separate islands to create one large landmass!

### Input
```
grid = [[1,1],[1,1]]
```

### Output
```
4
```

### Explanation
The grid is already completely filled with land - the largest possible island already exists with size 4.

## Solution

Use **Disjoint Set Union (DSU)** to pre-compute all existing island sizes, then for each water cell (0), simulate converting it to land and calculate the potential island size by summing unique neighboring island sizes.

## Intuition

### Visual Problem Breakdown

```
Original Grid:
┌─────┬─────┬─────┬─────┐
│  1  │  0  │  1  │  1  │
├─────┼─────┼─────┼─────┤
│  1  │  0  │  0  │  1  │
├─────┼─────┼─────┼─────┤
│  0  │  1  │  1  │  0  │
├─────┼─────┼─────┼─────┤
│  1  │  1  │  0  │  0  │
└─────┴─────┴─────┴─────┘

Initial Islands (before any changes):
Island A: {(0,0), (1,0)} → Size 2
Island B: {(0,2), (0,3), (1,3)} → Size 3  
Island C: {(2,1), (2,2), (3,0), (3,1)} → Size 4
```

### Strategy Visualization

```
Testing position (1,1) - Convert 0 → 1:
┌─────┬─────┬─────┬─────┐
│  1  │  0  │  1  │  1  │
├─────┼─────┼─────┼─────┤
│  1  │  X  │  0  │  1  │ ← Converting this 0 to 1
├─────┼─────┼─────┼─────┤
│  0  │  1  │  1  │  0  │
├─────┼─────┼─────┼─────┤
│  1  │  1  │  0  │  0  │
└─────┴─────┴─────┴─────┘

Check neighbors of (1,1):
↑ (0,1): 0 (water) - no connection
↓ (2,1): 1 (land) - connects to Island C (size 4)
← (1,0): 1 (land) - connects to Island A (size 2)  
→ (1,2): 0 (water) - no connection

Potential island size = 1 (new cell) + 4 (Island C) + 2 (Island A) = 7
```

### Key Challenge: Avoiding Double Counting

```
Problematic Case:
┌─────┬─────┬─────┐
│  1  │  1  │  1  │ ← All belong to same island
├─────┼─────┼─────┤
│  1  │  0  │  1  │ ← Converting (1,1) to 1
├─────┼─────┼─────┤
│  1  │  1  │  1  │ ← All belong to same island
└─────┴─────┴─────┘

Without proper handling:
- Up neighbor: Island size 3
- Down neighbor: Island size 3  
- Left neighbor: Island size 3
- Right neighbor: Island size 3
- Total: 1 + 3 + 3 + 3 + 3 = 13 ❌ (WRONG!)

With DSU ultimate parent tracking:
- All neighbors have same ultimate parent
- Add island size only once: 1 + 8 = 9 ✅ (CORRECT!)
```

### DSU Parent Tracking System

```
Cell Numbering (for 3×3 grid):
┌─────┬─────┬─────┐
│  0  │  1  │  2  │
├─────┼─────┼─────┤
│  3  │  4  │  5  │
├─────┼─────┼─────┤
│  6  │  7  │  8  │
└─────┴─────┴─────┘

DSU After Initial Processing:
parent[0] → 0, parent[1] → 0, parent[2] → 0  (merged into component 0)
parent[3] → 0, parent[5] → 0  (merged into component 0)
parent[6] → 0, parent[7] → 0, parent[8] → 0  (merged into component 0)

Ultimate Parent: 0 (represents the entire island of size 8)
```

The brilliant insight is that **DSU automatically handles the merging complexity** - we just need to track unique parent components when calculating potential island sizes!

## Approach Steps

1. **Build initial islands**: Use DSU to connect all existing land cells (1s) into their respective islands
2. **For each water cell (0)**:
   - Assume we convert it to land (adds +1 to size)
   - Check all 4 neighbors  
   - For each land neighbor, find its ultimate parent (island representative)
   - Use a set to track unique neighboring islands (avoids double counting)
   - Sum sizes of all unique neighboring islands
3. **Track maximum**: Keep track of the largest possible island size found
4. **Handle edge case**: If no water cells exist, return total grid size

## Code

```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set (Union-Find) for n elements.
        Each node is its own parent initially, with size = 1.
        """
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        """
        Find the ultimate parent (representative) of a node.
        Uses path compression optimization.
        """
        if node == self.parent[node]:
            return node
        # Path compression: directly connect node to its ultimate parent
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        """
        Union two sets by size.
        Returns True if union happened, False if already connected.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return False  # already in the same set
        
        # Union by size: attach smaller tree under larger one
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
        
        # Direction vectors for 4-directional movement (up, down, left, right)
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        
        # Step 1: Build initial islands by connecting adjacent land cells
        for row in range(n):
            for col in range(n):
                if grid[row][col] == 1:
                    current_node = row * n + col
                    
                    # Check all 4 neighbors
                    for dr, dc in directions:
                        new_row, new_col = row + dr, col + dc
                        
                        # If neighbor is within bounds and is land, union them
                        if (0 <= new_row < n and 0 <= new_col < n and 
                            grid[new_row][new_col] == 1):
                            neighbor_node = new_row * n + new_col
                            ds.union_by_size(current_node, neighbor_node)
        
        # Step 2: Try converting each water cell to land
        max_island_size = 0
        has_water = False
        
        for row in range(n):
            for col in range(n):
                if grid[row][col] == 0:
                    has_water = True
                    
                    # Set to store unique neighboring island parents
                    unique_parents = set()
                    potential_size = 1  # Adding the current cell
                    
                    # Check all 4 neighbors
                    for dr, dc in directions:
                        new_row, new_col = row + dr, col + dc
                        
                        # If neighbor is within bounds and is land
                        if (0 <= new_row < n and 0 <= new_col < n and 
                            grid[new_row][new_col] == 1):
                            neighbor_node = new_row * n + new_col
                            parent = ds.find(neighbor_node)
                            
                            # Add to set to avoid double counting
                            unique_parents.add(parent)
                    
                    # Sum sizes of all unique neighboring islands
                    for parent in unique_parents:
                        potential_size += ds.size[parent]
                    
                    max_island_size = max(max_island_size, potential_size)
        
        # Step 3: Handle edge case - if grid has no water (all 1s)
        if not has_water:
            return n * n
        
        return max_island_size
```

## Time and Space Complexity

### Time Complexity: **O(N² × α(N²))**
- **N²** = total cells in the grid
- **α(N²)** = inverse Ackermann function (practically constant)
- **Phase 1**: O(N²) to traverse grid and perform union operations
- **Phase 2**: O(N²) to check each water cell and its neighbors
- **Overall**: Effectively **O(N²)** since α is nearly constant

### Space Complexity: **O(N²)**
- **O(N²)**: DSU parent and size arrays for all N² cells
- **O(1)**: Additional space for sets and variables (constant per operation)

### Simplified Explanation
- **Time**: We visit each cell twice - once to build initial islands, once to test conversion possibilities. Like surveying land twice - first to map existing territories, then to plan optimal expansion.
- **Space**: We need to track the "family tree" of connected cells and remember which cells belong to which island group.

---

# Most stones removed with same row or column
## Problem Description

Given n stones placed on a 2D plane at integer coordinates, find the maximum number of stones that can be removed. A stone can only be removed if it shares the same row OR column with another stone that hasn't been removed yet. The goal is to remove as many stones as possible while following this rule.

## Examples

### Input
```
n = 6
stones = [[0,0], [0,1], [1,0], [1,2], [2,1], [2,2]]
```

### Output
```
5
```

### Explanation
We can remove 5 stones, leaving only 1 stone remaining. One possible removal sequence: Remove [0,0] (shares row with [0,1]), then [1,0] (shares column with [0,0]), and so on. The last stone cannot be removed as it would have no stones sharing its row or column.

### Input
```
n = 6  
stones = [[0,0], [0,2], [1,3], [3,1], [3,2], [4,3]]
```

### Output
```
4
```

### Explanation
We can remove 4 stones from the 6 total stones. The stones form 2 separate groups, and from each group we must leave at least 1 stone.

## Solution

Use **Disjoint Set Union (DSU)** to group stones that are connected through shared rows or columns. The maximum stones we can remove equals **total stones minus the number of connected components** (since we must leave at least 1 stone per component).

## Intuition

### Core Insight: Connected Components

```
Key Formula: Max Removable = Total Stones - Number of Components

Why? In each connected group, we can remove all stones except the last one!
```

### Visual Problem Analysis

```
Example 1 Visualization:
stones = [[0,0], [0,1], [1,0], [1,2], [2,1], [2,2]]

Grid Representation:
    Col0  Col1  Col2
Row0  ●     ●     ○     ← Stones at (0,0) and (0,1)
Row1  ●     ○     ●     ← Stones at (1,0) and (1,2)  
Row2  ○     ●     ●     ← Stones at (2,1) and (2,2)

Connection Analysis:
- (0,0) connects to (0,1) via Row 0
- (0,0) connects to (1,0) via Col 0  
- (0,1) connects to (2,1) via Col 1
- (1,0) connects to (1,2) via Row 1
- (1,2) connects to (2,2) via Col 2
- (2,1) connects to (2,2) via Row 2

All stones are transitively connected → 1 component
Max removable = 6 - 1 = 5 stones
```

### Revolutionary Approach: Row-Column Union

Instead of connecting stones directly, we connect **rows to columns**!

```
Traditional Approach (Complex):
Stone (0,0) ↔ Stone (0,1) ↔ Stone (1,0) ↔ ...
Need to check every pair of stones = O(N²)

DSU Approach (Elegant):
Row 0 ↔ Col 0 (because stone at (0,0))
Row 0 ↔ Col 1 (because stone at (0,1))  
Row 1 ↔ Col 0 (because stone at (1,0))
...

Node Mapping System:
Rows: 0, 1, 2, 3, ...
Cols: maxRow+1, maxRow+2, maxRow+3, ... (offset to avoid collision)

Example with maxRow = 2:
Row nodes: 0, 1, 2
Col nodes: 3, 4, 5 (representing cols 0, 1, 2)
```

### Step-by-Step DSU Process

```
Stone Positions: [[0,0], [0,1], [1,0], [1,2], [2,1], [2,2]]
Max Row = 2, Max Col = 2
Node mapping: Rows = {0,1,2}, Cols = {3,4,5}

Step 1: Process stone (0,0)
Union(Row0=0, Col0=3)
Components: {0,3}, {1}, {2}, {4}, {5}

Step 2: Process stone (0,1)  
Union(Row0=0, Col1=4)
Components: {0,3,4}, {1}, {2}, {5}

Step 3: Process stone (1,0)
Union(Row1=1, Col0=3) → But 3 is already connected to 0
Components: {0,1,3,4}, {2}, {5}

Step 4: Process stone (1,2)
Union(Row1=1, Col2=5) → 1 is connected to main component
Components: {0,1,3,4,5}, {2}

Step 5: Process stone (2,1)
Union(Row2=2, Col1=4) → 4 is connected to main component  
Components: {0,1,2,3,4,5}

Step 6: Process stone (2,2)
Union(Row2=2, Col2=5) → Both already in main component
Components: {0,1,2,3,4,5}

Final: 1 connected component
Answer: 6 stones - 1 component = 5 removable stones
```

### Why This Works: Transitive Connection Magic

```
Row-Column bridges create transitive connections:

Stone A(0,0) → Row0 ← Stone B(0,1) → Col1 ← Stone C(2,1)

Even though A and C don't share row/column directly,
they're connected through the Row0-Col1 bridge!

This is like a relay race where runners pass the baton:
Stone → Row → Column → Stone → Row → Column → ...
```

## Approach Steps

1. **Find grid dimensions**: Determine maxRow and maxCol from all stone positions
2. **Initialize DSU**: Create DSU with enough nodes for all rows and columns
3. **Map coordinates to nodes**: 
   - Row i → Node i
   - Column j → Node (maxRow + 1 + j)
4. **Union row-column pairs**: For each stone at (r,c), union row node r with column node (maxRow + 1 + c)
5. **Count unique components**: Find how many unique parent nodes exist among all stone-occupied rows and columns
6. **Calculate result**: Return total stones minus number of components

## Code

```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set (Union-Find) for n elements.
        Each node is its own parent initially, with size = 1.
        """
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node: int) -> int:
        """
        Find the ultimate parent (representative) of a node.
        Uses path compression optimization.
        """
        if node == self.parent[node]:
            return node
        # Path compression: directly connect node to its ultimate parent
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        """
        Union two sets by size.
        Returns True if union happened, False if already connected.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return False  # already in the same set
        
        # Union by size: attach smaller tree under larger one
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
        
        return True

class Solution:
    def maxRemove(self, stones, n):
        # Step 1: Find the maximum row and column indices
        max_row = max_col = 0
        for row, col in stones:
            max_row = max(max_row, row)
            max_col = max(max_col, col)
        
        # Step 2: Initialize DSU with enough nodes for all rows and columns
        # Rows: 0 to max_row, Columns: (max_row + 1) to (max_row + 1 + max_col)
        total_nodes = max_row + max_col + 2
        ds = DisjointSet(total_nodes)
        
        # Step 3: Union each stone's row with its column
        for row, col in stones:
            row_node = row
            col_node = max_row + 1 + col  # Offset columns to avoid collision
            ds.union_by_size(row_node, col_node)
        
        # Step 4: Count unique connected components
        # Only consider nodes that actually have stones
        unique_parents = set()
        for row, col in stones:
            row_node = row
            col_node = max_row + 1 + col
            
            # Add ultimate parents of both row and column nodes
            unique_parents.add(ds.find(row_node))
            unique_parents.add(ds.find(col_node))
        
        # Step 5: Calculate result
        # Each component can remove all stones except 1
        num_components = len(unique_parents)
        return n - num_components

# Alternative cleaner implementation
class SolutionOptimized:
    def maxRemove(self, stones, n):
        max_row = max_col = 0
        for row, col in stones:
            max_row = max(max_row, row)
            max_col = max(max_col, col)
        
        ds = DisjointSet(max_row + max_col + 2)
        stone_nodes = set()  # Track nodes that have stones
        
        # Union rows with columns and track stone nodes
        for row, col in stones:
            row_node = row
            col_node = max_row + 1 + col
            ds.union_by_size(row_node, col_node)
            stone_nodes.add(row_node)
            stone_nodes.add(col_node)
        
        # Count unique components among stone nodes
        unique_components = len(set(ds.find(node) for node in stone_nodes))
        
        return n - unique_components
```

## Time and Space Complexity

### Time Complexity: **O(N × α(R+C))**
- **N** = number of stones
- **R** = maximum row index, **C** = maximum column index  
- **α(R+C)** = inverse Ackermann function (practically constant)
- **Processing**: O(N) to process all stones with DSU operations
- **Counting**: O(N) to count unique components
- **Overall**: Effectively **O(N)** since α is nearly constant

### Space Complexity: **O(R + C)**
- **O(R + C)**: DSU parent and size arrays for all row and column nodes
- **O(N)**: Set to track unique parent components
- **Overall**: **O(R + C + N)**

### Simplified Explanation  
- **Time**: We visit each stone once to build connections, then count components. Like organizing a relay team - first we connect teammates, then count how many complete teams we have.
- **Space**: We need memory to track the "family tree" of connected rows and columns, plus remember which nodes actually contain stones.
