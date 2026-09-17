# Cartographer's Log

You're an expedition cartographer surveying a newly discovered archipelago represented as an N x N grid. Your ship's log always records an island by the **first tile the scouts ever set foot on** (found by sweeping the map row by row), and every subsequent tile is logged as a direction relative to the previous tile, in the fixed scouting order R, U, D, L. Given a coordinate, report which tile the island log started at, and the exact sequence of directions the scouts logged to reach that coordinate.

## Input Format

N
N lines, each with N space-separated integers (0 or 1)
a b (the query point, 0-indexed row and column)

## Constraints

1 ≤ N ≤ 500
0 ≤ a, b < N
grid[i][j] ∈ {0, 1}

## Output Format

If `grid[a][b] == 0`:

```text
NO ISLAND
```

Else, print two lines:

```text
x y
S m1 m2 ... mk
```

where `(x, y)` is the leader cell and the second line is the path array (space-separated).

## Sample Input 0

```text
5
1 1 0 0 0
0 1 0 0 1
0 0 0 1 1
0 0 0 0 0
1 0 0 0 1
2 4
```

## Sample Output 0

```text
1 4
S D
```

## Explanation 0

The island containing `(2,4)` is `{(1,4), (2,4), (2,3)}`. Scanning row-major, `(1,4)` is hit before `(2,3)` / `(2,4)`, so leader = `(1,4)`.

DFS order is `R, U, D, L`. From `(1,4)`, `R` and `U` are invalid, then `D` reaches `(2,4)`, which is the target.

## Sample Input 1

```text
5
1 1 0 0 0
0 1 0 0 1
0 0 0 1 1
0 0 0 0 0
1 0 0 0 1
2 3
```

## Sample Output 1

```text
1 4
S D L
```

## Explanation 1

The same island is used. From leader `(1,4)`, `D` reaches `(2,4)`, and then `L` reaches `(2,3)`.

## Sample Input 2

```text
1
0
0 0
```

## Sample Output 2

```text
NO ISLAND
```

---

# Approach

### 1. Rank each island

I scan the grid row by row.

Whenever I find a `1`, I start a recursive traversal and replace all connected `1`s with a unique number starting from `2`.

For example:

```text
1 1 0
0 1 0
0 0 1
```

can become:

```text
2 2 0
0 2 0
0 0 3
```

This lets me identify which island the query belongs to.

### 2. Find the query island

After ranking, I check:

```java
int island = grid[q[0]][q[1]];
```

If it is `0`, the query is water, so I print:

```text
NO ISLAND
```

### 3. Find the leader

I scan the ranked grid again from top-left to bottom-right.

The first cell having the same island number as the query is the leader.

### 4. DFS from the leader

I start DFS from the leader using the fixed order:

```text
R → U → D → L
```

The DFS only moves to cells belonging to the same island.

Instead of using a separate `visited[][]`, I mark a visited cell as `-1` directly in the grid.

The path is stored as a `String` and each direction is appended as DFS moves.

---

# Java Code

```java
import java.io.*;
import java.util.*;

public class Solution {
    static int n;
    static int ranking;
    static int[][] dirs;
    static int[] q;
    static int[][] grid;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        n = sc.nextInt();
        ranking = 2;
        grid = new int[n][n];

        dirs = new int[][]{
                {0,1},   // right
                {-1,0},  // up
                {1,0},   // down
                {0,-1}   // left
        };

        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                grid[i][j] = sc.nextInt();
            }
        }

        // grp and rank the islands
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == 1){
                    rank(i, j, grid);
                    ranking++;
                }
            }
        }

        // query points
        q = new int[2];
        q[0] = sc.nextInt();
        q[1] = sc.nextInt();

        int island = grid[q[0]][q[1]];

        if(grid[q[0]][q[1]] == 0){
            System.out.println("NO ISLAND");
            return;
        }

        int[] lead = new int[2];

        // find lead coord
        for(int i = 0; i < n; i++){
            boolean found = false;

            for(int j = 0; j < n; j++){
                if(grid[i][j] == island){
                    lead[0] = i;
                    lead[1] = j;
                    found = true;
                    break;
                }
            }

            if(found) break;
        }

        System.out.println(lead[0] + " " + lead[1]);

        dfs(lead[0], lead[1], "S", island);
    }

    static void dfs(int x, int y, String path, int island){
        if(x < 0 || y < 0 || x == n || y == n || grid[x][y] != island)
            return;

        grid[x][y] = -1; // marking visited

        if(x == q[0] && y == q[1]){
            System.out.println(path);
            return;
        }

        // right
        dfs(x, y + 1, path + " R", island);

        // up
        dfs(x - 1, y, path + " U", island);

        // down
        dfs(x + 1, y, path + " D", island);

        // left
        dfs(x, y - 1, path + " L", island);
    }

    static void rank(int r, int c, int[][] grid){
        if(r < 0 || c < 0 || r == n || c == n || grid[r][c] != 1)
            return;

        grid[r][c] = ranking;

        for(int[] d : dirs){
            rank(r + d[0], c + d[1], grid);
        }
    }
}
```
