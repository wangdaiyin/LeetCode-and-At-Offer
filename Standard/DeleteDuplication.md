## 问题描述
删除链表中重复的结点：在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

## 解法
链表的删除，思路要点：前向节点、当前节点、后向节点的变换；时间复杂度为O(n)

## C++代码
```
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
ListNode* deleteDuplication(ListNode* pHead)
    {
        if(pHead==nullptr)
            return nullptr;
        ListNode* pPreNode = nullptr; //记录前向节点
        ListNode* pNode = pHead; //记录当前节点
        ListNode* pNext = nullptr; //记录后向节点
        while(pNode!=nullptr){
            pNext = pNode->next;
            //如果当前节点的值等于其下一个节点的值，则删除这些节点
            if(pNext!=nullptr&&pNext->val==pNode->val){
                ListNode* TobeDeleteNode = pNode;
                int Duplicatevalue = pNode->val;
                while(TobeDeleteNode!=nullptr&&TobeDeleteNode->val==Duplicatevalue){
                    pNext = TobeDeleteNode->next;
                    delete TobeDeleteNode;
                    TobeDeleteNode = nullptr;
                    TobeDeleteNode = pNext;
                }
                //如果当前被删除的结点包括头结点，则前向节点仍旧为空，头结点更换
                if(pNode == pHead){
                    pPreNode = nullptr;
                    pHead = pNext;
                }
                else{//否则，设定前向节点的下一个结点
                    pPreNode->next = pNext;
                }
                pNode = pNext;
            }
            else{//往下一个结点遍历
                pPreNode = pNode;
                pNode = pNode->next;
            }
        }
        return pHead;
    }
```