## 问题描述
从尾到头打印链表:输入一个链表，从尾到头打印链表每个节点的值。

## 解法
思路1：每次从头进入寻找，时间复杂度为O(n^2)
思路2：构造反向链表，时间复杂度为O(n)，但需要额外空间
思路3：利用栈辅助输出，依次遍历链表入栈，后进先出，时间复杂度为O(n),也需要辅助栈O(n)额外空间
思路4：利用递归替代栈,时间复杂度为O(n)
*以下仅提供思路4代码*

## C++代码
```
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
    vector<int> res;
    vector<int> printListFromTailToHead(ListNode* head) {
        if(head!=nullptr)
        {
            if(head->next!=nullptr)
                printListFromTailToHead(head->next);
            cout<<head->val;
            res.push_back(head->val);
        }
        return res;
```