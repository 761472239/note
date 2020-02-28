#  **问题 1547: [蓝桥杯][算法提高VIP]理财计划** 

题目描述

银行近期推出了一款新的理财计划“重复计息储蓄”。储户只需在每个月月初存入固定金额的现金，银行就会在每个月月底根据储户账户内的金额算出该月的利息并将利息存入用户账号。现在如果某人每月存入k元，请你帮他计算一下，n月后，他可以获得多少收益。

输入

输入数据仅一行，包括两个整数k(100< =k< =10000)、n(1< =n< =48)和一个小数p(0.001< =p< =0.01)，分别表示每月存入的金额、存款时长、存款利息。 

输出

输出数据仅一个数，表示可以得到的收益。 

样例输入

```
1000 6 0.01 
```

样例输出

```java
213.53
```

```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            double a = sc.nextDouble();
            int n = sc.nextInt();
            double b = sc.nextDouble();
            double t = 0;
            double m = a;
            double lx = 0;
            while (n-- > 0) {
                t = m * b;
                lx += t;
                m = m + a + t;
            }
            int res = (int) (lx*100);
            System.out.print(res/100.0);
        }
    }

}
```



# 问题 1561: [蓝桥杯][算法提高VIP]计算质因子

题目描述

输入一个整数，输出其所有质因子。

数据规模和约定
1< =n< =10000。

输入

输入只有一行，包含一个整数n。 

输出

输出一行，包含若干个整数，为n的所有质因子，按照从小到大的顺序排列。 

样例输入

```
6 
```

样例输出

```java
2 3
```

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-26
 * Time: 18:17
 * Description:
 */
public class 计算质因子 {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            for (int i = 2; i < n; i++) {
                int flag = 0;
                for (int j = 2; j <= Math.sqrt(i); j++) {
                    if (i % j == 0)
                        flag++;
                }
                if (flag == 0 && n % i == 0)
                    System.out.print(i + " ");
            }
        }
    }

}	
```



#  问题 1569: [蓝桥杯][算法提高VIP]输入输出格式练习

题目描述

按格式格式读入一个3位的整数、一个实数、一个字符 。
并按格式输出 一个整数占8位左对齐、一个实数占8位右对齐、一个字符 ，并用|隔开。

输入

输出

无

样例输入

```
123456.789|a 
```

样例输出

```java
123     |   456.8|a
```

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-26
 * Time: 18:24
 * Description:按格式格式读入一个3位的整数、一个实数、一个字符  。
 * 并按格式输出  一个整数占8位左对齐、一个实数占8位右对齐、一个字符  ，并用|隔开。
 */
public class 输入输出格式练习 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String str = sc.nextLine();
            int num1 = Integer.parseInt(str.substring(0, 3));
            double num2 = 0;
            char c[] = str.toCharArray();
            char cc = 0;
            for (int i = 0; i < c.length; i++) {
                if (c[i] == '|') {
                    num2 = Double.parseDouble(str.substring(3, i));
                    cc = c[i + 1];
                }
            }
            System.out.printf("%-8d|%8.1f|%c", num1, num2, cc);
        }
    }

}
```



#  问题 1571: [蓝桥杯][算法提高VIP]输出正反三角形

题目描述

使用循环结构打印下述图形，打印行数n由用户输入。图中每行事实上包括两部分，中间间隔空格字符数m也由用户输入。

注意：两行之间没有空行。

输入

无

输出

无

样例输入

```
5 4 
```

样例输出

```java
        *    *********
       ***    *******
      *****    *****
     *******    ***
    *********    *
```

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-26
 * Time: 19:20
 * Description:
 *      *   *****
 *     ***   ***
 *    *****   *
 */
public class 输出正反三角形 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int m = sc.nextInt();

            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    System.out.print(" ");
                }
                for (int j = 0; j < 3 * n - 1 + m; j++) {
                    if (j < n - i - 1
                            || j > (n - i - 1 + 2 * i + 1 + m) + (2 * n - 1 - (2 * i + 1))
                            || ((j >= n - i - 1 + 2 * i + 1) && (j < n - i - 1 + 2 * i + 1 + m))
                    ) {
                        System.out.print(" ");
                    } else {
                        System.out.print("*");
                    }
                }
                System.out.println();
            }
        }
    }
}
```



```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++)
                System.out.printf(" ");
            for (int j = n - i; j > 1; j--)
                System.out.printf(" ");
            for (int k = 0; k <= i * 2; k++)
                System.out.printf("*");
            for (int j = 0; j < m; j++)
                System.out.printf(" ");
            for (int k = 1; k < (n - i) * 2; k++)
                System.out.printf("*");
            System.out.printf("\n");
        }
    }
}
```

#  **问题 1585: [蓝桥杯][算法训练VIP]链表数据求和操作** 

题目描述

**读入10个复数**，建立对应链表，然后求所有复数的和。

输入

无

输出

无

样例输入

```
1  2 
1  3 
4  5 
2  3 
3  1 
2  1 
4  2 
2  2 
3  3 
1  1 
```

样例输出

```
23+23i
```

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-26
 * Time: 21:55
 * Description:
 */
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int sum1 = 0;
        int sum2 = 0;
        for (int i = 0; i < 10; i++) {
            int x1 = sc.nextInt();
            int x2 = sc.nextInt();

            sum1 += x1;
            sum2 += x2;
        }
        System.out.println(sum1 + "+" + sum2 + "i");
    }

}
```



#  **问题 2194: [蓝桥杯][2018年第九届真题]递增三元组** 

题目描述

给定三个整数数组
A = [A1, A2, … AN],
B = [B1, B2, … BN],
C = [C1, C2, … CN]，
请你统计有多少个三元组(i, j, k) 满足：
\1. 1 <= i, j, k <= N
\2. Ai < Bj < Ck

输入

第一行包含一个整数N。 第二行包含N个整数A1, A2, ... AN。 第三行包含N个整数B1, B2, ... BN。 第四行包含N个整数C1, C2, ... CN。

输出

一个整数表示答案

样例输入

```
3
1 1 1
2 2 2
3 3 3
```

样例输出

```
27
```

提示

对于30%的数据，1 <= N <= 100 对于60%的数据，1 <= N <= 1000 对于100%的数据，1 <= N <= 100000 0 <= Ai, Bi, Ci <= 100000

```
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-26
 * Time: 22:27
 * Description:
 */
public class 递增三元组 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int arr1[] = new int[n];
            int arr2[] = new int[n];
            int arr3[] = new int[n];
            for (int i = 0; i < n; i++) {
                arr1[i] = sc.nextInt();
            }
            for (int i = 0; i < n; i++) {
                arr2[i] = sc.nextInt();
            }
            for (int i = 0; i < n; i++) {
                arr3[i] = sc.nextInt();
            }
            int res = 0;

            /**
             * 方法一
             */
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (arr1[i] < arr2[j]) {
                        for (int k = 0; k < n; k++) {
                            if (arr2[j] < arr3[k]) {
                                res++;
                            }
                        }
                    }
                }
            }
            System.out.println(res);
            
        }
    }

}
```

# 问题 1084: 用筛法求之N内的素数。

时间限制: 1Sec 内存限制: 64MB 提交: 8861 解决: 5268

题目描述

用筛法求之N内的素数。

输入

N

输出

0～N的素数

样例输入

```
100
```

样例输出

```
2
3
5
7
11
13
17
19
23
29
31
37
41
43
47
53
59
61
67
71
73
79
83
89
97
```

方法一：

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-27
 * Time: 13:25
 * Description:
 */
public class 用筛法求之N内的素数 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            for (int i = 0; i < n; i++) {
                if (ss(i)) {
                    System.out.println(i);
                }
            }
        }
    }

    private static boolean ss(int i) {
        if (i < 2) {
            return false;
        }
        int flag = 0;
        for (int j = 2; j * j <= i; j++) {
            if (i % j == 0) {
                flag++;
            }
        }
        if (flag != 0)
            return false;
        return true;
    }
}
```

方法二：

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-27
 * Time: 13:39
 * Description:
 */
public class 用筛法求之N内的素数2 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int arr[] = new int[10001];
            arr[0] = arr[1] = 1;
            for (int i = 1; i < n + 1; i++) {
                if (arr[i] == 0) {
                    for (int j = i + i; j < n + 1; j += i) {
                        arr[j] = 1;
                    }
                }
            }

            for (int i = 0; i < n + 1; i++) {
                if (arr[i] == 0)
                    System.out.println(i);
            }
        }
    }
}
```

#  **问题 1094: 字符串的输入输出处理** 

题目描述

字符串的输入输出处理。

输入

第一行是一个正整数N，最大为100。之后是多行字符串（行数大于N）， 每一行字符串可能含有空格，字符数不超过1000。

输出

先将输入中的前N行字符串（可能含有空格）原样输出，再将余下的字符串（不含有空格）以空格或回车分割依次按行输出。每行输出之间输出一个空行。

样例输入

```
2
www.dotcpp.com DOTCPP
A C M
D O T CPP
```

样例输出

```java
www.dotcpp.com DOTCPP

A C M

D

O

T

CPP
```

方法一：

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-27
 * Time: 13:58
 * Description:
 */
public class 字符串的输入输出处理 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        sc.nextLine();
        for (int i = 0; i < n; i++) {
            String str = sc.nextLine();
            System.out.println(str + "\n");
        }
        while (sc.hasNext()) {
            String str = sc.next();
            System.out.println(str + "\n");
        }
    }
}
```

方法二：

```java
	public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int i = 0;
        int k = 1;
        String str[] = new String[1000];
        while (scanner.hasNext()) {
            for (; i < n + 1; i++) {
                str[i] = scanner.nextLine();
            }
            str[i] = scanner.next();
            i++;
            while (k < i) {
                System.out.println(str[k] + "\n");
                k++;
            }
        }
    }
```



# **问题 1095: The 3n + 1 problem** 

题目描述

Consider the following algorithm to generate a sequence of numbers. Start with an integer n. If n is even, divide by 2. If n is odd, multiply by 3 and add 1. Repeat this process with the new value of n, terminating when n = 1. For example, the following sequence of numbers will be generated for n = 22: 22 11 34 17 52 26 13 40 20 10 5 16 8 4 2 1 It is conjectured (but not yet proven) that this algorithm will terminate at n = 1 for every integer n. Still, the conjecture holds for all integers up to at least 1, 000, 000. For an input n, the cycle-length of n is the number of numbers generated up to and including the 1. In the example above, the cycle length of 22 is 16. Given any two numbers i and j, you are to determine the maximum cycle length over all numbers between i and j, including both endpoints.

考虑下面的算法来生成一个数字序列。从整数n开始。如果n是偶数，则除以2。如果n是奇数，乘以3再加1。用新值n重复此过程，当n=1时终止。例如，将为n=22生成以下数字序列：22 11 34 17 52 26 13 40 20 10 5 16 8 4 2 1据推测（但尚未证明），对于每个整数n，此算法将在n=1处终止。尽管如此，对于所有小于或等于1000000的整数，此推测仍然成立。对于输入n，n的循环长度是在1之前（包括1）生成的数目。在上面的例子中，22的循环长度是16。给定两个数字i和j，你要确定在i和j之间的所有数字上的最大周期长度，包括两个端点。

输入

The input will consist of a series of pairs of integers i and j, one pair of integers per line. All integers will be less than 1,000,000 and greater than 0.

输出

For each pair of input integers i and j, output i, j in the same order in which they appeared in the input and then the maximum cycle length for integers between and including i and j. These three numbers should be separated by one space, with all three numbers on one line and with one line of output for each line of input.

样例输入

```
1 10
100 200
201 210
900 1000
```

样例输出

```
1 10 20
100 200 125
201 210 89
900 1000 174
```

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-27
 * Time: 15:14
 * Description:
 */
public class The3n1problem {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int x1 = sc.nextInt();
            int x2 = sc.nextInt();
            System.out.print(x1 + " " + x2 + " ");
            if (x1 > x2) {
                int t = x1;
                x1 = x2;
                x2 = t;
            }
            int max = 0;
            for (int i = x1; i <= x2; i++) {
                int temp = len(i);
                if (temp > max) {
                    max = temp;
                }
            }
            System.out.println(max);
        }
    }

    private static int len(int i) {
        int len = 0;
        while (i != 1) {
            if (i % 2 == 0) {
                i = i / 2;
            } else {
                i = i * 3 + 1;
            }
            len++;
        }
        return len + 1;
    }
}
```



#  **问题 1097: 蛇行矩阵** 

题目描述

蛇形矩阵是由1开始的自然数依次排列成的一个矩阵上三角形。

输入

本题有多组数据，每组数据由一个正整数N组成。（N不大于100）

输出

对于每一组数据，输出一个N行的蛇形矩阵。两组输出之间不要额外的空行。矩阵三角中同一行的数字用一个空格分开。行尾不要多余的空格。

样例输入

```
5
```

样例输出

```java
1 3 6 10 15
2 5 9 14
4 8 13
7 12
11
```

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-27
 * Time: 15:45
 * Description:
 */
public class 蛇行矩阵 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int arr[][] = new int[n][n];
            arr[0][0] = 1;
            for (int i = 0; i < n - 1; i++) {
                for (int j = 1; j < n; j++) {
                    arr[i][j] = arr[i][j - 1] + j + i + 1;
                }
                arr[i + 1][0] = arr[i][0] + i + 1;
            }

            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n - i; j++) {
                    System.out.print(arr[i][j] + " ");
                }
                System.out.println();
            }
        }
    }
}

```

