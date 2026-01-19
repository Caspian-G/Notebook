# P1359 游艇租用

## 题目描述

长江游艇俱乐部在长江上设置了 *n* 个游艇出租站 1,2,…,*n*。游客可在这些游艇出租站租用游艇，并在下游的任何一个游艇出租站归还游艇。游艇出租站 *i* 到游艇出租站 *j* 之间的租金为 *r**i*,*j*（1≤*i*<*j*≤*n*）。试设计一个算法，计算出从游艇出租站 1 到游艇出租站 *n* 所需的最少租金。

## 输入格式

第一行中有一个正整数 *n*，表示有 *n* 个游艇出租站。接下来的 *n*−1 行是一个半矩阵 *r**i*,*j*（1≤*i*<*j*≤*n*）。

## 输出格式

输出计算出的从游艇出租站 1 到游艇出租站 *n* 所需的最少租金。

## 输入输出样例

**输入 #1**

```
3
5 15
7
```

**输出 #1**

```
12
```

## 说明/提示

1≤*n*≤200，保证计算过程中任何时刻数值都不超过 106。



## 解

使用DP思想，通过状态方程求解，求解过程中使用顺序递归

另外其实还可使用Dijistra算法求解单元最短路径的思路

```c++
#include <bits/stdc++.h>
using namespace std;
int DP(const vector<vector<int>> &a , int i , vector<int>& r);
int n;

int main(){
    cin >> n;
    //存储数组初始化
    vector<vector<int>> a (n+1, vector<int>(n+1,0));
    vector<int> r (n+1, -1);
    for( int i = 1 ; i < n ; i++ ){
        for ( int j = i+1 ; j <= n ; j++ )
        {
            cin >> a[i][j];
        }
    }
    int result = DP(a,n,r);
    cout << result;
    return 0;
}

int DP(const vector<vector<int>> &a , int i , vector<int> &r){
    if( i == 1 ) r[1] = 0;
    for ( int k = 2; k <= i ; k++ ){
        r[k] = a[1][k];
        for( int j = 2 ; j < k ; j++ )
        {
            if( r[k] > (r[j] + a[j][k] )) r[k] = r[j] + a[j][k];
        }
    }
    return r[i];
}
```
