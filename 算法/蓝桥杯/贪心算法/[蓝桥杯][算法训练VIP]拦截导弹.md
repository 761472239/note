输入导弹依次飞来的高度（雷达给出的高度数据是不大于30000的正整数），计算这套系统最多能拦截多少导弹，如果要拦截所有导弹最少要配备多少套这种导弹拦截系统。

输入

一行，为导弹依次飞来的高度

输出

两行，分别是最多能拦截的导弹数与要拦截所有导弹最少要配备的系统数

样例输入

> 389  207  155  300  299  170  158  65

样例输出

> 6
> 2



```java
package 贪心;

import java.util.Arrays;
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-02-04
 * Time: 21:07
 * Description:
 * 地址：https://www.dotcpp.com/oj/problem1627.html
 */
public class 拦截导弹 {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String s = sc.nextLine().trim();
            String str[] = s.split(" ");
            int arr[] = new int[str.length];
            for (int i = 0; i < str.length; i++) {
                if (!str[i].equals(""))
                    arr[i] = Integer.parseInt(str[i]);
            }
            int dp[] = new int[arr.length];
            int dp1[] = new int[arr.length];

            for (int i = 0; i < arr.length; i++) {
                dp[i] = 1;
                dp1[i] = 1;
                for (int j = 0; j < i; j++) {
                    if (arr[i] <= arr[j]) {
                        dp[i] = Math.max(dp[i], dp[j] + 1);
                    } else {
                        dp1[i] = Math.max(dp1[i], dp1[j] + 1);
                    }
                }
            }


            int res = 0;
            int ans = 0;
            for (int i = 0; i < arr.length; i++) {
                res = Math.max(res, dp[i]);
                ans = Math.max(ans, dp1[i]);
            }
            System.out.println(res + "\n" + ans);
        }
    }

}

```

