## Dijkstra
【时间复杂度O(V2)】
求从一个顶点到其余各顶点的最短路径算法
### 思路
用邻接矩阵存图，每次找到一个没有访问过的、距离原点最近的点（dis[i]存的是源点到未访问集合中各点的距离），用这个点去更新所有和它相连的点到原点的距离。

### 代码
```cpp
const int INF = 0x3f3f3f3f;
int n, m, s, t;//点数、边数、起点、终点
int mp[1005][1005], dis[10005], vis[1005];//注意范围，定义10的4次方会爆栈
int u, v, len;//前一点，后一点，边长
int main()
{
	fill(dis, dis + 10005, INF);
	fill(mp[0], mp[0] + 1005 * 1005, INF);
	memset(vis, 0, sizeof(vis));
	scanf("%d%d%d%d", &n, &m, &s, &t);
	while (m--)
	{
		scanf("%d%d%d", &u, &v, &len);
		if (mp[u][v] > len)//注意去重
		{
			mp[u][v] = len;//无向图（一定要看清题意是无向图还是有向图）
			mp[v][u] = len;
		}
	}
	for (int i = 1; i <= n; i++)
		dis[i] = mp[i][s];
	int tmp;
	dis[s] = 0;
	while (1)
	{
		tmp = 0;
		for (int i = 1; i <= n; i++)//下个要找的点一定是距离原点最近的点，否则会出错
		{
			if (vis[i] == 0 && dis[tmp] > dis[i])
				tmp = i;
		}
		if (tmp == 0)//没有点可以找了
			break;
		vis[tmp] = 1;//标记访问过了
		for (int i = 1; i <= n; i++)
			dis[i] = min(dis[i], dis[tmp] + mp[tmp][i]);
	}
	printf("%d\n", dis[t]);
	return 0;
}
```