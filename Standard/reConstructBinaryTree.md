## 问题描述
重建二叉树:输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

## 解法
1.前序遍历序列的第一个数为当前根节点，head->val为该数；
2.在中序遍历中找到当前根节点，统计当前根节点左边的序列(即左子树的中序遍历)长度len；当前根节点右边的序列(即右子树的中序遍历) 
3.根据左子树长度在前序遍历中数出当前根节点后len个数的序列，即左子树的前序遍历；剩下的序列即右子树的前序遍历
4.对左子树(不为空的情况下)和右子树(不为空的情况下)两个子问题递归求解，head->left和head->right分别为该左子树和右子树问题返回的根节点；递归终止条件为子问题中只剩一个数，返回两个指针为空的叶子节点。
递归公式类似快速排序，最坏情况下，时间复杂度为O(n^2), 平均时间复杂度为O(nlogn)。

## C++代码
```
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.empty()||vin.empty())
            return nullptr;
        int pre_left = 0;
        int pre_right = pre.size()-1;
        int vin_left = 0;
        int vin_right = vin.size()-1;
        //原问题
        return reConstructBT(pre,pre_left,pre_right,vin,vin_left,vin_right);
    }
    TreeNode* reConstructBT(vector<int> pre,int pre_left,int pre_right,vector<int> vin,int vin_left,int vin_right){
        TreeNode* res = new TreeNode(0);
        res->val = pre[pre_left];
        res->left=res->right=nullptr;
        //递归终止条件
        if(pre_left==pre_right){
            return res;
        }
        //步骤2，通过len_left计算左右子树各自的序列范围
        int len_left = 0;
        for(int i = vin_left;i<=vin_right;i++)
        {
            if(vin[i]==pre[pre_left])
                break;
            len_left++;
        }
        //注意判断左/右子树是否为空
        if(len_left>0) 
            //左子树子问题
            res->left = reConstructBT(pre,pre_left+1,pre_left+len_left,vin,vin_left,vin_left+len_left-1);
        if(len_left<pre_right-pre_left)
            //右子树子问题
            res->right = reConstructBT(pre,pre_left+len_left+1,pre_right,vin,vin_left+len_left+1,vin_right);
        return res;
    }
```