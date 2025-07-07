LC847. Shortest Path Visiting All Nodes
=======================================

# Intuition
The problem requires finding the shortest path that visits every node in an undirected, connected graph. The solution combines two key algorithmic techniques:
1. Breadth-First Search (BFS) to find shortest distances between all pairs of nodes
2. Travelling Salesman Problem (TSP) to find the minimum path that visits all nodes

# Approach

## BFS Distance Calculation
1. For each node as a starting point, perform a BFS:
   - Use a queue to explore the graph level by level
   - Track visited nodes using a bit mask
   - Calculate the shortest distance to every other node
   - Store these distances in a 2D `dist` array

## Travelling Salesman Algorithm
1. Use dynamic programming with bit manipulation
2. Key components:
   - `mask`: Represents which nodes have been visited
   - `pos`: Current node position
   - Recursively explore all possible paths
   - Memoize results to avoid redundant calculations

### Key Steps in Travelling Salesman:
- Base case: If all nodes visited, return 0
- Check memoized results to avoid recomputation
- For each unvisited node:
  - Calculate path length by adding distance to next node
  - Recursively find minimum path for remaining nodes

# Complexity
- Time complexity: $$O(n * 2^n)$$
  - BFS for each node: $$O(n^2)$$
  - Travelling Salesman: $$O(n * 2^n)$$

- Space complexity: $$O(n * 2^n)$$
  - Memoization array `dp`
  - Storing distances between nodes

# Key Techniques
1. Bit Manipulation
   - Use bit masks to track visited nodes
   - Efficient set operations using bitwise AND/OR

2. Dynamic Programming
   - Memoize intermediate results
   - Avoid redundant recursive calls

3. Breadth-First Search
   - Find shortest paths between nodes
   - Explore graph level by level

The solution elegantly combines graph traversal and optimization techniques to solve a complex path-finding problem efficiently.

# Code
```cpp []
class Solution {
public:
    int dist[12][12];
    int dp[12][4096];  // 1<<12 - 1; all possible combination of nodes for travelling salesman
    int n;

    int shortestPathLength(vector<vector<int>>& graph) {
        n = (int)graph.size();
        int all = (1<<n) - 1;
        int ans = INT_MAX;
        
        for (int i=0; i<n; i++)
            for (int j=0; j<all; j++)
                dp[i][j] = -1;

        /* breadth first search to find shortest path between
         * any two vertices in the graph
         */
        for (int i=0; i<n; i++) {
            int lev[n]; // shortest path from i to all other vertices
            queue<int> q;
            int visited = 1<<i;
            
            q.push(i);
            lev[i] = 0; // self loop
            
            while (!q.empty()) {
                int u = q.front();
                q.pop();

                for (int j=0; j<graph[u].size(); j++) {
                    int v = graph[u][j];
                    if (!(visited & 1<<v)) {
                        visited = visited | 1<<v;
                        q.push(v);
                        lev[v] = lev[u] + 1;
                    }
                }
            }

            for (int j=0; j<n; j++)
                dist[i][j] = lev[j];
        }

        /* the minimum path to visit all the vertices from a 
         * given starting vertex is given by travelling salesman
         * algorithm
         */
        for (int i=0; i<n; i++)
            ans = min(ans, travellingSalesman(1<<i, i));

        return ans;
    }

    int travellingSalesman(int mask, int pos) {
        
        int ans = INT_MAX;

        // all nodes have been visited, no more distances to cover
        if (mask == (1<<n)-1)
            return 0;

        if (dp[pos][mask] != -1)
            return dp[pos][mask];
        
        for (int next=0; next<n; next++) 
            if (!(mask & 1<<next) && next!=pos) {
                ans = min(ans, dist[pos][next] + 
                    travellingSalesman(mask | 1<<next, next));
            }

        return dp[pos][mask] = ans;
    }
};
```
