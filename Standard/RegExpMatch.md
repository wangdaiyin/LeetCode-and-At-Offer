## 问题描述
正则表达式匹配：请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

输入：字符串str, 字符串pattern
输出：True  or  false

## 解法
递归，情况要考虑全面
当pattern下一个字符是*时，进行如下判断：
* 当两个字符串的当前字符匹配(ch或.)，移到下一步或跳过(RegExp往后移动两个字符）或者保持原状(RegExp往后移动两个字符）；
* 当两个字符串的当前字符不匹配(ch或.)，跳过(RegExp往后移动两个字符）
当pattern下一个字符不是*时，若两个字符串的当前字符匹配，则跳过(RegExp往后移动两个字符）
否则为false

时间复杂度为O(n)

## C++代码
```
bool match(char* str, char* pattern)
    {
        if(str==nullptr||pattern==nullptr)
            return false;
        
        //直到当*str == '\0' && *pattern == '\0'时返回true
        if(*str == '\0' && *pattern == '\0')
            return true;
        //匹配完后pattern有剩余字符
        if(*str != '\0' && *pattern == '\0')
            return false;
        
        if(*(pattern+1)=='*'){
            if(*str == *pattern || (*str!='\0'&&*pattern == '.'))
                return match(str+1,pattern)||match(str+1,pattern+2)||match(str,pattern+2);
            else
                return match(str,pattern+2);
        }
        if(*str == *pattern || (*str!='\0'&&*pattern == '.'))
            return match(str+1,pattern+1);
        
        return false;
    }
```