##求区间第k大的数

输入n,m代表n个数，m次询问
接下来输入n个数，表示需要查询的序列
下面m行输入三个数l,r,k,表示在l到r之间查询第k大的数

```cpp
#include<cstdio>
#include<iostream>
#include<sstream>
#include<cstdlib>
#include<cstring>
#include<string>
#include<climits>
#include<cmath>
#include<algorithm>
#include<queue>
#include<vector>
#include<stack>
#include<set>
#include<map>
#define INF 0x3f3f3f3f
#define eps 1e-8
using namespace std;
const int MAXN = 100005;
const int MAXM = 2000005;
vector<int> hash;
int a[MAXN];
int tot;
struct NODE
{
	int count;
	int left, right;
};
int root[MAXN];
NODE node[MAXM];
int null;
int newnode(int count, int left, int right)
{
	node[tot].count = count;
	node[tot].left = left;
	node[tot].right = right;
	return tot++;
}
int insert(int rt, int l, int r, int k)
{
	if (l <= k && k <= r)
	{
		if (l == r)
		{
			return newnode(node[rt].count + 1, 0, 0);
		}
		int m = (l + r) >> 1;
		return newnode(node[rt].count + 1,
			insert(node[rt].left, l, m, k),
			insert(node[rt].right, m + 1, r, k));
	}
	return rt;
}
int query(int p, int q, int l, int r, int k)
{
	if (l == r)
	{
		return hash[l];
	}
	int m = (l + r) >> 1;
	int cot = node[node[q].left].count - node[node[p].left].count;
	if (cot >= k)
	{
		return query(node[p].left, node[q].left, l, m, k);
	}
	return query(node[p].right, node[q].right, m + 1, r, k - cot);
}
int main()
{
	int cas;
	scanf("%d", &cas);
	while (cas--)
	{
		int n, q;
		scanf("%d%d", &n, &q);
		hash.clear();
		tot = 0;
		for (int i = 1; i <= n; i++)
		{
			scanf("%d", &a[i]);
			hash.push_back(a[i]);
		}
		sort(hash.begin(), hash.end());
		hash.erase(unique(hash.begin(), hash.end()), hash.end());
		int m = hash.size();
		null = newnode(0, 0, 0);
		root[0] = newnode(0, 0, 0);
		for (int i = 1; i <= n; i++)
		{
			a[i] = lower_bound(hash.begin(), hash.end(), a[i]) - hash.begin();
			root[i] = insert(root[i - 1], 0, m - 1, a[i]);
		}
		while (q--)
		{
			int l, r, k;
			scanf("%d%d%d", &l, &r, &k);
			printf("%d\n", query(root[l - 1], root[r], 0, m - 1, k));
		}
	}
	return 0;
}

```