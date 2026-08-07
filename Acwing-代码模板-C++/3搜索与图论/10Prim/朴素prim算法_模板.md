#### 时间复杂度是`O(n2+m)`, `n`表示点数，`m`表示边数
```
int n; // n点表示点数
int g[N][N]; // 邻接矩阵，存储所有边
int dist[N]; // 存储其他点到当前最小生成数的距离
bool st[N]; // 存储每个点是否已经在生成树中

// 如果图不连通，则返回INF（值是ox3f3f3f3），否则返回最小生成数的数边权重之和
int prim(){
    memset(dist, 0x3f, sizeof dist);

    int res = 0;
    for (int i = 0; i < n; i ++){
        int t = -1;
        for (int j = 1; j < n ; j ++){
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        }

        if (i && dist[t] == INF) return INF;

        if (i) res += dist[t];
        st[t] = true;

        for (int j = 1; j <= n; j ++) dist[j] = min(dist[j], t[t][j]);    
    }

    return res;
}
```