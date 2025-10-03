# Spanning Tree and Minimum Spanning Tree Explained

## **1. Spanning Tree**

A **spanning tree** is a special type of subgraph of a weighted or unweighted graph. It has the following characteristics:

* **Contains all N nodes** from the original graph.
* Has exactly **N-1 edges**.
* All nodes are **reachable** from each other (graph is connected).
* It is a **tree** (meaning it contains no cycles).

### **Example**

* Suppose we have an **undirected weighted graph** with:

  * `N = 6` (nodes)
  * `M = 7` (edges)
<img src="https://static.takeuforward.org/premium/Graphs/Minimum%20Spanning%20Tree/MST%20theory/-LeZMmokj" />
* A spanning tree will be a subset of this graph with **6 nodes** and **5 edges** connecting all nodes without any cycles.
<img src="https://static.takeuforward.org/premium/Graphs/Minimum%20Spanning%20Tree/MST%20theory/-_60gIKbC" />
<img src="https://static.takeuforward.org/premium/Graphs/Minimum%20Spanning%20Tree/MST%20theory/-dwOhCPsZ" />

For example:

* **Spanning Tree 1**: Weight = 18
* **Spanning Tree 2**: Weight = 24
* **Spanning Tree 3**: Weight = 18

> **Note:** A graph can have **multiple spanning trees**.

### **Weight of a Spanning Tree**

The **weight** of a spanning tree is the **sum of the weights of all its edges**.

---

## **2. Minimum Spanning Tree (MST)**

A **minimum spanning tree** is the spanning tree with the **smallest possible weight** among all spanning trees of the graph.

* If we list all spanning trees of a graph and calculate their weights, the MST will be the one with the **minimum total edge weight**.
* In the above example:

  * The MST has a total weight of **16** (smaller than 18 or 24).
  * A graph can have **more than one MST** if there are ties in weight.

<img src="https://static.takeuforward.org/premium/Graphs/Minimum%20Spanning%20Tree/MST%20theory/image-s-DJL0Jv" />

---

## **3. Algorithms to Find MST**

Two main algorithms are used to find the MST of a graph:

1. **Prim's Algorithm**

   * Starts with a single node and keeps adding the smallest possible edge that connects a new node.

2. **Kruskal's Algorithm**

   * Sorts all edges by weight and adds them one by one, ensuring no cycles are formed.

---

## **4. Real-Life Use Cases of MST**

The MST concept is widely used in real-world problems where we need to **connect multiple points with the minimum cost**.

### **Examples:**

1. **Telecommunications Networks**

   * Laying out cables to connect all phone exchanges with **minimum cable length**.

2. **Computer Networks**

   * Designing Local Area Networks (LAN) or Wide Area Networks (WAN) using the **least number of cables and networking hardware**.

3. **Transportation Networks**

   * Building road or railway systems connecting all cities without **redundant routes**.

4. **Water Supply Networks**

   * Creating the most cost-efficient pipeline network that connects all regions.

---

**In short:**

* **Spanning Tree:** All nodes connected, N-1 edges, no cycles.
* **Minimum Spanning Tree:** The spanning tree with the smallest total weight.
* **Algorithms:** Prim's, Kruskal's.
* **Applications:** Networks, transportation, utilities.

---

# Prim's Algorithm Explained

## **1. Introduction**

**Prim's Algorithm** is a greedy algorithm used to find the **Minimum Spanning Tree (MST)** of a **connected, weighted, undirected graph**.

The idea is to **start from a single node** and keep adding the **smallest edge** that connects a new node to the growing tree until all nodes are included.

---

## **2. Key Concepts**

* **MST Goal:** Connect all nodes with minimum total edge weight.
* **Greedy Choice:** Always choose the smallest weight edge that adds a new vertex to the tree.
* **No Cycles:** The selected edges must not form cycles.

---

## **3. Steps of Prim's Algorithm**

1. **Start** with any node (let's call it the starting vertex).
2. **Mark** this node as part of the MST.
3. From all edges that connect the MST to nodes not yet in the MST, **choose the edge with the smallest weight**.
4. **Add** the selected edge and the new node to the MST.
5. **Repeat** steps 3 and 4 until all nodes are included.

---

## **4. Example**

Let’s say we have 5 nodes (A, B, C, D, E) with weighted edges:

1. Start from A.
2. Find the smallest edge from A to another node → (A, B) with weight 2.
3. Now MST has A and B. Look for smallest edge connecting MST to new node → (B, C) with weight 3.
4. Repeat until all nodes are connected.

---

## **5. Python Implementation**

```python
import heapq

def prim_mst(nodes,graph):
    MST=[]
    MSTWeight=0
    visited={node:False for node in nodes}
    queue=[]
    src=nodes[0]
    visited[src]=True
    edges=[(adj_weight,src,adj_node) for adj_node,adj_weight in graph[src]]
    heapq.heapify(edges)
    while edges:
        weight,from_node,to_node=heapq.heappop(edges)
        if(not visited[to_node]):
            visited[to_node]=True
            MST.append((from_node,to_node,weight))
            MSTWeight+=weight
            for adj_node,adj_weight in graph[to_node]:
                if(not visited[adj_node]):
                    heapq.heappush(edges,(adj_weight,to_node,adj_node))
    return MST,MSTWeight

nodes=['A','B','C','D','E']
graph = {
    'A': [('B', 2), ('C', 3)],
    'B': [('A', 2), ('C', 1), ('D', 4)],
    'C': [('A', 3), ('B', 1), ('D', 5), ('E', 6)],
    'D': [('B', 4), ('C', 5), ('E', 7)],
    'E': [('C', 6), ('D', 7)]
}

mst, cost = prim_mst(nodes,graph)
print("MST Edges:", mst)
print("Total Cost:", cost)

```

**Output:**

```
MST Edges: [('A', 'B', 2), ('B', 'C', 1), ('B', 'D', 4), ('C', 'E', 6)]
Total Cost: 13
```

---

## **6. Time Complexity**

* Using a **min-heap/priority queue**: **O(E log V)**
* Without priority queue (simple array): **O(V²)**

Where:

* **V** = number of vertices
* **E** = number of edges

---

---

