# [1407C - Chocolate Bunny](https://codeforces.com/contest/1407/problem/C)

## 题意

给一个``n``，代表序列中有``n``个数，这些数范围在1到``n``且不重复

你可以询问任意两个下标不超过``2n``次，格式为：``? i j``，系统会给出第i个数模第j个数的结果

当你知道这个序列的结果时，输出``! ``后输出该序列，中间用空格隔开

## 题解

因为``a>b​``时，``a%b<b``，且``b%a=b``

可以得到``a>b``等价于``a%b < b%a``

因此每次选择两个数互模进行比较即可

## 代码

这道题很奇怪的一点是，使用``scanf``和``printf``会报错``Idleness limit exceeded on test 1``

```cpp
#include<bits/stdc++.h>
using namespace std;
int n, m;
int ans[10005];
int ask(int x,int y)
{
    cout << "? " << x << " " << y << endl;
    cin >> m;
    return m;
}
int main()
{
    cin >> n;
    int tmp = 1;
    for (int i = 2; i <= n;i++)
    {
        int a = ask(tmp, i);
        int b = ask(i, tmp);
        if(a>b) //若(tmp%i) > (i%tmp) 则 (tmp<i)
        {
            ans[tmp] = a;
            tmp = i;
        }
        else    //若(tmp%i) < (i%tmp) 则 (i<tmp)
        {
            ans[i] = b;
        }
    }
    ans[tmp] = n;
    cout << "!";
    for (int i = 1; i <= n;i++)
    {
        cout << " "<< ans[i] ;
    }
    cout << endl;
    //system("pause");
    return 0;
}
```

