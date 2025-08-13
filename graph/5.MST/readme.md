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
