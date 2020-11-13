## Kruskal
【时间复杂度O(Elog|E|)】
求带权连通图的最小生成树。
### 思路
按照边权从小到大，用并查集将没连的点连上，且连的是点数减一条边
### 代码
```cpp
const int maxn = 105;
const int M = 1e4+10;
int father[maxn];//父节点
int edgenum, sum, p, r;//已经加入的边的数量, 最小生成树的边权总和, 顶点数，边数
struct Edge
{
	int u, v;
	int edge;
}e[M];
int cmp(Edge a, Edge b)
{
	return a.edge < b.edge;
}
void init()//并查集开始
{
	for (int i = 0; i <= p; i++)
		father[i] = i;
}
int find(int a)
{
	return (father[a] == a )? a : father[a]=find(father[a]);
}
bool merge(int a,int b)
{
	int aa = find(a), bb = find(b);
	if (aa == bb)//已经连在一起了，就跳过
		return false;
	else//否则连上
	{
		father[aa] = father[bb];
		return true;
	}
}
int main()
{
	while (~scanf("%d", &p))
	{
		edgenum = 0, sum=0;
		if (!p)
			break;
		init();
		scanf("%d", &r);//边数
		for (int i = 0; i < r; i++)
			scanf("%d%d%d", &e[i].u, &e[i].v, &e[i].edge);//无需去重
		sort(e, e + r, cmp);//按边权从小到大排序
		for (int i = 0; i < r; i++)
		{
			if (edgenum == p - 1)//找完了p-1条边就跳出
				break;
			if (merge(e[i].u, e[i].v))//有新的边则加入，已经连过边了就不加
			{
				edgenum++;
				sum += e[i].edge;
			}
		}
		printf("%d\n", sum);
	}
	return 0;
}