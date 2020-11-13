## Prim
【时间复杂度O(N2)】
在带权连通图里搜索最小生成树的算法。
### 思路
邻接矩阵存图，随便定一个起点，每次遍历所有点，找没被访问过且dis最小的点（dis[i]存的是被上个点更新后的，且与i最近的点的距离，即已访问集合到未访问集合中各点的距离），用这个点去更新其它点到这个点的最短距离。
```cpp
void Prim() 
{
    int i, j, k, tmp, ans;
    for (i = 1; i <= n; i++)
        dis[i] = INF;//初始化 
    dis[1] = 0;
    for (i = 1; i <= n; i++)
    {
        tmp = INF;
        for (j = 1; j <= n; j++) 
        {
            if (!vis[j] && tmp > dis[j]) //找出最小距离的节点
            {
                tmp = dis[j];
                k = j;
            }
        }
        vis[k] = 1;//把访问的节点做标记
        for (j = 1; j <= n; j++) 
        {
            if (!vis[j] && dis[j] > map[k][j])
                dis[j] = map[k][j];//更新最短距离 
        }
    }
}
```