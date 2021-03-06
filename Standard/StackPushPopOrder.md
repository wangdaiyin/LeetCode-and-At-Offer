## 问题描述
栈的入栈出栈：输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

## 解法
时间复杂度为O(n)，需要额外空间(栈)O(n)
比较容易，直接看代码，考察知识点：栈

## C++代码
```
bool IsPopOrder(vector<int> pushV,vector<int> popV) {
    bool result = false;
    if (pushV.empty()||popV.empty())
    {
        return result;
    }
    stack<int> dataStack;
    vector<int>::iterator iter = popV.begin();
    for (int i = 0; i<pushV.size();i++)
    {
        //将入栈序列依次入栈
        dataStack.push(pushV[i]);
        //当栈中最顶元素等于出栈序列里的指定首位数
        while (!dataStack.empty() && dataStack.top()==*iter)
        {
            dataStack.pop();
            iter++;
        }    
    }

    //如果出栈队列全部匹配完，则为true，否则为false
    result = dataStack.empty();
    return result;
}
```