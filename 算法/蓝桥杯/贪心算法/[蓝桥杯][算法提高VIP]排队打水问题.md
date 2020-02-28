# 问题 1527: [蓝桥杯][算法提高VIP]排队打水问题

时间限制: 1Sec 内存限制: 128MB 提交: 242 解决: 109

题目描述

有n个人排队到r个水龙头去打水，他们装满水桶的时间t1、t2………..tn为整数且各不相等，应如何安排他们的打水顺序才能使他们总共花费的时间最少？

数据规模和约定
其中80%的数据保证n< =10





输入

第一行n，r (n< =500,r< =75) 
第二行为n个人打水所用的时间Ti (Ti< =100)；

输出

最少的花费时间 

样例输入

```
3  2 
1  2  3 
```

样例输出

```
7
```

```java
package 贪心;

import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-02-05
 * Time: 21:40
 * Description:
 * https://www.dotcpp.com/oj/problem1527.html
 */
public class 排队打水问题 {

    /*public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int r = sc.nextInt();
            int arr[] = new int[n+1];
            int time[] = new int[n+1];
            for (int i = 1; i <= n; i++) {
                arr[i] = sc.nextInt();
            }
            Arrays.sort(arr);
            int j = 0;
            int min = 0;
            for (int i = 1; i <= n; i++) {
                j++;
                if (j == r + 1) {
                    j = 1;
                }
                time[j] += arr[i];
                min += time[j];

            }
            System.out.println(min);

        }
    }*/


    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int r = sc.nextInt();
            int arr[] = new int[n];
            for (int i = 0; i < n; i++) {
                arr[i] = sc.nextInt();
            }
            Arrays.sort(arr);
            //1 2 3 4 5 6
            int time[] = new int[r];

            
            //类似的写法
            /*for(int i = 0; i < n; i ++)
            {
                if(i >= m)  f[i] += f[i - m];
                sum += f[i];
            }*/
            int res = 0;
            for (int j = 0; j < n; j++) {
                time[j % r] += arr[j];
                res += time[j % r];
            }
            System.out.println(res);
        }
    }
}

```

