# 算法基础

[程序设计与算法(二) 算法基础_中国大学MOOC (icourse163.org)](https://www.icourse163.org/course/PKU-1001894005) 

这个文档是这个课程的笔记。代码都是手敲，文字部分来自课程PPT，个人理解总结较少

**复习时一定补上！**🥕(挖坑)

...

## 枚举

#### 完美立方

...

#### 生理周期

...

#### 称硬币

...

> ​	有12枚硬币。其中有11枚真币和1枚假币。假币和真币重量不同，但不知道假币比真币轻还是重。现在, 用一架天平称了这些币三次，告诉你称的结果，请你 找出假币并且确定假币是轻是重（数据保证一定能找出来）

> •**输入样例**  注意：天平左右的硬币数总是相等的 
>
> ABCD UFGH even (平)
>
> ABCI EFJK up (右向上)
>
> ABU EFGH even (平) 
>
> •**输出样例** 
>
> K is the counterfeit coin and it is light

...

假设A是轻假币，带入测试...再假设A是重假币，带入测试...

依次测试ABCD EFGH IJKL

```c
#include <iostream>
#include <cstring>
using namespace std;
char left[3][7];//表示天平左边硬币
char right[3][7];//表示天平右边硬币
char result[3][7];//表示称重结果
bool isFake(char c,bool light);
//light为真表示假设假币为轻，否则表示假币为重
int main(){
    int t;
    cin >> t;
    while(t--){
        for(int i=0;i<3;i++){
            cin >>left[i] >> right[i] >> result[i];
        }//将三组测量数据传入数组
        for(char c='A';c<='L';c++){
            //依次测试每个字符
            if(isFake(c,true)){
                cout << c << ",it is light.\n";
                break;
            }else if(isFake(c,false)){
                cout << c << ",it is heavy.\n";
                break;
            }
        }
    }
    return 0;
}
bool isFake(char c,bool light){
    for(int i=0;i<3;i++){
        char *pLeft,*pRight;
        if(light){
            pLeft = left[i];
            pRight = right[i];            
        }else{
            pLeft = right[i];
            pRight = left[i];
        }
        switch(result[i][0]){
            case 'u':s
                if(strchr(pRight,c)==NULL)
                    return false;
                break;
            case 'd':
                if(strchr(pLeft,c)==NULL)
                    return false;
                break;
            case 'e':
                if(strchr(pLeft,c)||strchr(pRight,c))
                    return false;
                break;
        }
    }
    return true;
}
```

...

## 递归

#### 求阶乘

📕:用递归解决多重循环

```c
int Factorial(int n){
    if(n==0){
        return 1;
    }else{
        return n*Factorial(n-1);
    }
}
```

...

#### 汉诺塔

汉诺塔是递归的经典问题...

> ​	古代有一个梵塔，塔内有三个座A、B、C, A座上有64个盘子，盘子大小不等，大的在下，小的在上。有一个和尚想把这64个盘子从A座移到C座，但每次只能允许移动一个盘子，并且在移动过程中，3个座上的盘子  始终保持大盘在下，小盘在上。在移动过程中可以利用B座，要求输出移动的步骤。

<p align="center"><img src="image\hanoi.png" style="zoom: 80%;" /></p>

```c
int main(){
    int n;
    cin >> n;盘子的数目
    Hanoi(n,'A','B','C');//将A上的盘子移到C,B做中转
	return 0;
}
//将src上的n个盘子，以mid为中转，移到dest.
void Hanoi(int n,char src,char mid,char dest){
    if(n==1){//只有一个盘子
        cout << src << "->" << dest << endl;//将一个盘子直接从src移到dest
    	return;
    }
    else{
        Hanoi(n-1,src,dest,mid); //将n-1个盘子以dest为中转，移到mid
        cout << src << "->" << dest << endl; //将一个盘子从src移到dest
        Hanoi(n-1,mid,src,dest);//再将这n-1个盘子以src为中转，移到dest
    	return;
    }
}
```

...

#### N皇后

......

#### 逆波兰表达式求值

📕:用递归解决递归形式的问题

逆波兰表达式的定义： 

1) 一个数是一个逆波兰表达式，值为该数
2) `运算符 逆波兰表达式 逆波兰表达式`是逆波兰表达式，值为两个逆波兰表达式的值运算的结果.

> ​	例题：逆波兰表达式是一种把运算符前置的算术表达式，例如普通6  表达式2 + 3的逆波兰表示法为+ 2 3o逆波兰表达式的优点是运算符之间不必有优先级关系，也不必用括号改变运算次序，例如`(2+3)×4`的逆波兰表示法为* + 2 3 4。本题求解逆波兰表达式的值  其中运算符包括+ - * /四个。 输入 输入为一行，其中运算符和运算数之间都用空格分隔，运算数是浮点数输出，输出为一行，表达式的值。 

...

```c++
#include <cstdlib>
double exp(){
    char s[20];
    cin >> s;
    switch(s[0]){
        case '+':return exp()+exp();
        case '-':return exp()-exp();
        case '*':return exp()*exp();
        case '/':return exp()/exp();
        default: return atof(s);//把一个字符串转换成double类型
        break;
    }
}
int main(){
    pritnf("%lf",exp());
    return 0;
}
```

...

#### 表达式计算

📕:用递归解决递归形式的问题

......

#### 爬楼梯

📕:用递归将问题分解为规模更小的子问题进行求解

> ​	例题:咖哥爬楼梯，他可以每次走1级或者2级，输入楼梯的级教，求**不同的走法数**。
>
> ​	例如：楼梯一共有3级，他可以每次都走一级，或者第一次走一 级，第二次走两级，也可以第一次走两级，第二次走一级，一共3种方法。 
>
> ​	输入包含若干行，每行包含一个正整数N,代表楼梯级数，1 <=  N <= 30输出不同的走法数，每一行输入对应一行输出

...

​	先分析第一步:可以走两级，也可以走一级。如果总共有n级台阶，就是一个f(n)的问题。走完第一步后，剩下的就变成了f(n-1)或f(n-2)的问题。后面就是新的第一步...

​	👉🏻: n级台阶的走法🥢先走一级后，n-1级台阶的走法 ➕ 先走两级后，n-2级台阶的走法

$$
f(n) = f(n-1) + f(n-2)
$$

```c++
#include <iostream>
using namespace std;
int staris(n){
	if(n==1){
        return 1;
    }else if(n==2){
        return 2;
    }else{
        return staris(n-1)+staris(n-2);
    }
}
int main(){
    int N;
    while(cin>>N){
        cout <<staris(N)<< endl;
    }
	return 0;
}
```

...

#### 放苹果

...

[程序设计与算法（二）算法基础_中国大学MOOC(慕课) (icourse163.org)](https://www.icourse163.org/learn/PKU-1001894005?tid=1450413466#/learn/content?type=detail&id=1214952541&cid=1219124594&replay=true) 

...

> ​	把M个同样的苹果放在N个同样的盘子里，允许有的盘子空着不放，问共有多少种不同的分法？ 5, 1, 1和1, 5, 1是同一种分法。

...

设`i`个苹果放在`k`个盘子里的总放法数是`f(i,k)`, 则

`k>i`时，盘子比苹果多，此时至少有`k-i`个盘子是空的0；这样就等价于将`i`个苹果放在`i`个盘子里。即
$$
f(i,k)=f(i,i)
$$
...

`k<=i`时，...。即总放法🥢有盘子为空的放法 ➕没盘子为空的放法。当有盘子为空时，...。**当没盘子为空**时，每个盘子至少有一个苹果。即先将k个苹果放到k个盘子里，再将剩下的k-i苹果分盘。

$$
f(i,k)=f(i,k-1)+f(i-k,k)
$$

...;l

...

```c++
int appleNum(int apple,int plate){
    if(apple == 0){ // 没苹果，所有盘子里的苹果都为0，也是一种结果
        return 1;
    }else if(plate == 0){// 没盘子
		return 0;
    }else if(apple < plate){
        return appleNum(apple,plate-1)+
            	appleNum(apple-plate,plate);
	}else {
        return appleNum(apple,apple);
    }
}
int main(){
    int t,m,n;
    cin >> t;
    while(t--){
        cin >> m >> n;
        cout << appleNum(m,n) << endl;
    }
    return 0;
}
```

...

#### 算24

📕:用递归将问题分解为规模更小的子问题进行求解

​	给出4个小于10个正整数，你可以**使用加减乘除4种运算以及括号把这4个数连接起来得到一个表达式**。现在的问题是，是否存在一种方式使得得到的表达式的结果等于24。 这里加减乘除以及括号的运算结果和运算的优先级跟我们平常的定义一致（这里的除法定义是实数除法）。 

​	比如，对于5, 5, 5, 1,我们知道5 * (5 - 1/5)=24，因此可以得到24。又比如，对于1, 1, 4, 2,我们怎么也不能得到24。

​	输入数据包括多行，每行给出一组测试数据，包括4个小于10个正整数。最后一组测试数据中包括4个0,表示输入的结束，这组数据不用处理。对于每一组测试数据，输出一行，如果可以得到24,输出“YES” ：  否则，输出“N0”。

​	n个数算24，**必须有两个数要先算**。这两个数算的结果，和剩余的n-2个数，就构成了n-1个数求24的问题。

​	**边界条件：算一个数是24**

```c++
#include <iostream>
#include <cmath>
using namespace std;
double a[5];
bool isZero(double x){ //浮点数x小于10^(-6)就认为是0
    return fabs(x) <= 1e-6;
}

bool count24(double a[],int n){
    if(n == 1){
        if(isZero(a[0]-24)){
            return true;
        }else{
            return false;
        }
    }else{
        double b[5];//临时存放数组
        for(int i=0;i<n-1;++i){
            for(int j=i+1;j<n;++j){// 先枚举两个数的组合
                int m = 0;
                for(int k=0;k<n;++k){
                    if(k!=i && k!=j){//把i，j两个数排除
                        b[m++]=a[k];
                    }
                }// ↑把其余的数放到数组b中
                b[m] = a[i]+a[j];
                if(count24(b,m+1)) return true;
                b[m] = a[i]-a[j];
                if(count24(b,m+1)) return true;
                b[m] = a[j]-a[i];
                if(count24(b,m+1)) return true;
                b[m] = a[i]*a[j];
                if(count24(b,m+1)) return true;
                // ↓算除法时要注意除数不能为0
                if(!isZero(a[j])){
                	b[m] = a[i]/a[j];
                    if(count24(b,m+1)) return true;
                }
                if(!isZero(a[i])){
                    b[m] = a[j}/a[i];
                    if(count24(b,m+1)) return true;
                }
            }
        }
    }
}
```

...

## 二分

...

**时间复杂度：** 

[时间复杂度_百度百科](https://baike.baidu.com/item/时间复杂性)

...

#### 二分查找

​	写一个函数BinarySeach,在包含size个元素的、从小到大排序的int数组a里查找元素  P。如果找到，则返回元素下标；如果找不到，则返回-1。

​	*要求复杂度O(log(n))。*

```c++
int BinarySearch(int a[],int size,int p){
    int left = 0;// 数组左端点
    int right = size - 1;// 数组右端点
    while(left <= right){
        int mid = left+(right-left)/2;// 取区间中点的下标
        if(p == a[mid]){
            return mid;
        }else if(p > a[mid]){
            // 如果查找的数大于a[mid]中的元素，则mid左边的元素全部舍弃。
            // 即把数组的左端点left移到a[mid+1]处
            left = mid + 1;
        }else{
            right = mid - 1;
        }
    }
    return -1;
}
```

​	写一个函数LowerBound,在包含size个元素的、从小到大排序的int数组a里查找比给定整数p小的，下标最大的元素。找到则返回其下标：找不到则返回-1。

​	*复杂度O(log(n))*

```c++
int LowerBound(int a[],int size,int p){
    int L = 0;// 区间左端点
    int R = size - 1;// 区间右端点
    int lastPos = -1;// 目前为止找到的最优解
    while(L <= R){
        int mid = L+(R-L)/2;// 查找区间中间元素下标
        if(a[mid] >= p){
			R = mid -1;
        }else{
            lastPos = mid;
            L = mid + 1;
        }
    }
    return lastPos;
}
```

...

#### 求方程的根

...

#### 找一对数

...

#### 农夫和牛奶

...

## 分治

...

#### 归并排序

#### 快速排序

#### 输出前m大的数

#### 求排列的逆序数

## 动态规划

#### 数字三角形

#### 最长上升子序列

#### 最长公共子序列

## 深度优先搜索

#### 在图上寻找路径和遍历

#### 图的表示方法:邻接矩阵和邻接表

#### 城堡问题

#### 踩方格

#### 寻路问题

#### 生日蛋糕

## 广度优先搜索

### 抓住这头牛

### 迷宫问题

### 八数码

## 贪心算法

#### 圣诞老人的蛋糕

#### 电影节

#### 分配畜栏

#### 放置雷达

#### 钓鱼

...

## 测验

#### 待定..

...