## 问题描述
BFS:从上往下打印出二叉树的每个节点，同层节点从左至右打印。

## 解法
利用队列实现宽度优先搜索遍历。
时间复杂度为O(n),最多额外利用O(n)的辅助队列

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
    vector<int> PrintFromTopToBottom(TreeNode* pRoot) {
        vector<int> res;
        if(pRoot==nullptr)
            return res;
        deque<TreeNode *> dequeTreeNode;
        dequeTreeNode.push_back(pRoot);
        while(!dequeTreeNode.empty())
        {
            TreeNode * pNode = dequeTreeNode.front();
            dequeTreeNode.pop_front();
            //cout<<pNode->val;
            res.push_back(pNode->val);
            if(pNode->left!=nullptr)
                dequeTreeNode.push_back(pNode->left);
            if(pNode->right!=nullptr)
                dequeTreeNode.push_back(pNode->right);
        }
        return res;
    }
};
```