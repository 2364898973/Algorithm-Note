#### Bellman-Ford算法
##### 算法步骤：
for 1~n  
&emsp;for 所有边(a,b,w)    
&emsp;&emsp;dist[b]=min(dist[a]+w,dist[b])&emsp;O(nm)  
**更新过程叫松弛操作，经过遍历后可以证明对于所有边：dist[b]<=dist[a]+w(三角不等式)。**   
**注意：如果有负权回路，不一定存在最短路。**  
**可以求负环，但时间复杂度高。适用于有边数限制的。其他用SPFA。**  
**可以不用用邻接表存储，直接用结构体。**  
**时间复杂度：O(nm)** 
```
int n, m;       // n表示点数，m表示边数
int dist[N];        // dist[x]存储1到x的最短路距离

struct Edge     // 边，a表示出点，b表示入点，w表示边的权重
{
    int a, b, w;
}edges[M];

// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    // 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，路径中至少存在两个相同的点，说明图中存在负权回路。
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < m; j ++ )
        {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            if (dist[b] > dist[a] + w)
                dist[b] = dist[a] + w;
        }
    }

    if (dist[n] > 0x3f3f3f3f / 2) return -1;//不是dist[n]==0x3f3f3f3f,由于存在负权边。
    return dist[n];
}
```

