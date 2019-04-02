# Assignment 4

## Solution 1

For each vertex `u ∈ G.V`, we need to count the occurrence of u in the adjacency list. This will give us the number of incoming edges to u. We store this in a dictionary keyed by the vertex with number of incoming edges as value. This takes us `O(V+E)`.

We look for the vertex with value 0 i.e one with `zero` incoming edges. We remove that vertex from the dictionary and add it to a `_queue`. Searching such a vertex is an `O(V)` time operation.

We now need to remove the outgoing edges from this vertex. We do so by decrementing the count of incoming edges of the vertices connected to this vertex in the dictionary.

We look again for a vertex with zero incoming edges in the dictionary and add it to the `_queue`. We repeatedly do this step until the dictionary is empty. This again costs us `O(V+E)`, since we go through each vertex and edges at most once.

If we cannot find a vertex with zero incoming edges to add to the `_queue`, the graph contains cycles.

## Solution 2

### DFS(G)

```[C]
1. For each vertex u ∈ G.V
    a. u.color = WHITE
    b. u.dad = NIL
2. DFS# = 0
3. For each vertex u ∈ G.V
    a. If u.color = WHITE
        i. DFS-MAIN(G,u)
```

### DFS-MAIN(G, u)

```[C++]
1. DFS#++                   /* discovery number */
2. DFS[u] = DFS#            /* discovery array */
3. u.color = GRAY
4. For each vertex v ∈ G.Adj[u]
    a. If v.color = WHITE
        i. v.dad = u
        ii. DFS-MAIN(G, v)
5. u.color = BLACK
6. DFS#++
7. Finish[u]= DFS#         /* when finished, vertex turned black */
```

### STRONGLY-CONNECTED-COMPONENTS (G)

```[C]
1. Call DFS(G) and compute DFS[ ] and Finish[ ] array.
2. Compute G-Transpose – Reverse all the edges in the Adjacency List
3. Color all vertices WHITE.
4. Call DFS(G-Transpose) but in DFS(G), consider vertices in decreasing order of corresponding values of Finish[] (finish time) as computed in 1.
5. Each tree in the depth-first forest Gc formed in line 4 is a strong connected component with at most one edge between two vertices in the component graph Gc.
```

### Analysis

The DFS(G) takes `O(V+E)` since the graph is stored in an Adjacency list and we visit a Gray vertex only once before it is turned Black. The running time of step 4 in `DFS-MAIN(G)` is `O(deg(u))`. Hence the total cost of DFS is `O(V+E)`.

Computing `G-Transpose` requires to go through each vertex only once to look for its outgoing edges and then reverse them. This will too take `O(V+E)` time. Coloring each vertex white is an `O(V)` time task.Hence this algorithm takes total `O(V+E)` time.

## Solution 3

We call STRONGLY-CONNECTED-COMPONENTS(G) on the given graph G. It leaves us with completely connected components. If the given source and destination pair s, t belonging to different connected components - As the connected components are connected to each other with at most one edge, the problem reduces to finding all the possible paths from s to the vertex in CC1 which connects to CC2 AND from that vertex in CC2 to the destination, t. As all the vertices in a connected component are reachable from each other, we can find the possible paths by traversing the Adjacency list of s and then traverse the edges connected to it and continue do so, unless we reach THAT vertex in CC1. Similarly for CC2. As there is at most one edge connecting both the connecting components, the total number of paths will be m*n.
 
4.	Call STRONGLY-CONNECTED-COMPONENTS(G)
5.	Now we have DFS forests
6.	
