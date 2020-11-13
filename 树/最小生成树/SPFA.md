## SPFA
【时间复杂度O(VE)】
求含负权边的单源最短路径，以及判负权环

### 代码
```cpp
const int maxn = 1e5 + 5;//点的最大范围
const int maxm = 1e6 + 5;//边的最大范围
const int INF = 0x3f3f3f3f;
int n, m, s, t;//点数，边数，入口点，出口点
int head[maxn];//head[i]保存的是以i为起点所连接的所有边中编号最大的那个
int vis[maxn];//存点有没有访问过
int dis[maxn];//存点到s的距离
int num = 1;
struct edge
{
	int u;//前一点
	int v;//后一点
	int w;//边长
	int next;//下一条边的序号
}e[maxm];
queue<int> que;
void add(int u, int v, int w)//add为链式前向星算法
{
	e[num].u = u,	e[num].v = v;
	e[num].w = w;
	e[num].next = head[u];//no[num].next存的是上一条连接在起点u上的边的编号
	head[u] = num++;//记录u现在所连的编号
}
int main()
{
	scanf("%d%d%d%d", &n, &m, &s, &t);
	int u, v, w;
	fill(dis, dis + maxn, INF);
	memset(head, -1, sizeof(head));
	memset(vis, false, sizeof(vis));
	num = 1;
	for (int i = 1; i <= m; i++)
	{
		scanf("%d%d%d", &u, &v, &w);
		add(u, v, w);
		add(v, u, w);
	}
	que.push(s);//放入起点
	dis[s] = 0, vis[s] = true;
	while (!que.empty())
	{
		int u = que.front();
		que.pop();
		vis[u] = false;//更新队列外的点
		for (int i = head[u]; i != -1; i = e[i].next)//从tmp连的最后一条边开始找上条边
		{
			if (dis[e[i].v] > dis[u] + e[i].w)
			{
				dis[e[i].v] = dis[u] + e[i].w;//更新最短路
				if (!vis[e[i].v])
				{
					vis[e[i].v] = true;
					que.push(e[i].v);
				}
			}
		}
	}
	printf("%d\n", dis[t]);
	return 0;
}
```