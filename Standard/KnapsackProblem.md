## 问题描述
背包问题：背包最多装10kg重量的物品，现有如下物品：
v1=1，v2=3,v3=5,v4=9
w1=2, w2=3, w3=4,w4=7
请问如何装使得所装物品价值最大？

输入：背包总重量、物品数目、每个物品的价值及重量
输出：所装物品价值最大值 / 所装物品清单

## 解法
动态规划,时间复杂度为O(nb) (n种物品，最大重量b)，所需额外空间O(nb)
令f[k][y]表示只容许装前k中物品，背包总重不超过y时的背包的最大重量，则
f[0][j]=0;f[i][0]=0；
f[i][j] = max(f[i-1][j],f[i][j-weights[i-1]] + values[i-1])；
若是0-1背包问题，则递归公式转换为
f[i][j] = max(f[i-1][j],f[i-1][j-weights[i-1]] + values[i-1])
此外，设立结果标记矩阵追踪解

## C++代码
```
void knapsackProblem(int totalWeight,int n,vector<int> values,vector<int> weights)
{
    //f[k][y]表示只容许装前k中物品，背包总重不超过y时的背包的最大重量
    vector<vector<int>> f(n+1,vector<int>(totalWeight+1,0));
    //所装物品清单标记矩阵,记录得到最大值时所用到物品的最大标号
    vector<vector<int>> flag(n+1,vector<int>(totalWeight+1,0));
    for (int y = 0;y<=totalWeight;y++)
    {
        f[1][y] = values[0]* y/weights[0];
    }
    
    for (int i=1;i<=n;i++)
    {
        for (int j=1;j<=totalWeight;j++)
        {
            //若每件物品为0-1，则为f[i-1][j-weights[i-1]] + values[i-1]
            if (j-weights[i-1]>=0 && f[i-1][j] <= f[i][j-weights[i-1]] + values[i-1]){
                f[i][j] = f[i][j-weights[i-1]] + values[i-1]; 
                flag[i][j] = i-1;
            }
            else{
                f[i][j] =f[i-1][j];
                flag[i][j] = flag[i-1][j];
            }
        }
    }
    //输出所装物品价值最大值
    cout<<f[n][totalWeight]<<endl;
    //输出所装物品清单
    vector<int> nums(n,0); //记录n种物品的装入量
    int i = n;
    int y = totalWeight;
    while (flag[i][y]!=0)
    {
        i = flag[i][y]+1;
        nums[i-1]++;
        y = y-weights[i-1];
    }
    for (int i=0; i<n;i++)
    {
        //if (nums[i-1]!=0)
        cout<<"第"<<i+1<<"种物品个数为"<<nums[i]<<endl;;
        
    }
}
```


