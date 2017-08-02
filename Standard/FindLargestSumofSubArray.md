## 问题描述
输入一个整型数组，数组中有正数也有负数，数组中的一个或者连续多个整数组成一个子数组。求所有子数组的和的最大值。要求时间复杂度为O(n)。

## 解法
解法1：枚举数组的所有连续子数组并求出他们的和，复杂度为O(n^2),不满足要求
解法2：动态规划，用f[i]表示以第i个数字结尾的子数组的最大和，如果i=0或者f[i-1]<0,f[i]=data[i],否则f[i]=f[i-1]+data[i].时间复杂度为O(n)，满足要求。


## C++代码
##动态规划##
```
int FindGreatestSumOfSubArray(vector<int> array) {
    	//输入判断
        //if(array.empty()){
            //throw new std::exception("Invalid Parameters!");
        	//return 0;
    	//}
        int length = array.size();
        int * f = new int[length]();
        f[0]=array[0];
        int result = f[0];
        for(int i=1;i<length;i++)
            {
            if(f[i-1]<=0)
                {
                f[i]=array[i];
            }
            else{
                f[i]=f[i-1]+array[i];
            }
            if(result<f[i])
                result=f[i];
        }
        delete [] f;
        return result;
  }
```