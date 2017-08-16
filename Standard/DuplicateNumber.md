## 问题描述
第一个重复的数字：在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

## 解法
**思路1**：先排序再统计重复，时间复杂度为O(nlogn)
**思路2**：利用O(n)的哈希表，从头到尾扫描数组的每个数字，判断哈希表中是否已存在该数字…… 时间复杂度为O(n)，需要额外的O(n)的空间
**思路3**: 考虑到0到n-1间的数与数组下标的对应关系，从头到尾扫描数组中每个数字，当扫描到下标为i的数字时，首先比较这个数字m是否等于i，若相等，则扫描下一个；如果不相等，则比较m和a[m]，若不相等则交换，相等则输出一个重复数。时间负责度为O(n)，无需额外空间。

## C++代码
**思路1**
```
bool duplicate(int numbers[], int length, int* duplication) {
	//输入判断
	if(numbers == nullptr ||length<=0)
		return false;
	for (int i =0;i<length;i++)
	{
		if(numbers[i]<0||numbers[i]>length-1)
			return false;
	}
	//排序
	QuickSort(numbers,0,length-1); //见SortAlgorithms.md
	//输出重复数
	for (int i=0;i<length-1;i++)
	{
		if(numbers[i+1]==numbers[i])
		{
			*duplication = numbers[i];
			return true;
		}
		
	}
}
```

**思路2**
```
bool duplicate(int numbers[], int length, int* duplication) {
	//输入判断
	if(numbers == nullptr ||length<=0)
		return false;
	for (int i =0;i<length;i++)
	{
		if(numbers[i]<0||numbers[i]>length-1)
			return false;
	}
	//初始化哈希表
	int * tmparray = new int[length]();
	tmparray[0]=-1;
	//遍历
	for(int i =0;i<length;i++)
	{
		if(numbers[i]==tmparray[numbers[i]])
		{
			*duplication =numbers[i];
			return true;
		}
		else
		{
			tmparray[numbers[i]]=numbers[i];
		}
	}
	delete [] tmparray;
	return false;
}
```

**思路3**
```
bool duplicate(int numbers[], int length, int* duplication) {
	//输入判断
	if(numbers == nullptr ||length<=0)
		return false;
	for (int i =0;i<length;i++)
	{
		if(numbers[i]<0||numbers[i]>length-1)
			return false;
	}
	//遍历
	for(int i =0;i<length;i++)
	{
		while(numbers[i]!=i)
		{
			if(numbers[i]==numbers[numbers[i]])
			{
				*duplication =numbers[i];
				return true;
			}
			int tmp = numbers[i];
			numbers[i]=numbers[tmp];
			numbers[tmp] = tmp;
		}
	}
}
```