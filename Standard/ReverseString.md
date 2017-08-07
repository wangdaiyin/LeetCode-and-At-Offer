## 问题描述
题目一：翻转单词顺序，例“I am a student.”翻转成“student. a am I”
题目二：左旋转字符串，即对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。

## 解法
题目一：先翻转整个句子，再翻转每个单词, 时间复杂度为O(n)。
题目二：[思路拓展]先翻转k位前一部分和剩余后面部分的字符，再翻转整个字符串; 但其实如果容许另外申请O(n)内存，直接用一个新字符串来保存旋转后结果会很简单。时间复杂度为O(n)。

## C++代码
**题目一：翻转单词顺序**
```
string ReverseSentence(string str) {
        //输入检查
        if(str.empty())
            return "";
        //翻转整个句子
        int len = str.length();
        Reverse(str,0,len-1);
        //翻转句子中的每个单词
        int begin=0,end=0;
        while(str[begin]!='\0')
            {
            if(str[begin]==' ')
                {
                begin++;
                end++;
            }
            else if(str[end]==' '||str[end]=='\0')
                {
                Reverse(str,begin,--end);
                begin=++end;
            }
            else end++;
        }
        return str;
    }
    void Reverse(string &str,int begin,int end){
        while(begin < end){
            swap(str[begin++],str[end--]);
        }
    }
    void swap(char& a,char& b)
        {
        char tmp = a;
        a=b;
        b=tmp;
    }
```
**题目二：左旋转字符串**
```
string LeftRotateString(string &str, int n)
    {
        //输入判断
        int nLength = str.size();
        if(nLength<n) 
            return "";
        if(!str.empty() && n <= nLength)
        {
            if(n >= 0 && n <= nLength)
            {
                int pFirstStart = 0;
                int pFirstEnd = n - 1;
                int pSecondStart = n;
                int pSecondEnd = nLength - 1;
 
                // 翻转字符串的前面n个字符
                Reverse(str, pFirstStart, pFirstEnd);
                // 翻转字符串的后面部分
                Reverse(str, pSecondStart, pSecondEnd);
                // 翻转整个字符串
                Reverse(str, pFirstStart, pSecondEnd);
            }
        }
        return str;
    }
```