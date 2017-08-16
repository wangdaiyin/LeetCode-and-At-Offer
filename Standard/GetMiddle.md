## 问题描述
得到中位数：如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

## 解法
解法1：数组，基于Partition，插入新数据的时间为O(1)，时间复杂度为O(n)；若数组已排序，则O(1)，但插入新数据的时间为O(n)
解法2：二叉搜索树，插入新数据的平均时间为O(logn)，但若极度不平衡最差仍为O(n),得到中位数也是如此。
解法3：AVL树(平衡的二叉搜索树)，插入和获取中位数的时间复杂度均为O(logn)，但无现有函数库提供该结构模板；
解法4：最大堆&最小堆实现，只需保证中位数左右两边分别为比其小和比其大的数，两部分内部无需排序，只需要获取到左一部分的最大值和右一部分的最小值。具体实现如下：
1.为保证数据平均分配到两个堆中，依次隔开插入最小堆和最大堆；
2.为保证最大堆的数据都小鱼最小堆的数据，

*以下只提供解法4的实现代码，解法1的代码可参考GetLeastKNumbers、SelectMiddleNumber*

## C++代码
```
class Solution {
public:
multiset<int,greater<int>> big; //实现最大堆
multiset<int,less<int>> small; //实现最小堆
void Insert(int num)
{
    static int count =0;
    //当输入数据次数为偶数时，插入最大堆
    if(count%2==0)
    {
        if (small.empty()||num<*small.begin()){
            big.insert(num);
        }
        else{//输入的数比最小堆的最小值要大时，调整数据
            big.insert(*small.begin());
            small.erase(small.begin());
            small.insert(num);
        }
    }
    else
    {
        if (big.empty()||num>*big.begin()){
            small.insert(num);
        }
        else{//输入的数比最大堆的最大值要小时，调整数据
            small.insert(*big.begin());
            big.erase(big.begin());
            big.insert(num);
        }
    }
    count++;
}

double GetMedian()
{ 
    if ((big.size()+small.size())&1) //为奇数
        return (double)*big.begin();
    else
        return (double)(*big.begin()+*small.begin())/2;
}

};
```