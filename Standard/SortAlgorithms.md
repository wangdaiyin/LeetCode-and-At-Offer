## 问题描述
输入一个整型数组，输出排序后的数组

## 解法
解法1：冒泡排序，时间复杂度为O(n^2)
解法2：插入排序，时间复杂度为O(n^2)
解法3：选择排序，时间复杂度为O(n^2)
解法4：归并排序，时间复杂度为O(nlogn)
解法5：快速排序，最坏时间复杂度为O(n^2)，平均复杂度为O(nlogn)
解法6：堆排序，时间复杂度为O(nlogn)

## C++代码
**冒泡排序**
```
void BubbleSort(int array[],int length)
{
	//输入判断，略
	for(int i =0;i<length;i++)
	{
		for(int j = length-1;j>i;j--)
		{
			if (array[j]<array[j-1])
			{
				swap(array[j],array[j-1]);
			} 
		}
	}
}
```
**插入排序**
```
void InsertSort(int array[],int length)
{
	for(int i=1;i<length-1;i++)
	{
		//当前欲排序的数
		int nextNum = array[i];
		int j;
		for(j=i-1;j>=0;j--)
		{
			if (array[j]>nextNum)
			{
				array[j+1]=array[j];
			}
			else{
				break;
			}
		}
		array[j+1]=nextNum;
	}
}
```
**选择排序**
```
void SelectSort(int array[],int length)
{
	//依次比对获取array[i]-array[length-1]间最大值
	for(int i=0;i<length-1;i++) 
	{
		int index=i;//最大值索引
		int tmp = a[i];
		for(int j=i+1;j<length;++j)
		{
			if(a[j]<tmp)
			{
				index=j;
				tmp=a[j];
			}
		}
		if(index!=i)
			swap(a[i],a[index]);
	}
}
```
**归并排序**
```
void MergeSortImp(int array[],int start, int end,int result[])
{
	if (array==nullptr||start>=end)
	{
		return;
	}
	//二分数组
	int middle = (end-start)/2;
	MergeSortImp(array,start,start+middle,result);
	MergeSortImp(array,start+middle+1,end,result);
	//合并两个子数组
	Merge(array,start,end,result);
	//把排序后的区间数据复制到原始数据中
	for (int k =start;k<=end;k++)
	{
		array[k]=result[k];
	}
}
void Merge(int array[],int start,int end,int result[])
{
	int middle=(end-start)/2;
	int result_index=start,i=start,j=start+middle+1;
	while (i<=start+middle&&j<=end)
	{
		if (array[i]<array[j])
			result[result_index++]=array[i++];
		else
			result[result_index++]=array[j++];
	}
	while (i<=start+middle)
		result[result_index++]=array[i++];
	while (j<=end)
		result[result_index++]=array[j++];
}
```
**快速排序**
```
int Partition(int array[],int first,int last)；

void QuickSort(int array[],int first,int last)
{
	if (first<last)
	{
		//分区
		int index=Partition(array,first,last);
		//递归
		QuickSort(array,first,index-1);
		QuickSort(array,index+1,last);
	}	
	return ;
}
int Partition(int array[],int first,int last)
{
    //输入判断
	if (array==nullptr||first<0||first>=last)
		return 0;
	int key = array[first];
	int left=first;
	int right=last;
	while(left<right){
		while (array[right]>key)
		{
			right--;
		}
		while (array[left]<=key)
		{
			left++;
		}
		if (left<right)
		{
			swap(array[left],array[right]);
		}
	}
	swap(array[first],array[right]);
	return right;
}
```
**堆排序**
```
void MaxHeapify(int a[],int root,int first,int end)；//维护最大堆
void BuildMaxHeap(int a[],int length);//构造最大堆
void HeapSort(int a[],int length)
{
	//先构造一个最大堆,A[0]为最大值
	BuildMaxHeap(a,length);
	for(int i =length-1;i>=1;i--)
	{
		swap(a[i],a[0]);
		MaxHeapify(a,0,0,i-1);
	}
}

void BuildMaxHeap(int a[],int length)
{
	//从下往上遍历每一个根节点调整树使其维持最大堆特性,根节点为a[i]的左右子节点为a[2*i+1],a[2*i+2]
	for (int i =(length)/2-1;i>=0;i--)  //因为数组从0开始计数，所以i=(length)/2-1
	{
		MaxHeapify(a,i,0,length-1);
	}
}
//维护最大堆特性的函数
void MaxHeapify(int a[],int root,int first,int end)
{
	//找到左右节点、根节点三个数当中最大的那个数，将最大的那个数和root进行互换
	int leftNode = 2*root+1;
	int rightNode = 2*root+2;
	int largest = root;
	if (a[leftNode]>a[root]&&leftNode<=end)
	{
		largest = leftNode;
	}
	if (a[rightNode]>a[largest]&&rightNode<=end)
	{
		largest = rightNode;
	}
	if (largest!=root)
	{
		swap(a[root],a[largest]);
		//交换有可能破坏子树的最大堆性质，所以对所交换的那个子节点进行一次维护，而未交换的那个子节点，根据我们的假设，是保持着最大堆性质的。
		MaxHeapify(a,largest,first,end);
	}
}
```