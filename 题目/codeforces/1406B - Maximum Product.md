# [1406B - Maximum Product](https://codeforces.com/contest/1406/problem/B)

## 题意

给一串序列，元素范围在[-3000,3000]，找出任意五个数乘积最大的值。

## 题解

### 错误思路1

先排序，第一个数取数组最大的一个数，其他四个数在数组左右两边，两对两对乘积比较，取乘积大的。

* 当数组全为负数时，肯定尽可能靠右取最大的五个数。而不是两两相乘取绝对值最大，因为结果肯定是负的。

### 错误思路2

先排序，从左到右取偶数个负数，将他们变成正数，再排序。最后取最大的5个数。

* 这样最后取的五个数中，不一定能取到偶数个原来的负数，这样就错了。

### 正确思路

一共三种情况

* 取最大五个数
* 取两个负数，三个非负数
* 取四个负数，一个非负数



## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;
int t, n;
long long a[maxn];
int main()
{
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d",&n);
        memset(a, 0, sizeof(a));
        long long ans = 1;
        for(int i=0;i<n;i++)
        {
            scanf("%lld",&a[i]);
        }
        sort(a, a + n);
        long long x1 = a[n - 1] * a[n - 2] * a[n - 3] * a[n - 4] * a[n - 5];
        long long x2 = a[0] * a[1] * a[n - 1] * a[n - 2] * a[n - 3];
        long long x3 = a[0] * a[1] * a[2] * a[3] * a[n - 1];
        printf("%lld\n", max(x1, max(x2, x3)));
    }
    system("pause");
    return 0;
}
```

