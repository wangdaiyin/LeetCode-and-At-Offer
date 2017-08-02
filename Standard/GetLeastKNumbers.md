## 问题描述
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

## 解法
解法1：先排序再输出，时间复杂度为O(nlogn)，此处代码略
解法2：同样基于Partition找到第k大个数，使得比第k个数小的都位于它的左边，时间复杂度为O(n)，但需要可以修改输入数组，或者提供O(n)备份空间。
解法3：建立一个大小为k的数据容器来存储最小的k个数字，若容器未满，则直接插入，若容器已满，则比较输入数和容器内最大值的大小，若比最大值小则替换最大值，否则直接输入下一个数。这里需要做的3件事情：1.k个整数中寻找最大数；2.在容器中删除最大数；3.插入一个新的数值；这三步可以用二叉树（最大堆）实现（可以基于STL模板中的multiset<int,greater<int>>实现）,这3步只需logk的时间复杂度，故该解法的时间复杂度为O(nlogk),适合处理海量数据。

## C++代码
##解法2##
```
int Partition(int * input,int n,int start,int end)
{
	//输入检查，略
	int index = start;
	while (start<end)
	{
		while (input[start]>=input[index])
		{
			start++;
		}
		while (input[end]<input[index])
		{
			end--;
		}
		if (start<end)
		{
			swap(input[start],input[end]);
		}
	}
	swap(input[start],input[end]);
	index = end;
	return index;
}

int * GetLeastKNumbers(int * input,int n,int k)
{
	int * output = new int[k]();
	//输入判断，略
	int start = 0;
	int end = n-1;
	int index = Partition(input,n,start,end);
	while (index != k)
	{
		if (index>k)
		{
			end=index-1;
			index = Partition(input,n,start,end);
		}
		else
		{
			start=index+1;
			index = Partition(input,n,start,end);
		}
	}
	for (int i=0;i<k;i++)
	{
		output[i]=input[i];
	}
	return output;
}
```

##解法3##
```
#include <vector>
#include <set>
#include <functional> //greater

void GetLeastKNumbers(const vector<int>& data,multiset<int,greater<int>>& leastNumbers,int k){
	leastNumbers.clear();
	//输入判断
	if (k<1|data.size()<k)
	{
		return;
	}
	//依次输入每一个数
	for (vector<int>::const_iterator it_input = data.begin();it_input!=data.end();++it_input)
	{
		//若容器未满，则直接插入
		if (leastNumbers.size()<k)
		{
			leastNumbers.insert(*it_input);
		}
		else //若容器已满，则比较输入数和容器内最大值的大小，若比最大值小则替换最大值，否则直接输入下一个数。
		{
			multiset<int,greater<int>>::iterator it_output_max=leastNumbers.begin(); //利用STL模板得到最大值
			if (*it_input<*it_output_max)
			{
				leastNumbers.erase(it_output_max);
				leastNumbers.insert(*it_input);
			}
		}
	}
}
```
##解法3_2：使用最大堆##
vector<int> GetLeastNumbers(vector<int> input, int k) { 
        int length=input.size();
		//输入判断
        if(length<=0||k>length) 
			return vector<int>();
         
        vector<int> res(input.begin(),input.begin()+k);
        //建堆
        make_heap(res.begin(),res.end());
         
        for(int i=k;i<length;i++)
        {
            if(input[i]<res[0]) //最大堆中最大值在res[0]
            {
                //先从堆中pop,交换res[0]和res[k-1]位置
                pop_heap(res.begin(),res.end());
				//删除
                res.pop_back();
                //在容器中先push新元素，再执行堆维护
                res.push_back(input[i]);
				//维护堆
                push_heap(res.begin(),res.end());
            }
        }
        //堆排序，使其从小到大输出
        sort_heap(res.begin(),res.end());
         
        return res;
         
    }
