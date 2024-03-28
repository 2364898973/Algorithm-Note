#### SPFA算法（队列优化的Bellman-Ford算法）
在bellman-ford算法中的dist[b]=min(dist[b],dist[a]+w)进行优化。**只有dist[a]减小，dist[b]才减小**  
##### 算法步骤：
1. queue<-1
2. while queue不为空
>1. t<-q.front()  
>   q.pop()
>2. 更新t的所有出边&emsp;t->b  
>   queue<-b  
##### 时间复杂度:平均情况下 O(m),最坏情况下 O(nm),n表示点数，m表示边数。
```
int n;      // 总点数
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储每个点到1号点的最短距离
bool st[N];     // 存储每个点是否在队列中

// 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
int spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    queue<int> q;
    q.push(1);
    st[1] = true;

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if (!st[j])     // 如果队列中已存在j，则不需要将j重复插入
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;//有些情况应返回0x3f3f3f3f,因为dist[n]可能等于-1
    return dist[n];
}
```
#### SPFA判断图中是否存在负环
**原理(抽屉原理)：**  
```
dist[x]:从第一个点到第x个点的最短距离
cnt[x]:从第一个点到第x个点的边数
dist[x]=dst[t]+w[i],cnt[x]=cnt[t]+1
若cnt[x]>=n,则至少经过n+1个点，由抽屉原理，一定存在环，而这条路是最短路，一定是负环
```
##### 时间复杂度是 O(nm),n表示点数，m表示边数。
```
int n;      // 总点数
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N], cnt[N];        // dist[x]存储1号点到x的最短距离，cnt[x]存储1到x的最短路中经过的点数
bool st[N];     // 存储每个点是否在队列中

// 如果存在负环，则返回true，否则返回false。
bool spfa()
{
    // 不需要初始化dist数组
    // 原理：如果某条最短路径上有n个点（除了自己），那么加上自己之后一共有n+1个点，由抽屉原理一定有两个点相同，所以存在环。

    queue<int> q;
    for (int i = 1; i <= n; i ++ )
    {
        q.push(i);
        st[i] = true;
    }

    while (q.size())
    {
        auto t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;       // 如果从1号点到x的最短路中包含至少n个点（不包括自己），则说明存在环
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    return false;
}
```
