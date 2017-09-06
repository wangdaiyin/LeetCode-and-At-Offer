## 问题描述
2017年阿里巴巴校招算法工程师编程题(一)：
现在城市有N个路口，每个路口有自己的编号，从0到N-1，每个路口还有自己的交通控制信号，例如0,3表示0号路口的交通信号每3个时刻变化一次，即0到3时刻0号路口允许通过，3到6时刻不允许通过，而6到9时刻又允许通过；以此类推，所有路口的允许通行都从时刻0开始。同时城市中存在M条道路将这N个路口相连接起来，确保从一个路口到另一个路口都可达，每条路由两个端点加上通行所需要的时间表示。现在给定起始路口和目的路口，从0时刻出发,请问最快能在什么时刻到达？

编译器版本: gcc 4.8.4
请使用标准输入输出(stdin，stdout) ；请把所有程序写在一个文件里，勿使用已禁用图形、文件、网络、系统相关的头文件和操作，如sys/stat.h , unistd.h , curl/curl.h , process.h
时间限制: 1S (C/C++以外的语言为: 3 S)   内存限制: 128M (C/C++以外的语言为: 640 M)
输入:
N路口的个数 N个路口，第一列是路口id，第二列是该路口通行周期 
M 路的条数
M条路，第一列路一端的路口，第二列路另一端的路口，第三列通行所需要的时间 
S起点，T目标点
输出:
最短时间
输入范例:
9
0 3
1 5
2 7
3 3
4 5
5 7
6 9
7 3
8 5
14
0 1 4
0 7 8
1 2 8
1 7 11
2 3 7
2 5 4
2 8 2
3 4 9
3 5 14
4 5 10
5 6 2
6 8 6
6 7 1
7 8 7
0
4
输出范例:
28

## 解法
动态规划，类似Floyd最短路径算法，时间复杂度O(n^3)，空间负责度O(n^2)


## C++代码
```
#include <iostream>
#include <vector>
#include <numeric>
#include <limits>

using namespace std;

/** 请完成下面这个函数，实现题目要求的功能 **/
 /** 当然，你也可以不按照这个模板来作答，完全按照自己的想法来 ^-^  **/
int minTravelTime(int N, vector < vector < int > > intersections, int M, vector < vector < int > > roads, int s, int t) {
    vector<vector<int>> pathMetric(N,vector<int>(N,INT_MAX/N));
    for (int i=0;i<M;i++)
    {
        if(i<N)
            pathMetric[i][i]=0;
        pathMetric[roads[i][0]][roads[i][1]] = roads[i][2];
    }
    
    for (int i=0;i<N;i++)
    {
        for (int j=0;j<N;j++)
        {
            for(int k=0;k<N;k++)
            {
                if (i!=j && pathMetric[i][j]>pathMetric[i][k]+pathMetric[k][j]+max(0,pathMetric[i][k]%(2*intersections[k][1])-intersections[k][1]))
                {
                    pathMetric[i][j] = pathMetric[i][k]+pathMetric[k][j]+max(0,pathMetric[i][k]/(2*intersections[k][1])-intersections[k][1]);
                }
            }
        }
    }

}

int main() {
    ////为避免每次命令行输入，可以通过重定向到文件实现快速输入
    //freopen( "E:\\input_r.txt", "r", stdin);
    int res;

    int _N;
    cin >> _N;
    cin.ignore (std::numeric_limits<std::streamsize>::max(), '\n');


    int _intersections_rows = _N;
    int _intersections_cols = 2;

    vector< vector < int > > _intersections(_intersections_rows);
    for(int _intersections_i=0; _intersections_i<_intersections_rows; _intersections_i++) {
        for(int _intersections_j=0; _intersections_j<_intersections_cols; _intersections_j++) {
            int _intersections_tmp;
            cin >> _intersections_tmp;
            _intersections[_intersections_i].push_back(_intersections_tmp);
        }
    }
    int _M;
    cin >> _M;
    cin.ignore (std::numeric_limits<std::streamsize>::max(), '\n');


    int _roads_rows = _M;
    int _roads_cols = 3;

    vector< vector < int > > _roads(_roads_rows);
    for(int _roads_i=0; _roads_i<_roads_rows; _roads_i++) {
        for(int _roads_j=0; _roads_j<_roads_cols; _roads_j++) {
            int _roads_tmp;
            cin >> _roads_tmp;
            _roads[_roads_i].push_back(_roads_tmp);
        }
    }
    int _s;
    cin >> _s;
    cin.ignore (std::numeric_limits<std::streamsize>::max(), '\n');


    int _t;
    cin >> _t;
    cin.ignore (std::numeric_limits<std::streamsize>::max(), '\n');


    
    res = minTravelTime(_N, _intersections, _M, _roads, _s, _t);
    cout << res << endl;
    
    return 0;

}
```