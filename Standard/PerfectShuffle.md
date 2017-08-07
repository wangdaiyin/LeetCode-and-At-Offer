## 问题描述
完美洗牌问题：给定一个已经降序排好序的正数数组，要求按「最小、最大、次小、次大……」的顺序重新排序。期望的时间复杂度为O(n)，空间复杂度为O(1)，即不能申请额外的数组。例如：输入[7,6,5,4,3,2,1]，输出[1,7,2,6,3,5,4]。

## 解法
通过索引转换，时间复杂度为O(n)，空间辅助度为O(1)
1. 旧位置i < 2/n：新位置j = 2 * i +1
2. 旧位置i > 2/n: 新位置j = 2 * (n - 1 - i) 
3. 旧位置i = 2/m: 新位置j = n-1
用一个变量coveredNum记录当前要正确排序的位置，然后为被覆盖掉的元素desArray[coveredNum]寻找自己的正确的位置，直到形成一个环为止；对原正数取负来标记其该位置是否已交换入正确顺序的数。

## C++代码
```
void IndexSwapCircle(int* desArray,int n, int coveredNum,int currentPos,int initPos);

void PerfectShuffle(int* desArray, int n)
{   
    //遍历所有位置使其按要求排好序
    for (int i=0;i<n;++i)
    {
        if (desArray[i]<0)
            continue;
        IndexSwapCircle(desArray,n,desArray[i],i,i);
    }
    //还原成正数
    for (int i=0;i<n;++i)
    {
        desArray[i] = -desArray[i];
        //printf("%d\t",desArray[i]);
    }
}
void IndexSwapCircle(int* desArray,int n, int coveredNum,int currentPos,int initPos)
{
    //索引转换
    if (currentPos==n/2)
        currentPos=n-1;
    else currentPos= (currentPos<n/2?2*currentPos+1:2*(n-1-currentPos));
    //当环走完一圈时
    if (currentPos==initPos)
    {
        desArray[currentPos] = -coveredNum;
        return;
    }
    //把旧位置的值替换到当前新位置去，并用取负数做为已按要求排好序的标记
    int tmp = desArray[currentPos];
    desArray[currentPos]=-coveredNum;
    coveredNum = tmp;
    IndexSwapCircle(desArray,n,coveredNum,currentPos,initPos);
}
```

参考资料：
1.[https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/02.09.md]:https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/02.09.md
