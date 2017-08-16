## 问题描述
在二维数组中查找：在一个二维数组[m,n]中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 解法
1.从右上角开始往左遍历数组；
2.如果该数大于查找数，则删除该列；如果该数小于查找数，则删除该行，如果等于该查找数则返回TRUE；
3.直到数组遍历完还没找到则返回FALSE。

最坏情况下，时间复杂度为O(m+n)


## C++代码
```
bool Find(int target, vector<vector<int> > array) {
        //bool res = false;
        if(array.size()<=0||array[0].size()<=0)
            return false;
        //从右上角开始往左遍历数组
        int i=0;
        int j = array[0].size()-1;
        while(j>=0&&i<array.size())
        {
            //如果该数大于查找数，则删除该列；如果该数小于查找数，则删除该行。如果等于该查找数则返回TRUE
            if(array[i][j]>target)
                j--;
            else if(array[i][j]<target)
                i++;
            else
                return true;
        }
        //直到数组遍历完还没找到则返回FALSE
        return false;
    }
```