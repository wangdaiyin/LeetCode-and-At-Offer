## 问题描述
数组子串划分：给定一串数字 判断是否存在这三个元素，它们将数字串分为四个子串，其中每个子串的数字之和均相同(该3个元素不纳入计算)。要求时间复杂度和空间复杂度均不能超过O(n)

测试用例：
{1,2,3,4,5,6,7,8,9,10,1,2,3,24,3,4,1,3,4,5,3,2,1,2,3,4,5,6,7} 输出 false
{1,1,1,1,1,1,1} 输出 true

## 解法
用O(n)的空间用来保存前缀和(不计该数), 用三个变量i,j,k(i<j<k)遍历数组，并根据前缀和的关系来判断是否存在正确划分。 

## C++代码
```
bool NumberDevidebyThree(int * numbers,int n)
{
    if (numbers==nullptr||n<7)
    {
        return false;
    }
    //O(n)的空间用来保存前缀和,不计该数
    int * sum = new int[n+1]();
    for (int i=0;i<n;i++)
    {
        sum[i+1] = sum[i]+numbers[i];
    }
    //虽然有两层循环，但因为i<j<k恒成立，sum 1~n 最多走两遍，故时间复杂度为O(n)
    int i,j,k;
    for (i=1,j=3,k=5;i<j;i++)
    {
        while (sum[j]-sum[i+1]<sum[i]&&j<n-3){
            j++;
        }
        if (j>=k){
            k=j+2;
        }
        while (sum[k]-sum[j+1]<sum[i]&&k<n-1){
            k++;
        }
        if (sum[i]==sum[j]-sum[i+1] && sum[i]==sum[k]-sum[j+1] && sum[i]==sum[n]-sum[k+1]){
            cout<<i<<j<<k;
            return true;
        }
    }
    return false;
}

```