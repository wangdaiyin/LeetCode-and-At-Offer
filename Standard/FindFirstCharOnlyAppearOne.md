## 问题描述
在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置。

## 解法
解法1：穷举法，时间复杂度为O(n^2)
解法2：哈希表(键值对)，时间复杂度为O(n),但需要不多于O(n)的额外空间；由于是字符都不需要用map实现，可以直接用长度为256的数组存储所有的字符(8位)。

## C++代码
```
int FindFirstCharOnlyAppearOne(char* pstr)
{
    if (pstr==nullptr){
        return -1;
    }
    int result=-1;
    int hash[256];
    for (int i=0;i<256;i++){
        hash[i]=0;
    }
    int length = strlen(pstr);
    for (int i=0;i<length;i++){
        hash[pstr[i]]+=1;       
    }
    for (int i=0;i<length;i++){
        if (hash[pstr[i]]==1){
            result = i;
            return result;
        }
    }
    return result;
}
```
## 拓展：返回字符流中第一个不重复的字符

```
class Solution
{
//尽管字符可以用数组实现更简单，但这里为了进一步的拓展性，我仍旧使用map来实现哈希表
    vector<int> vec;
    map<char,int> hashTable;
  //Insert one char from stringstream
    void Insert(char ch)
    {
         vec.push_back(ch);
        hashTable[ch]++;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        for(vector<int>::iterator iter = vec.begin();iter != vec.end(); iter++){
            if(hashTable[*iter] == 1)
                return *iter;
        }
        return '#';
    }
};
```