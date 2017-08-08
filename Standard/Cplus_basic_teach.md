
```
//学生：高一 已有一定JAVA基础
/*课程记录
//2017.08.07 15:40-17:40
//
*/
#include "head.h"; //自定义头文件
//宏定义常量/表达式
#define PI 3.1415926
#define MAX (a>b)?a:b
#define HW "Helloworld"

void firstDemo();
void C_InputOutput();
void Cplusplus_InputOutput()

int main()
{
	//初识c++
	firstDemo();
	
	//C++简单输入输出 
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
	
	//C++强制类型转换static_cast、dynamic_cast、const_cast和reinterpret_cast: http://www.jellythink.com/archives/205
	
	int i2 = static_cast<int> (PI); 
	
	//运算符与表达式：赋值、算术、逻辑、关系、复合、自增自减、位>>/<<、逗号、条件  //优先级   扫过   
	//lesson1 End
	
	//语句结构、变量作用域、全局变量、局部变量
	
	//复合数据类型:数组、类、结构、共用体、枚举……
	
	//函数：定义、调用、函数传参、返回、函数指针
	
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
	fout.open("E:\\input.txt",ios::out);//或者ofstream fout1("E:\\input.txt",ios::out)
	if (!fout)
	{
	cout<<"Open Failed!"<<endl;
	}
	fout<<"XX"; //类比cout使用
	fout.flush();//文件内容刷新
	fout.close();

	*/
	
}
```
