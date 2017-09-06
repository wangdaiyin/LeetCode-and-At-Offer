## 问题描述
合并两个排序的链表：输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

## 解法
思路1：递归，时间复杂度为O(n+m)，n,m分别为两个原链表的长度
思路2：非递归, 时间复杂度为O(n+m)

## C++代码
```
**递归版本**
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
            val(x), next(NULL) {
    }
};*/

ListNode* MergeList(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1==nullptr)
            return pHead2;
        else if(pHead2==nullptr)
            return pHead1;
        
        ListNode * res = nullptr;
        if(pHead1->val > pHead2->val){
            res = pHead2;
            res->next = Merge(pHead1,pHead2->next);
        }
        else{
            res = pHead1;
            res->next = Merge(pHead1->next,pHead2);
        }
        return res;
    }

```
**非递归版本**
```
ListNode* MergeList(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1==nullptr)
            return pHead2;
        else if(pHead2==nullptr)
            return pHead1;
        
        ListNode * res = nullptr;
        //取较小值作头结点
        if(pHead1->val<=pHead2->val){
            res=pHead1;
            pHead1=pHead1->next;
        }
        else{
            res=pHead2;
            pHead2=pHead2->next;
        }  
        
        ListNode* pTmp = res; //合并后的链表的工作指针
        while(pHead1!=nullptr && pHead2!=nullptr){ ////当有一个链表到结尾时，循环结束
            if(pHead1->val < pHead2->val){
                pTmp->next = pHead1;
                pHead1 = pHead1->next;
                pTmp = pTmp->next;
            }
            else{
                pTmp->next = pHead2;
                pHead1 = pHead2->next;
                pTmp = pTmp->next;
            }
        }
        //遍历未遍历完的链表中剩余元素
        if(pHead1==nullptr)
            pTmp->next = pHead2;
        if(pHead2==nullptr)
            pTmp->next = pHead1;
        
        return res;
    }
```