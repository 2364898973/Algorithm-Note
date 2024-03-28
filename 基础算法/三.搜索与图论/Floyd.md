#### Floyd算法

##### 算法步骤：
>for(k=1;k<=n;k++)  
>&emsp;for(int i=1;i<=n;i++)  
>&emsp;&emsp;for(int j=1;j<=n;j++)  
>&emsp;&emsp;&emsp;d[i,j]=min(d[i,j],d[i,k]+d[k,j])
##### 时间复杂度是 O(n<sup>3</sup>),n表示点数。   
```
初始化：
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            if (i == j) d[i][j] = 0;
            else d[i][j] = INF;

// 算法结束后，d[a][b]表示a到b的最短距离
void floyd()
{
    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```
