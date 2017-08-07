
```
//2017.08.07 15:40-17:40
#include "head.h";
#define PI 3.1415926
#define MAX (a>b)?a:b
#define HW "Helloworld"

int main()
{
	//初识c++
	firstDemo();
	//C++简单输入输出 http://www.cnblogs.com/lucyjiayou/archive/2012/01/05/2312684.html  
	//http://blog.csdn.net/lmerissa/article/details/50536469
	//int n,m;
	//cin>>n>>m;//(cin>>n)类的函数调用，返回的类型仍然是cin
	//sum(n);
	//常量、变量，常量的类型
	const int constn = PI;
	string str = HW;
	//简单数据类型，bool,char,int,float,double,long,short,定义/声明，使用，类型转换（强制、自动转换） unsigned   后缀unsigned int i=10ul，
	int i = (int) PI; //static_cast、dynamic_cast、const_cast和reinterpret_cast: http://www.jellythink.com/archives/205
	int i2 = static_cast<int> (PI); 
	unsigned int a = 10;
	//i = -a;
	//a=-10;
	//复合数据类型:数组、类、结构、共用体、枚举……

	//运算符与表达式：赋值、算术、逻辑、关系、复合、自增自减、位>>/<<、逗号、条件
	//优先级


	system("pause");
	return 0;
}

//#include "head.h"

void firstDemo()
{
	cout<<"Hello World!"<<" C++!"<<"\n";
	printf("%.2f:%s\n",1.00067,"Hello World,C!");
}

int sum(int n)
{
	int sum = 0;
	for(int i=0;i<=n;i++)
		sum += i;
	cout<<sum<<endl;
	return sum;
}

```
