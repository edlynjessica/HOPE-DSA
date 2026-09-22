# Prim's Algorithm — Minimum Spanning Tree

## Problem

Given a connected, undirected, weighted graph, find the **minimum total weight of its Minimum Spanning Tree (MST)** using Prim's Algorithm.

An MST:

* connects all vertices
* contains exactly `V - 1` edges
* contains no cycles
* has minimum possible total edge weight

---

## Approach

I use:

* **Adjacency List** to store the graph
* **PriorityQueue (Min Heap)** to always get the minimum-weight candidate edge
* **Visited array** to keep track of vertices already included in the MST

### Main idea

Start from vertex `0`.

1. Mark vertex `0` as visited.
2. Add all edges of vertex `0` to the PriorityQueue.
3. Take the minimum-weight edge from the PriorityQueue.
4. If its destination vertex is already visited, skip it.
5. Otherwise:

   * add its weight to the answer
   * mark the destination as visited
   * add all edges from this newly visited vertex to unvisited vertices into the PriorityQueue.
6. Repeat until `V - 1` edges are selected.

### Important idea

The PriorityQueue contains **candidate edges** that can connect the current MST to an unvisited vertex.

For example:

```text
MST vertices = {0, 1}

Candidate edges:
0 → 3 = 5
1 → 2 = 1
```

The PriorityQueue chooses:

```text
1 → 2 = 1
```

because its weight is smaller.

---

## Why `visited[curr.end]`?

The graph is undirected, so every edge is stored twice.

For example:

```text
0 --3-- 1
```

is stored as:

```text
0 → 1 = 3
1 → 0 = 3
```

After `0 → 1` is selected, the reverse edge `1 → 0` may later be present in the PriorityQueue.

Since `0` is already part of the MST:

```java
if(visited[curr.end]) continue;
```

skips that edge.

---

## Why use `while(count < V-1)`?

The MST must contain exactly `V - 1` **selected edges**.

We increment `count` only when an edge is actually selected:

```java
ans += curr.w;
visited[curr.end] = true;
count++;
```

This is important because the PriorityQueue can contain edges whose destination is already visited.

Those edges are skipped and should **not** count toward the `V - 1` MST edges.

---

## PriorityQueue Comparator

```java
(a,b) -> {
    if(a.w != b.w) return a.w-b.w;
    else if(a.start != b.start) return a.start-b.start;
    return a.end-b.end;
}
```

The important priority is:

```text
weight → start → end
```

The **weight must come first** because Prim's Algorithm always chooses the minimum-weight candidate edge.

`start` and `end` are only tie-breakers when two edges have the same weight.

---

## Dry Run

Consider:

```text
V = 4

Edges:
0 --3-- 1
0 --5-- 3
1 --1-- 2
2 --8-- 3
```

### Step 1 — Start from vertex 0

```java
visited[0] = true;
```

```text
Visited = {0}
```

Add all edges of `0`:

```text
PQ:
(0,1,3)
(0,3,5)
```

---

### Step 2 — Pick minimum edge

PQ gives:

```text
(0,1,3)
```

`1` is not visited, so select it.

```text
MST:
0 → 1 = 3

Visited = {0,1}
Answer = 3
count = 1
```

Now add edges of newly visited vertex `1`.

Its edges are:

```text
1 → 0 = 3
1 → 2 = 1
```

`0` is already visited, so skip it.

Add:

```text
1 → 2 = 1
```

PQ becomes:

```text
(1,2,1)
(0,3,5)
```

---

### Step 3 — Pick minimum edge

PQ gives:

```text
(1,2,1)
```

`2` is unvisited, so select it.

```text
MST:
0 → 1 = 3
1 → 2 = 1

Visited = {0,1,2}
Answer = 4
count = 2
```

Add edges of vertex `2`:

```text
2 → 1 = 1   ❌ visited
2 → 3 = 8   ✅ unvisited
```

Add:

```text
2 → 3 = 8
```

PQ:

```text
(0,3,5)
(2,3,8)
```

---

### Step 4 — Pick minimum edge

PQ gives:

```text
(0,3,5)
```

`3` is unvisited, so select it.

```text
MST:
0 → 1 = 3
1 → 2 = 1
0 → 3 = 5

Visited = {0,1,2,3}
Answer = 9
count = 3
```

Since:

```text
count = V - 1
3 = 4 - 1
```

the loop ends.

### Final Answer

```text
9
```

---

## Code

```java
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

class Solution {
    public int spanningTree(int V, int[][] edges) {

        List<List<Pair>> adj = new ArrayList<>();

        int ec = edges.length;

        for(int i=0;i<V;i++){
            adj.add(new ArrayList<>());
        }

        // Build adjacency list
        for(int i=0;i<ec;i++){

            int[] e = edges[i];

            int u = e[0];
            int v = e[1];
            int w = e[2];

            adj.get(u).add(new Pair(u,v,w));
            adj.get(v).add(new Pair(v,u,w));
        }

        boolean[] visited = new boolean[V];

        // Min Heap: weight -> start -> end
        PriorityQueue<Pair> pq = new PriorityQueue<>(
            (a,b) -> {
                if(a.w != b.w)
                    return a.w-b.w;

                else if(a.start != b.start)
                    return a.start-b.start;

                return a.end-b.end;
            }
        );

        // Start Prim's Algorithm from vertex 0
        for(Pair p : adj.get(0)){
            pq.add(p);
        }

        visited[0] = true;

        int count = 0;
        int ans = 0;

        while(count < V-1){

            Pair curr = pq.poll();

            // Destination already belongs to MST
            if(visited[curr.end])
                continue;

            // Select this edge
            ans += curr.w;
            visited[curr.end] = true;
            count++;

            // Add new candidate edges
            for(Pair p : adj.get(curr.end)){
                if(!visited[p.end]){
                    pq.add(p);
                }
            }
        }

        return ans;
    }
}
```

## Complexity

Using an adjacency list and PriorityQueue:

```text
Time:  O(E log E)
Space: O(V + E)
```

The important pattern to remember:

```text
visited vertex
      ↓
add its edges to PQ
      ↓
PQ gives minimum-weight candidate
      ↓
if destination unvisited
      ↓
select edge
      ↓
new vertex becomes visited
      ↓
repeat
```
