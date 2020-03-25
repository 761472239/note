> 来自——[P1451 求细胞数量 - 洛谷 | 计算机科学教育新生态](https://www.luogu.com.cn/problem/P1451)


【问题描述】
一矩形阵列由数字0到9组成，数字1到9代表细胞，细胞的定义为沿细胞数字上下左右还是细胞数字为同一细胞，求所给矩形阵列的细胞个数。如下阵列有4个细胞。

0234500067
1034560500
2045600671
0000000089
Input

【输入格式】
整数m、n（m行n列）矩阵
【输入样例】

```
4 10
0234500067
1034560500
2045600671
0000000089
```

Output

【输出格式】
细胞的个数

【输出样例】

```
4
```

BFS

```java
import java.util.Scanner;

/**

 * Created with IntelliJ IDEA.

 * Mail: 761472239@qq.com

 * Date: 2019-12-05

 * Time: 15:17

 * Description:
   */
   public class Main1 {
   static int[][] cell = null;
   static int m;
   static int n;

   public static void main(String[] args) {

       Scanner sc = new Scanner(System.in);
       while (sc.hasNext()) {
       
           int num = 0;
           m = sc.nextInt();
           n = sc.nextInt();
           sc.nextLine();
           cell = new int[m][n];
           for (int i = 0; i < m; i++) {
               char c[] = sc.nextLine().toCharArray();
               for (int j = 0; j < n; j++) {
                   cell[i][j] = c[j] - 48;
               }
           }
       
           for (int i = 0; i < m; i++) {
               for (int j = 0; j < n; j++) {
       
                   if (cell[i][j] != 0) {
                       num++;
                       bfs(i, j);
                   }
               }
           }
           System.out.println(num);
       }


    }
    
    private static void bfs(int x, int y) {
        if (cell[x][y] == 0) {
            return;
        }
    
        if ((cell[x][y] != 0)) {
            cell[x][y] = 0;
        }
    
        if (x + 1 < m) {
            dfs(x + 1, y);
        }
        if (x >= 1) {
            dfs(x - 1, y);
        }
        if (y + 1 < n) {
            dfs(x, y + 1);
        }
        if (y >= 1) {
            dfs(x, y - 1);
        }
    
    }

}


```


