#  **问题 1887: [蓝桥杯][2017年第八届真题]正则问题** 

题目描述

考虑一种简单的正则表达式：
只由 x ( ) | 组成的正则表达式。
小明想求出这个正则表达式能接受的最长字符串的长度。


例如 ((xx|xxx)x|(x|xx))xx 能接受的最长字符串是： xxxxxx，长度是6。

输入

一个由x()|组成的正则表达式。输入长度不超过100，保证合法。

输出

这个正则表达式能接受的最长字符串的长度。

样例输入

```
((xx|xxx)x|(x|xx))xx
```

样例输出

```java
6
```

 (    (xx|**xxx**) **x**   |   (x|xx)  )    **xx** 能接受的最长字符串是： xxxxxx，长度是6。 

((2|3)1|(1|2))2

2|3



( 进

遇到x ++

遇到|判断

遇到）后面的x +

遇到| 再判断

dfs方法：

```java
package 字符串;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-29
 * Time: 19:32
 * Description:
 */
public class 正则问题 {

    private static int p;
    private static char a[];

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String str = sc.next();
            a = str.toCharArray();
            int res = dfs();
            System.out.println(res);
        }
    }

    private static int dfs() {
        int ans = 0;
        int result = 0;
        int len = a.length;
        while (p < len) {
            if (a[p] == 'x') {
                p++;
                ans++;
            } else if (a[p] == '(') {
                p++;
                ans = ans + dfs();
            } else if (a[p] == ')') {
                p++;
                break;
            } else {
                p++;
                result = Math.max(result, ans);
                ans = 0;
            }
        }
        result = Math.max(result, ans);
        return result;
    }

}
```

