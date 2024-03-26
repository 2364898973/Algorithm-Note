#### 朴素dijkstra算法
##### 算法步骤：
s:当前已确定最短距离的点集合。  
1. dist[1]=0,dist[i]=0x3f3f3f3f
2. for i:1~n  
  2.1 t<-不在s中的，距离最近的点。 O(n^2)   
  2.2 s<-t。  O(n)  
  2.3 用t更新其他点的距离。  O(m)**相当于遍历所有边**  

**用邻接矩阵存储图**  
**时间复杂是O(n^2+m),n表示点数,m表示边数。**  
```
int g[N][N];  // 存储每条边
int dist[N];  // 存储1号点到每个点的最短距离
bool st[N];   // 存储每个点的最短路是否已经确定

// 求1号点到n号点的最短路，如果不存在则返回-1
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    for (int i = 0; i < n - 1; i ++ )
    {
        int t = -1;     // 在还未确定最短路的点中，寻找距离最小的点
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        // 用t更新其他点的距离
        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);

        st[t] = true;
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```
#### 堆优化版dijkstra
##### 算法步骤：
s:当前已确定最短距离的点集合。  
1. dist[1]=0,dist[i]=0x3f3f3f3f
2. for i:1~n  
  2.1 t<-不在s中的，距离最近的点。 O(n^2)->O(n)**利用堆进行优化，找最近点O(1)**   
  2.2 s<-t。  O(n)  
  2.3 用t更新其他点的距离。  O(m)**相当于遍历所有边**->O(mlogn)**堆修改O(logn)**  

**用邻接表存储图**  
**时间复杂度O(mlogn)，n表示点数，m表示边数。**  
##### 堆
1. 手写堆**支持修改元素，堆里一直有n个数**O(mlogn)  
2. 优先队列(priority_queue)**不支持修改元素，堆里可能有m个数(m>n):实现方式为冗余，每次修改向堆插入一个元素**O(mlogm)->由于m<=n^2,O(mlogm)=O(mlogn)
```
typedef pair<int, int> PII;

int n;      // 点的数量
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储所有点到1号点的距离
bool st[N];     // 存储每个点的最短距离是否已确定

// 求1号点到n号点的最短距离，如果不存在，则返回-1
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});      // first存储距离，second存储节点编号

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.second, distance = t.first;

        if (st[ver]) continue;   //如果是冗余点，则不处理
        st[ver] = true;

        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > distance + w[i])
            {
                dist[j] = distance + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```
