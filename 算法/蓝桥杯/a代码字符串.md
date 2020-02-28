#  **问题 1461: [蓝桥杯][基础练习VIP]FJ的字符串** 

题目描述

FJ在沙盘上写了这样一些字符串：

A1 = “A”

A2 = “ABA”

A3 = “ABACABA”

A4 = “ABACABADABACABA”

… …

你能找出其中的规律并写所有的数列AN吗？

输入

仅有一个数：N ≤ 26。

输出

请输出相应的字符串AN，以一个换行符结束。输出中不得含有多余的空格或换行、回车符。 

样例输入

```
3 
```

样例输出

```
ABACABA
```

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-27
 * Time: 16:09
 * Description:
 */
public class FJ的字符串 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();

            StringBuffer buffer = new StringBuffer("A");

            for (int i = 1; i < n; i++) {
                String temp = String.valueOf((char) ('A' + i));
                buffer.append(temp + buffer.toString());
            }
            System.out.println(buffer.toString());
        }
    }
}
```



#  **问题 1466: [蓝桥杯][基础练习VIP]字符串对比** 

题目描述

给定两个仅由大写字母或小写字母组成的字符串(长度介于1到10之间)，它们之间的关系是以下4中情况之一：

1：两个字符串长度不等。比如 Beijing 和 Hebei

2：两个字符串不仅长度相等，而且相应位置上的字符完全一致(区分大小写)，比如 Beijing 和 Beijing

3：两个字符串长度相等，相应位置上的字符仅在不区分大小写的前提下才能达到完全一致（也就是说，它并不满足情况2）。比如 beijing 和 BEIjing

4：两个字符串长度相等，但是即使是不区分大小写也不能使这两个字符串一致。比如 Beijing 和 Nanjing

编程判断输入的两个字符串之间的关系属于这四类中的哪一类，给出所属的类的编号。

输入

包括两行，每行都是一个字符串 

输出

仅有一个数字，表明这两个字符串的关系编号 

样例输入

```
BEIjing 

beiJing  
```

样例输出

```java
3
```

**Java中Scanner类中的方法next()和nextLine()都是吸取输入台输入的字符，区别：**

- 　**next()不会吸取字符前/后的空格/Tab键，只吸取字符，开始吸取字符（字符前后不算）直到遇到空格/Tab键/回车截止吸取；**
- 　**nextLine()吸取字符前后的空格/Tab键，回车键截止。**

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-27
 * Time: 16:24
 * Description:
 */
public class 字符串对比 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String str1 = sc.next();
            String str2 = sc.next();
            if (str1.length() != str2.length()) {//先比较长度
                System.out.println(1);
            } else {//在长度不相等的情况下比较下面两个
                if (str1.equals(str2)) {
                    System.out.println(2);
                } else {
                    if (str1.toUpperCase().equals(str2.toUpperCase())) {
                        System.out.println(3);
                    } else {
                        System.out.println(4);
                    }
                }
            }
        }
    }
}
```

#  **问题 1467: [蓝桥杯][基础练习VIP]完美的代价** 

题目描述

回文串，是一种特殊的字符串，它从左往右读和从右往左读是一样的。小龙龙认为回文串才是完美的。现在给你一个串，它不一定是回文的，请你计算最少的交换次数使得该串变成一个完美的回文串。

交换的定义是：交换两个相邻的字符

例如mamad

第一次交换 ad : mamda

第二次交换 md : madma

第三次交换 ma : madam (回文！完美！)



输入

第一行是一个整数N，表示接下来的字符串的长度(N < = 8000) 

第二行是一个字符串，长度为N.只包含小写字母 

输出

如果可能，输出最少的交换次数。 

否则输出Impossible 

样例输入

```
5 

mamad 
```

样例输出

```java
3
```

【分析】对于字符串第一个字符，从字符串的最后一个字符开始匹配，如果找到第一个匹配的位置，将它换到倒数第二的位置，并记录这一次转换所需的次数；如果没有找到匹配的位置，这个字符可能就会是最中间的那个字符，用一个布尔变量记录是否需要将这个可能是中间的字符是否存在。题目的关键就是这个可能是中间的字符需不需要换到中间位置的这种情况，如果将这个字符换到中间，那么以后的字符每次变换都会改变这个中间字符的位置。这个中间字符的左边是已经变换好的，只需要将中间字符的右边作变换就可以了，所以可以将中间字符在最后做变换（下面的代码没有在最后实际作变换，只是统计了它变换需要的次数），最后将右边的字符作回文处理就可以了，如果处理的过程中再次出现可能是中间的字符，那么这种情况就是不可能的了。
————————————————
版权声明：本文为CSDN博主「柳婼」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/liuchuo/article/details/56677012

```java
import java.util.Scanner;
public class 完美的代价 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String str = sc.next();
        char arr[] = str.toCharArray();
        sc.close();
        //回文
        if (palindrome(arr, 0, n - 1)) {
            System.out.println(cnt);
        } else {
            System.out.println("Impossible");
        }
    }

    //交换次数
    private static int cnt = 0;
    private static boolean haveMiddle = false;

    /**
     * @param arr 数组
     * @param i   0
     * @param i1  最后索引
     * @param oe  true为偶数 false 为基数
     *            1. 先判断奇偶，奇数有中间字母，偶数没有。
     *            2. odd-even 奇偶
     * @return false不是回文
     */
    private static boolean palindrome(char[] arr, int a, int b) {
        if (b <= a)
            return true;
        for (int i = b; i > a; i--) {
            //从右往左遍历，如果有一样的则调换，如果没有则判断是否可能为中间字母。
            if (arr[a] == arr[i]) {
                exchange(arr, i, b);
                cnt += exchangeTimes(i, b);
                return palindrome(arr, a + 1, b - 1);
            }
        }
        // 遍历了一遍，没有找到一样的
        if (!haveMiddle) {
            haveMiddle = true;
            cnt += exchangeTimes(a, arr.length / 2);
            return palindrome(arr, a + 1, b);
        }
        return false;
    }

    private static int exchangeTimes(int a, int b) {
        return b - a;
    }

    //一个字符顺序交换
    private static void exchange(char[] arr, int a, int b) {
        char temp = arr[a];
        for (int i = a; i < b; i++) {
            arr[i] = arr[i + 1];
        }
        arr[b] = temp;
    }
}
```

#  **问题 1620: [蓝桥杯][算法训练VIP]字符串的展开** 

题目描述

在初赛普及组的“阅读程序写结果”的问题中，我们曾给出一个字符串展开的例子：如果在输入的字符串中，含有 类似于“d-h”或者“4-8”的字串，我们就把它当作一种简写，输出时，用连续递增的字母获数字串替代其中的减号，即，将上面两个子串分别输出为 “defgh”和“45678”。在本题中，我们通过增加一些参数的设置，使字符串的展开更为灵活。具体约定如下：
(1) 遇到下面的情况需要做字符串的展开：在输入的字符串中，出现了减号“-”，减号两侧同为小写字母或同为数字，且按照ASCII码的顺序，减号右边的字符严格大于左边的字符。
(2) 参数p1：展开方式。p1=1时，对于字母子串，填充小写字母；p1=2时，对于字母子串，填充大写字母。这两种情况下数字子串的填充方式相同。p1=3时，不论是字母子串还是数字字串，都用与要填充的字母个数相同的星号“*”来填充。
(3) 参数p2：填充字符的重复个数。p2=k表示同一个字符要连续填充k个。例如，当p2=3时，子串“d-h”应扩展为“deeefffgggh”。减号两边的字符不变。
(4) 参数p3：是否改为逆序：p3=1表示维持原来顺序，p3=2表示采用逆序输出，注意这时候仍然不包括减号两端的字符。例如当p1=1、p2=2、p3=2时，子串“d-h”应扩展为“dggffeeh”。
(5) 如果减号右边的字符恰好是左边字符的后继，只删除中间的减号，例如：“d-e”应输出为“de”，“3-4”应输出为“34”。如果减号右边的字符按照 ASCII码的顺序小于或等于左边字符，输出时，要保留中间的减号，例如：“d-d”应输出为“d-d”，“3-1”应输出为“3-1”。

数据规模和约定
100%的数据满足：1< =p1< =3，1< =p2< =8，1< =p3< =2。字符串长度不超过100



输入

输入包括两行： 
第1行为用空格隔开的3个正整数，一次表示参数p1，p2，p3。 
第2行为一行字符串，仅由数字、小写字母和减号“-”组成。行首和行末均无空格。 

输出

输出只有一行，为展开后的字符串。 

样例输入

```
1  2  1 
abcs-w1234-9s-4zz
```

样例输出

```java
abcsttuuvvw1234556677889s-4zz
```

输入

1 2 1
d-gd-e3-4d-d3-1

输出

deeffgdee344d-d3-1     

2 2 2
d-gd-e3-4d-d3-1
1-3D-D443EEDGFFEED              



1.  考虑去除字符串的空格
2. d-d=>d-d
3. d-e=>de
4. d-g=>deeffg

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-28
 * Time: 17:50
 * Description:
 */
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            int c = sc.nextInt();
            String str = sc.next();
            char arr[] = new char[101];
            for (int i = 0; i < str.length(); i++) {
                arr[i] = str.charAt(i);
            }
            StringBuffer buffer = new StringBuffer();
            for (int i = 0; i < arr.length - 2; i++) {
                if (i > 0 && arr[i] == '-' && (
                        (arr[i - 1] >= 'a' && arr[i - 1] <= 'z' && arr[i + 1] <= 'z' && arr[i + 1] >= 'a') ||
                                (arr[i - 1] >= '0' && arr[i - 1] <= '9' && arr[i + 1] <= '9' && arr[i + 1] >= '0')
                )) {
                    if (arr[i - 1] == arr[i + 1] - 1) {
                        buffer.append(arr[i + 1]);
                        continue;
                    }
                    if (arr[i - 1] >= arr[i + 1]) {
                        buffer.append(arr[i]);
                        continue;
                    }
                    for (char k = arr[i - 1]; k < arr[i + 1] - 1; k++) {
                        for (int j = 0; j < b; j++) {
                            buffer.append((char) (k + 1));
                        }
                    }
                } else {
                    buffer.append(arr[i]);
                }
            }
            String res = buffer.toString().trim();
            if (a == 1) {
                res = res.toLowerCase();
            } else {
                res = res.toUpperCase();
            }
            if (c == 2) {
                res = new StringBuffer(res).reverse().toString();
            }
            System.out.println(res);
        }
    }

}
```

| 1813139 | [小白](https://www.dotcpp.com/oj/userinfo.php?user=761472239) | [1620](https://www.dotcpp.com/oj/problem1620.html) | 答案错误73% | 16972 | 308  | [Java](https://www.dotcpp.com/oj/problem1620.html?sid=1813139&lang=3#editor) | [2169 B](https://www.dotcpp.com/oj/showsource.php?id=1813139) | 2020-01-28 20:21:38 | judger |
| ------- | ------------------------------------------------------------ | -------------------------------------------------- | ----------- | ----- | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------- | ------ |
|         |                                                              |                                                    |             |       |      |                                                              |                                                              |                     |        |

#  **问题 1621: [蓝桥杯][算法训练VIP]字符串编辑** 

题目描述

> 从键盘输入一个字符串（长度< =40个字符），并以字符’.’结束。编辑功能有：1 D：删除一个字符，命令的方式为：D a 其中a为被删除的字符，例如：D s 表示删除字符’s’，若字符串中有多个 ‘s’,则删除第一次出现的。
> 2 I：插入一个字符，命令的格式为：I a1 a2 其中a1表示插入到指定字符前面，a2表示将要插入的字符。例如：I s d 表示在指定字符 ’s’ 的前面插入字符 ‘d’ ，若原串中有多个 ‘s’，则插入在最后一个字符的前面。
> 3 R：替换一个字符，命令格式为：R a1 a2 其中a1为被替换的字符，a2为替换的字符，若在原串中有多个a1则应全部替换。
> 在编辑过程中，若出现被改的字符不存在时，则给出提示信息。

样例解释

> 命令为删去s，第一个在字符中出现的s在This中，即得到结果。

输入

> 输入共两行，第一行为原串(以’.’结束)，第二行为命令（输入方式参见“问题描述” 。

输出

> 输出共一行，为修改后的字符串或输出指定字符不存在的提示信息。 

样例输入

```
This is a book. 
D  s 
```

样例输出

```
Thi is a book.
```

方法一：

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-28
 * Time: 20:35
 * Description:
 */
public class 字符串编辑 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        int l = str.indexOf(".");
        str = str.substring(0, l + 1);

        String ml = sc.next();
        switch (ml) {
            case "D":
                String m = sc.next();
                int j = str.indexOf(m);
                if (j < 0) {
                    System.out.println("指定字符不存在");
                } else {
                    str = str.replaceFirst(m, "");
                }
                break;
            case "I":
                String a1 = sc.next();
                String a2 = sc.next();
                int i = str.lastIndexOf(a1);
                if (i < 0) {
                    System.out.println("指定字符不存在");
                } else {
                    a2 = str.substring(0, i) + a2;
                    str = a2 + str.substring(i);
                }
                break;
            case "R":
                String a3 = sc.next();
                String a4 = sc.next();
                if (str.indexOf(a3) < 0) {
                    System.out.println("指定字符不存在");
                } else {
                    str = str.replaceAll(a3, a4);
                }
                break;
            default:
                sc.close();

        }
        System.out.println(str);

    }
}
```

方法二：（StringBuffer有封装好的方法）

```java
import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-28
 * Time: 21:16
 * Description:
 */
public class 字符串编辑2 {

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        while (scan.hasNext()) {
            String s1 = scan.nextLine();
            int t1 = s1.indexOf('.');
            String s = s1.substring(0, t1 + 1);//截取以.为结束的字符串
            //System.out.println(s);
            StringBuffer ss = new StringBuffer(s);//转换成StringBuffer类型，便于后面的操作
            //输入指令
            String order = scan.next();
            switch (order) {
                case "D": {
                    String del = scan.next();
                    t1 = ss.indexOf(del);
                    if (t1 == -1)
                        System.out.println("指定字符不存在");
                    else {
                        ss.deleteCharAt(t1);//删除指定索引的字符
                    }
                    break;
                }
                case "I": {
                    String a1 = scan.next();
                    String a2 = scan.next();
                    t1 = ss.lastIndexOf(a1);
                    if (t1 == -1)
                        System.out.println("指定字符不存在");
                    else {
                        ss.insert(t1, a2);//插入
                    }
                    break;
                }
                case "R": {
                    String a3 = scan.next();
                    String a4 = scan.next();
                    t1 = ss.indexOf(a3);
                    if (t1 == -1)
                        System.out.println("指定字符不存在");
                    else {
                        while (t1 != -1)
                        {
                            char c = a4.charAt(0);
                            ss.setCharAt(t1, c);
                            t1 = ss.indexOf(a3);
                        }
                    }
                    break;
                }
            }
            System.out.println(ss);
            scan.close();
        }
    }

}
```

 以下是 StringBuffer 类支持的主要方法： 

| 序号 | 方法描述                                                     |
| :--- | :----------------------------------------------------------- |
| 1    | public StringBuffer append(String s) 将指定的字符串追加到此字符序列。 |
| 2    | public StringBuffer reverse()  将此字符序列用其反转形式取代。 |
| 3    | public delete(int start, int end) 移除此序列的子字符串中的字符。 |
| 4    | public insert(int offset, int i) 将 `int` 参数的字符串表示形式插入此序列中。 |
| 5    | replace(int start, int end, String str) 使用给定 `String` 中的字符替换此序列的子字符串中的字符。 |

下面的列表里的方法和 String 类的方法类似：

| 序号 | 方法描述                                                     |
| :--- | :----------------------------------------------------------- |
| 1    | int capacity() 返回当前容量。                                |
| 2    | char charAt(int index) 返回此序列中指定索引处的 `char` 值。  |
| 3    | void ensureCapacity(int minimumCapacity) 确保容量至少等于指定的最小值。 |
| 4    | void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin) 将字符从此序列复制到目标字符数组 `dst`。 |
| 5    | int indexOf(String str) 返回第一次出现的指定子字符串在该字符串中的索引。 |
| 6    | int indexOf(String str, int fromIndex) 从指定的索引处开始，返回第一次出现的指定子字符串在该字符串中的索引。 |
| 7    | int lastIndexOf(String str) 返回最右边出现的指定子字符串在此字符串中的索引。 |
| 8    | int lastIndexOf(String str, int fromIndex) 返回 String 对象中子字符串最后出现的位置。 |
| 9    | int length()  返回长度（字符数）。                           |
| 10   | void setCharAt(int index, char ch) 将给定索引处的字符设置为 `ch`。 |
| 11   | void setLength(int newLength) 设置字符序列的长度。           |
| 12   | CharSequence subSequence(int start, int end) 返回一个新的字符序列，该字符序列是此序列的子序列。 |
| 13   | String substring(int start) 返回一个新的 `String`，它包含此字符序列当前所包含的字符子序列。 |
| 14   | String substring(int start, int end) 返回一个新的 `String`，它包含此序列当前所包含的字符子序列。 |
| 15   | String toString() 返回此序列中数据的字符串表示形式。         |

# 问题 1646: [蓝桥杯][算法训练VIP]比较字符串

时间限制: 1Sec 内存限制: 128MB 提交: 205 解决: 106

题目描述

编程实现两个字符串s1和s2的字典序比较。（保证每一个字符串不是另一个的前缀，且长度在100以内）。若s1和s2相等，输出0；若它们不相等，则指出其第一个不同字符的ASCII码的差值：如果s1> s2，则差值为正；如果s1< s2，则差值为负。

输入

输出

无

样例输入

```
java  basic 
```

样例输出

```
8 
```

```java
package 字符串;

import java.util.Scanner;

public class 比较字符串 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String a = sc.next();
        String b = sc.next();
        System.out.println(a.compareTo(b));

    }
}
```





```java
package 字符串;

import java.util.HashSet;
import java.util.Scanner;
import java.util.Set;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-28
 * Time: 22:46
 * Description:
 */
public class 切开字符串 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            String str = sc.next();
            int max = 0;
            int index = 0;//分割的索引
            for (int i = 1; i < n - 1; i++) {
                Set<String> set1 = new HashSet<>();
                Set<String> set2 = new HashSet<>();
                String temp1 = str.substring(0, i);
                String temp2 = str.substring(i, n);
//                System.out.println("temp1__"+temp1);
//                System.out.println("temp2__"+temp2);
                for (int j = 0; j < i; j++) {
                    for (int k = j + 1; k <= i; k++) {
//                        System.out.println("1__" + temp1.substring(j, k));
                        if (even(temp1.substring(j, k)) && palindrome(temp1.substring(j, k))) {
                            set1.add(temp1.substring(j, k));
                        }
                    }
                }
                for (int j = 0; j < n - i; j++) {
                    for (int k = j + 1; k <= n - i; k++) {
//                        System.out.println("2__" + temp2.substring(j, k));
                        if (!even(temp2.substring(j, k)) || !palindrome(temp2.substring(j, k))) {
                            set2.add(temp2.substring(j, k));
                        }
                    }
                }
                if (set1.size() * set2.size() > max) {
                    max = set1.size() * set2.size();
                    index = i;
                }
            }
            System.out.println(max);
        }
    }

    private static boolean palindrome(String str) {
        if (str.length() == 1) {
            return true;
        } else {
            boolean flag = true;
            for (int i = 0; i < str.length() / 2; i++) {
                if (str.charAt(i) != str.charAt(str.length() - 1 - i)) {
                    flag = false;
                }
            }
            return flag;
        }
    }
    private static boolean even(String str) {
        return str.length() % 2 != 0;
    }

}

```

#  **问题 1828: [蓝桥杯][2015年第六届真题]密文搜索** 

#  **问题 1833: [蓝桥杯][2015年第六届真题]奇怪的数列** 

题目描述

从X星截获一份电码，是一些数字，如下：
13
1113
3113
132113
1113122113
....

YY博士经彻夜研究，发现了规律：
第一行的数字随便是什么，以后每一行都是对上一行“读出来”
比如第2行，是对第1行的描述，意思是：1个1，1个3，所以是：1113
第3行，意思是：3个1,1个3，所以是：3113

请你编写一个程序，可以从初始数字开始，连续进行这样的变换。

输入

第一行输入一个数字组成的串，不超过100位
第二行，一个数字n，表示需要你连续变换多少次，n不超过20

输出一个串，表示最后一次变换完的结果。

输出

输出一个串，表示最后一次变换完的结果。



样例输入

```
5
7
```

样例输出

```
13211321322115
```



```java
package 字符串;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-29
 * Time: 17:29
 * Description:
 */
public class 奇怪的数列 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String str = sc.next();
            int n = sc.nextInt();

            int q = 0;
            for (int i = 0; i < n; i++) {
                int num = 1;
                String temp = "";
                for (int j = 0; j < str.length(); j++) {
                    if (str.length() == 1) {
                        temp = 1 + str;
                    } else {
                        if (j + 1 < str.length() && str.charAt(j) == str.charAt(j + 1)) {
                            num++;
                            continue;
                        }
                        temp += num + "" + (str.charAt(j) - '0');
                        num = 1;
                    }
                }
                str = temp;
            }
            System.out.println(str);
        }
    }

}

```



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



# **问题 1900: [蓝桥杯][算法提高VIP]摩尔斯电码** 

题目描述

摩尔斯电码破译。类似于乔林教材第213页的例6.5，要求输入摩尔斯码，返回英文。请不要使用"zylib.h"，只能使用标准库函数。用' * '表示' . '，中间空格用' | '表示，只转化字符表。

摩尔斯码定义见：http://baike.baidu.com/view/84585.htm?fromId=253988。

![img](http://lx.lanqiao.cn/RequireFile.do?fid=dafbnrF3)

输入

无

输出

无

样例输入

```
无
```

样例输出

```
无
```



```java
package 字符串;

import java.util.Scanner;

/**
 * Created with IntelliJ IDEA.
 * User: 76147
 * Date: 2020-01-29
 * Time: 21:47
 * Description: 暴力法
 */
public class 摩尔斯电码 {
    static String[] f = {"*-", "-***", "-*-*", "-**", "*", "**-*",
            "-**", "****", "**", "*---", "-*-", "*-**", "--", "-*",
            "---", "*--*", "--*-", "*-*", "***", "-", "**-", "***-",
            "*--", "-**-", "-*--", "--**"};

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String arr[] = sc.next().split("\\|");
            String res = "";
            for (int i = 0; i < arr.length; i++) {
                for (int j = 0; j < f.length; j++) {
                    if (arr[i].equals(f[j])) {
                        res += (char)('a'+j);
                    }
                }
            }
            System.out.println(res);
        }
    }

}
```



> 注意 分隔符|，需要用 `\\|来转义`
>
> 输出要小写



# 