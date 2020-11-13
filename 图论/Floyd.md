## Floyd
【时间复杂度O(V3)，空间复杂度O(N2)】

求任意两点间的最短路径算法
### 思路
两点之间的距离dis[i][j]，由点k更新:dis[i][j] = dis[i][k] + dis[k][j]
```cpp
int d[105][105];
const int INF = 0x3f3f3f3f;
int main() {
	int n, m;
	while (cin >> n >> m) {
		int u, v, w;
		memset(d, 0, sizeof(d));
		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= n; j++) {
				if (i == j)
					d[i][j] = 0;
				else
					d[i][j] = INF;
			}
		for (int i = 0; i < m; i++)//输入
		{
			cin >> u >> v >> w;
			d[u][v] = d[v][u] = min(w, d[u][v]);//注意去重
		}
		for (int k = 1; k <= n; k++) {//k在最外层
			for (int i = 1; i <= n; i++) {
				for (int j = 1; j <= n; j++) {
					if (d[i][k] + d[k][j] < d[i][j]) 
						d[i][j] = d[i][k] + d[k][j];
				}
			}
		}
		...//输出
	return 0;
}
```