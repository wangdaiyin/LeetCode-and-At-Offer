## 题目描述
地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？
时间限制：1s， 空间限制：32768K

## 解法
回溯法，时间复杂度为O(4^n)，需要O(mn)的矩阵空间大小，类似FindPathinMetrix

## C++代码
```
int getDigitSum(int num);
int movingCountCore(int threshold,int rows,int cols,int ri,int ci,bool* visited);
//计算可移动范围
int movingCount(int threshold, int rows, int cols)
    {
        //输入判断
        if(threshold<=0||rows<1||cols<1){
            return 0;
        }
        //设定标记矩阵
        bool * visited = new bool[rows*cols](); 
        //从（0,0）出发,遍历可到达范围
        int count = movingCountCore(threshold,rows,cols,0,0,visited);
        delete [] visited;
        return count;
    }
//递归遍历可到达范围
int movingCountCore(int threshold,int rows,int cols,int ri,int ci,bool* visited){
        
        int count = 0;
        //输入判断、各自限定条件判断
        if(ri>=0&&ri<rows&&ci>=0&&ci<cols&&!visited[ri*cols+ci]&&(getDigitSum(ri)+getDigitSum(ci)<=threshold))
           {
            visited[ri*cols+ci]=true;
            count = 1+movingCountCore(threshold,rows,cols,ri,ci-1,visited)+movingCountCore(threshold,rows,cols,ri,ci+1,visited)
                +movingCountCore(threshold,rows,cols,ri-1,ci,visited)+movingCountCore(threshold,rows,cols,ri+1,ci,visited);
        }
        return count;
    }
    //计算各自行列坐标的数位之和
    int getDigitSum(int num)
       {
        int sum = 0;
        while(num>0){
            sum += num%10;
        	num = num/10;
        }
        return sum;
    }
```