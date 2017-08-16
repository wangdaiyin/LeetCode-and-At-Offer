## 问题描述
选择中位数：数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

## 解法
解法1：先排序再去中间数，时间复杂度为O(nlogn)，这里不作实现；
解法2：选择第n/2大的数字，有成熟的时间复杂度为O(n)的选择算法，这里不作实现；
解法3：基于类似快速排序中的Partition思路，时间复杂度为O(n)；
解法4: 利用所求数与其他数的次数关系（保存两个值：数组中的数字和次数），顺序遍历数组，当次数为0时，保存下一个数字并把次数设为1。当遍历到下一个数字时，如果下一个数组与保存的数组相同，则次数加1，否则次数减1，则最后剩下的那个数字为所求的数，时间复杂度为O(n)。

## C++代码
##解法3##
```
int NumberPartition(int* array,int length,int start,int end)；//Partition划分

int MoreThanHalfNum(int* array,int length)
{
	//输入检查
	if (array==nullptr||length<=0)
	{
		return 0;
	}
	int start = 0, end = length-1;
	//对数组进行划分，左边比a[0]大，右边比a[0]小
	int index = NumberPartition(array,length,start,end);
	int middle = length>>1;
	while (index!=middle)
	{
		if(index>middle)  //结果(中位数)位于index左边
		{
			index=NumberPartition(array,length,start,index-1);
		}
		else //结果(中位数)位于index右边
		{
			index=NumberPartition(array,length,index+1,end);
		}
	}
	int result = array[middle];
	//检查结果是否过半，此处省略
	//if(!CheckMoreThanHalfNum(array,length,result))
     //       return 0;

	return result;
}

int NumberPartition(int* array,int length,int start,int end)
{
	//输入检查
	if (array==nullptr||length<=0||end>=length||start<0||start>=end)
	{
		//throw new std::exception("Invalid Parameters!");
		return 0;
	}
	int index=start;

	while (start<end)
	{
		while (array[start]<=array[index])
		{
			start++;
		}
		while (array[end]>array[index])
		{
			end--;
		}
		if (start<end)
		{
			swap(array[start],array[end]);//交换两个整数
		}
	}
	swap(array[index],array[end]);
	index = end;
	return index;
}
```
##解法4##
```
bool CheckMoreThanHalfNum(vector<int> numbers,int result);//结果检查

int MoreThanHalfNum_Solution(vector<int> numbers) {
    	//输入判断
        if(numbers.empty())
        {   
            cout<<"Invalid Input!";
        	return 0;
        }
        //记录当前保存数字和次数
        int result = numbers[0];
        int times = 1;
        for(int i =1;i<numbers.size();++i)
        {
            if(times==0)
            {
                result = numbers[i];
                times==1;
            }
            else if(result==numbers[i])
                times++;
            else
                times--;
        }
        //检查是否过半
        if(!CheckMoreThanHalfNum(numbers,result))
            return 0;
        return result;
            
    }

bool CheckMoreThanHalfNum(vector<int> numbers,int result)
    {
        int times=0;
        for(int i=0;i<numbers.size();++i)
        {
            if(result==numbers[i])
                times++;
        }
        if(times>numbers.size()/2)
            return true;
        return false;
    }
```