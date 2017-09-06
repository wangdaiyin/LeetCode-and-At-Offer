## 问题描述
二叉搜索树转双向链表：输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

## 解法
借助中序遍历和转换：递归、保存已转换链表的最后一个结点进行左子树、根、右子树间链表连接
时间复杂度为O(n)

## C++代码
```
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};*/
class Solution {
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        //保存已转成链表后的最后一个节点，用于左子树、根、右子树的连接
        TreeNode* pLastNodeinList = nullptr;
        if(pRootOfTree==nullptr)
            return nullptr;
        Convert2List(pRootOfTree,&pLastNodeinList);
        //从末尾节点得到起始节点
        TreeNode* pHead = pLastNodeinList;
        while(pHead!=nullptr&&pHead->left!=nullptr)
            pHead = pHead->left;
        return pHead;
    }
    void Convert2List(TreeNode* pRootOfTree,TreeNode** pLastNodeinList)
    {
        if(pRootOfTree==nullptr)
            return;
        //当前结点
        TreeNode* pCurrent = pRootOfTree;
        //遍历和转换左子树
        if(pCurrent->left!=nullptr)
            Convert2List(pCurrent->left,pLastNodeinList);
        //转换当前结点：连接左子树最后一个结点
        pCurrent->left = *pLastNodeinList;
        if(*pLastNodeinList!=nullptr)
            (*pLastNodeinList)->right = pCurrent;
        //更新已转换链表的最后一个结点
        *pLastNodeinList = pCurrent;
        //遍历和转换右子树
        if(pCurrent->right!=nullptr)
            Convert2List(pCurrent->right,pLastNodeinList);
    }
};
```