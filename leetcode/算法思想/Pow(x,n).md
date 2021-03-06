# Pow(x,n)
## leetcode 50， medium
### 实现 pow(x, n) ，即计算 x 的 n 次幂函数（即，xn）。
<br>

### 示例 1：
```
输入：x = 2.00000, n = 10
输出：1024.00000
```
### 示例 2：
```
输入：x = 2.10000, n = 3
输出：9.26100
```
### 示例 3：
```
输入：x = 2.00000, n = -2
输出：0.25000
解释：2^-2 = 1/2^2 = 1/4 = 0.25
```

提示：

- `-100.0 < x < 100.0`
- `-2^31 <= n <= 2^31 - 1`
- `-104 <= x^n <= 104`

<br>
<br>
<br>

首先`n`的取值范围是`[-2^31,2^31-1]`,当`n`为负数时,`n`的幂即为`n`的倒数的幂.所以取:
```
x = 1/x
```
<br>

其次对负数`n`，需要求绝对值，因为`INT_MIN`取绝对值后会溢出，所以选择`longlong`结构存储.
```
long long m = abs(n)
```
<br>

当`m%2`为奇数时，`m/2`丢掉了一个`1`，这里的意思是：
```
m^9 = m * m^4 * m^4
```
但实际循环的时候：
```
m^(9/2) = m^4 * m^4
```
所以这里有：
```
if(m%2==1){
    res = res * x;
}
```
诸如`8`这一类可以直接二分到底的数,只有最后才会出现`m % 2 == 1(m == 1时)`.

所以初始化`res = 1`,最后返回`res * x`的值
```
x^8 = x^4 * x^4 
x^4 = x^2 * x^2
x^2 = x * x 
```
但是如`10`这一类的数,当`m`二分到`5`时，`5 % 2 = 2`.丢掉了一个`2`.
```
x^10 = x^5 * x^5 
x^5 = x * x^2 * x^2
x^2 = x * x 
```
所以`res`在返回前的含义其实是记录下丢掉的`x`.

所以诸如`8`这类的数，最后只需乘上`res=1`,而其他的数则需要通过乘上`res`补上丢掉的`x`

<br>

```
class Solution {
public:
    double myPow(double x, int n) {
        long long m =abs(n);
        double res =1;
        if(n<0){
            x = 1/x;
        }
        while(m){
            if(m%2==1){
                res = res*x;
            }
            x *=x;
            m/=2;
        }
        return res;
    }
};
```
