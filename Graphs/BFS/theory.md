# BFS Traversal in Graphs

## 1. What is BFS?
BFS stands for **Breadth First Search**. It is a graph traversal algorithm that visits nodes **level by level**.  
First, it visits the starting node, then all its neighbors, then the neighbors of those neighbors, and so on. BFS uses a **queue** to keep track of the next node to visit, and a **visited array/set** to avoid visiting the same node more than once. 

## 2. Main Idea of BFS
BFS explores the graph in **layers**:
- Level 0: starting node
- Level 1: direct neighbors
- Level 2: neighbors of neighbors
- and so on

This is why BFS is also called **level-order traversal** in tree-like structures. 
## 3. Data Structures Used
BFS mainly uses:
- **Queue** → to process nodes in FIFO order
- **Visited array / visited set** → to mark already visited nodes
- **Graph representation** → adjacency list or adjacency matrix 

## 4. BFS Algorithm Steps
1. Start from a source node.
2. Mark it as visited.
3. Put it into the queue.
4. Remove a node from the front of the queue.
5. Visit all its unvisited adjacent nodes.
6. Mark each new node as visited and add it to the queue.
7. Repeat until the queue becomes empty.

## 5. Simple Example
Suppose the graph starts from node **A**:

- Visit A
- Then visit all neighbors of A
- Then visit all neighbors of those neighbors

So the order looks like:

`A -> first level neighbors -> second level neighbors -> ...`

This is the key BFS pattern: **near nodes first, far nodes later**. 

## 6. Why We Need the Visited Array
Graphs can contain cycles.  
Without a visited array, BFS may visit the same node again and again, causing repeated work or even an infinite loop in cyclic graphs. The visited structure prevents this problem.

## 7. Important Properties of BFS
- BFS visits nodes **in increasing distance from the source**.
- BFS is **not** deep-first; it moves outward level by level.
- BFS is very useful when the **shortest path in an unweighted graph** is needed.

## 8. Applications of BFS
BFS is used in:
- finding shortest path in an **unweighted graph**
- level-order traversal
- checking connectivity
- exploring all nodes reachable from a source
- problems related to layers, neighbors, and minimum number of steps 

## 9. Time and Space Complexity
For a graph with **V vertices** and **E edges**:
- **Time Complexity:** `O(V + E)`
- **Space Complexity:** `O(V)` for queue and visited storage

Some sources present this as `O(N + 2E)` or similar depending on representation, but the standard simplified form is `O(V + E)`. 

## 10. BFS in C++

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

/*
    BFS (Breadth First Search)
    ---------------------------
    - Visits nodes level by level.
    - Uses a queue (FIFO: First In, First Out).
    - Uses a visited array to avoid visiting the same node again.
    - Works well for unweighted graphs and shortest path by number of edges.
*/

void bfs(int start, vector<vector<int>>& adj, vector<bool>& visited) {
    queue<int> q;              // Queue stores nodes to be processed

    visited[start] = true;      // Mark the starting node as visited
    q.push(start);              // Put the starting node into the queue

    while (!q.empty()) {
        int node = q.front();   // Get the front node
        q.pop();                // Remove it from the queue

        cout << node << " ";    // Visit / print the node

        // Check all neighbors of the current node
        for (int neighbor : adj[node]) {
            // If neighbor is not visited, then visit it
            if (!visited[neighbor]) {
                visited[neighbor] = true; // Mark as visited immediately
                q.push(neighbor);         // Add to queue for future processing
            }
        }
    }
}

int main() {
    int n, m;
    cout << "Enter number of vertices and edges: ";
    cin >> n >> m;

    // adjacency list
    vector<vector<int>> adj(n);

    cout << "Enter " << m << " edges (u v):" << endl;
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;

        // For an undirected graph, add both directions
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    int start;
    cout << "Enter starting node: ";
    cin >> start;

    vector<bool> visited(n, false);

    cout << "BFS traversal: ";
    bfs(start, adj, visited);
    cout << endl;

    return 0;
}
```

### Example input
```
Enter number of vertices and edges: 5 5
Enter 5 edges (u v):
0 1
0 2
1 3
1 4
2 4
Enter starting node: 0
```

### Example output
```
BFS traversal: 0 1 2 3 4
```