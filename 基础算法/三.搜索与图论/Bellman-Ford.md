#### Bellman-Ford算法
##### 算法步骤：
for 1~n  
&emsp;for 所有边(a,b,w)    
&emsp;&emsp;dist[b]=min(dist[a]+w,dist[b])&emsp;O(nm)  
**更新过程叫松弛操作，经过遍历后可以证明对于所有边：dist[b]<=dist[a]+w(三角不等式)**   
**注意：如果有负权回路，不一定存在最短路**  
**可以求负环，但时间复杂度高。适用于有边数限制的。其他用SPFA。**  
**可以不用用邻接表存储，直接用结构体**  
**时间复杂度：O(nm)** 

