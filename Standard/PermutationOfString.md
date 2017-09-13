## 问题描述
字符串的排列：输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

## 解法
递归求解：1）把字符串两部分，一部分是字符串的第一个字符，另一部分是第一个字符之后的所有字符，这后一部分是n-1的子问题；2）拿第一个字符和它后面的字符逐个比对，如果不相同则交换。时间复杂度为O(n!)

拓展：假如是要求字符串的所有组合。
思路1：同样地，我们使用递归求解长度为m(m>1)的所有组合：1）把字符串两部分，第一个字符和其余字符；2）如果组合中包含第一个字符，则下一步在剩余的字符中选取m-1个字符；若不包含，则在剩余的字符中选取m个字符。
思路2：假如只能遍历字符串一遍，则可采用位图思想

## C++代码
**字符串的排列**
```
void Perm(string str, int begin, int end, vector<string>& vec);、、

vector<string> Permutation(string str) 
{
        
    if (str == "")
        return vector<string>();
    vector<string> vec;
    Perm(str, 0, str.length(), vec);
    sort(vec.begin(),vec.end());
    return vec;
}
void swap(char& a, char& b)
{
    char temp = a;
    a = b;
    b = temp;
}
void Perm(string str, int begin, int end, vector<string>& vec)
{
    if (begin == end) 
    {   //如果要计算全排列个数，则在此设立全局变量或局部静态变量统计       
        vec.push_back(str);
    }
    else{
        for (int i = begin; i < end; ++i)
        {
            if(i!=begin&&str[begin]==str[i])
                continue;
            swap(str[begin], str[i]);
            Perm(str, begin + 1, end, vec);
            //记得归位
            swap(str[begin], str[i]);
        }
    }
}
//另一种实现：以字符指针传参
void Permutation(char* pStr, char* pBegin，vector<string>& vec)  
{  
    //输入判断，略  
    if(*pBegin == '\0')
    {
        vec.push_back(pStr);     
        //printf("%s\n",pStr);  
    }
    else  
    {  
        for(char* pCh = pBegin; *pCh != '\0'; pCh++)  
        {  
            if(pCh!=pBegin && *pCh==*pBegin)
                continue;
            swap(*pBegin,*pCh);  
            Permutation(pStr, pBegin+1);  
            swap(*pBegin,*pCh);  
        }  
    }  
}  


```
**拓展：字符串的组合**
```
//在字符串str中得到长度为m的组合result_m, 并保存到results
void Combination(char * str,int m,vector<char> result_m,vector<vector<char>> results)；

vector<vector<char>> CombinationOfString(string str)
{
    if (str=="")
        return vector<string>();
    vector<vector<char>> results;
    vector<char> result_m;
    int n = str.length();//设字符串长度为n
    for(int m=1;m<=n;++m) 
    {
        //获取长度为m的组合
        Combination((char*)str.c_str(),m,result_m,results);
    }
    return results;
}
//在字符串str中得到长度为m的组合result_m, 并保存到results
void Combination(char * str,int m,vector<char> result_m,vector<vector<char>> results)
{
    string tmp="";
    if (m==0)
    {
        results.push_back(result_m);
        /*for (vector<char>::iterator iter=result_m.begin();iter!=result_m.end();++iter)
        {
            cout<<*iter;
        }
        printf("\n");*/
        return;
    }
    if (*str=='\0')
    {
        return;
    }
    result_m.push_back(*str);
    Combination(str+1,m-1,result_m,results);
    result_m.pop_back();
    Combination(str+1,m,result_m,results);
}
```
**字符串的组合-思路2：只遍历一次数组**
vector<string> CombinationOfString2(vector<string> data)
{
    if(data.size()==0) 
        return vector<string>();
    int len = 1<<data.size();
    vector<string> res(len);
    for(int i = 0; i<data.size();i++)
        for(int j=1;j<len;j++){
            if(j&(1<<i))
                res[j] = res[j]+data[i];
        }
        
    return res;
}
