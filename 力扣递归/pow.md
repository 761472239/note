# 方法一：暴力法

**超时**

```java
class Solution {
    public double myPow(double x, int n) {
        if(n<0){
            n = -n;
            x = 1/x;
        }
        double res = 1;
        for(int i=0;i<n;i++){
            res*=x;
        }
        return res;
        
    }
}
```

# 方法二：递归快速幂算法

**思路**

假设我们已经得到了 $x ^ n$的结果，那么如何才能算出 $x ^ {2 * n }$？显然，我们不需要再乘上 n 个 x。 使用公式 $(x ^ n) ^ 2 = x ^ {2 * n }$，只需要一次计算,我们就可以得到 $x ^ {2 * n }$。这种优化可以降低算法的时间复杂度。

```java
class Solution {
    private double fastPow(double x, long n) {
        if (n == 0) {
            return 1.0;
        }
        double half = fastPow(x, n / 2);
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }

        return fastPow(x, N);
    }
};
```



# 方法三：迭代快速幂算法

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double ans = 1;
        double current_product = x;
        for (long i = N; i > 0; i /= 2) {
            if ((i % 2) == 1) {
                ans = ans * current_product;
            }
            current_product = current_product * current_product;
        }
        return ans;
    }
};
```

