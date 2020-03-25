一个坐标可以从 -infinity 延伸到 +infinity 的 无限大的 棋盘上，你的 骑士 驻扎在坐标为 [0, 0] 的方格里。

骑士的走法和中国象棋中的马相似，走 “日” 字：即先向左（或右）走 1 格，再向上（或下）走 2 格；或先向左（或右）走 2 格，再向上（或下）走 1 格。

每次移动，他都可以按图示八个方向之一前进。

![img](assets/aHR0cHM6Ly9hc3NldHMubGVldGNvZGUtY24uY29tL2FsaXl1bi1sYy11cGxvYWQvdXBsb2Fkcy8yMDE5LzA5LzIxL2tuaWdodC5wbmc.jpg)

现在，骑士需要前去征服坐标为 `[x, y]` 的部落，请你为他规划路线。

最后返回所需的最小移动次数即可。本题确保答案是一定存在的。



示例 1：

输入：x = 2, y = 1
输出：1
解释：[0, 0] → [2, 1]
示例 2：

输入：x = 5, y = 5
输出：4
解释：[0, 0] → [2, 1] → [4, 2] → [3, 4] → [5, 5]


提示：

|x| + |y| <= 300



**思路：**

问最短距离一般就是BFS，

但此题需要注意棋盘无限大，所以一定要限制下一步跳的范围，不能跳的太远，不然稳定超时。

一个优化： 让x和y都变成绝对值形式，因为可以跳的八个方向都是对称的，在哪个象限并没有区别。

别的也没啥特别的，可能就是方向向量和普通的八方向方向向量有所区别。

```java
package bfs;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-03-05
 * Time: 16:24
 * Description:从棋盘的一个点走到另一个点的最小步数，走的规则是国际象棋中骑士的走法，即走日字形
 */
public class 骑士出行 {
    static int x1, y1, x2, y2;
    static int dir[][] = {{2, 1}, {2, -1}, {-2, 1},
            {-2, -1}, {1, 2}, {-1, 2}, {1, -2}, {-1, -2}};//走位数组
    static int n;
    static int fp[][];

    static class Node {
        int x;
        int y;
        int dis;//距离起点的距离

        public Node(int x, int y, int dis) {
            this.x = x;
            this.y = y;
            this.dis = dis;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        fp = new int[n][n];
        x1 = sc.nextInt();
        y1 = sc.nextInt();
        x2 = sc.nextInt();
        y2 = sc.nextInt();
        int ans = bfs(x1, y1, x2, y2, n);
        System.out.println(ans);
    }

    private static int bfs(int x1, int y1, int x2, int y2, int n) {
        if (x1 == x2 && y1 == y2) {
            return 0;
        }
        Queue<Node> q = new LinkedList<>();
        q.offer(new Node(x1, y1, 0));
        fp[x1][y1] = 1;
        while (!q.isEmpty()) {
            Node node = q.poll();
            int dis = node.dis;
            for (int[] a : dir) {
                int xx = node.x + a[0];
                int yy = node.y + a[1];
                if (xx < 0 || yy < 0 || xx >= n || yy >= n || fp[xx][yy] == 1) {
                    continue;
                }
                if (xx == x2 && yy == y2) {
                    return dis + 1;
                }
                fp[xx][yy] = 1;
                q.offer(new Node(xx, yy, dis + 1));
            }

        }
        return 0;
    }
}

```

