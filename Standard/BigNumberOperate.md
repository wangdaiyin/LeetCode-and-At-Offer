## 问题描述
大整数运算：加法、乘法，很基础的问题，细节请百度

## 解法
用数组来储存运算数，这里大整数加法的时间复杂度为O(n),大整数乘法的时间复杂度为O(mn)，
补充说明：王晓东编著的《算法设计与分析》（第二版）关于两个n位大整数乘法的算法，其时间复杂度O（n^log3）

## C++代码
##大整数加法##
```
char * bigNumberAdd(char * number1,char *number2){
	//输入判断
	if (number1==nullptr||number2==nullptr)
	{
		return nullptr;
	}
	//寻找number1、number2中的较大数,并将较大数的长度赋给n，接着创建两个临时数组分别保存number1、number2，高位补0，补充：这一步可以优化
	int length1 = strlen(number1);
	int length2 = strlen(number2);
	int n = length1;
	char * tmp_num;
	char * tmp_num2;
	if (length1>length2)
	{
		n=length1;
		tmp_num2 = number1;
		tmp_num =new char[n+1];
		memset(tmp_num,'0',length1-length2);
		//strncpy(num2+length1-length2,number2,length2);
		for (int i =1;i<=length2;i++)
		{
			tmp_num[n-i]=number2[length2-i];
		}
		tmp_num[n]='\0';
	}
	else{
		n=length2;
		tmp_num2 = number2;
		tmp_num =new char[n+1];
		memset(tmp_num,'0',length2-length1);
		for (int i =1;i<=length1;i++)
		{
			tmp_num[n-i]=number1[length1-i];
		}
		tmp_num[n]='\0';
	}
	//创建临时数组保存结果sum
	char * sum = new char[n+2];
	memset(sum,'0',n+1);
	sum[n+1]='\0';
	//大整数加法
	int nTakeOver = 0;
	for (int i = n-1;i>=0;--i)
	{
		int nSum=tmp_num[i]-'0'+tmp_num2[i]-'0'+nTakeOver;
		if (nSum>=10) //判断进位
		{
			nSum-=10;
			nTakeOver = 1;
			sum[i+1]=nSum+'0';
		}
		else
		{
			nTakeOver = 0;
			sum[i+1]=nSum+'0';
		}
	}
	//判断首位是否进位到1
	if (nTakeOver==0)
	{
		delete [] tmp_num;
		delete [] tmp_num2;
		return sum+1;
	}	
	sum[0]='0'+nTakeOver;
	delete [] tmp_num;
	delete [] tmp_num2;
	return sum;	
}
```
##大整数乘法##
```
char* multi(char* number_a, char* number_b) {  

	int len_a = strlen(number_a); //计算长度  
	int len_b = strlen(number_b);  

	int* num_arr = new int[len_a+len_b];  
	memset(num_arr, 0, sizeof(int)*(len_a+len_b)); //置0  
	//计算每一位  
	for (int i=len_a-1; i>=0; --i) { 
		for (int j=len_b-1; j>=0; --j) {  
			//理解这个运算关系式很重要
			num_arr[i+j+1] += (number_b[j]-'0')*(number_a[i]-'0');  
		}  
	}  
	//进位  
	for (int i=len_a+len_b-1; i>=0; --i) { 
		if (num_arr[i] >= 10) {  
			num_arr[i-1] += num_arr[i]/10;  
			num_arr[i] %= 10;  
		}  
	}  
	//int数组转为char型数组，方便输出
	char* result = new char[len_a+len_b+1];  

	for( int i=0; i<(len_a+len_b); ++i){  
		result[i] = (char)(((int)'0')+num_arr[i]);  
	}  
	result[len_a+len_b] = '\0'; //添加结束符  

	delete[] num_arr;  

	return result;  
} 
```
