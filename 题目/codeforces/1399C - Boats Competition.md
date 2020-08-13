# [1399C - Boats Competition](https://codeforces.com/contest/1399/problem/C)

## 题解

这道题根本不用考虑该怎么配对，才能使体重相同的组合最多。

w的范围是[1,50]，两两相加最小2、最大不超过100。

所以直接排序，再枚举2-100的体重和，计算体重相同的有几对，数量最多的就是答案。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
int t, n, a[55],sum[105];
int main()
{
    scanf("%d",&t);
    while(t--)
    {
        int ans = 0;
        memset(sum, 0, sizeof(sum));
        scanf("%d", &n);
        for(int i=1;i<=n;i++)
        {
            scanf("%d", &a[i]);
        }
        sort(a + 1, a + n + 1);
        for (int i = 2; i <= 100;i++)   //所有可能的和
        {
            int l = 1, r = n;
            while(l<r)
            {
                if(a[l]+a[r]==i)		//刚好相等，数量增加
                {
                    sum[i]++;
                    l++, r--;
                }
                else if(a[l]+a[r]>i)	//体重高了，r往左一步
                    r--;
                else				   //体重低了，l往右一步
                    l++;
            }
            ans = max(ans, sum[i]);
        }
        printf("%d\n", ans);
    }
    //system("pause");
    return 0;
}

```

