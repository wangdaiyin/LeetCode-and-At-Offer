## 问题描述
用两个栈来实现一个队列，完成队列的Push和Pop操作。

## 解法
用栈1作Push栈，用栈2做Pop栈，用模板类和模板函数实现，详情看代码。Push操作时间复杂度为O(1)，Pop操作最坏情况下时间复杂度为O(n)。

延伸：用两个队列实现栈

## C++代码
```
template<typename T> class CQueue
{
private:
    stack<T> stack1;
    stack<T> stack2;

public:
    void push(const T& node);
    T pop();
};

template<typename T>
void CQueue<T>::push(const T& node) {
    stack1.push(node);
}

template<typename T>
T CQueue<T>::pop(){
    //以stack2作为pop栈
        T res;
        if(stack2.empty())
        {
            //if(stack1.empty())
            //    throw new exception("Queue is empty!");
            while(!stack1.empty())
            {
                T tmpData = stack1.top();
                stack1.pop();
                stack2.push(tmpData);
            }
            res = stack2.top();
            stack2.pop();
            return res;
        }
        res = stack2.top();
        stack2.pop();
        return res;
}

```