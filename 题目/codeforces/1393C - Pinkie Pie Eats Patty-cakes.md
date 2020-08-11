# [1393C - Pinkie Pie Eats Patty-cakes](https://codeforces.com/contest/1393/problem/C)
## 题解
题目要求的是：让数组a中大小相同的数相隔的尽可能远，求满足该条件的相同数之间的最小距离。
* 对于 ``2 2 2 2 1 1 3`` 这类序列，起作用的显然是`2`，只要固定好`2`的位置，将`2`看做隔板，隔开其他数即可。不用考虑里面的数具体怎么填，因为其他重复数的个数<`2`，每个空格填一个就一定能实现其最小间距 ≥ `2`的最小间距。
* 而对于``2 2 2 2 3 3 3 3 4 5 6 7 ``这类序列，起作用的是`2`和`3`两个，可以将`23`看成是一个整体作为隔板，将其他数隔开。

因此，设`n`为数组`a`中数的个数，用数组`b`来记录`a`中每个数出现过几次，并将其从大到小排序，则`b[0]`就是重复最多的数出现的次数。让`cnt`记录有几个数出现了`b[0]`次。
(如：``2 2 2 2 3 3 3 3 4 5 6 7 ``中，`2`和`3`都出现了`4`次，因此`b[0]=4`,`cnt=2`)
则`ans=(n-b[0]*cnt)/(b[0]-1)+(cnt-1)`
* `n-b[0]*cnt`表示剩余要被隔开的数的个数，即`4 5 6 7`
* `b[0]-1`表示除了序列最左边需要一个隔板，其余隔板都用来隔开其他数，这样才能保证隔板间距最大
* 上两个相除就是每个隔间有多少个数了
* `+(cnt-1)`表示加上隔板的厚度-1，即`2`和`2`之间的距离是：隔板间的个数+一个`3`
## 代码
```cpp
#include<bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;
int t, n, a[maxn], b[maxn];
int cmp(int x,int y)
{
    return x > y;
}
int main()
{
    scanf("%d", &t);
    while(t--)
    {
        memset(a, 0, sizeof(a));
        memset(b, 0, sizeof(b));
        scanf("%d", &n);
        for (int i = 0; i < n;i++)
        {
            scanf("%d", &a[i]);     //a[i]的值最大到n
            b[a[i]]++;              //即b的序号最大到n
        }
        sort(b, b + n + 1, cmp);
        int ans = 0, cnt = 1;
        for (int i = 1; i <= n;i++)
        {
            if(b[i]==b[i-1])
                cnt++;
            else
                break;
        }
        ans = (n - b[0] * cnt) / (b[0] - 1) + (cnt - 1);
        printf("%d\n", ans);
    }
    system("pause");
    return 0;
}
```