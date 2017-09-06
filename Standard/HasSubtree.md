## 问题描述
输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

## 解法
循环或递归，一般采用递归书写更为简便。
1.在树A中查找与根节点的值一样的节点；
2.判断树A中以R为根节点的子树是不是和B具有相同的结构。
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
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if(pRoot1==nullptr || pRoot2==nullptr)
            return false;
        //利用短路特性
        return DoesT1hasT2(pRoot1,pRoot2)||HasSubtree(pRoot1->left,pRoot2)||HasSubtree(pRoot1->right,pRoot2);

    }
    bool DoesT1hasT2(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        //如果Tree2已经遍历完了都能对应的上，返回true
        if(pRoot2==nullptr)
            return true;
        //如果Tree2还没有遍历完，Tree1却遍历完了。返回false
        if(pRoot1==nullptr)
            return false;
        //如果其中有一个点没有对应上，返回false
        if(pRoot1->val!=pRoot2->val)
            return false;
        //如果根节点对应的上，那么就分别去子节点里面匹配
        return DoesT1hasT2(pRoot1->left,pRoot2->left) && DoesT1hasT2(pRoot1->right,pRoot2->right);
    }
};


```