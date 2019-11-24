## 树链剖分求lca
[[102012G Rikka with Intersections of Paths](https://codeforces.com/gym/102012/problem/G)]
time limit：6 seconds
memory limit：1024 megabytes

### 题意
t组测试数据，给你一棵树，有n个点，n-1条边。
给你m条被标记的路径，和一个k，求在其中任选k条路径，使得至少有一个点是被路径共用的，这样的方案有几个？答案模1e7.

### 思路
对点进行标记，记录有多少条边是经过这个点的。
那么至少公用一个节点，相当于在这个点所连的所有边中任意选k条，即ans=（$C{_{边数}^k}$之和）。
但是这样选会有重复的，如何去重呢？
任选一个节点作为根节点，从上到下算出每个节点是几条路径所连接的两个点的lca，ans-=(C${_{边数-lca数}^k}$之和)就是答案。

### 代码
树链剖分+线段树+lca+排列组合
 
Time：5958 ms 
Memory：48200 KB 

```cpp

#include<bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a))
#define lson l, m, rt << 1
#define rson m + 1, r, rt << 1 | 1
const int N = 3e5 + 10;
const int mod = 1e9 + 7;
int sum[N << 2], lazy[N << 2]; //线段树求和
int first[N], tot;             //邻接表
//分别为:重儿子，每个节点新编号，父亲，编号，深度，子树个数，所在重链的顶部
int son[N], id[N], fa[N], cnt, dep[N], siz[N], top[N], fac[N], ifac[N], inv[N];
int w[N], wt[N]; //初始点权,新编号点权
int res = 0;     //查询答案
int t, n, m, k, x, y;
long long ans;
int du[N], lca[N];//存有几条边经过这个节点,记录这个点是几个边所连接点的lca
struct edge
{
	int v, next;
} e[N << 1];
struct P
{
	int u, v;
}p[N];
void add_edge(int u, int v)
{
	e[++tot].v = v;
	e[tot].next = first[u];
	first[u] = tot;
}

//-----------------------------------------------------线段树
void pushup(int rt)
{
	sum[rt] = (sum[rt << 1] + sum[rt << 1 | 1]) % mod;
}
void pushdown(int rt, int m) //下放lazy标记
{
	if (lazy[rt])
	{
		lazy[rt << 1] += lazy[rt];                 //给左儿子下放lazy
		lazy[rt << 1 | 1] += lazy[rt];             //给右儿子下放lazy
		sum[rt << 1] += lazy[rt] * (m - (m >> 1)); //更新sum
		sum[rt << 1] %= mod;
		sum[rt << 1 | 1] += lazy[rt] * (m >> 1);
		sum[rt << 1 | 1] %= mod;
		lazy[rt] = 0;
	}
}
void build(int l, int r, int rt)
{
	lazy[rt] = 0;
	if (l == r)
	{
		sum[rt] = wt[l]; //新的编号点权
		sum[rt] %= mod;
		return;
	}
	int m = (l + r) >> 1;
	build(lson);
	build(rson);
	pushup(rt);
}
void update(int L, int R, int c, int l, int r, int rt)
{
	if (L <= l && r <= R)
	{
		lazy[rt] += c;
		sum[rt] += c * (r - l + 1);
		sum[rt] %= mod;
		return;
	}
	pushdown(rt, r - l + 1);
	int m = (l + r) >> 1;
	if (L <= m)
		update(L, R, c, lson);
	if (R > m)
		update(L, R, c, rson);
	pushup(rt);
}
void query(int L, int R, int l, int r, int rt)
{
	if (L <= l && r <= R)
	{
		res += sum[rt];
		res %= mod;
		return;
	}
	pushdown(rt, r - l + 1);
	int m = (l + r) >> 1;
	if (L <= m)
		query(L, R, lson);
	if (R > m)
		query(L, R, rson);
}
//-----------------------------------------------------树链剖分

//处理出fa[],dep[],siz[],son[]
void dfs1(int u, int f, int deep)
{
	dep[u] = deep;   //标记深度
	fa[u] = f;       //标记节点的父亲
	siz[u] = 1;      //记录每个节点子树大小
	int maxson = -1; //记录重儿子数量
	//printf("u=%d,fa=%d,depth=%d,", u, f, deep);
	for (int i = first[u]; i; i = e[i].next)
	{
		int v = e[i].v;
		//printf("i=%d,v=%d\n", i, v);
		if (v == f)
			continue;
		dfs1(v, u, deep + 1);
		siz[u] += siz[v];
		if (siz[v] > maxson) //儿子里最多siz就是重儿子
		{
			son[u] = v; //标记u的重儿子为v
			maxson = siz[v];
		}
	}
}

//处理出top[],wt[],id[]
void dfs2(int u, int topf)
{
	id[u] = ++cnt;  //每个节点的新编号
	wt[cnt] = w[u]; //新编号的对应权值
	top[u] = topf;  //标记每个重链的顶端
	if (!son[u])    //没有儿子时返回
		return;
	dfs2(son[u], topf); //搜索下一个重儿子
	for (int i = first[u]; i; i = e[i].next)
	{
		int v = e[i].v;
		if (v == fa[u] || v == son[u]) //处理轻儿子
			continue;
		dfs2(v, v); //每一个轻儿子都有一个从自己开始的链
	}
}

void updrange(int x, int y, int k)//更新x到y最短路径上的所有值
{
	k %= mod;
	while (top[x] != top[y])
	{
		if (dep[top[x]] < dep[top[y]]) //使x深度较大
			swap(x, y);
		update(id[top[x]], id[x], k, 1, n, 1);
		x = fa[top[x]];
	}
	if (dep[x] > dep[y]) //使x深度较小
		swap(x, y);
	update(id[x], id[y], k, 1, n, 1);
}

int qrange(int x, int y)//查询x到y最短路径上的值
{
	int ans = 0;
	while (top[x] != top[y]) //当两个点不在同一条链上
	{
		if (dep[top[x]] < dep[top[y]]) //使x深度较大
			swap(x, y);
		res = 0;
		query(id[top[x]], id[x], 1, n, 1);
		//ans加上x点到x所在链顶端这一段区间的点权和
		ans += res;
		ans %= mod;
		x = fa[top[x]]; //x跳到x所在链顶端的这个点的上面一个点
	}
	//当两个点处于同一条链
	if (dep[x] > dep[y]) //使x深度较小
		swap(x, y);
	res = 0;
	query(id[x], id[y], 1, n, 1);
	ans += res;
	return ans % mod;
}

int qlca(int x, int y)//查询x和y的lca
{
	while (top[x] != top[y])
	{
		if (dep[top[x]] < dep[top[y]])//y深
		{
			y = fa[top[y]];
		}
		else//x深
		{
			x = fa[top[x]];
		}
	}
	if (dep[x] > dep[y])
		swap(x, y);
	return x;
}

//-----------------------------------------------------排列组合

void init()
{
	ans = 0, tot = 0, cnt = 0, res = 0;
	memset(sum, 0, sizeof(sum));
	memset(lazy, 0, sizeof(lazy));
	memset(first, 0, sizeof(first));
	memset(son, 0, sizeof(son));
	memset(id, 0, sizeof(id));
	memset(fa, 0, sizeof(fa));
	memset(dep, 0, sizeof(dep));
	memset(siz, 0, sizeof(siz));
	memset(top, 0, sizeof(top));
	memset(w, 0, sizeof(w));
	memset(wt, 0, sizeof(wt));
	memset(du, 0, sizeof(du));
	memset(lca, 0, sizeof(lca));
	memset(e, 0, sizeof(e));
	memset(p, 0, sizeof(p));
	fac[0] = ifac[0] = inv[0] = 1;
	fac[1] = ifac[1] = inv[1] = 1;
	for (int i = 2; i < N; i++)
	{
		fac[i] = fac[i - 1] * (long long)i % mod;
		inv[i] = (mod - mod / i) * (long long)inv[mod % i] % mod;
		ifac[i] = ifac[i - 1] * (long long)inv[i] % mod;
	}
}

inline int C(int n, int m)
{
	if (m > n) return 0;
	return fac[n] * (long long)ifac[n - m] % mod * ifac[m] % mod;
}

int main()
{
	scanf("%d", &t);
	while (t--)
	{
		init();
		scanf("%d%d%d", &n, &m, &k);
		for (int i = 1; i < n; i++)
		{
			scanf("%d%d", &x, &y);
			add_edge(x, y), add_edge(y, x);
		}
		build(1, n, 1);
		dfs1(1, 0, 1);
		dfs2(1, 1);
		for (int i = 1; i <= m; i++)
		{
			scanf("%d%d", &p[i].u, &p[i].v);
			updrange(p[i].u, p[i].v, 1);
		}
		for (int i = 1; i <= n; i++)
		{
			du[i] = qrange(i, i);
			if (du[i] < k)
				continue;
			ans += C(du[i], k);
			ans %= mod;
		}
		for (int i = 1; i <= m; i++)
		{
			lca[qlca(p[i].u, p[i].v)]++;
		}
		for (int i = 1; i <= n; i++)
		{
			if (du[i] - lca[i] < k)
				continue;
			ans -= C(du[i] - lca[i], k);
			ans = (ans % mod + mod) % mod;
		}
		printf("%lld\n", ans);
	}
	return 0;
}

```


### 题解代码1
树链剖分+树状数组+lca+排列组合
Time：2012 ms 
Memory：32800 KB 
```cpp
#include<bits/stdc++.h>
#define LL unsigned long long
using namespace std;
const int mod = 1e9 + 7;
const int N = 3e5 + 7;
int id[N], top[N], son[N], siz[N], fa[N], dep[N], c[N];
struct segment { int l, r, lca; } s[N];
int last[N], g[N << 1], nxt[N << 1];
int fac[N], ifac[N], inv[N];
int num, n, m, e, p;

inline void addedge(int x, int y)
{
	g[++e] = y; nxt[e] = last[x]; last[x] = e;
}

inline void update(int x, int y)
{
	for (int i = x; i < N; i += i & -i)
		c[i] += y;
}

inline int getsum(int x)
{
	int res = 0;
	for (int i = x; i; i -= i & -i)
		res += c[i];
	return res;
}

void dfs1(int u, int d, int f)
{
	son[u] = 0; dep[u] = d; siz[u] = 1;
	for (int i = last[u]; i; i = nxt[i])
		if (g[i] != f)
		{
			fa[g[i]] = u;
			dfs1(g[i], d + 1, u);
			siz[u] += siz[g[i]];
			if (siz[g[i]] > siz[son[u]])
				son[u] = g[i];
		}
}

void dfs2(int u, int f)
{
	top[u] = f; id[u] = ++num;
	if (son[u]) dfs2(son[u], f);
	for (int i = last[u]; i; i = nxt[i])
		if (g[i] != son[u] && g[i] != fa[u])
			dfs2(g[i], g[i]);
}



inline void change(int u, int v)
{
	int tp1 = top[u], tp2 = top[v];

	while (tp1 != tp2)
	{
		if (dep[tp1] < dep[tp2]) { swap(tp1, tp2); swap(u, v); }
		update(id[tp1], 1); update(id[u] + 1, -1);
		u = fa[tp1]; tp1 = top[u];
	}
	if (dep[u] > dep[v]) swap(u, v);
	update(id[u], 1); 
	update(id[v] + 1, -1);
}

inline int LCA(int u, int v)
{
	if (u == v) return u;
	int tp1 = top[u], tp2 = top[v];
	while (tp1 != tp2)
	{
		if (dep[tp1] < dep[tp2]) { swap(tp1, tp2); swap(u, v); }
		u = fa[tp1]; tp1 = top[u];
	}
	if (dep[u] > dep[v]) swap(u, v);
	return u;
}

inline bool cmp(segment a, segment b)
{
	return dep[a.lca] < dep[b.lca];
}

inline void init()
{
	fac[0] = ifac[0] = inv[0] = 1;
	fac[1] = ifac[1] = inv[1] = 1;
	for (int i = 2; i < N; i++)
	{
		fac[i] = fac[i - 1] * (LL)i % mod;
		inv[i] = (mod - mod / i) * (LL)inv[mod % i] % mod;
		ifac[i] = ifac[i - 1] * (LL)inv[i] % mod;
	}
}



inline int C(int n, int m)
{
	if (m > n) return 0;
	return fac[n] * (LL)ifac[n - m] % mod * ifac[m] % mod;
}

int main()
{
	int T;
	init();
	scanf("%d", &T);
	while (T--)
	{
		e = num = 0; LL ans = 0;
		memset(c, 0, sizeof(c));
		memset(last, 0, sizeof(last));
		scanf("%d%d%d", &n, &m, &p);
		for (int i = 1; i < n; i++)
		{
			int u, v;
			scanf("%d%d", &u, &v);
			addedge(u, v); addedge(v, u);
		}
		dfs1(1, 1, 1); dfs2(1, 1);
		for (int i = 1; i <= m; i++)
		{
			int u, v;
			scanf("%d%d", &u, &v);
			s[i] = segment{ u,v,LCA(u,v) };
		}
		sort(s + 1, s + 1 + m, cmp);
		for (int i = 1; i <= m; i++)
		{
			ans = (ans + C(getsum(id[s[i].lca]), p - 1)) % mod;
			change(s[i].l, s[i].r);
		}
		printf("%lld\n", ans);
	}
}

```