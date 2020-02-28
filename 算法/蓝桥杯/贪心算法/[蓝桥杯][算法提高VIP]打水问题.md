#  [[蓝桥杯\][算法提高VIP]打水问题](https://www.dotcpp.com/oj/problem1523.html) 

问题 1523: [蓝桥杯][算法提高VIP]打水问题

时间限制: 1Sec 内存限制: 128MB 提交: 325 解决: 149

题目描述

N个人要打水，有M个水龙头，第i个人打水所需时间为Ti，请安排一个合理的方案使得所有人的等待时间之和尽量小。

提示
一种最佳打水方案是，将N个人按照Ti从小到大的顺序依次分配到M个龙头打水。
例如样例中，Ti从小到大排序为1，2，3，4，5，6，7，将他们依次分配到3个龙头，则去龙头一打水的为1，4，7；去龙头二打水的为2,5；去第三个龙头打水的为3,6。
第一个龙头打水的人总等待时间 = 0 + 1 + (1 + 4) = 6
第二个龙头打水的人总等待时间 = 0 + 2 = 2
第三个龙头打水的人总等待时间 = 0 + 3 = 3
所以总的等待时间 = 6 + 2 + 3 = 11



输入

第一行两个正整数N M 接下来一行N个正整数Ti。 
N,M< =1000，Ti< =1000 

输出

最小的等待时间之和。（不需要输出具体的安排方案） 

样例输入

```
7  3 
3  6  1  4  2  5  7 
```

样例输出

```
11
```



```java
package 贪心;

import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-02-06
 * Time: 10:45
 * Description:
 * 地址：https://www.dotcpp.com/oj/problem1523.html
 */
public class 打水问题 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int m = sc.nextInt();
            int arr[] = new int[n + 1];

            for (int i = 1; i <= n; i++) {
                arr[i] = sc.nextInt();
            }
            Arrays.sort(arr);
            System.out.println(Arrays.toString(arr));
            int dp[] = new int[m];
            int res = 0;
            for (int i = 1; i <= n; i++) {

                if (i <= m) {
                    res += 0;
                } else {
                    res += dp[(i - 1) % m];
                }
                dp[(i - 1) % m] += arr[i];
            }
            System.out.println(res);
        }
    }

}

```

