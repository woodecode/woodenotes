# MATLAB

## 课程导入

```matlab
eg:

求解 x^2-3x+1=0

1.
>>p=[1,-3,1];	//建立多项式系数向量
>>x=roots(p)	//多项式求根函数
x=
	2.6180
	0.3820

2.
//利用fzero()函数
>>f=@(x) x*x-3*x+1;
>>x1=fzero(f,0.5)
x1=
	 0.3820
>>x2=fzero(f,2.5)
x2=
	 2.6180

```



## 基础知识

### 系统环境

认识用户界面区！

```matlab
命令行 命令

cd x:\xxx //设置当前文件夹、跳转到相应文件夹 （该文件夹必须已经存在！

clear //清除工作区的全部变量


... //(三个小数点)续行符 类似Python中的 \ 续行符
```

工作区用于存储各种变量和结果

搜索顺序：变量 -> 函数 -> 当前文件夹下的程序文件 -> 特定的文件搜索路径

也就是对于同名的变量和函数，使用时会识别为变量而不是函数 ，文件亦然

```matlab
//设置文件搜索路径
path(path,'x:\xx')
//主页->搜索路径 也可以
```



### 数值数据

数据类型有

`整型` 无符号和带符号

```matlab
uint8(x) //将x转为无符号8位整型 最大255
int8(x)  //将x转为带符号8位整型 最大127

//因为int用最高位表示正(0)负(1)，所以范围比uint小
```

`浮点 `  单精度(4字节)(single) 	双精度(8字节)(default)

```matlab
```

 

`复数` 

虚部用 **i 或 j** 表示，实部和虚部都为双精度浮点数

```matlab
real()	//求复数实部
imag()	//求复数虚部
```



常用

```matlab
format 函数设置数据输出格式

format long	//输出格式设置为Long格式
format  	//回到默认格式
```



### 变量及其操作



### 矩阵表示



### 矩阵元素引用



### 基本运算



### 字符串处理



## 矩阵处理



## 流程控制



## 绘图



## 数值分析与多项式计算



## 数值微积分与方程求解



## 符号计算



## 图形用户界面设计



## Simulink系统仿真



## 外部程序接口

