# The Mystery of the Haunted Grid

## Problem Summary

Given an `m × n` grid:

* `-1` → wall
* `0` → gate
* `2147483647` → empty room

For every empty room, replace it with the **shortest distance to the nearest gate**.

If an empty room cannot reach any gate, leave it as `2147483647`.

### Example

**Input**

```text
4 4
2147483647 -1 0 2147483647
2147483647 2147483647 2147483647 -1
2147483647 -1 2147483647 -1
0 -1 2147483647 2147483647
```

**Output**

```text
3 -1 0 1
2 2 1 -1
1 -1 2 -1
0 -1 3 4
```

## Approach

This problem is naturally solved using **Multi-Source BFS**:

* Start BFS simultaneously from all gates (`0`).
* Expand level by level.
* The first time we reach an empty room gives its shortest distance.

However, I solved it using **DFS as practice**.

### DFS Idea

For every gate:

1. Start DFS with distance `0`.
2. Move in the four possible directions.
3. Stop when:

   * We go outside the grid.
   * We hit a wall.
   * The current cell already has a smaller distance.
   * The cell was already visited as a gate.
4. Update the room with the minimum distance found.

The important pruning condition is:

```java
grid[r][c] < dist
```

If the cell already contains a smaller distance, there is no reason to continue from that path.

## Complexity

For the standard **Multi-Source BFS** solution:

* **Time:** `O(m × n)`
* **Space:** `O(m × n)`

For this DFS implementation, the worst-case behavior can be worse than BFS because multiple gates may explore overlapping regions.

## Java — DFS Solution

```java
import java.io.*;
import java.util.*;

public class Solution {

    static int m;
    static int n;
    static int[][] dirs;
    static boolean[][] visited;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        m = sc.nextInt();
        n = sc.nextInt();

        int[][] grid = new int[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                grid[i][j] = sc.nextInt();
            }
        }

        visited = new boolean[m][n];

        dirs = new int[][] {
            {0, 1},
            {1, 0},
            {-1, 0},
            {0, -1}
        };

        // Run DFS from every gate
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) {
                    dfs(i, j, 0, grid);
                }
            }
        }

        // Print result
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                System.out.print(grid[i][j] + " ");
            }
            System.out.println();
        }
    }

    static void dfs(int r, int c, int dist, int[][] grid) {

        // Boundary / wall / already better distance
        if (r < 0 || c < 0 || r == m || c == n ||
            grid[r][c] == -1 ||
            grid[r][c] < dist ||
            visited[r][c]) {
            return;
        }

        // Gate
        if (grid[r][c] == 0) {
            visited[r][c] = true;
        }
        // Unvisited empty room
        else if (grid[r][c] == Integer.MAX_VALUE) {
            grid[r][c] = dist;
        }
        // Already has a distance, keep the minimum
        else {
            grid[r][c] = Math.min(grid[r][c], dist);
        }

        // Explore 4 directions
        for (int[] d : dirs) {
            dfs(r + d[0], c + d[1], dist + 1, grid);
        }
    }
}
```

## Note

> **Standard solution:** Multi-Source BFS
> **My practice solution:** DFS with distance-based pruning.

The BFS solution is preferable for this problem because shortest paths in an unweighted grid are naturally handled level-by-level.
