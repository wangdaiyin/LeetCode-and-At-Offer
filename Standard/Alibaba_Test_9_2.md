## 问题描述
2017年阿里巴巴校招算法工程师编程题(二)：
菜鸟仓库是一个很大很神奇的地方，各种琳琅满目的商品整整齐齐地摆放在一排排货架上，通常一种品类(sku)的商品会放置在货架的某一个格子中，格子设有统一的编号，方便工人们拣选。 有一天沐哲去菜鸟仓库参观，无意中发现第1个货架格子编码为1，第2-3个分别为1,2，第4-6个格子分别是1，2，3，第7-10个格子编号分别是1,2,3,4，每个格子编号都是0-9中的一个整数，且相邻格子的编号连在一起有如下规律 1|12|123|1234|...|123456789101112131415|...|123456789101112131415……n 这个仓库存放的商品品类非常丰富，共有1千万多个货架格子。沐哲很好奇，他想快速知道第k个格子编号是多少？
编译器版本: gcc 4.8.4
请使用标准输入输出(stdin，stdout) ；请把所有程序写在一个文件里，勿使用已禁用图形、文件、网络、系统相关的头文件和操作，如sys/stat.h , unistd.h , curl/curl.h , process.h
时间限制: 3S (C/C++以外的语言为: 5 S)   内存限制: 128M (C/C++以外的语言为: 640 M)
输入:
货物的序号K，是一个整数
输出:
货物的编码
输入范例:
10
输出范例:
4

## 解法
1.第一步，确定n处于几位数编号的范围：
* 1位数最多有iNum[1]=9个，编号最多有iSum[1] = iSum[0]+ 1*iNum[1]个，i位数段空间内能容量编号总数目为NumSum[1] = iNum[1]*iSum[0]+1*(1+2+...+iSum[1]) = iNum[1]*iSum[0]+1*(1+iNum[1])*iNum[1]/2; 截止当前编号总数目为nSum = NumSum[1]；
* 2位数最多有iNum[2]=90个,编号最多有iSum[2] = iSum[1]+ 2*iNum[2], NumSum[2] = iNum[2]*iSum[1]+ 2*(1+iNum[2])*iNum[2]/2; 截止当前编号总数目为nSum += NumSum[2]；
* ……；
* i位数最多有iNum[i] = 10^i-10^(i-1)个，编号最多有iSum[i] = iSum[i-1]+i*iNum[i], i位数段空间内能容量编号总数目为numSum[i] = iNum[i]*iSum[1-1] + i*(1+iNum[i])*iNum[i]/2;截止当前编号总数目为nSum += NumSum[i]；
不断增加i，当n<=nSum时,跳出循环，此时，记录i；
最终，可以得到n处于i位数所表达编号的范围，更新nRes为在该编号范围内的剩余货仓数；

2.第二步，确定n在该范围内具体哪一段货架格子：
该范围内一共有iNum[i]段货架格子，第j段货架格子可表达的编号有ijSum = iSum[i-1]+i*j个，截止第j段货架格子的编号总数目ijNumSum += ijSum; //(初始为0）,即等于j*iSum[i-1] + i*(1+j)*j/2;  1<=j<=iNum[i]
不断增加j，当nRes<=ijNumSum时，跳出循环，此时，记录j；
最终，可以得到处于第j段的范围，更新nRes为在该范围内的剩余货仓数；

3.第三步，确定n处于第j段内哪一个数字里：resNum
先确定处于该范围内用几位数表达的编号空间：为k位数；
再确定处于用k位数表达的编号空间的第nRes个编号,即处于第(nRes+k-1)/k个数：resNum = pow(10,k-1)-1+(nRes+k-1)/k;

4.第四步，确定n在resNum数中的的货仓编号
n的货舱编号为nRes第(nRes+k-1)%k位数：resDigit位数,以0起算，接着获取最终的结果x = nRes中第resDigit位数

最坏时间复杂度为O(logn)


## C++代码
```
#include <stdio.h>  
#include <math.h>  
#include <stdlib.h>  

int Get(int n){
    int x;
    // do something
    int x = 0;
    // do something
    //第一步，确定处于几位数编号的范围
    vector<int> iNum(10,0); //i位数的数目最多为iNum[i],1千万多个货架格子，最后能到达的位数不会超过9。
    vector<int> iSum(10,0); //i位数可表达时编号最多为iSum[i]
    vector<long> numSum(10,0); //i位数段空间内能容量编号总数目
    int i=1,nSum=0; //i位数， 截止当前编号总数目为nSum;
    while (n>nSum)
    {
        iNum[i] = pow(10,i)-pow(10,i-1);
        iSum[i] = iSum[i-1]+i*iNum[i];
        numSum[i] = iNum[i]*iSum[1-1] + i*(1+iNum[i])*iNum[i]/2;
        nSum += numSum[i];
        i++;
    } //n处于i位数所表达编号的范围
    i--; 
    nSum -= numSum[i];
    int nRes = n - nSum; //更新nRes为在该编号范围内的剩余货仓数，

    //第二步，确定在该范围内具体哪一段货架格子
    //该范围内一共有iNum[i]段货架格子,第j段货架格子可表达的编号有ijSum = iSum[i-1]+i*j个，
    //截止第j段货架格子的编号总数目ijNumSum += j*iSum[i-1] + i*(1+j)*j/2;  1<=j<=iNum[i]
    int j=1,ijSum,ijNumSum=0;  
    while (nRes>ijNumSum)
    {
        ijSum = iSum[i-1]+i*j;
        ijNumSum += ijSum; //即等于j*iSum[i-1] + i*(1+j)*j/2;
        j++;
    } //确定在该范围内第j段货架格子
    j--;
    ijNumSum -= ijSum;
    nRes -= ijNumSum;  //在第j段货架格子中第nRes个货仓

    //第三步，确定n处于第j段内哪一个数字：resNum
    int k = 1; //处在第j段内k位数的编号空间，先假设为1
    while (nRes > iSum[k])  ////确定处于该范围内用几位数表达的编号空间：为k位数
    {
        k++;
    }
    nRes -= iSum[k-1]; //处于用k位数表达的编号空间的第nRes个编号,即处于第(nRes+k-1)/k个数,设为resNum
    int resNum = pow(10,k-1)-1+(nRes+k-1)/k;
    
    //第四步，确定n在resNum数中的货仓编号
    int resDigit = (nRes+k-1)%k; //n的货舱编号为resNum第resDigit位数,以0起算
    while (resDigit<k)
    {
        x = resNum%10;
        resNum = resNum/10;
        resDigit++;
    }
    return x;
}

int main()  
{  
    int n;
    scanf("%d",&n);
    int r = Get(n);
        
    printf("%d\n",r); 
    return 0; 
}
```