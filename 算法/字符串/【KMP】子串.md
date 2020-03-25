[子串](https://ac.nowcoder.com/acm/problem/13253)

给出一个正整数n，我们把1..n在k进制下的表示连起来记为s(n,k)，例如s(16,16)=123456789ABCDEF10, s(5,2)=11011100101。现在对于给定的n和字符串t，我们想知道是否存在一个k(2 ≤ k ≤ 16)，使得t是s(n,k)的子串。

## 输入描述:

第一行一个整数n(1 ≤ n ≤ 50,000)。
第二行一个字符串t(长度 ≤ 1,000,000)

## 输出描述:

"yes"表示存在满足条件的k，否则输出"no"

示例1

## 输入

8
01112

输出

yes





# KMP 超时

```java
import java.util.Scanner;

public class Main {
    public static int[] getNext(char[] p) {
        int pLen = p.length;
        int[] next = new int[pLen];
        int k = -1;
        int j = 0;
        next[0] = -1; // next数组中next[0]为-1
        while (j < pLen - 1) {
            if (k == -1 || p[j] == p[k]) {
                k++;
                j++;
                // 修改next数组求法
                if (p[j] != p[k]) {
                    next[j] = k;// KMPStringMatcher中只有这一行
                } else {
                    // 不能出现p[j] = p[next[j]],所以如果出现这种情况则继续递归,如 k = next[k],
                    // k = next[[next[k]]
                    next[j] = next[k];
                }
            } else {
                k = next[k];
            }
        }
        return next;
    }

    public static int indexOf(String source, String pattern) {
        int i = 0, j = 0;
        char[] src = source.toCharArray();
        char[] ptn = pattern.toCharArray();
        int sLen = src.length;
        int pLen = ptn.length;
        int[] next = getNext(ptn);
        while (i < sLen && j < pLen) {
            // 如果j = -1,或者当前字符匹配成功(src[i] = ptn[j]),都让i++,j++
            if (j == -1 || src[i] == ptn[j]) {
                i++;
                j++;
            } else {
                // 如果j!=-1且当前字符匹配失败,则令i不变,j=next[j],即让pattern模式串右移j-next[j]个单位
                j = next[j];
            }
        }
        if (j == pLen)
            return i - j;
        return -1;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String pattern = sc.next();
        StringBuffer source;
        for (int i = 2; i <= 16; i++) {
            source = new StringBuffer();
            for (int j = 1; j <= n; j++) {
                source.append(Integer.toString(j, i));
                if (indexOf(source.toString(), pattern) > 0) {
                    System.out.println("yes");
                    return;
                }
            }
        }
        System.out.println("no");
    }

}


```

# 使用 contains

```java
import java.util.Scanner;
 
public class Main {
 
    public static void main(String[] args) {
        Scanner Cin=new Scanner(System.in);
        int n=Cin.nextInt();
        String t=Cin.next();
        StringBuffer s=new StringBuffer();
        for(int k=2;k<=16;k++)
        {
            s=new StringBuffer();
            for(int i=1;i<=n;i++)
            {
                s.append(Integer.toString(i,k));
            }
            String ss=s.toString().toUpperCase();
            if(ss.contains(t))
            {
                System.out.println("yes");
                return ;
            }
 
        }
        System.out.println("no");
 
    }
}
```

