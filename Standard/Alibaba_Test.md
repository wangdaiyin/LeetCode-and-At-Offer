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
时间复杂度为O()


## C++代码
```
**编译器版本**: gcc 4.8.4
请使用标准输入输出(stdin，stdout)；请把所有程序写在一个文件里，勿使用已禁用图形、文件、网络、系统相关的头文件和操作，如sys/stat.h , unistd.h , curl/curl.h , process.h 
/** 请完成下面这个函数，实现题目要求的功能 **/
/** 当然，你也可以不按照这个模板来作答，完全按照自己的想法来 ^-^  **/
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