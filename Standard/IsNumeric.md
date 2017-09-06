## 问题描述
表示数值的字符串：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

## 解法
思路1：关键在于数字格式分析，其次代码结构分析与重用，最后注意传参时需要按址传参(使用char **);具体参考代码及其注释。
时间复杂度为O(n)，n为字符串长度

思路2：根据表示数值的字符串遵循的规则来判断：在数值之前可能有一个“+”或“-”，接下来是0到9的数位表示数值的整数部分，如果数值是一个小数，那么小数点后面可能会有若干个0到9的数位表示数值的小数部分。如果用科学计数法表示，接下来是一个‘e’或者‘E’，以及紧跟着一个整数（可以有正负号）表示指数。通过变量标记是否存在整数部分、小数部分、指数部分及各自的数量。

思路3：直接利用字符串类的正则表达式匹配

## C++代码
思路1：
```
class Solution {
public:
    bool isNumeric(char* str) //A[.B][e|EC]或者.B[e|Ec],A&C都可能以+/-开头的整数数位串，B只能是0~9间的数位串
    {
        bool numeric = false;
        if(str == nullptr)
            return false;
        //判断A是否是以+/-开头的整数数位串，A也可能省略
        numeric = scanInteger(&str);
        if(*str=='.'){ //如果出现小数点，判断B是否是0~9间的数位串
            str++;
            numeric = scanUnsignedInteger(&str)||numeric; //考虑到A可以省略，故而是||
        }
        if(*str=='E'||*str=='e'){ //如果出现e|E，判断C是否是以+/-开头的整数数位串
            str++;
            numeric = numeric&&scanInteger(&str);
        }
        //若此时以致字符串末尾，则返回numeric，否则返回false
        if(*str == '\0')
            return numeric;
        else
            return false;
    }
    //判断是否是以+/-开头的整数数位串
    bool scanInteger(char** str)
    {
        if(**str == '+' || **str == '-')
            (*str)++;
        return scanUnsignedInteger(str);
    }
    ////判断是否是0~9的数位串
    bool scanUnsignedInteger(char** str)
    {
        char * before = *str;
        while(**str!='\0' && **str>='0' && **str<='9')
            (*str)++;
        return *str>before;
    }
};
```
思路2：根据表示数值的字符串遵循的规则来判断，当代码不进行模块化并重用时的代码实现
```
/*
根据表示数值的字符串遵循的规则来判断：在数值之前可能有一个“+”或“-”，接下来是0到9的数位表示数值的整数部分，如果数值是一个小数，那么小数点后面可能会有若干个0到9的数位表示数值的小数部分。如果用科学计数法表示，接下来是一个‘e’或者‘E’，以及紧跟着一个整数（可以有正负号）表示指数。通过变量标记是否存在整数部分、小数部分、指数部分及各自的数量。
*/
class Solution {
public:
    bool isNumeric(char* string)
    {
        if(string==NULL)
            return false;
        if(*string=='+'||*string=='-')
            string++;
        if(*string=='\0')
            return false;
        int dot=0,num=0,nume=0;//分别用来标记小数点、整数部分和e指数是否存在
        while(*string!='\0'){
            if(*string>='0'&&*string<='9')
            {  
                string++;
                num=1;
            }
            else if(*string=='.'){
                if(dot>0||nume>0)
                    return false;
                string++;
                dot=1;
            }
            else if(*string=='e'||*string=='E')
                {
                  if(num==0||nume>0)
                      return false;
                  string++;
                  nume++;
                  if(*string=='+'||*string=='-')
                      string++;
                 if(*string=='\0')
                     return false;
                }
            else
                return false;
        }
        return true;
    }

```
思路3：直接利用字符串类的正则表达式匹配
```
public class Solution {
    public bool isNumeric(char[] str) {
        String string = String.valueOf(str);
        return string.matches("[\\+-]?[0-9]*(\\.[0-9]*)?([eE][\\+-]?[0-9]+)?");
    }
}
```