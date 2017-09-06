## 问题描述
旋转数组的最小数字：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

## 解法
要特别注意特殊输入数据的处理。
类似二分查找，设首尾两个指针控制查找区域。
注意考虑排序数组中数字相同的情况，如0 1 1 1 1。
平均时间复杂度为O(logn)，最坏情况下时间复杂度为O(n)

## C++代码
```
int minNumberInRotateArray(vector<int> rotateArray) {
        //输入判断
        if(rotateArray.empty())
            return 0;
        //首尾两个索引
        int index_first = 0;
        int index_last = rotateArray.size()-1;
        int index_middle = index_first; //初始考虑未翻转时已排序情况，最小值在第一位
        while(rotateArray[index_first]>=rotateArray[index_last]){
            if(index_last-index_first==1){
                index_middle = index_last;
                break;
            }
            index_middle = (index_first+index_last)/2;
            //如果中间值大于首位值，则最小值位于中间值右边，否则位于中间值左边
            if(rotateArray[index_middle]>=rotateArray[index_first])
                index_first = index_middle;
            else if(rotateArray[index_middle]<=rotateArray[index_last])
                index_last = index_middle;
            
        }
        return rotateArray[index_middle];
    }
```