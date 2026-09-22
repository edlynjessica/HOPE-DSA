# Dijkstra's Algorithm — Single Source Shortest Path

## Problem

Given a weighted graph, find the **shortest distance from a source vertex to every other vertex** using Dijkstra's Algorithm.

Here, the source vertex is:

```text
0
```

---

## Intuition

Think of `dist[i]` as:

> **The shortest distance currently known from the source `0` to vertex `i`.**

Initially, only the source is reachable:

```text
dist[0] = 0
dist[others] = ∞
```

At every step:

> **Pick the unvisited vertex with the smallest known distance, then use it to improve the distances of its neighbours.**

Once the smallest-distance vertex is selected, its shortest distance is finalized.

```text
Find closest unvisited vertex
          ↓
Process its neighbours
          ↓
Try to find shorter paths
          ↓
Update dist[]
          ↓
Mark vertex visited
          ↓
Repeat
```

---

## Approach

I use:

* **Adjacency Matrix** to store the graph
* `dist[]` to store the shortest known distance from source `0`
* `visited[]` to mark vertices whose shortest distance has been finalized

### Main idea

1. Initialize every distance to `Integer.MAX_VALUE`.
2. Set the source distance:

   ```java
   dist[0] = 0;
   ```
3. Find the unvisited vertex with the smallest `dist[]`.
4. Use this vertex to relax/update its neighbours.
5. Mark the current vertex as visited.
6. Repeat until all vertices are processed.

---

## What is Relaxation?

Relaxation means checking whether going through the current vertex gives us a shorter path.

Suppose:

```text
dist[src] = 4
src ----3----> end
```

Then the distance through `src` is:

```text
dist[src] + edge weight
= 4 + 3
= 7
```

If:

```text
dist[end] > 7
```

we update:

```java
dist[end] = 7;
```

In the code:

```java
if(adj[src][end] != 0 &&
   !visited[end] &&
   dist[src] + adj[src][end] < dist[end])
{
    dist[end] = adj[src][end] + dist[src];
}
```

---

## Prim vs Dijkstra

They look similar, but they solve different problems.

### Prim

```text
Find the cheapest EDGE
→ build MST
```

### Dijkstra

```text
Find the closest VERTEX
→ build shortest paths from a source
```

In Prim, we care about:

```text
edge weight
```

In Dijkstra, we care about:

```text
total distance from source
```

---

## Dry Run

Consider:

```text
4 4

0 --3-- 1
0 --5-- 3
1 --1-- 2
2 --8-- 3
```

### Initial State

Source is `0`.

```text
dist = [0, ∞, ∞, ∞]

visited = [F, F, F, F]
```

---

### Step 1 — Pick vertex 0

The smallest unvisited distance is:

```text
dist[0] = 0
```

So:

```text
src = 0
```

Process its neighbours.

#### Vertex 1

```text
dist[0] + 3
= 0 + 3
= 3
```

So:

```text
dist[1] = 3
```

#### Vertex 3

```text
dist[0] + 5
= 0 + 5
= 5
```

So:

```text
dist[3] = 5
```

Now:

```text
dist = [0, 3, ∞, 5]

visited = [T, F, F, F]
```

---

### Step 2 — Pick vertex 1

Among unvisited vertices:

```text
dist[1] = 3
dist[3] = 5
dist[2] = ∞
```

Smallest is:

```text
src = 1
```

Process vertex `1`.

Its edge to `2` has weight `1`.

```text
dist[1] + 1
= 3 + 1
= 4
```

So:

```text
dist[2] = 4
```

Now:

```text
dist = [0, 3, 4, 5]

visited = [T, T, F, F]
```

---

### Step 3 — Pick vertex 2

Smallest unvisited distance:

```text
dist[2] = 4
```

So:

```text
src = 2
```

Process its neighbours.

For vertex `3`:

```text
dist[2] + 8
= 4 + 8
= 12
```

But currently:

```text
dist[3] = 5
```

Since:

```text
12 > 5
```

we don't update it.

Now:

```text
dist = [0, 3, 4, 5]

visited = [T, T, T, F]
```

---

### Step 4 — Pick vertex 3

Only vertex `3` remains.

```text
src = 3
dist[3] = 5
```

No shorter paths are found.

Final:

```text
dist = [0, 3, 4, 5]
```

### Output

```text
0 0
1 3
2 4
3 5
```

---

## Code

```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {

        Scanner sc=new Scanner(System.in);

        int V=sc.nextInt();
        int e=sc.nextInt();

        int[][] adj=new int[V][V];

        for(int i=0;i<e;i++){

            int u=sc.nextInt();
            int v=sc.nextInt();
            int w=sc.nextInt();

            adj[u][v]=w;
            adj[v][u]=w;
        }

        boolean[] visited=new boolean[V];

        int[] dist=new int[V];

        Arrays.fill(dist,Integer.MAX_VALUE);

        dist[0]=0;

        for(int i=0;i<V-1;i++){

            int min=Integer.MAX_VALUE;
            int src=0;

            // Find the unvisited vertex with minimum distance
            for(int j=0;j<V;j++){

                if(!visited[j] && dist[j]<min){

                    min=dist[j];
                    src=j;
                }
            }

            // Update distances of neighbours
            for(int end=0;end<V;end++){

                if(adj[src][end]!=0 &&
                   !visited[end] &&
                   dist[src]+adj[src][end]<dist[end]){

                    dist[end]=adj[src][end]+dist[src];
                }
            }

            // Shortest distance of src is finalized
            visited[src]=true;
        }

        for(int i=0;i<V;i++){

            System.out.println(i+" "+dist[i]);
        }
    }
}
```

---

## Complexity

Using an adjacency matrix and scanning all vertices to find the minimum:

```text
Time:  O(V²)
Space: O(V²)
```

The `O(V²)` time comes from:

1. Finding the minimum-distance unvisited vertex → `O(V)`
2. Scanning its neighbours → `O(V)`
3. Repeating for `V` vertices

So:

```text
O(V × V)
= O(V²)
```

---

## Key Pattern

Remember Dijkstra as:

```text
dist[0] = 0
dist[others] = ∞

        ↓

pick minimum unvisited dist

        ↓

relax neighbours

dist[end] =
    min(dist[end],
        dist[src] + edgeWeight)

        ↓

mark src visited

        ↓

repeat
```

The **one line to remember** is:

```java
dist[end] = Math.min(
    dist[end],
    dist[src] + adj[src][end]
);
```

That is the core of Dijkstra's Algorithm.
