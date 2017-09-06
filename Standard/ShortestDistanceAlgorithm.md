## 问题描述
最短路径算法：在一个带权有向网络G=(V,E,W)中，边权非负，求源点S出发到达每个其他节点的最短路径；

## 解法
**解法1**：Dijsktra算法，贪心法，时间复杂度为O(n^2);
算法描述：设G=(V,E,W)是一个带非负权有向图，把图中顶点集合V分成两组，第一组为已求出最短路径的顶点集合（用S表示，初始时S中只有一个源点，以后每求得一条最短路径 , 就将加入到集合S中，直到全部顶点都加入到S中，算法就结束了），第二组为其余未确定最短路径的顶点集合（用U表示,即不在S中的点集合），按最短路径长度的递增次序依次把第二组的顶点加入S中。在加入的过程中，总保持从源点v到S中各顶点的最短路径长度不大于从源点v到U中任何顶点的最短路径长度。此外，每个顶点v'对应一个距离P[v']，S中的顶点的距离就是从v到此顶点的最短路径长度，U中的顶点的距离，是从v到此顶点只包括S中的顶点为中间顶点的当前最短路径长度。

**解法2**：Floyd算法，动态规划，时间复杂度O(n^3)，空间负责度O(n^2)；它可解决任意两点间的最短路径的一种算法，可以正确处理有向图或负权的最短路径问题，同时也被用于计算有向图的传递闭包。
动态规划分析：我们的目标是寻找从点i到点j的最短路径，从任意节点i到任意节点j的最短路径不外乎2种可能，1是直接从i到j，2是从i经过若干个节点k到j。所以，我们假设Dis(i,j)为节点u到节点v的最短路径的距离，对于每一个节点k，我们检查Dis(i,k) + Dis(k,j) < Dis(i,j)是否成立，如果成立，证明从i到k再到j的路径比i直接到j的路径短，我们便设置Dis(i,j) = Dis(i,k) + Dis(k,j)，这样一来，当我们遍历完所有节点k，Dis(i,j)中记录的便是i到j的最短路径的距离。
算法描述：1.从任意一条单边路径开始。所有两点之间的距离是边的权，如果两点之间没有边相连，则权为无穷大。对于每一对顶点 u 和 v，看看是否存在一个顶点 w 使得从 u 到 w 再到 v 比己知的路径更短。如果是更新它。

## C++代码
**Dijsktra算法**
```
const int  MAXINT = 32767; //代替无穷大
//const int MAXNUM = 10; //顶点数
//int A[MAXUNM][MAXNUM]; //边的权重
int dist[MAXNUM];
int prev[MAXNUM]; //prev[v]记录v0顶点到其余顶点v的最短路径中v的前一个顶点

void Dijkstra(int ** A,const int MAXNUM,int v0)  //为简单起见可先调整v0到索引为0的位置，即v0=0
{
  　　bool S[MAXNUM];     // 判断是否已存入该点到S集合中,值为TRUE当且仅当v∈S,即已经求得从v0到v的最短路径
      int n=MAXNUM;
      //初始化标记S[vi],prev[vi]，以及初始时v0-i的距离dist[vi] 
  　　for(int vi=0; vi<=n; ++vi)  
 　　 {
      　　dist[vi] = A[v0][vi];
      　　S[vi] = false;                              // 初始时都未用过该点
      　　if(dist[vi] == MAXINT)    
            　　prev[vi] = -1;
 　　     else 
            　　prev[vi] = v0;
   　　}
   　 dist[v0] = 0; 
   　 S[v0] = true; //初始时，v0顶点属于S集合　　
      //开始主循环，依次求得v0到某个顶点v的最短路径，并将v加到S集合中
 　　 for(int i=1; i<n; i++)
 　　 {
       　　int mindist = MAXINT;
       　　int u = v0; 　　                            // u保存当前邻接点中距离最小的点的号码 
      　　 for(int j=0; j<n; ++j)
      　　    if( !S[j] && dist[j]<mindist)
      　　    {
         　　       u = j;                             
         　 　      mindist = dist[j];
       　　   }
       　　S[u] = true;                                //离顶点v0最近的u加入到S中，且其距离为min
       　　for(int j=0; j<n; ++j)
       		 {
       　　    if( !S[j] && dist[u] + A[u][j] < dist[j])  //更新在通过新加入的u点后离v0点更短的路径 
       　　    {
                   dist[j] = dist[u] + A[u][j];           //更新dist 
                   prev[j] = u;                           //记录前驱顶点 
        　     }
        	 }
   　　}
}    

```
**Floyd算法**
```
typedef struct          
{        
    char vertex[VertexNum];                                //顶点表，n=VertexNum         
    int  edges[VertexNum][VertexNum];                      //邻接矩阵,可看做边表         
    int  n,e;                                              //图中当前的顶点数和边数         
}MGraph; 

void Floyd(MGraph g)
{
    //矩阵A是最短路径矩阵，而矩阵Path记录u,v两点之间最短路径所必须经过的点
     int n=g.n;
 　　int *A[n] = new int*[n]; 
     for(int i=0;i<n;++i)
        A[i]=new int[n];   
 　　int *path[n] = new int*[n]; 
     for(int i=0;i<n;++i)
        path[i]=new int[n];  

 　　int i,j,k;
    //初始化邻接矩阵和路径记录矩阵
 　　for(i=0;i<n;i++)
     {
    　　 for(j=0;j<n;j++)
    　　 { 　　
            A[i][j]=g.edges[i][j];
         　 path[i][j]=-1;
     　  }
     } 
     //从底向上依次计算最短路径A[i,j]及其最优时所选取的k
 　　for(i=0;i<n;i++)
 　　{ 
      　for(j=0;j<n;j++)　　
          for(k=0;k<n;k++)
            if(A[i][j]>(A[i][k]+A[k][j]))
            {
              A[i][j]=A[i][k]+A[k][j];
              path[i][j]=k;
            }
     } 
} 

```

其他参考资料：
1.[http://blog.csdn.net/u012856866/article/details/38724617]:http://blog.csdn.net/u012856866/article/details/38724617
2.[http://www.cnblogs.com/biyeymyhjob/archive/2012/07/31/2615833.html]:http://www.cnblogs.com/biyeymyhjob/archive/2012/07/31/2615833.html