# C++![](https://ae01.alicdn.com/kf/H8b34d2e2be98499e9320b7cc640496b07.png)

[c++]: https://www.icourse163.org/course/PKU-1002029030	"程序设计与算法（三）C++面向对象程序设计"

## C到C++

### 引用

#### 引用

定义:`类型名 & 引用名 = 某变量名;`

```c
int n = 4;
int & r = n;//r引用了n，r的类型是int &
```

上式表示`r`引用了`n`，引用后**r等价于n**，r相当于n的一个别名。

> 定义引用时**一定要初始化**成引用某个变量

> 初始化后，它就**一直引用该变量**，不会再引用别的变量

> 引用**只能引用变量**，不能引用常量和表达式

```c
int a = 4;
int &r1 = a;//r1引用a
int &r2 = r1;//r2也引用a
```

引用可以代替指针达到**更加简洁易懂**的效果。下面分别用指针和引用实现交换两变量

```c
//使用<指针>交换两变量
void swap(int *a,int *b){
    int tmp;
    tmp = *a;*a = *b;*b = tmp;
}
int n1,n2;
swap(&n1,&n2);
//使用<引用>交换两变量
void swap(int &a,int &b){
	int tmp;
    tmp=a;a=b;b=tmp;
}
int n1,n2;
swap(n1,n2);
```

引用可以直接作为函数的返回值，这在实际使用中十分常见

```c
int n=4;
int & setValue(){
    return n;
}
int main(){
    setValue()=40;//这等价于对n赋值为40
    cout << n;//output:40
	return 0;
}
```

#### 常引用

在定义引用前加`const`关键字，即构成常引用。不能通过常引用改变其引用的内容。

```c
int n=3;
const int &r = n;// r引用n
r = 26;//通过r对n进行赋值会导致编译出错
n = 8；//直接对n赋值没有问题，因为n是int类型
```

#### 常引用和非常引用的转换

`const T &`和`T &`是不同的类型.

`T &`类型的引用或`T`类型的变量可以用来初始化`const T &`类型的引用。

> 这里应该有一个解释

`const T`类型的常变量和`const T &`类型的引用则不能用来初始化`T &`类型的引用，除非进行强制类型转换

> 这里应该有一个解释

### const 关键字

#### 定义常量

`const`关键字的基本用法是定义一个常量。即定义之后不能改变其内容。

```c
const double Pi = 3.14;
```

#### 常量指针

在定义指针前面加上const关键字即为定义了一个常量指针。**不能通过该常量指针修改**其指向的内容。但是不代表指向的内容不能被修改。常量指针的**指向可以被修改**.

```c
int n,m;
const int *p = &n;
*p = 5;//编译出错compile error
n = 4;//ok
p = &m;//ok,常量指针的指向可以被修改.
```

不能把常量指针赋值给非常量指针,反过来可以.

```c
const int *p1 ;
int *p2;
p1 = p2;//ok
p2 = p1; //error
p2 = (int*) p1; //ok,强制类型转换
```

```c
此处应该有解释
```

...

### 动态内存分配

c语言中有`malloc`库函数来实现,在C++中使用`new`运算符来实现动态内存分配.

#### 分配变量

![image-20220129121748602](image\image-20220129121748602.png)

```c
```

#### 分配数组

![image-20220129121836205](image\image-20220129121836205.png)

![image-20220129121956535](image\image-20220129121956535.png)

...

```c
```

![image-20220129122047147](image\image-20220129122047147.png)

#### 释放分配空间

用`new`动态分配的内存空间,要用`delete`运算符释放.

使用`delete`释放的指针**必须**指向`new`出来的空间.

一片分配的空间**只能被delete一次**.

```c++
int *p = new int;
*p = 5;
delete p;
```

使用``delete`释放动态分配的数组时,要使用以下形式

```c++
delete []p;//p是指针
```

该指针也**必须**指向`new`出来的空间(数组).

```c++
int *p = new int[20];
p[0] = 1;
delete []p;//释放空间
```

如果释放时没有加`[]`，编译和运行都不会有什么问题，但是可能会导致不能完全回收该空间。

...

### 内联函数和重载函数

#### 内联函数

​	函数调用是有时间开销的。如果函数本身只有几条语句，执行非常快，而且函数被 反复执行很多次，相比之下调用函数所产 生的这个开销就会显得比较大。 

​	为了减少函数调用的开销，引入了内联函数机制。编译器处理对内联函数的调用语句时，是将整个函数的代码插入到调用语句处，而不会产生调用函数的语句。

​	函数定义前面加`inline`关键字，即可定义内联函数。

```c++
inline int max(int a,int b){
	if(a > b){
        return a;
    }
    return b;
}
```

当运行下面这条语句时：

```c++
k = max(n1,n2);
```

编译器**大概**会将其编译成下面这样。

```c++
if(n1 > n2){
    tmp = n1;
}else{
    tmp = n2;
}
k = tmp;
```

#### 重载函数

​	一个或多个函数，名字相同但参数个数或参数类型不同，这叫做函数的重载。 

```c++
// 以下三个函数是重载关系
int Max(double f1 ,double f2) { }
int Max(int n1,int n2) { }
int Max(int n1,int n2,int n3) { }
```

​	函数重载使得函数命名变得简单。编译器根据调用语句的中的实参的个数和类型判断应该调用哪个函数.

#### 函数的缺省参数

缺省值即为默认值。c++中，定义函数的时候可以让**最右边的连续若干个参数**有缺省值，那么调用函数的时候，若相应位置不写参数，参数就是缺省值。

```c++
void func(int x1,int x2=2,int x3=3){}

func(10); // 等效于 func(10,2,3)
func(10,8); // 等效于 func(10,8,3)
func(10,,8); // 不行，只能最右边的连续若干个参数缺省
```

## 类和对象

...

#### 对象的内存分配

​	和结构变量一样，对象所占用的内存空间的大小，等于所有**成员变量**的大小之和。 对于上面的CRectangle类，sizeof (CRectangIe) =  8 每个对象各有自己的存储空间。一个对象的某个成员变量被改变了，不会影响到另一个对象。

#### 对象间的运算

​	和结构变量一样，对象之间可以用"="进行赋值，但是不能用 "="、"!="、">="、"<="进行比较，除非这些运算符经过了"**重载**"。 

#### 类成员的可访问范围

在类的定义中，用下列访问范围关键字来说明类成员 可被访问的范围：

如果某个成员没有下面的关键字，默认认为是**私有成员** 

```c++
class className(){
    public:// 公有成员，可以在任何地方访问
    private:// 私有成员，只能在成员函数内访问
    protected:// 保护成员，以后再说(待定...)
};
```

...

```c++
// 类定义
class className {
    public:
    	int fun1();
};
// 成员函数
int className::fun1(){
    /***函数体***/
}
```

#### 成员函数的重载及参数缺省

成员函数可以重载...

成员函数可以设置缺省参数...

#### 构造函数

​	构造函数是成员函数的一种，**名字与类名相同**，可以有参数，**不能有返回值(void也不行)** 

​	作用是对对象**进行初始化**，如给成员变量赋初值。

​	构造函数**可以重载** 

```c++
className::className(){
	...
}
```

> ​	如果定义类时没写构造函数，则编译器生成一个默认的无参数的构造函数。默认构造函数无参数，不做任何操作。

例程:

```c++
// 定义
class complex{
    private:
    	double real;
    	double imag;
    public:
    	complex(double r,double i);
};
complex::complex(double r,double i){
    real = r;imag = i;
}
// 使用时
complex z1(1,0);
complex *z3 = new complex(3,4);// 动态分配空间
```

#### 复制构造函数

...

[程序设计与算法（三）C++面向对象程序设计_中国大学MOOC(慕课) (icourse163.org)](https://www.icourse163.org/learn/PKU-1002029030?tid=1450432459#/learn/content?type=detail&id=1214925553&cid=1219120301&replay=true) 

```c++
class className{
    public:
    	className(className &);
	    className(const className &);
};
className::className(className &){
    ...
}
className::className(const className &){
    ...
}
    
```

...



#### 类型转换构造函数

...

#### 析构函数

​	析构函数名字与类名相同，在前面加 `~` ，没有参数和返回值，一个类最多只能有一个析构函数。

​	和构造函数类似，析构函数对象消亡时即**自动被调用**。

​	一般析构函数用来在对象消亡前做善后工作，比如释放分配的空间等。

```c++
class className{
    public:
    	className();// 构造函数
    	~className();// 析构函数
};
className::~className(){
    ...
}
```

如果定义类时没写析构函数，则编译器生成块省析构函数。 缺省析构函数什么也不做。 

```c++
class className{};
className *p = new className[3];//构造函数会调用三次
delete []p;// 析构函数会调用三次
```
​	*：上面的delete语句若不使用`[]`，只会调用一次析构函数，即只会析构对象数组的第一项。

...

#### 构造函数析构函数调用时机

...

#### C++程序到C程序的翻译

下面是一段简单的C++代码片段，我们将尝试将其翻译成C程序。

```c++
calss CCar{
    public:
    	int price;
    	void setPrice(int p);
};
void CCar::setPrice(int p){
	price = p;
}
int main(){
	CCar car;
    car.setPrice(2000);
    return 0;
}
```

​	改成C程序后，我们将CCar的成员变量用**结构**的方式表达，成员函数改成**全局函数**。car.setPrice()语句是对car对象进行setPrice操作，在C中为了明确setPrice函数操作的对象，会将car的地址作为参数传到该函数的内部。上面的这段C++程序就会被翻译成下面这样。

> 翻译后的函数参数会比c++程序写出来的参数个数多一个。

```c
struct CCar{
    int price;
};
void setPrice(struct CCar *this,int p){
	this->price = p;
}
int main(){
  	struct CCar car;
    setPrice(&car,20000);
    return 0;
}
```

...

#### this指针

[程序设计与算法（三）C++面向对象程序设计_中国大学MOOC(慕课) (icourse163.org)](https://www.icourse163.org/learn/PKU-1002029030?tid=1450432459#/learn/content?type=detail&id=1214925558&cid=1219120305&replay=true) 

this指针的作用就是指向成员函数所作用的**对象**。**非静态成员函数**中可以直接使用this来代表指向该函数作用的对象的指针。

#### this指针和静态成员函数

**静态成员函数**中不能使用使用this指针！

因为静态成员函数并不具体作用在某个对象！

因此，静态成员函数的真实的参数的个数，就是程序中写出的参数个数！

...

#### 静态成员变量/函数

...

在说明前面加了`static`关键字的成员就属于静态成员。

```c++
class rectangle{
    private:
    	int w,h;
    	static int totalArea;// 静态成员变量
	    static int totalNumber;
    public:
    	rectangle(int w,int h);
		~rectangle();
    	static void printTotal();// 静态成员函数
}
```

普通成员变量每个对象有各自的一份，而静态成员变量一共就一份，**为所有对象共享**。

静态成员变量**不会**被sizeof()计算在内。静态成员**不需要通过对象**就能访问。

```c++
class rect{
	int n;
    static int s;
}
sizeof(rect); //结果为4
```

静态成员变量本质上**是全局变量**，哪怕一个对象都不存在，类的静态成员变量也存在。

静态成员函数本质上**是全局函数**。

设置静态成员这种机制的目的是将和某些类紧密相关的全局变量和函数写到类里面，看上去像一个整体，易于维护和理解。

>  	**必须**在定义类的文件中对静态成员变局进行一次说明或**初始化**。否则编译能通过，链接不能通过。

```c++
int rectangle::totalArea = 0;
int rectangle::totalNumber = 0;
```

...

调用静态成员函数,可以使用`类名::函数`，也可以使用`某一对象.函数`，两者没有区别。

```c++
rectangle::printTotal();
r1.printTotal();
```

需要注意的是，静态成员函数中不能访问非静态成员变量，也不能调用非静态成员函数。

> 静态成员属于类，也不属于类

...

##### 🚨

:仅在构造函数和析构函数中操作静态成员变量可能会有一些缺点。详情看下面链接，视频后半部分

[程序设计与算法（三）C++面向对象程序设计_中国大学MOOC(慕课) (icourse163.org)](https://www.icourse163.org/learn/PKU-1002029030?tid=1450432459#/learn/content?type=detail&id=1214925559&cid=1219120306) 

...

#### 成员对象和封闭类

有成员对象的类叫封闭类。即某一类的成员是其他类的对象。

##### 🚨

...

##### 封闭类构造函数和析构函数的执行顺序 

**构造**：封闭类对象生成时，先执行所有对象成员的构造函数，然后才执行封闭类的构造函数。

**调用次序**：对象成员的构造函数调用次序和对象成员在类中的说明次序一致  ,与它聒区成员初始化列表中出现的次序无关。

**析构**：当封闭类的对象消亡时，先执行封闭类的析构函数，然后再执行  成员对象的析构函数。次序和构造函数的调用次序相反。

...

##### 🚨

...

#### 常量对象、常量成员函数

...

##### 🚨

在类的成员函数说明后面可以加`const`关键字，则该成员函数成为常量成员的数。

常量成员函数执行期间不应修改其所作用的对象。因此，在常量成员函数中不能修改成员变量的值(静态成员变量除外)，也不能调用同类的非常量成员函数(静态成员函数除外)。

...

**常量成员函数的重载** 

两个成员函数，名字和参数表都一样，但是一个是const, 一个不是。这也算重载。

```c++
class test{
  public:
    void fun(){}
    void fun() const{}
};
```



...

#### 友元(friends)

友元分为`友元函数`和`友元类`两种。

⚠️友元类之间的关系不能传递，不能继承。

##### 友元函数

一个类的友元函数可以访问该类的私有成员。

...

##### 友元类

如果A是B的友元类，那么A的成员函数可以访问B的私有成员。

...

## 运算符重载


​	当有两个复数对象`complex_a`、`complex_b`。我们希望可以实现两个复数的相加运算。直接使用`complex_a  + complex_b`是不被允许的，因为"+"只适用于基本数据类型。虽然可以使用函数来实现这一功能，但是显然没有直接写a+b实现来得简洁方便。**使用运算符重载来扩展C++提供的运算符的适用范围使之可以作用于对象**。运算符重载**本质上依然是函数**。

```c++
返回值类型 operator 运算符 (形参表){
    
}
```

示例：

```c++
class Complex{
	public:
    	double real,imag;// 复数实虚部
    	Complex(double r=0.0,double i=0.0):real(),imag(){}// 构造函数 	
    	Complex operator-(const Complex &c);
};
// 写在类内部
Complex Complex::operator-(const Complex &c){
	return Complex(real-c.real,imag-c.imag);// 返回临时对象
}
// 写在外部
Complex opreator+(const Complex &a,const Complex &b){
    return Complex(a.real+b.real,a.imag+b.imag);// 返回临时对象
}

// 重载为成员函数时，参数个数为运算符数减一;
// 重载为普通函数时，参数个数为运算符数;

int main(){
	Complex a(4,4),b(1,1),c;
    c = a + b;// 等价于c=operator+(a,b);
    c = a - b; // a-b等价于a.operator-(b);
    return 0;
}
```

...

### 赋值运算符重载

​	有时候希望赋值运算符两边的类型可以不匹配。 比如，把一个int类型变量赋值给一个Complex对象，  或把一个char *类型的字符串串对象, 此时就需要重载赋值运算符“=” 。

​	**赋值运算符“=”只能重载为成员函数**, 不能重载为全局函数。

```c++
class String{
    private:
    	char *str;
    public:
    	String():str(new char[1]){ 
            //":str(new char[1])"叫做初始化列表，初始化之后才能在函数体内操作"str"。
            str[0]=0;// 将str初始化为一个空字符串
        }
    	const char *c_str(){
            return str;
        }
    	String & operator = (const char *s);// 赋值运算符重载
    	String::~String(){
            delete []str;
        }// 析构函数，释放str空间。
};

// 重载运算符"="使得String类型的对象obj可以执行obj="Hello World"这样的语句。
String & String::operator = (const char *s){
    delete []str;// 释放掉str空间
    str = new char[strlen(s)+1];// 为str分配一个新的位置
    strcpy(str,s);// 把s的内容拷贝到str中。
    return *this;
}

int main(){
    String s;
    s = "Hello World";// 这等价于s.operator=("Hello World");
	return 0;
}
```



...

### 运算符重载为友元

...

### 可变长数组类的实现

...

### 流插入运算符重载

...

### 流提取运算符重载

### 类型转换运算符重载

### 自增减运算符重载

...

## 继承

### 继承和派生

### 继承关系和复合关系

### 覆盖和保护成员

### 派生类的构造函数

### 公有继承的赋值兼容规则

...

## 多态

### 虚函数与多态

### 多态的实现原理

### 虚析构函数

### 纯虚函数

### 抽象类

...

## C11新特性

...

### 强制类型转换

### 异常处理

...
