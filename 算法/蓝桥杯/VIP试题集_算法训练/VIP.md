# [VIP试题 找零钱](http://lx.lanqiao.cn/problem.page?gpid=T586)

问题描述

　　有n个人正在饭堂排队买海北鸡饭。每份海北鸡饭要25元。奇怪的是，每个人手里只有一张钞票（每张钞票的面值为**25、50、100**元），而且饭堂阿姨一开始没有任何零钱。请问饭堂阿姨能否给所有人找零（假设饭堂阿姨足够聪明）

输入格式

　　第一行一个整数n，表示排队的人数。

　　接下来n个整数a[1],a[2],...,a[n]。a[i]表示第i位学生手里钞票的价值（i越小，在队伍里越靠前）

输出格式

　　输出YES或者NO

样例输入

> 4
> 25 25 50 50

样例输出

> YES

样例输入

> 2
> 25 100

样例输出

> NO

样例输入

> 4
> 25 25 50 100

样例输出

> YES

数据规模和约定

> n不超过1000000





```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Queue<Integer> queue = new LinkedList<>();
        int n = sc.nextInt();
        int m25 = 0;
        int m50 = 0;

        for (int i = 0; i < n; i++) {
            int m = sc.nextInt();
            if (m == 25) {
                m25++;
            } else {
                if (m == 100) {
                    if (m50 >= 1 && m25 >= 1) {
                        m50--;
                        m25--;
                        continue;
                    }
                    if (m25 >= 3) {
                        m25 -= 3;
                        continue;
                    }
                    System.out.println("NO");
                    return;
                } else {
                    if (m25 >= 1) {
                        m25--;
                        m50++;
                       continue;
                    }
                    System.out.println("NO");
                    return;
                }
            }
        }
        System.out.println("YES");
    }
}
```





# [VIP试题 多阶乘计算](http://lx.lanqiao.cn/problem.page?gpid=T585)



问题描述

　　我们知道，阶乘n!表示$n*(n-1)*(n-2)*......*2*1$, 类似的，可以定义多阶乘计算，例如：$5！！=5*3*1$,依次可以有$!...!(k个！，可以简单表示为n(k)!)=n*(n-k)*(n-2k)*....（直到最后一个数<=0）$。
　　现给定一组数据n、k、m,当m=1时，计算并输出n(1)!+n(2)!+......+n(k)!的值，m=2时计算并输出$n(1)!+n(2)!+......+n(k)!$的各个位上的数字之和。

输入格式

　　两行，第一行为n和k，第二行为m。

输出格式

　　一行,为$n(1)!+n(2)!+......+n(k)!$的值或$n(1)!+n(2)!+......+n(k)!$的各个位上的数字之和。

样例输入

> 5 1
> 2

样例输出

> 3

数据规模和约定

> 　0 < k < n <= 20

特殊值

> 15 5
> 1
> 1307676428400

> 7 3
> 2 
> 16



```java
package 蓝桥杯.VIP算法训练;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-03-18
 * Time: 21:33
 * Description:
 */
public class 多阶乘计算 {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int k = sc.nextInt();
        int m = sc.nextInt();

        long ans = 0;

        /*n(1)!+n(2)!+......+n(k)!*/
        for (int i = 1; i <= k; i++) {
            int nn = n;
            long t = 1;
            while (nn > 0) {
                t *= nn;
                nn -= i;
            }
            ans += t;
        }
        if (m == 1) {
            System.out.println(ans);
        } else {
            long t = 0;
            while (ans > 0) {
                t += ans % 10;
                ans /= 10;
            }
            System.out.println(t);
        }


    }

}

```

