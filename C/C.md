### 指针

也称作"指针变量"，大小为4个字节(32位系统)或8个字节(64位系统)。其内容代表一个**内存地址**。

...test

#### 相互赋值

#### 指针运算

#### 作为函数参数

#### 和数组

#### 和二维数组

#### 指向指针的指针

#### 字符串

#### 字符串库函数

#### void指针

#### 内存操作函数

#### 函数指针

...

### 位运算

#### &"与"

通常用来将某变量中的某些位**清零**同时保持其他位不变；

也可以用来**获取某变量中的某一位**。

将

#### |"或"

通常用来将某变量中的某些位**置1**同时保持其他位不变；

#### ^"异或"

通常用来将某变量中的某些位**取反**同时保持其他位不变；

特点: 如果`a^b=c`,那么就有`c^b=a`和 `c^a=b`;此规律**可以用来进行简单的加密和解密**.(a是原文，b是密钥，c密文)

可以不通过临时变量交换两个变量的值

```c
int a = 5, b = 7;
a = a ^ b;
b = b ^ a;
a = a ^ b;
//就完成了a b的交换
```

#### ~"非"

二进制取反

#### 左移<< n

左移n位，高位抛弃，低位补零

实际上，左移1位意味着数据**乘**2，左移n位意味着数据**乘**2^n;

**左移操作比乘法快得多**.

#### 右移>> n

右移n位，低位抛弃，高位符号位是1,右移后高位补1;是0,右移后高位补0;

实际上，左移1位意味着数据**除以**2，左移n位意味着数据**除以**2^n,结果**往小取整**;

### STL(标准模板库)

#### sort排序

```c++
#include <algorithm>

//sort排序
int a[]={15,4,3,9,7,2,6}
sort(a,a+3); //{3,4,15,9,7,2,6}
//对元素类型为T(int,double,...)的基本类型数组从大到小排序
sort(a+1,a+4,greater<int>()); //{15,9,4,3,7,2,6}
//z

```

#### 二分查找

```c
// 二分查找是对排好序的数组进行查找
// 查找范围[n1,n2) n1为0时，+n1可以不写;
// 返回值为true(找到了)false(没找到)

// 基本类型数组的二分查找
// "等于"的含义:a等于b <=> a<b 和 b<a 都不成立
binary_search(数组名+n1,数组名+n2,值);// n1,n2都是int类型的表达式，可以包含变量。

// 在用自定义规则排好序的、元素为任意的T类型的数组中进行二本查找
// "等于"的含义:a等于b <=> "a必须在b前面" 和 "b必须在a前面" 都不成立
binary_search(数组名+n1,数组名+n2,值,排序规则结构名());

```

![image-20220121142029898](C:\Users\Q\AppData\Roaming\Typora\typora-user-images\image-20220121142029898.png)

```c
binary_search();//二分查找
lower_bound();//二分查找下界
upper_bound();//二分查找上界

#include <algorithm>

// 自定义排序规则
struct Rule{
    bool opeartor()(const int &a1,const int &a2)const{
        return a1%10 < a2%10;
    }
}
```

![image-20220121142909244](C:\Users\Q\AppData\Roaming\Typora\typora-user-images\image-20220121142909244.png)

![image-20220121143124076](C:\Users\Q\AppData\Roaming\Typora\typora-user-images\image-20220121143124076.png)

#### 平衡二叉树

##### **STL中的平衡二叉树数据结构** 

> 有时需要在大量增加、删除数据的同时， 还要进行大量数据的查找

> 希望增加数据、删除数据、查找数据都能在**log(n)**复杂度完成

> 排序+二分查找显然不可以，因加入新数据就要重新排序 

>可以使用“平衡二叉树”数据结构存放数据,体现在STL中，就是以下四种“排序容器”： 

>**multiset**    **set**    **multimap**    **map**

...

##### multiset

```c
multiset<T> st; 
//Function
st.insert();//添加元素
st.find();//查找元素
st.erase();//删除元素
//复杂度都是log(n)
```

​	定义了一个multiset变量st, st里面可以存放T类型的数据，**并且能自动排序**。开始st为空。

​	排序规则：表达式`a < b`为**true**, 则a排在b前面. 

##### 迭代器

multiset上的迭代器

```c
miultiset<T>::iterator p;
```

​	p是迭代器，相当于指针，可用于指向multiset中的元素。访问multiset中的元素要通 
过迭代器。
​	**与指针的不同**：
​	multiset上的迭代器可++ , -- ,用 != 和 == 比较, **不可比大小**,**不可加减整数**,**不可相减** 

```c
//multiset 用法
#include <iostream>
#include <cstring>
#include <set> //使用multiset和set需要此头文件
using namespace std;
int main()(
    multiset<int> st;
    int a[10]={1,14,12,13,7,13,21,19,8,8};
    for(int i = 0;i < 10; ++i)
    	st.insert(a[i]); //插入的是a [i]的复制品 
    multiset<int>::iterator i; //迭代器，近似于指针 
    for (i = st.begin();i != st.end(); ++i)
    	cout << *i << endl;
    cout << endl;
}
//输出：1,7,8,8,12,13,13,14,19,21
```



```c
	i = st.lower_bound(13);
	//返回最靠后的迭代器 在，使得[begin(),it)中的元素
	//都在13前面，复杂度log(n)
	cout << *i << endl;
	//1,7,8,8,12,13,13,14,19,21,
	i = st.upper_bound(8);
	//返回最靠前的迭代器it,使得[it,end())中的元素
	//都在8后面，复杂度log(n)
	cout << *i << endl;
	st .erase (i) ; //删除迭代器i指向的元素，即12
	for (i = st.begin();i != st.end() ;++i)
		cout << *i << ",";
	return 0;
}
/*
输出:
13
12
1,7,8,8,13,13,14,19,21,22,
*/
```

...

#### set

#### multimap

#### map

...
