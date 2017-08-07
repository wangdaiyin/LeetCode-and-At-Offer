# 阿里巴巴算法工程师2017年校招在线编程题

## 问题描述
小明春节期间的相亲不幸以失败告终，体贴的主管安排了和另一个部门的联谊。根据每个人的“相亲意愿指数”，小明的部门派出m位男同学，另一个部门派出n位女同学。原计划将每对男女组成一个小组进行游戏，但由于各部门情况不一样，不能做到m和n完全相等，所以允许出现1对多的分组（1男多女或1女多男），甚至允许某个（可以不止1个）同学参与到最多2个不同的小组。同时分组也需要参考相亲意愿指数，以男同学为例，若男同学A比男同学B指数更高，则与A成组的任意一个女同学的指数都不能低于与B成组的任意一个女同学，反之亦然。此外，根据我们的大数据分析，可以计算出任意一对男女同学的匹配度。请你根据这些信息，对参加活动的男女同学进行分组，当然不能有任何一个人落单哦。要求在满足规则的前提下，最大化所有分组匹配度的和。（若1男和1女成组，该分组匹配度即该男女同学的匹配度；若1男和2女成组，则该分组的匹配度为该男同学和2位女同学匹配度的和）

时间限制: 3S (C/C++以外的语言为: 5 S)   内存限制: 128M (C/C++以外的语言为: 640 M)

**输入**:
m n 男同学相亲指数A[m]，1行m列，空格分隔 女同学相亲指数B[n]，1行n列，空格分隔 匹配度C[m][n]，m行n列，空格分隔

**输出**:
m行，第i行对应着第i个男生匹配的到的女同学（们）的下标（从0开始），多个下标之间用空格分隔

**输入范例:**

	2

	3

	2 3

	3 2 1

	2 4 1

	3 2 3


**输出范例:**
    1 2

	0 1

## 解法
这道题由二分图匹配问题中经典的稳定婚姻问题[后附]变动而来，但条件变量变得更多更复杂了，不过还是可以类比稳定婚姻问题加以思考和解决。算法思路如下：
1.首先对男生和女生依次进行相亲意愿指数排序得到两个新数组_A_New和_B_New；
2.依次调整原男女生匹配度的二维数组，填充进排好序的新匹配度二维数组_C_New里；
3.动态规划：i+1个男生和j+1个女生匹配的最大匹配度DP[i][j] = max(DP[i-1][j], DP[i][j-1]) + _C_New[i][j], i>0,j>0,即max(i-1个男生和j个女生匹配的最大匹配度, i-1个男生和j个女生匹配的最大匹配度)加上第i个男生和第j个女生的匹配度. 若i=0或j=0，则DP[i][j]即为1男和所有女生或者1女和所有男生配对后的和。 进一步说，相亲指数排名最低的是_A_New[i],_B_New[j]，若要满足题目条件，则必须有_A_New[i]和_B_New[j]匹配，然后才能是排名更高的i-1个男生和j个女生匹配 或者 i个男生和排名更高的j-1个女生匹配。
4.通过标记DP[i][j]取最大值时i,j的取值逆推回去得到对应排序后男生女生索引的分组；
5.根据排序前后数组索引的对应关系得到原男生女生索引的分组，再按原索引排序输出。

根据输入范例我画了一个草图，如下：
![思路草图]:https://github.com/wangdaiyin/LeetCode-and-At-Offer/blob/master/Standard/Alibaba_Test.jpg

*说明*：这里尽管想到了一种解决方案，但对这道题目中有一个条件【允许出现1对多的分组（1男多女或1女多男），甚至允许某个（可以不止1个）同学参与到最多2个不同的小组】，我的理解是介于可能存在如m少于n一半的情况，应该是容许一对多匹配而不是一个人最多与两个人匹配，而条件里说的最多2组就不明白是什么意思，考虑时忽略掉了；不知道理解的对不对，抑或说题目本身设定存在问题。按这个考虑，输入范例里可能是会存在草稿里所写的两种最大分配结果的，但如果说一个人最多与两人匹配，可以只得到输出范例的结果，但推导中像1男3女的情况便会出错，这个限定是矛盾的。

*多说几句*：之前我没有接触过二分图匹配也没看到过稳定婚姻问题等类似的题，开始拿到这道题时，想着是组合优化问题，考虑过贪心策略（如在满足约束的情况下，每次取最大匹配数的对，直到所有人都已配对）、动态规划（两个变量[男生数和女生数]间是否可由其子问题计算而来，当时没想清楚递推式，感觉约束复杂，有的一点思路也觉得缺少理论上的支撑）、回溯法（类比于在所有满足约束的分组匹配的搜索空间中寻求最大化等，但短时间内思虑一番，脑子都是一片浆糊……直到后来看到二分图匹配中的稳定婚姻问题，算是有了类比的方向……知识覆盖面的重要性！

*最后，如果有大神有其他解法也欢迎与我(kongxin15wdy@163.com)交流或指教！*

## C++代码
**编译器版本**: gcc 4.8.4
请使用标准输入输出(stdin，stdout)；请把所有程序写在一个文件里，勿使用已禁用图形、文件、网络、系统相关的头文件和操作，如sys/stat.h , unistd.h , curl/curl.h , process.h 
```
/** 请完成下面这个函数，实现题目要求的功能 **/
/** 当然，你也可以不按照这个模板来作答，完全按照自己的想法来 ^-^  **/
/** 由于以上思路跟题目约束还存在一些矛盾点，解题思路已在上面详细地进行了说明，而且就算这里代码实现了也无法进行测验，这里暂且不作代码实现。如果以后有机会接触到阿里出题的人或者知道此题解法的大神，确定思路无误后再行代码实现。**/
vector < string > match(int m, int n, vector < double > A, vector < double > B, vector < vector < double > > C) {

}

int main() {
	vector < string > res;

	int _m;
	cin >> _m;

	int _n;
	cin >> _n;

	int _A_size = _m;
	vector<double> _A;
	double _A_item;
	for(int _A_i=0; _A_i<_A_size; _A_i++) {
		cin >> _A_item;
		_A.push_back(_A_item);
	}
	int _B_size = _n;
	vector<double> _B;
	double _B_item;
	for(int _B_i=0; _B_i<_B_size; _B_i++) {
		cin >> _B_item;
		_B.push_back(_B_item);
	}
	int _C_rows = _m;
	int _C_cols = _n;
	vector< vector < double > > _C(_C_rows);
	for(int _C_i=0; _C_i<_C_rows; _C_i++) {
		for(int _C_j=0; _C_j<_C_cols; _C_j++) {
			double _C_tmp;
			cin >> _C_tmp;
			_C[_C_i].push_back(_C_tmp);
		}
	}


	res = match(_m, _n, _A, _B, _C);
	for(int res_i=0; res_i < res.size(); res_i++) {
		cout << res[res_i] << endl;;
	}

	return 0;

}
```
## Python代码(Python2.7)
```
#这里暂时没作实现，思路请参考C++版
#!/bin/python
import sys
import os
/** 请完成下面这个函数，实现题目要求的功能 **/
 /** 当然，你也可以不按照这个模板来作答，完全按照自己的想法来 ^-^  **/

def  match(m, n, A, B, C):

_m = int(raw_input())

_n = int(raw_input())

_A_cnt = _m
_A_i=0
_A = []
while _A_i < _A_cnt:
    _A_item = float(raw_input())
    _A.append(_A_item)
    _A_i+=1

_B_cnt = _n
_B_i=0
_B = []
while _B_i < _B_cnt:
    _B_item = float(raw_input())
    _B.append(_B_item)
    _B_i+=1

_C_rows = _m
_C_cols = _n

_C = []
for _C_i in xrange(_C_rows):
    _C_temp = map(float,raw_input().strip().split(' '))
    _C.append(_C_temp)

  
res = match(_m, _n, _A, _B, _C)

for res_cur in res:
    print str(res_cur)

```
## 附：稳定婚姻问题
### 问题描述
有N男N女，每个人都按照他对异性的喜欢程度排名。现在需要写出一个算法安排这N个男的、N个女的结婚，要求两个人的婚姻应该是稳定的。何为稳定？有两对夫妻M1 F2，M2 F1。M1心目中更喜欢F1，且F1更喜欢M1，显然这样的婚姻是不稳定的，随时都可能发生M1和F1私奔的情况。所以在做出匹配选择的时候（也就是结婚的时候），我们需要做出稳定的选择，以防这种情况的发生。
输入：男生女士的总对数N，每个男生对每个女士的喜欢程度int manIntention[maxn][maxn]，每个女士对每个男生的喜欢程度int womanIntention[maxn][maxn]。
输出：稳定搭配后每个男生(/每个女生)对应的结婚对象编号。

### 算法描述
1.先把所有的男生都加到队列里，队列里的就表示当前还单身的男生;
2.每次从队列里拿出一个男生，然后从她最喜欢的女生开始匹配，如果当前的女生尝试追求过，那么就不用追求了，如果当前的女生没有伴侣，那么可以直接匹配上，如果有伴侣，那么就看看当前这个男生和女生之前的伴侣在那个女生中更喜欢谁，如果更喜欢当先的这个男生，那么当前男生就和这个女生匹配，女生之前匹配过的直接变成单身，被扔回队列，否则，继续找下一个女生，直到找到一个能匹配上的为止；
3.重复2，一直到队列为空，即已全部匹配完成。
由于每个男生最多考虑每个女士各一次，且考虑时间为O(1)，故时间复杂度为O(n^2).

### 代码
```
//输入
//const int maxn = 10;          //男生-女生总对数
//int manIntention[maxn][maxn]; //manIntention[i][j]=k表示i男第j个喜欢k女；
//int womanIntention[maxn][maxn]; //womanIntention[i][j]=k表示i女第k个喜欢的是j男,即在i女心目中j男的排名是k；

int* StableMarriage(const int n, int** manIntention, int** womanIntention)
{
    int* next = new int[n];             //next[i]记录编号i的男士将要求婚的女生在其心目中的排名
    //输出
    int* husband = new int[n];  //future_husband[i]记录编号i的女生所匹配的男生
    int* wife = new int[n];     //future_wife[i]记录编号i的男士所匹配的女生
    //算法描述1.
    queue<int> unmarriagedman;          //未订婚的男士队列
    for (int i=0;i<n;++i)
    {
        next[i]=0;                      //从0计数，即排名第一时等于0
        wife[i]=0,husband[i]=0;         //初始没有未婚妻
        unmarriagedman.push(i);         //将编号为0-n-1的男生都放入队列
    }
    //算法描述2&3.
    while (!unmarriagedman.empty())
    {
        int currentman = unmarriagedman.front();  //当前处理/出栈的男生
        unmarriagedman.pop();
        int currentwoman = manIntention[currentman][next[currentman]];  //当前求婚对象
        next[currentman]++;
        if (!husband[currentwoman])
        {
            //让currentman和currentwoman订婚
            wife[currentman]=currentwoman;
            husband[currentwoman]=currentman;
        }
        //如果当前男生在当前女生的心目中的排名高于其未婚夫，则两者订婚，并将未婚夫放入队列，
        else if(womanIntention[currentwoman][currentman]<womanIntention[currentwoman][husband[currentwoman]]);
        {
            wife[husband[currentwoman]]=0;
            unmarriagedman.push(husband[currentwoman]);
            wife[currentman]=currentwoman;
            husband[currentwoman]=currentman;
        }
        //直接被拒，重新入队列，下次再来
        else{
            unmarriagedman.push(currentman);
        }
    }

    return wife;
}

```
