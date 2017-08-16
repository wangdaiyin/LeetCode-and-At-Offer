## 问题描述
替换空格：请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

## 解法
思路1：遍历字符串，每次碰到空格时进行替换，空格后的所有字符都后移2字节，时间复杂度为O(n^2)
思路2：先统计空格个数num，再申请O(n+2*num)的额外空间，也可以声明vector动态数组；遍历字符串，当是空格时替换成%20push，若不是则保持原有字符push。时间复杂度为O(n)。
思路3：先统计空格个数num，转换后的长度应该为strlen(str)+2*num（该长度得小于字符数组总长度length）；声明两个指针，一个指向原有长度字符串的末尾，一个指向新长度字符串的末尾，依次往前遍历进行字符移动/替换。时间复杂度为O(n)，不需要额外空间
*题目简单，以下只实现思路3的代码。*

## C++代码
```
//思路3
void replaceSpace(char *str,int length) {
        if(str==nullptr)
            return;
        int count=0;
        for(int i =0;i<strlen(str);i++)
        {
            if(str[i]==' ')
                count++;
        }
        int new_length = strlen(str)+2*count;
        if(new_length>length)
            return;
        char *p1 = &(str[strlen(str)]); //'\0'开始
        char *p2 = &(str[new_length]);
        for(int i =strlen(str);i>=0;i--)
        {
            if(*p1==' '){
                p2 = p2-3;
                *(p2+1)='%';
                *(p2+2)='2';
                *(p2+3)='0';
            }
            else{
                *p2 = *p1;
                p2=p2-1;
            }
            p1--;
        }
    }

```