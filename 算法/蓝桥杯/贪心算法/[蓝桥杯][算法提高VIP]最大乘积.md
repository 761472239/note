#  **问题 1936: [蓝桥杯][算法提高VIP]最大乘积** 

**题目描述**

对于n个数，从中取出m个数，如何取使得这m个数的乘积最大呢？

**输入**

第一行一个数表示数据组数
每组输入数据共2行：
第1行给出总共的数字的个数n和要取的数的个数m，1<=n<=m<=15，
第2行依次给出这n个数，其中每个数字的范围满足:a[i]的绝对值小于等于4。

**输出**

每组数据输出1行，为最大的乘积。

**样例输入**

```
1
5 5
1 2 3 4 2
```

**样例输出**

```
48
```

**地址**

 https://www.dotcpp.com/oj/problem1936.html 

**标签**

[贪心](https://www.dotcpp.com/oj/problemset.php?tag=10015) 

**分析**

因为a[i]有正数也有负数，负负得正，所以要先把数列排序，分开正负，分别两两相乘进行比较。

负数两两相乘，正数就单个相乘。

当有多个数字，要选择少量的数字的时候，不用担心，因为已经排好序了，经过判断会直接选择，后面最大的数字。

```java
package 贪心;

import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-02-02
 * Time: 9:57
 * Description: 贪心
 * 地址：https://www.dotcpp.com/oj/problem1936.html
 */
public class 最大乘积 {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int a = sc.nextInt();
        for (int i = 0; i < a; i++) {
            int n = sc.nextInt();
            int m = sc.nextInt();
            int arr[] = new int[n];
            for (int j = 0; j < n; j++) {
                arr[j] = sc.nextInt();
            }
            Arrays.sort(arr);
            long res = 1;
            int p = 0, q = n - 1;
            while (m > 0) {
                if (p < n - 1 && q > 0 && (arr[p] * arr[p + 1] > arr[q] * arr[q - 1]) && m >= 2) {
                    res *= (arr[p] * arr[p + 1]);
                    p += 2;
                    m -= 2;
                } else {
                    res *= arr[q];
                    q--;
                    m--;
                }
            }
            System.out.println(res);
        }

    }

}

```

