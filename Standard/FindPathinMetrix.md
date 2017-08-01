## 题目描述
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 例如 a b c e s f c s a d e e 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。
时间限制：1s， 空间限制：32768K

## 解法
**回溯法**，时间复杂度为**O(4^n)**，但实际搜索速度会比O(4^n)快很多，需要O(mn)的矩阵空间大小。
在矩阵中任选一个格子作为路径的起点，假设矩阵中某个格子的字符为ch，并且该格子对应于路径上的第i个字符，则到相邻格子上寻找路径上的第i+1个字符。重复这个过程，直到找到路径上所有字符或者所有格子都作为起点已遍历完。
由于矩阵的格子在同一次路径不能重复进入，因此定义一个和字符矩阵大小一样的布尔值矩阵，用来标志路径是否已经进入了每个格子。

## C++代码
```
    bool searchPath(char * matrix,int rows,int cols,int row,int col,char *str,int& pathLength,bool* visited){
        //输入判断
        if(str[pathLength]=='\0')
            return true;
        //判断对应第i个字符位置的所有邻居中是否存在第i+1个字符
        bool hasSearchPath= false;
        //判断输入的数matrix[row*cols+col]是否等于str[pathLength]
        if(row>=0 && row<rows && col>=0 && col<cols && !visited[row*cols+col] && matrix[row*cols+col]==str[pathLength]) //合理利用短路避免矩阵越界
            {
             //在前后左右搜索第二个数，并不断迭代，若未成功则回溯
            pathLength++;
            visited[row*cols+col]=true;
            hasSearchPath = searchPath(matrix,rows,cols,row,col-1,str,pathLength,visited)||searchPath(matrix,rows,cols,row,col+1,str,pathLength,visited)||searchPath(matrix,rows,cols,row-1,col,str,pathLength,visited)||searchPath(matrix,rows,cols,row+1,col,str,pathLength,visited);
            if(!hasSearchPath) //回溯
                {
                pathLength--; 
                visited[row*cols+col]=false;
            }
        }
        return hasSearchPath;
    }
    bool hasPath(char* matrix, int rows, int cols, char* str)
    {
    	//输入判断
        if(matrix==nullptr||rows<1||cols<1||str==nullptr)
            return false;
        
        //设立是否走过的标记矩阵，并初始化为0
        bool * visited = new bool[rows*cols]();
        //memset(visited,0,rows*cols);
        
        //记录路径长度
        int pathLength = 0;
        //遍历矩阵
        for(int row=0;row<rows;row++){
            for(int col=0;col<cols;col++)
                {
                //如果以[row,col]作为第一个数出发 搜寻到寻找路径，返回为真
                if(searchPath(matrix,rows,cols,row,col,str,pathLength,visited)){
                    return true;
                }
            }
        }
        delete [] visited;
        return false;
    }
```