## 问题描述
二叉树中和为某一值的路径:输入一颗二叉树和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

## 解法
回溯法
1.二叉树遍历与回溯：递归，终止条件：叶节点
2.路径记录：用vector，添加与删除节点值

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
    vector<vector<int> > FindPath(TreeNode* pRoot,int expectNumber) {
        vector<vector<int>> res;
        if(pRoot==nullptr)
            return res;
        vector<int> path;
        int currentSum = 0;
        FindPathDetail(pRoot,expectNumber,path,currentSum,res);
        return res;
    }
    void FindPathDetail(TreeNode* pRoot,int expectNumber,vector<int>& path,int currentSum,vector<vector<int>>& res)
    {
        if(pRoot==nullptr)
            return;
        
        //添加当前结点到path，更新当前和
        path.push_back(pRoot->val);
        currentSum += pRoot->val;

        //当该结点为叶节点时且路径上节点值的和等于属于的值，则输出这条路径
        bool isLeaf = (pRoot->left==nullptr && pRoot->right==nullptr);
        if(isLeaf && currentSum==expectNumber){
            res.push_back(path);
        }

        //当该结点不是叶节点时，则遍历它的子节点
        if(pRoot->left!=nullptr)
            FindPathDetail(pRoot->left,expectNumber,path,currentSum,res);
        if(pRoot->right!=nullptr)
            FindPathDetail(pRoot->right,expectNumber,path,currentSum,res);

        //返回父结点前，在path中删除当前结点
        path.pop_back();
        return;
    }
};
```