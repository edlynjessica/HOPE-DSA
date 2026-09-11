# Number of Islands — Variation

> **Variation of LeetCode 200: Number of Islands**

## Problem

Given a binary matrix `grid` containing `0`s and `1`s, find all the islands in the grid.

An island is a group of connected `1`s, where cells are connected **up, down, left, or right**.

Instead of simply counting the islands, **assign a unique number(ranking) to each island**.

Start numbering the islands from `1`.

---

### Example

**Input:**

```text
4 5

1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

**Output:**

```text
1 1 0 0 0
1 1 0 0 0
0 0 2 0 0
0 0 0 3 3
```

Here:

* The first island is replaced by `1`
* The second island is replaced by `2`
* The third island is replaced by `3`
* `0` represents water

## Approach

1. Traverse every cell in the matrix.
2. When a `1` is found, start DFS.
3. Replace all connected `1`s with the current island number.
4. Increment the island number.
5. Finally, subtract `1` from every non-zero cell to convert the labels back to `1, 2, 3...`.

## Java code
```java
import java.util.*;

public class Main {
    static int name;
    static int m;
    static int n;
    public static void main(String[] args) {
      Scanner sc=new Scanner(System.in);
      m=sc.nextInt();
      n=sc.nextInt();
      name=2;
      int[][] grid=new int[m][n];
      for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
          grid[i][j]=sc.nextInt();
        }
      }
      for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
          if(grid[i][j]==1){
            dfs(i,j,grid);
            name++;
          }
        }
      }
      for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
          if(grid[i][j]!=0){
            grid[i][j]-=1;
          }
        }
      }
      for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
          System.out.print(grid[i][j]+" ");
        }
        System.out.println();
      }

    }
    public static void dfs(int r,int c,int[][] grid){
      if(r<0 || c<0 || r==m || c==n || grid[r][c]!=1) return;
      grid[r][c]=name;
      dfs(r+1,c,grid);
      dfs(r-1,c,grid);
      dfs(r,c+1,grid);
      dfs(r,c-1,grid);
    }
}
```

## Complexity

* **Time:** `O(m × n)`
* **Space:** `O(m × n)` in the worst case due to DFS recursion.

## Related Problem

[LeetCode 200 — Number of Islands](https://leetcode.com/problems/number-of-islands/)
