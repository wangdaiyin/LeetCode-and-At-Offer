## 问题描述
求两个字符串的最长公共子串,输入两个字符串，输出两行：第一行为最长公共子串长度，第二行为最长公共子串。

## 解法
动态规划，令C[i,j]为字符串(X0,X1,……,Xi)和字符串(Y0,Y1,……,Yj)的公共子串，若Xi==Yj，则C[i,j]=C[i-1,j-1]+1；若Xi!=Yj，C[i,j]=max{C[i,j-1],C[i-1,j]}；初始化C[i,0]=0，C[0,j]=0，0<=i<m,0<=j<n. 时间复杂度为O(mn).

## C++代码
```
void TrackSolution(char* str1,char* str2,int** flag,int m,int n);

void FindLongestCommonSubstring(char* str1,char* str2)
{
    if (str1==nullptr||str2==nullptr){return;}
    int m=strlen(str1);
    int n=strlen(str2);
    //令C[i,j]为字符串(X0,X1,……,Xi)和字符串(Y0,Y1,……,Yj)的公共子串长度,并初始化C[i,0]=0，C[0,j]=0，0<=i<m,0<=j<n.
    int ** C =new int*[m];
    for (int i=0;i<m;i++){
        C[i]=new int[n];
        C[i][0]=0;
    }
    for (int j=0;j<n;j++){
        C[0][j]=0;
    }
    //定义标记数组,1表示str1[i]==str2[j],2表示不考虑Xi,3表示不考虑Yi
    int ** flag =new int*[m];
    for (int i=0;i<m;i++){
        flag[i]=new int[n];
    }

    for (int i=1;i<m;i++){
        for (int j=1;j<n;j++){
            if (str1[i]==str2[j]){
                C[i][j]=C[i-1][j-1]+1;
                flag[i][j]=1;
            }
            else{
                if (C[i-1][j]>C[i][j-1]){
                    C[i][j]=C[i-1][j];
                    flag[i][j]=2;
                }
                else{
                    C[i][j]=C[i][j-1];
                    flag[i][j]=3;
                }
            }
        }
    }

    //输出结果
    int res = C[m-1][n-1];
    printf("%d\n",res);
    //根据标记矩阵追踪解,并输出
    TrackSolution(str1,str2,flag,m-1,n-1);

    //清除动态内存，
    for (int i=0;i<m;i++)
    {
        delete[] flag[i];
        delete[] C[i];
    }
    delete[] flag;
    delete[] C;
    return; 
}
void TrackSolution(char* str1,char* str2,int** flag,int m,int n)
{
    switch (flag[m][n])
    {
    case 1:
        TrackSolution(str1,str2,flag,m-1,n-1);
        printf("%c,",str1[m]); 
        break;
    case 2:
        TrackSolution(str1,str2,flag,m-1,n);
        break;
    case 3:
        TrackSolution(str1,str2,flag,m,n-1);
        break;
    default:
        break;
    }
}

```