---
title: hihoCoder笔记(持续更新)
date: 2017-04-09 15:05:28
tags:
---
# #1497:Queen Attack
- 时间限制:10000ms  
- 单点时限:1000ms  
- 内存限制:256MB

## 描述
- There are N queens in an infinite chessboard. We say two queens may attack each other if they are in the same vertical line, horizontal line or diagonal line even if there are other queens sitting between them.
- Now given the positions of the queens, find out how many pairs may attack each other?

## 输入
* The first line contains an integer N.
* Then N lines follow. Each line contains 2 integers Ri and Ci indicating there is a queen in the Ri-th row and Ci-th column.  
* No two queens share the same position.  
* For 80% of the data, 1 <= N <= 1000
* For 100% of the data, 1 <= N <= 100000, 0 <= Ri, Ci <= 1000000000

## 输出
* One integer, the number of pairs may attack each other.

### 样例输入
> 5  
> 1 1  
> 2 2  
> 3 3  
> 1 3  
> 3 1

### 样例输出
> 10

## 思路
一开始采用最简单粗暴的算法，两个循环一个个的判断这个queen之间是不是会有冲突，但是写完之后提交就发现*TLE*超时了，一开始以为是编程语言的关系（因为用的是Python写的），但是后来看了别人的思路之后才发现并不是这个原因，算法思路才是关键。思路大概是计算有多少个queen在相同的行，有多少在相同的列，有多少在相同的主对角线，有多少在相同的次对角线，然后用排列组合公式 $C_n^2=\frac{n\times(n-1)}{2}$ 来计算配对数。下面是我用Python写的代码，*AC*。

## 代码
 ``` python
number = int(raw_input())
queens = []
count = 0
#行列对角线的索引字典
row = dict()
col = dict()
right = dict()
left = dict()
#建立索引，统计每行、列、对角线上相冲突的queen的个数
def find_all(data_dict, data):
    if data_dict.get(data) != None:
        data_dict[data] += 1
    else:
        data_dict[data] =1
        
for i in range(number):
    x, y = [int(x) for x in raw_input().split()]
    queens.append((x,y))
    find_all(row, queens[i][0])
    find_all(col, queens[i][1])
    find_all(right, queens[i][0]+queens[i][1])
    find_all(left, queens[i][1]-queens[i][0])

row_num = sum([value*(value-1)/2 for value in row.itervalues()])
col_num = sum([value*(value-1)/2 for value in col.itervalues()])
right_num = sum([value*(value-1)/2 for value in right.itervalues()])
left_num = sum([value*(value-1)/2 for value in left.itervalues()])
#因为题目中说了不会有位置重叠的queen，所以只需要把计数加起来就行了
count = row_num + col_num + right_num + left_num
print count
 ```


# #1498:Diligent Robots
- 时间限制:10000ms  
- 单点时限:1000ms  
- 内存限制:256MB

##	 描述
- There are N jobs to be finished. It takes a robot 1 hour to finish one job.
- At the beginning you have only one robot. Luckily a robot may build more robots identical to itself. It takes a robot Q hours to build another robot.  
- So what is the minimum number of hours to finish N jobs?
- Note two or more robots working on the same job or building the same robot won't accelerate the progress.

## 输入
- The first line contains 2 integers, N and Q.  
- For 70% of the data, 1 <= N <= 1000000  
- For 100% of the data, 1 <= N <= 1000000000000, 1 <= Q <= 1000

## 输出
- The minimum number of hours.

### 样例输入
> 10 1

### 样例输出
> 5

## 思路
这道题一开始想的很复杂，我不光考虑了（1）所有机器人都不复制，都干活的决策，和（2）所有机器人都复制，不干活的决策，还考虑了（3）有部分机器人干活，部分机器人复制的决策，担心第三种决策会使得总时间最少，如同下图中红线所示，在某个总任务数（如水平虚线所示），红线所代表的第三种情况用时最少（红线先于其他两条直线交于虚线）。但是我推导了一遍发现不可能出现这种情况，第三种情况的线应该是与其他两条黑线交于一点（如蓝线所示）。
![diligent-robots](http://7xl454.com1.z0.glb.clouddn.com/diligent-robots.png)

### 推导过程
假设中认为为N，机器人复制需要q小时，在时间T1后，有机器人m个，第一种情况是不选择复制，让全部机器人干活，在时间T2之后的剩余时间为：$$h_1=\frac{N-qm}{m}$$
第二种情况为机器人全都复制，复制完成再干活，T2后剩余时间为：$$h_2=\frac{N}{2m}$$
第三种情况为x个机器人复制，(m-x)个机器人干活，剩余时间为：$$h_3=\frac{N-q(m-x)}{m+x}=\frac{N-qm+qx}{m+x}$$
我们可以看出来第一种情况就是第三种情况x=0的特例，第二种情况就是x=m的特例。我们可以把$h_3$看成x的函数，分析它的单调性，设(N-qm)=a,q=b,m=c,则$h_3$对x求导可得：$$\begin{equation}\begin{split}\frac{d}{dx}h_3&=\frac{d}{dx}\big(\frac{a+bx}{c+x}\big)=\frac{b(c+x)-(a+bx)}{(c+x)^2}\\\\
&=\frac{bc-a}{(c+x)^2}=\frac{2qm-N}{(m+x)^2}\end{split}\end{equation}$$
导数的符号取决于(2qm-N)的符号，而q,m,N都是定值，导数的符号与x无关，所以$h_3$是单调的，我们可以得知要么第一种情况用时少，要么第二种情况用时少，不可能会出现第三种情况(0<x<m)用时少，所以可以推测上图中蓝线与另外两条黑线交于同一点。除此之外，如果在一段时间内复制的次数一定，那一定是先复制完再干活是最快的，所以我们需要讨论的情况只限于我们复制多少次再干活。看到别人有用二分法的去搜索完成N个任务需要多久成功*AC*。下面是他人用C++完成的算法。

## 代码
 ```c++
#include <vector>
#include <string>
#include <iostream>
#include <algorithm>
#include <sstream>
#include <map>
using namespace std;
//用来判断能否再limitTime里完成n个任务，复制所需要q个小时
bool ok(long long n, long long limitTime, long long q) {
    long long count = (limitTime / q);
    //由于题中所说任务数最大是1000000000000个
    //所以如果能复制log2(1000000000000)~=40次，那么一定可以完成
    //原作者可能数错了0的个数，但是不影响结果
    if(count > 45) return true;
    for(long long int i=count; i>=0; i--) {
    		//1LL代表long long类型的1，<<运算符为位左移，相当于乘2^i
        long long machines = (1LL << i);
        //判断是否能完成
        if((limitTime - i * q) * machines >= n) return true;
    }
    return false;
}

int main() {
    long long int n, q;
    while (cin >> n >> q) {
        long long l = 0, h = n;
        while(l + 1< h) {
        		//二分法搜索
            long long mid = ((l + h) >> 1);
            if(ok(n, mid, q)) {
                h = mid;
            } else {
                l = mid;
            }
        }
        cout << h << endl;
    }
}
 ```

