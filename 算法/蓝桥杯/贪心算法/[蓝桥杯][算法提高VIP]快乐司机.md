# 问题 1917: [蓝桥杯][算法提高VIP]快乐司机

时间限制: 1Sec 内存限制: 128MB 提交: 290 解决: 101

**题目描述**

司机所**拉货物为散货**，如大米、面粉、沙石、泥土......
现在知道了汽车核载重量为w，可供选择的物品的数量n。每个物品的重量为gi,价值为pi。求汽车可装载的最大价值。（n<10000,w<10000,0<gi<=100,0<=pi<=100)

**输入**

输入第一行为由空格分开的两个整数n w
第二行到第n+1行，每行有两个整数，由空格分开，分别表示gi和pi

**输出**

最大价值（保留一位小数）

**样例输入**

```
5 36
99 87
68 36
79 43
75 94
7 35
```

**样例输出**

```
71.3
```

**提示**

无

**标签**

[排序](https://www.dotcpp.com/oj/problemset.php?tag=10005) [贪心](https://www.dotcpp.com/oj/problemset.php?tag=10015) 



```java
package 贪心;

import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-02-02
 * Time: 11:18
 * Description: 贪心 排序
 * 地址：https://www.dotcpp.com/oj/problem1917.html
 */
public class 快乐司机 {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int m = sc.nextInt();
            int gi[] = new int[n];
            double pi[] = new double[n];
            for (int i = 0; i < n; i++) {
                gi[i] = sc.nextInt();
                pi[i] = sc.nextInt();
                pi[i] = (1.0 * pi[i] / gi[i]);
            }
            sort(pi, gi, n);
            double res = 0.0;
            for (int i = n - 1; i >= 0; i--) {
                if (m >= gi[i]) {
                    res += pi[i] * gi[i];
                    m -= gi[i];
                } else {
                    res += m * pi[i];
                    break;
                }
            }
            System.out.printf("%.1f",res);
        }
    }

    private static void sort(double[] pi, int[] gi, int n) {
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < n - i; j++) {

                if (pi[j] > pi[j + 1]) {
                    double temp = pi[j];
                    pi[j] = pi[j + 1];
                    pi[j + 1] = temp;

                    int t = gi[j];
                    gi[j] = gi[j + 1];
                    gi[j + 1] = t;
                }
            }
        }
    }

}
```

