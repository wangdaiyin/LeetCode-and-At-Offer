
```
//学生：高一 已有一定JAVA基础
/*课程记录
//2017.08.07 15:40-17:50
//2017.08.09 15:50-18:00
//2017.08.10 15:30-17:50
*/
/*算法练习网站
//http://noi.openjudge.cn
//http://codevs.cn/problem/
*/
#include "head.h"; //自定义头文件
//宏定义常量/表达式
#define PI 3.1415926
#define MAX (a>b)?a:b
#define HW "Helloworld"

//类定义
class ANIMAL
{
public:
	ANIMAL():_type("ANIMAL"){};
	virtual void OutPutname(){cout<<"ANIMAL";};
private:
	string _type ;
};
class DOG:public ANIMAL
{
public:
	DOG():_name("大黄"),_type("DOG"){};
	void OutPutname(){cout<<_name;};
	void OutPuttype(){cout<<_type;};
private:
	string _name ;
	string _type ;
};

//函数声明
void firstDemo();
void C_InputOutput();
void Cplusplus_InputOutput();
void TypesTransform();//强制类型转换
int Fabonaci(int n);
int Fabonaci2(int n);

int main()
{
	//初识c++
	firstDemo();
	
	//C&C++简单输入输出 
	C_InputOutput();
	Cplusplus_InputOutput()
	
	//常量、变量，常量的类型
	const int constn = PI;
	string str = HW;
	//简单数据类型，bool,char,int,float,double,long,short,定义/声明，使用，类型转换（强制、自动转换） unsigned   后缀unsigned int i=10ul，
	int i = (int) PI; 
	unsigned int a = 10;
	//i = -a;
	//a=-10;
	//string-int互相转换 http://blog.csdn.net/u010510020/article/details/73799996
	string s = "12";   
	int a = atoi(s.c_str());  
	s = to_string(a);
	//C++强制类型转换static_cast、dynamic_cast、const_cast和reinterpret_cast: http://www.jellythink.com/archives/205
	
	int i2 = static_cast<int> (PI); 
	
	//运算符与表达式：赋值、算术、逻辑、关系、复合、自增自减、位>>/<<、逗号、条件  //优先级   扫过   
	//lesson1 End
	
	//语句结构、变量作用域、全局变量、局部变量
	
	//复合数据类型:数组、类、结构、共用体、枚举……
	
	//函数：定义、调用、函数传参、返回、函数指针
	//递归：以斐波那契数列为例，拓展到n台阶走法
	Fabonaci(10);  //递归
	Fabonaci2(10); //迭代
	//内联函数、函数重载、函数模板
	
	//内存模型、new、

对象和类
	
	system("pause");
	return 0;
}

void firstDemo()
{
	cout<<"Hello World!"<<" C++!"<<"\n";
	printf("%.2f:%s\n",1.00067,"Hello World,C!");
}

//斐波那契数列1 1 2 3 5 8 13 21 34 ...
//得到第n个斐波那契数
int Fabonaci(int n)
{
	//int result = 0;
	if(n == 1)
		return 1;
	else if(n == 2)
		return 1;
	else
		return Fabonaci(n-1)+Fabonaci(n-2);
}
int Fabonaci2(int n)
{
	int result=0;
	int firstnum = 1;
	int secondnum =1;
	for(int i=2;i<n;i++)
	{
		result = firstnum+secondnum;
		firstnum = secondnum;
		secondnum = result;
	}
	return result;
}

void C_InputOutput()
{
	//参考资料：http://blog.csdn.net/lmerissa/article/details/50536469 
	//       & http://blog.csdn.net/heixiaolong7/article/details/50856169
	/*命令行输入
	//scanf:在输入数值数据时，如输入空格、回车、Tab 键或遇非法字符（不属于数值的字符），认为该数据结束。
	//scanf 函数返回第一个非法输入之前的有效输入的个数
	*/
	int i;
	char pstr[30];
	scanf("%i%s",&i,pstr);
	/*getchar:输入一个任意字符，输入时暂存在输入缓冲区中，当按Enter时输入到内存；也可用于得到滞留缓存中的字符。
	//getchar常与输入EOF结束(windows-ctrl+z，linux-ctrl+d然后输入回车键)匹配使用，
	//只有在getchar()提示新的一次输入时，直接输入Ctrl+D才相当于文件结束符。
	//EOF作为行结束符时的情况，这时候输入Ctrl+D并不能结束getchar(),而只能引发getchar()提示下一轮的输入。
	*/
	char ch = getchar(); //'\n' 
	printf("%c",ch); 
	//gets:输入一个字符串，接收回车键前所有字符输入，且回车不会滞留在缓存中。也可从文件中读取，EOF为结束标志
	char * pstr_gets = gets(pstr); //返回值为获得的字符串的首地址。
	ch = getchar();   //待输入

	/*命令行输出
	//printf，格式字符同scanf,但无&
	//指定数据宽度和小数位数格式输出（右对齐）：%m.nf; float-只能保证6位有效小数，double-15位有效小数
	//输出的数据向左对齐：%-m.nf
	//%%输出%
	*/
	printf("%c\n",ch); 
	float f = 3.1864;
	printf("%-4.2f\n",f);
	//putchar:输出任意一个字符,与getchar对应
	putchar(ch);
	//puts(字符数组):将一个字符串（以 '\0' 结束的字符序列）输出到终端，其作用和 printf 输出字符串一致，故较少使用。
	

	/*文件输入
	//open-Mode有r-只读，w-只写，a-增补，"r+"-打开一个文件读/写，"w+"-创建一个文件读/写，"b"-二进制文件，"t"-默认文本文件
	*/
	FILE * fin; 
	fin = fopen("E:\\input.txt","rt"); 
	if (fin ==NULL)
	{
		printf("%s","file open failed!");
		return;
	}
	//fscanf(FILE *fp, char *format,arg_list),类似scanf
	fscanf(fin,"%d%f",&i,&f);
	//int fgetc(FILE *fp)
	ch = fgetc(fin);  //读取末尾的换行符
	printf("%c",ch);
	/*char *fgets(char *str, int num, FILE *fp):读一行不大于num-1的字符串，读取到换行符时不会将其省略
	//出现换行字符、读到文件尾或是已读了size-1个字符则停止读取，最后会加上NULL作为字符串结束。
	//若成功则返回s指针，返回NULL则表示有错误发生
	*/
	//while ((ch=fgetc(fin))!=EOF)  //逐字符直接读到文件末尾
	//	printf("%c",ch);//也可以用putchar(ch);
	char pstr_fgets_20[20];
	fgets(pstr_fgets_20,20,fin); //读入第2行 inputtest，含换行符
	char pstr_fgets_30[30];
	fgets(pstr_fgets_30,30,fin); //读入第3行 testing input by daniel，含换行符
	//size_t fread(void * ptr,size_t size,size_t nmemb,FILE * stream);从文件流中读取数据到ptr。一般不用
	//如下方式是在文本文件中读取不到int数的，一般按行读取，再解析出整数,读取整数用fscanf(fin,"%d",&i)
	//fread(&i,sizeof(int),1,fin);
	//int array[20]={0};
	//fread(array,sizeof(int),20,fin);
	fclose(fin);

	/*文件输出
	//跟输入对应
	*/
	FILE * fout;
	fout = fopen("E:\\output.txt","wt");
	if (!fout)
	{
		printf("%s","file open failed!");
		return;
	}
	//fprintf(FILE *fp, char *format,arg_list),类似printf
	fprintf(fout,"%d %f",i,f);
	//fputc(char ch,file *fp)
	fputc(ch,fout); //输入之前读取的换行符
	//int fputs(char *str, file *fp): 将str写入fp,fputs与puts的不同之处是fputs在打印时并不添加换行符。
	//若成功则返回写出的字符个数，返回EOF则表示有错误发生。
	fputs(pstr_fgets_20,fout); //pstr_fgets_20里自带换行符
	fputs(pstr_fgets_30,fout);
	//fputs("Hello World!",fout);
	//fputs("\n",fout);
	//size_t fwrite(const void * ptr,size_t size,size_t nmemb,FILE * stream); 一般也不用
	fclose(fout);

	/*文件输入输出重定向 
	//FILE * freopen(const char * path,const char * mode,FILE * stream); 
	//将原stream所打开的文件流关闭，然后打开参数path的文件
	*/
	freopen( "E:\\input.txt", "r", stdin);
	freopen( "E:\\output2.txt", "w+", stdout);
	//while(scanf("%c",&ch) == 1)
		//printf("%c",ch);
	while((ch=getchar())!=EOF)
		printf("%c",ch);
	
	
	/*其他：
	//int fseek (FILE *stream, long offset, int fromwhere);
	//fseek()函数的作用是将文件的位置指针设置到从fromwhere开始的第offset字节的位置上
	//fromwhere取值如下
	//SEEK_SET          0                从文件开头
	//SEEK_CUR         1                从文件指针的现行位置
	//SEEK_END          2               从文件末尾
	*/
	
}

void Cplusplus_InputOutput()
{
	//参考资料：http://blog.csdn.net/oniy_/article/details/39828247
	//       & http://www.cnblogs.com/findumars/p/5620538.html
	/*标准输入
	//cin>>XXX;
	//char ch;ch=cin.get()或cin.get(ch)用来接收任意字符; cin.get()常用于舍弃输入流中不需要的字符或者回车。
	//或者cin.get(字符数组名, 接收字符数目n)用来接收一行n-1的字符串,补'\0',可以接收空格，最后回车留在输入流中
	//cin.getline(ptr,n); //实际上有三个参数，cin.getline(字符数组,接受个数,结束字符)。当第三个参数省略时，系统默认为'\0'。
	//兼容C：gets(ptr),getchar()
	//getline(cin,str) //接受一个字符串，可以接收空格并输出，需包含“#include<string>” 不包括每行的换行符
	*/

	/*标准输出
	//格式控制：基数、宽域、小数位数、左对齐、浮点数科学计数法、重设输出格式状态
	//cout<<setbase(10)<<setw(4)<<setprecision(2)<<setiosflags(ios::left)<<setiosflags(ios::scientific)<<resetioflags();
	//也可以是函数调用形式cout.width(4);
	*/


	/*文件输入 fstream, ifstream
	//while(!cin.eof()) //判断是否是文件末尾 
	//fstream fs("E:\\input.txt",ios::in||ios::out);
	//二进制文件操作fs.read(char *buffer,int len);fs.write(const char * buffer,int len)
	ifstream fin;
	fin.open(fname,ios::in);
	fin.getline(ptr,n);
	fin.get(…);
	fin.close();
	*/

	/*文件输出 fstream, ofstream
	//
	ofstream fout;
	fout.open("E:\\output.txt",ios::out);//或者ofstream fout1("E:\\input.txt",ios::out)
	if (!fout)
	{
	cout<<"Open Failed!"<<endl;
	}
	fout<<"XX"; //类比cout使用
	fout.flush();//文件内容刷新
	fout.close();

	*/
	
}

void TypesTransform()
{
	//兼容C：(T)element 或者 T（element）
	//C++四种强制类型转换
	/************************************************************************/
	/*static_cast<T*>      (expression)                                     */
	/*http://www.cnblogs.com/QG-whz/p/4509710.html                          */
	/************************************************************************/
	//编译器隐式执行的任何类型转换都可以由static_cast来完成，比如int与float、double与char、enum与int之间的转换等。
	double a = 1.999;
	int b = static_cast<int>(a); //相当于a = b ;

	//使用static_cast可以找回存放在void*指针中的值。
	double a = 1.999;
	void * vptr = & a;
	double * dptr = static_cast<double*>(vptr);
	cout<<*dptr<<endl;//输出1.999

	//static_cast也可以用在于基类与派生类指针或引用类型之间的转换。然而它不做运行时的检查，不如dynamic_cast安全。
	//static_cast仅仅是依靠类型转换语句中提供的信息来进行转换，而dynamic_cast则会遍历整个类继承体系进行类型检查,
	//因此dynamic_cast在执行效率上比static_cast要差一些。
	//基类指针转为派生类指针,且该基类指针指向基类对象。
	ANIMAL * ani1 = new ANIMAL ;
	DOG * dog1 = static_cast<DOG*>(ani1);
	//dog1->OutPuttype();//错误，在ANIMAL类型指针不能调用方法OutPutType（）；在运行时出现错误。

	//基类指针转为派生类指针，且该基类指针指向派生类对象
	ANIMAL * ani3 = new DOG;
	DOG* dog3 = static_cast<DOG*>(ani3);
	dog3->OutPutname(); //正确

	//子类指针转为派生类指针
	DOG *dog2= new DOG;
	ANIMAL *ani2 = static_cast<DOG*>(dog2);
	ani2->OutPutname(); //正确，结果输出为大黄

	/************************************************************************/
	/*const_cast<T*>       (expression)                                     */
	/*http://www.cnblogs.com/QG-whz/p/4513136.html                          */
	/************************************************************************/
	//用来添加或删除表达式/变量的const性质。
	//主要用于函数参数传递
	const int constant = 21;
	int * num = const_cast<int*>(&constant);
	//对声明为const的变量来说，常量就是常量，任你各种转化，常量的值就是不会变。这是C++的一个承诺。
	const int constant = 26;
	const int* const_p = &constant;
	int* modifier = const_cast<int*>(const_p);
	*modifier = 3;
	cout<< "constant:  "<<constant<<endl; //仍为26,但地址一样，*modifier属于未定义行为，随编译器而异，原因不明
	cout<<"*modifier:  "<<*modifier<<endl;
	system("pause");
	//如果我们定义了一个非const的变量，却使用了一个指向const值的指针来指向它
	//int constant = 26;
	//const int* const_p = &constant;
	//int* modifier = const_cast<int*>(const_p);
	//*modifier = 3;
	//cout<< "constant:  "<<constant<<endl;  //3
	//cout<<"*modifier:  "<<*modifier<<endl;

	/************************************************************************/
	/*dynamic_cast<T*>     (expression)                                     */
	/http://www.cnblogs.com/QG-whz/p/4517336.html                           */
	/************************************************************************/
	//dynamic_cast提供RTTI（Run-Time Type Information），也就是运行时类型识别，使用dynamic_cast转换的Base类至少带有一个虚函数
	//用于类继承层次间的指针或引用转换，安全地向下转型，即基类对象的指针或引用转换为同一继承层次的其他指针或引用
	//对于“向下转型”有两种情况。一种是基类指针所指对象是派生类类型的，这种转换是安全的；
	//另一种是基类指针所指对象为基类类型，在这种情况下dynamic_cast在运行时做检查，转换失败，返回结果为0；
	//引用转换类似，但并不存在空引用，所以引用的dynamic_cast检测失败时会抛出一个bad_cast异常
	ANIMAL * ani_d = new ANIMAL ;
	DOG * dog_d = dynamic_cast<DOG*>(ani_d); //检查转换失败

	/************************************************************************/
	/*reinterpret_cast<T*> (expression)                                     */
	/************************************************************************/
	//以转化任何内置的数据类型为其他任何的数据类型，也可以转化任何指针类型为其他的类型。它甚至可以转化内置的数据类型为指针，无须考虑类型安全或者常量的情形。
	//，T必须是一个指针、引用、算术类型、指向函数的指针或指向一个类成员的指针。
	//比如能够用于诸如char* 到 int*，或者One_class* 到 Unrelated_class*等类似这样的转换，因此可能是不安全的。
	//一般不到万不得已不要使用。

}
```
