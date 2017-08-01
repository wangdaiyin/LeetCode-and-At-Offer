## 问题描述
在nXn格的国际象棋上摆放n个皇后，使其不能互相攻击，即任意两个皇后都不能处于同一行、同一列或同一斜线上，问有多少种摆法，并返回所有摆法。

## 解法
回溯法，时间复杂度为O(n^(n+1)),但实际运行中由于搜索树的剪枝，时间要少很多

## C++代码
**法一：根据棋盘矩阵，检查每个位置的有效性**
```
vector<vector<string>> solveNQueens(int n)
{
	vector<vector<string>> results;
	vector<string> nQueens(n,string(n,'.'));
	int count = 0;
	solveNQueens(results,nQueens,0,n,count);
	//cout<<count<<","<<results[0][0];  //以8皇后问题为例，将输出92种排列方式。
	return results;
}

void solveNQueens(vector<vector<string>> &res,vector<string> nQueens,int row,int &n, int & count)
{
	//如果已搜索完n行，则得到一种满足要求的排布
	if(row == n){
		res.push_back(nQueens);
		++count;
		return;
	}
	//每一次迭代中，对每一行都依次遍历每一列，判断其取值是否满足要求
	for (int col =0;col !=n;++col)
	{
		if (isValid(nQueens,row,col,n)) //也可以通过三个一维数组保存列col[n]、45对角线line45[2*n-1]、135对角线line135[2*n-1]来做有效性判别，见法二
		{
			nQueens[row][col] = 'Q';//若只是用vector<int>保存皇后位置，则此处为nQueens[row]=col;
			//递归搜索下一行
			solveNQueens(res,nQueens,row+1,n,count);
			//还原
			nQueens[row][col] = '.';
		}
	}

}
//判断每个位置的有效性
bool isValid(vector<string> &nQueens, int row, int col, int &n)
{
	//检查同一列是否存在其他皇后
	for (int i=0;i!=row;++i)
	{
		if (nQueens[i][col] =='Q')
		{
			return false;
		}
	}
	//检查45°斜线
	for (int i=row-1,j=col-1;i>=0&&j>=0;--i,--j)
	{
		if (nQueens[i][j] =='Q')
		{
			return false;
		}
	}
	//检查135°斜线
	for (int i=row-1,j=col+1;i>=0&&j<n;--i,++j)
	{
		if (nQueens[i][j] =='Q')
		{
			return false;
		}
	}
	return true;
}
```

**法二：利用三个一维数组保存列col[n]、45对角线line45[2*n-1]、135对角线line135[2*n-1]来做有效性判别** 
```
vector<vector<string> > solveNQueens(int n) {
        vector<vector<string> > res;
        vector<string> nQueens(n, tring(n, '.'));
        vector<int> flag_col(n, 1), flag_45(2 * n - 1, 1), flag_135(2 * n - 1, 1);
        int count = 0;
        solveNQueens(res, nQueens, flag_col, flag_45, flag_135, 0, n,count);
        return res;
    }

void solveNQueens(vector<vector<string>> &res,vector<string> nQueens,vector<int> &flag_col, vector<int> &flag_45, vector<int> &flag_135, int row, int &n, int &count) {
        if (row == n) {
            count++;
            res.push_back(nQueens);
            return;
        }
       for (int col = 0; col != n; ++col){
            if (flag_col[col] && flag_45[row + col] && flag_135[n - 1 + col - row]) {
                flag_col[col] = flag_45[row + col] = flag_135[n - 1 + col - row] = 0;
                nQueens[row][col] = 'Q';
                solveNQueens(res, nQueens, flag_col, flag_45, flag_135, row + 1, n);
                nQueens[row][col] = '.';
                flag_col[col] = flag_45[row + col] = flag_135[n - 1 + col - row] = 1;
            }
        }
    }
}

```