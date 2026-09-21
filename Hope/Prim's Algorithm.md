# Prim's Algorithm — Minimum Spanning Tree

## Problem

Given an **undirected, connected, weighted graph** `G(V, E)`, find its **Minimum Spanning Tree (MST)** using **Prim's Algorithm**.

Each edge is represented as:

```text
u v w
```

where:

* `u` = first vertex
* `v` = second vertex
* `w` = edge weight

The graph contains `V` vertices numbered from `0` to `V-1`.

The MST must contain exactly:

```text
V - 1 edges
```

and connect all vertices with minimum possible total weight.

---

## Prim's Algorithm

Prim's algorithm builds the MST by starting from any vertex and repeatedly selecting the **minimum-weight edge that connects a visited vertex to an unvisited vertex**.

### Basic idea

```text
Start with vertex 0
       ↓
Mark it as visited
       ↓
Look at all edges going from visited → unvisited
       ↓
Pick the minimum-weight edge
       ↓
Add that edge to MST
       ↓
Mark the new vertex as visited
       ↓
Repeat until V - 1 edges are selected
```

The important condition is:

```java
!visited[v]
```

We only consider edges that lead to an **unvisited vertex**.

This prevents cycles and ensures that every selected edge expands the MST.

---

# Approach 1 — Adjacency Matrix

The first implementation represents the graph using a 2D matrix:

```java
int[][] mat = new int[vertex][vertex];
```

For an edge:

```text
u v w
```

we store:

```java
mat[u][v] = w;
mat[v][u] = w;
```

because the graph is undirected.

### Example

For:

```text
0 1 3
0 3 5
1 2 1
```

the matrix represents the connections between every pair of vertices.

During Prim's algorithm, for every visited vertex, we scan all possible vertices to find the minimum edge leading to an unvisited vertex.

```java
for(int i = 0; i < vertex; i++){
    if(visited[i]){
        for(int j = 0; j < vertex; j++){
            if(!visited[j] &&
               mat[i][j] != 0 &&
               mat[i][j] < min){
                ...
            }
        }
    }
}
```

#### Java implementation

```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        Scanner sc=new Scanner(System.in);
        int vertex=sc.nextInt();
        int edge=sc.nextInt();
        int[][] mat=new int[vertex][vertex];
        boolean[] visited=new boolean[vertex];
        for(int i=0;i<edge;i++){
                int u=sc.nextInt();
                int v=sc.nextInt();
                int w=sc.nextInt();
                mat[u][v]=w;
                mat[v][u]=w;
        }
        visited[0]=true; // first node
        
        // MST has vertex-1 edges
        for(int e=0;e<vertex-1;e++){
                int min=Integer.MAX_VALUE;
                int x=0;
                int y=0;
                for(int i=0;i<vertex;i++){
                        if(visited[i]){ // u -> visited node
                                for(int j=0;j<vertex;j++){
                                        // v -> should be an unvisited node
                                        if(!visited[j] && mat[i][j]!=0 && mat[i][j]<min){
                                                min=mat[i][j];
                                                x=i;
                                                y=j;
                                        }
                                }
                        }
                }
                System.out.println(x+" "+y+" "+min);
                visited[y]=true;
        }
        
    }
}

```

---

### Complexity

For every MST edge, all vertices may be scanned from every visited vertex.

Overall:

```text
Time:  O(V²)
Space: O(V²)
```

The adjacency matrix is simple, but it uses `O(V²)` memory even when the graph has relatively few edges.

---

# Approach 2 — Adjacency List

The second implementation uses an adjacency list.

For every vertex, we maintain a list of its neighbouring edges:

```java
List<List<Pair>> adj = new ArrayList<>();
```

Each edge is represented using:

```java
class Pair {
    int start;
    int end;
    int w;
}
```

Since the graph is undirected, an edge:

```text
u v w
```

is stored twice:

```text
u → v (w)
v → u (w)
```

Therefore:

```java
Pair p1 = new Pair(u, v, w);
Pair p2 = new Pair(v, u, w);
```

and both are added to the adjacency list.

---

## Prim's Algorithm with the Adjacency List

The MST construction remains the same.

Start with:

```java
visited[0] = true;
```

Then repeat `V - 1` times:

1. Look at every visited vertex.
2. Look at its adjacent edges.
3. Ignore edges going to already visited vertices.
4. Select the minimum-weight remaining edge.
5. Add that edge to the MST.
6. Mark the newly reached vertex as visited.

The core condition is:

```java
if(!visited[curr.end] && curr.w < min)
```

This means:

> Among all edges connecting the current MST to an unvisited vertex, choose the one with the smallest weight.

---

#### Java Implementation

```java
import java.io.*;
import java.util.*;

class Pair{
  int start;
  int end;
  int w;
  Pair(int u,int v,int w){
    start=u;
    end=v;
    this.w=w;
  }
}

public class Solution {
    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        Scanner sc=new Scanner(System.in);
        int vertex=sc.nextInt();
        int edge=sc.nextInt();
        List<List<Pair>> adj=new ArrayList<>();
        
        for(int i=0;i<vertex;i++){
                adj.add(new ArrayList<>());
        }
        
        boolean[] visited=new boolean[vertex];
        for(int i=0;i<edge;i++){
                int u=sc.nextInt();
                int v=sc.nextInt();
                int w=sc.nextInt();
                Pair p1=new Pair(u,v,w);
                Pair p2=new Pair(v,u,w);
                if(i==0){
                        adj.get(u).add(p1);
                        adj.get(v).add(p2);
                }else{
                        boolean flag=true;
                        for(int j=0;j<adj.get(u).size();j++){
                                Pair check=adj.get(u).get(j);
                                if(check.end>v){
                                        adj.get(u).add(j,p1);
                                        flag=false;
                                        break;
                                }else if(check.end==v && check.w>w){
                                        adj.get(u).add(j,p1);
                                        flag=false;
                                        break;
                                }
                        }
                        if(flag) adj.get(u).add(p1);
                        flag=true;
                        for(int j=0;j<adj.get(v).size();j++){
                                Pair check=adj.get(v).get(j);
                                if(check.end>v){
                                        adj.get(v).add(j,p2);
                                        flag=false;
                                        break;
                                }else if(check.end==v && check.w>w){
                                        adj.get(v).add(j,p2);
                                        flag=false;
                                        break;
                                }
                        }
                        if(flag) adj.get(v).add(p2);
                }
        }
        visited[0]=true;
        for(int e=0;e<vertex-1;e++){
                int min=Integer.MAX_VALUE;
                int x=0;
                int y=0;
                for(int i=0;i<vertex;i++){
                        if(visited[i]){ // u
                        // nodes connected with 'u'
                                for(Pair curr:adj.get(i)){
                                        if(!visited[curr.end] && curr.w<min){
                                                x=i;
                                                y=curr.end;
                                                min=curr.w;
                                        }
                                }
                        }
                }
                System.out.println(x+" "+y+" "+min);
                visited[y]=true;
        }        
    }
}

```

---

# Edge Output Ordering

The original problem says that the order of the MST edges does not matter.

However, the required format for each edge is:

```text
v1 v2 w
```

with:

```text
v1 <= v2
```

So the smaller vertex should be printed first.

For example:

```text
3 1 5
```

should be printed as:

```text
1 3 5
```

---

## Additional Sorting Logic

For my implementation, I also maintain a deterministic ordering between edges.

The sorting priority is:

```text
Start → End → Weight
```

All in ascending order.

### Priority 1 — Start

Compare the first vertex.

```text
0 3 5
1 2 4
```

`0 3 5` comes first because `0 < 1`.

### Priority 2 — End

If the start vertices are equal, compare the end vertices.

```text
0 5 3
0 2 7
```

becomes:

```text
0 2 7
0 5 3
```

because `2 < 5`.

### Priority 3 — Weight

If both start and end are equal, compare the weight.

```text
0 2 8
0 2 3
```

becomes:

```text
0 2 3
0 2 8
```

So the complete ordering is:

```text
1st → start
2nd → end
3rd → weight
```

---

# Brute Force Implementation

The current adjacency-list version performs the minimum-edge search manually:

```java
int min = Integer.MAX_VALUE;

for(int i = 0; i < vertex; i++){
    if(visited[i]){
        for(Pair curr : adj.get(i)){
            if(!visited[curr.end] && curr.w < min){
                x = i;
                y = curr.end;
                min = curr.w;
            }
        }
    }
}
```

This works, but every iteration scans the edges of all visited vertices again.

The next optimization is to replace this manual minimum search with a:

```text
Priority Queue
```

---

# Example

### Input

```text
4 4
0 1 3
0 3 5
1 2 1
2 3 8
```

The graph is:

```text
0 --3-- 1 --1-- 2
|                 |
5                 8
|                 |
+------- 3 -------+
```

### Prim's Algorithm

Start from vertex `0`.

Available edges:

```text
0 - 1 : 3
0 - 3 : 5
```

Choose:

```text
0 1 3
```

Visited:

```text
0, 1
```

New available edge:

```text
1 - 2 : 1
```

Choose:

```text
1 2 1
```

Visited:

```text
0, 1, 2
```

Available edge to the remaining vertex:

```text
0 - 3 : 5
2 - 3 : 8
```

Choose:

```text
0 3 5
```

Now all four vertices are connected.

Therefore the MST contains:

```text
0 1 3
1 2 1
0 3 5
```

---

# Comparison

| Representation                  |                              Time |      Space | Main idea                 |
| ------------------------------- | --------------------------------: | ---------: | ------------------------- |
| Adjacency Matrix + Prim         |                           `O(V²)` |    `O(V²)` | Scan matrix               |
| Adjacency List + manual minimum | depends on repeated edge scanning | `O(V + E)` | Scan candidate edges      |
| Adjacency List + Priority Queue |                     `O(E log E)`* | `O(V + E)` | Keep minimum edge in heap |

*Depending on the exact priority-queue implementation and how duplicate/stale edges are handled, this is commonly expressed as `O(E log E)`; with the standard adjacency-list Prim implementation, it is often also written as `O(E log V)`.

---

## Key Takeaways

* Prim's algorithm grows the MST one vertex at a time.
* At every step, choose the minimum-weight edge from a **visited** vertex to an **unvisited** vertex.
* An adjacency matrix gives a simple `O(V²)` implementation.
* An adjacency list is more memory-efficient for sparse graphs.
* A priority queue avoids repeatedly searching all candidate edges for the minimum.
* For output, each edge should be printed with the smaller vertex first.
* When deterministic ordering is required, the edge comparison can be:

```text
start → end → weight
```
