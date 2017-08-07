## 问题描述
从字符串中找出一个最长的不包含重复字符的子字符串，并输出该最长子字符串的长度。ASCII码不同的字符即定义为不重复字符。为简单起见，也可假设字符串中只包含'a'-'z'的字符。

## 解法
思路1：找出字符串的所有子字符串(n^2)并判断是否包含重复的字符(n)，时间复杂度为O(n^3)。
思路2：动态规划，类似最大字段和，用f[i]表示以第i个字符结尾的子字符串的最大长度，如果第i个字符与前f[i-1]中的字符重复,则f[i]=min(第i个字符与前一个重复字符间的长度d,f[i-1]+1),否则f[i]=f[i-1]+1.最坏时间复杂度为O(n^2)，需要O(n)的额外空间时间复杂度为O(n)

## C++代码
```
void FindLongestNotRepeatedSubstring(char* pstr) 
{
    //输入判断
    if(pstr == nullptr){
        return;
    }
    int length = strlen(pstr);
    int * f = new int[length]();
    f[0]=1;
    int maxSubstrLength = 0;
    int maxSubstr_end = 0; 
    for(int i=1;i<length;i++)
    {
        //如果第i个字符与前f[i-1]中的字符重复,且第i个字符与前一个重复字符间的长度dis<f[i-1]+1，则f[i]=dis,否则f[i]=f[i-1]+1
        int dis=1;
        //遍历前f[i-1]中的字符
        for (int j=i-1;j>i-1-f[i-1];j--){
            if (pstr[j]!=pstr[i]){
                dis++;
            }
            else{
                break;
            }
        }
        if (dis<f[i-1]+1){
            f[i]=dis;
        }
        else{
            f[i]=f[i-1]+1;
        }
        if(maxSubstrLength<f[i]){ //取到f[i]中的最大值，即最长不重复子字符串长度，在这里可以记录最长子字符串的最后位置
            maxSubstrLength=f[i];
            maxSubstr_end=i;
        }

    }
    //输出结果
    printf("%d:",maxSubstrLength);
    for (int i=maxSubstr_end-maxSubstrLength+1;i<=maxSubstr_end;i++)
    {
        printf("%c",pstr[i]);
    }

    delete [] f;
    return;
}

```