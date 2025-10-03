# Complete Guide to Graph Data Structures

## Table of Contents
1. [Data Structure Classification](#data-structure-classification)
2. [What is a Graph?](#what-is-a-graph)
3. [Applications of Graphs](#applications-of-graphs)
4. [Types of Graphs](#types-of-graphs)
5. [Graph Terminology](#graph-terminology)
6. [Graph Representation](#graph-representation)

---

## Data Structure Classification

Data structures can be broadly classified into two main categories:

### Linear Data Structures
Data elements are arranged in a sequential (linear) manner.
- **Examples**: Arrays, Linked Lists, Stacks, Queues

### Non-Linear Data Structures
Data elements are not arranged in a sequential manner. They form a hierarchical or interconnected structure.
- **Examples**: Trees, Graphs

---

## What is a Graph?

A **Graph** is a non-linear data structure consisting of:
- **Nodes/Vertices/Points**: Individual data elements
- **Edges/Lines/Arcs**: Connections between pairs of nodes

### Key Definitions:
- A graph is a finite set of nodes and edges used to connect pairs of nodes
- A pictorial representation of a set of objects where some pairs are connected by links
- Can be considered as a cyclic tree

### Example Graph Structure:
```
Vertices: V = {A, B, C, D, E}
Edges: E = {(A,B), (B,C), (C,E), (E,D), (D,B), (D,A)}
```

---

## Applications of Graphs

Graphs have numerous real-world applications:

### Social Media & E-commerce
- **Recommendation Systems**: Used in social media platforms and e-commerce applications to suggest friends, products, or content

### Computer Science
- **Flow of Computation**: Represent computational processes and data flow

### Navigation Systems
- **Google Maps**: Uses graphs where road intersections are vertices and roads are edges
- **Shortest Path Algorithms**: Calculate optimal routes between locations

### World Wide Web
- **Web Page Linking**: Pages connect to others through hyperlinks, forming a graph structure

---

## Types of Graphs

### 1. Null Graph
- **Definition**: Contains vertices but no edges between them
- **Characteristics**: n nodes, zero edges

### 2. Finite Graph
- **Definition**: Contains a finite number of vertices and edges
- **Characteristics**: Limited, countable elements

### 3. Infinite Graph
- **Definition**: Contains infinite number of vertices and edges
- **Characteristics**: Unbounded structure

### 4. Cyclic Graph
- **Definition**: Contains at least one cycle
- **Characteristics**: Closed paths exist within the graph

### 5. Acyclic Graph
- **Definition**: Contains no cycles
- **Characteristics**: No closed paths, tree-like structure

### 6. Simple Graph
- **Definition**: Only one edge between any pair of vertices
- **Characteristics**: No multiple edges or self-loops

### 7. Multi Graph
- **Definition**: Multiple edges between pairs of vertices
- **Characteristics**: No self-loops but allows parallel edges

### 8. Complete Graph
- **Definition**: Edge exists between every pair of vertices
- **Characteristics**: Every complete graph is also a simple graph
- **Degree**: Each node has degree = (number of vertices - 1)

### 9. Pseudo Graph
- **Definition**: Contains at least one self-loop
- **Characteristics**: Vertices can connect to themselves

### 10. Regular Graph
- **Definition**: All vertices have equal degree
- **Note**: Complete graphs are regular, but not all regular graphs are complete

### 11. Connected Graph
- **Definition**: At least one path exists between every pair of vertices
- **Characteristics**: No isolated components

### 12. Disconnected Graph
- **Definition**: No path exists between at least one pair of vertices
- **Characteristics**: Contains isolated components

### 13. Directed Graph (Digraph)
- **Definition**: All edges have direction indicating traversal order
- **Representation**: G = (V, E) where edges map to ordered pairs (Vi, Vj)
- **Example**: Like following someone on social media (one-way relationship)

### 14. Undirected Graph
- **Definition**: No orientation/direction on edges
- **Characteristics**: All edges are bidirectional
- **Example**: Facebook friendship (mutual relationship)

### 15. Weighted Graph
- **Definition**: Edges have associated values/weights
- **Use Cases**: Represent cost, distance, or capacity

### 16. Unweighted Graph
- **Definition**: No values associated with edges
- **Default**: All graphs are unweighted unless specified

### 17. Dense & Sparse Graphs
- **Dense Graph**: Number of edges close to maximum possible
- **Sparse Graph**: Contains only a few edges

### 18. Bipartite Graph
- **Definition**: Vertex set can be split into two disjoint sets (V1, V2)
- **Characteristics**: Edges only connect vertices between different sets

### 19. Complete Bipartite Graph
- **Definition**: Every vertex in first set connects to every vertex in second set
- **Notation**: Kx,y (x vertices in first set, y in second set)

---

## Graph Terminology

### Basic Elements
- **Vertex (Node)**: Individual data element
- **Edge**: Path/connection between two vertices (endpoints)

### Vertex Relationships
- **Adjacent Vertices**: Two vertices connected by an edge
- **Incident Edge**: Edge connected to a vertex as an endpoint

### Edge Direction (for Directed Graphs)
- **Outgoing Edge**: Edge directed away from a vertex
- **Incoming Edge**: Edge directed toward a vertex

### Degree Concepts
- **Degree**: Number of edges incident to a vertex
- **In-degree**: Number of incoming edges to a vertex
- **Out-degree**: Number of outgoing edges from a vertex

### Special Elements
- **Parallel Edges**: Multiple edges between same pair of vertices
- **Self-loop**: Edge connecting a vertex to itself
- **Forest**: Graph with no cycles
- **Path**: Sequence of successive edges between nodes
- **Path Length**: Number of edges in a path
- **Simple Path**: Path with distinct vertices

### Special Vertices
- **Source Vertex**: In-degree = 0
- **Sink Vertex**: Out-degree = 0

### Connectivity
- **Strongly Connected**: Directed path exists between every pair of vertices (both directions)
- **Weakly Connected**: Becomes connected when all directed edges are replaced with undirected edges
- **Bridge**: Edge whose removal disconnects the graph

---

## Graph Representation

### 1. Adjacency Matrix

**Description**: 2D matrix representation where matrix[i][j] indicates connection between vertex i and vertex j.

**Matrix Size**: V × V (where V = number of vertices)

**Values**:
- **Unweighted**: 1 (edge exists) or 0 (no edge)
- **Weighted**: Actual weight value or 0 (no edge)

**For Directed Graphs**: matrix[i][j] = 1 means edge from vertex i to vertex j

**For Undirected Graphs**: matrix[i][j] = matrix[j][i] = 1 (symmetric matrix)

#### Advantages:
- Easy to understand and implement
- O(1) time complexity for:
  - Checking edge existence
  - Adding an edge
  - Removing an edge

#### Disadvantages:
- O(V²) space complexity (inefficient for sparse graphs)
- O(V²) time complexity for adding/removing vertices

### 2. Adjacency List

**Description**: Array of linked lists where each vertex maintains a list of its neighbors.

**Structure**: 
- Array indexed by vertex number
- Each array element points to a linked list of adjacent vertices

**Implementation Options**:
- Linked list representation
- Array-based representation

#### Advantages:
- Space efficient: O(V + E) where E = number of edges
- O(1) time for adding/deleting edges and vertices
- Clear visualization of adjacent nodes

#### Disadvantages:
- Slower for checking if two vertices are adjacent
- O(V) time complexity for adjacency testing

### 3. Incidence Matrix

**Description**: Matrix representation where rows represent vertices and columns represent edges.

**Matrix Size**: V × E (where V = vertices, E = edges)

**Values**:
- **0**: Vertex not connected to edge
- **1**: Vertex connected as outgoing endpoint
- **-1**: Vertex connected as incoming endpoint

**Use Case**: Particularly useful for directed graphs where edge direction matters

#### Advantages:
- Clear representation of vertex-edge relationships
- Useful for certain graph algorithms

#### Disadvantages:
- Space inefficient: O(V × E)
- More complex than other representations

### 4. Incidence List

**Description**: Each vertex maintains a list of edges incident to it.

**Structure**: Similar to adjacency list but stores edge information instead of just neighboring vertices.




