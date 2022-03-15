```markdown
22-3-15: 定时器进阶添加了能够显示小数点的功能;
```

# -----------⚠️-----------

138选择器

```c
#define Y4C ((P2 & 0x1f) | 0x80);
#define Y5E 0xa0
#define Y6E 0xc0
#define Y7E 0xe0
```

16位定时器,x位微妙(us)

```c
TH0 = (65535-x) / 256;
TL0 = (65535-x) % 256;
```

...

```c
```

...

# ----------🐝-----------

## LED指示灯基本控制

...

> ​	🚨CON3跳接WR和GND，U25D(74HC02或非门)中使**Y4=0**则会有Y4C = 1，使能U6(74HC573锁存器)来控制LED。Y4由U24(138译码器)控制。

```c
#include <STC15F2K60S2.H>
#define Y4C 0x80 //0x80=0b100,00000
unsigned char i;
void main(){
    P2 = Y4C;//使能LED
    //循环以下3次
    for(i=0;i<3;++i){
        P0 = 0X00;//灯全亮
        delay(300);//延时
        P0 = 0xff;//灯全灭
        delay(300);//延时
    }
    //循环以下8次
    for(i=0;i<8;++i){
        P0 = 0xff << i;// 依次点亮每个灯
        delay(300);
    }
}

```

...

## 蜂鸣器和继电器

...

> ​	🚨N_BUZZ (蜂鸣器控制)和N_RELAY (继电器控制)由U10(ULN2003)控制，需要使能Y5C(Y5=0)。**ULN2003是反相驱动，INx和OUTx反相**。
>
> N_BUZZ=1(**P06=0**) -> 蜂鸣器**灭掉**。
>
> N_RELAY=0(**P04=1**)

```c
#include <STC15F2K60S2.H>
#define Y5C ((P2 & 0x1f) | 0xa0);// 只改变高三位
void initSystem(){
    P2 = Y5C;
    P0 = 0x00;// 关掉蜂鸣器等外设
}
void main(){
    initSystem();// 关掉蜂鸣器等外设
    P2 = Y5C;
    P0 = 0x10;// N_RELAY=0
    delay();
    P0 = 0x00;// N_RELAY=1
}
```

...

## 数码管静态显示

...

任务目标：***

```c
#include <STC15F2K60S2.H>
#define Y6C ((P2 & 0x1f) | 0xc0);// 片选
#define Y7C ((P2 & 0x1f) | 0xe0);// 段选
unsgined char code seg_D[18]={}

void segDisplay(){
    P2 = Y6C;// 片选
    P0 = 0X01;
    P2 = Y7C;// 段选
    P0 = 0X82;
}
void segDisplay_bit(unsgined char dat,unsgined char pos){
    P2 = Y6C;// 片选
    P0 = pos;
    P2 = Y7C;// 段选
    P0 = dat;
}
void segShow(){
    unsgined char i,j;
    // 片选
    for(i = 0;i < 8; ++i){
        // 段选
        for(j = 0;j < 9; ++j){
            segDisplay_bit(seg_D[j],i);
            delay();// 延时几秒
        }
    }
}
void main(){
    segShow();
}
```

...

## 数码管动态显示

...

任务目标：***

```c
#include <STC15F2K60S2.H>
#define Y4C ((P2 & 0x1f) | 0x80);// LED
#define Y5C ((P2 & 0x1f) | 0xa0/
#define Y6C ((P2 & 0x1f) | 0xc0);// 片选
#define Y7C ((P2 & 0x1f) | 0xe0);// 段选
char str[]="2018xx12";
bool strdp[8]={0,0,0,0,0,0,1,0};
// 字库映射
char str2hex(char str){
    // 共阳数码管
    switch(str){
        case '0':return 0xc0;
        case '1':return 0xf9;
        case '2':return ;
        case '3':return ;
        case '4':return ;
        case '5':return ;
        case '6':return ;
        case '7':return ;
        case '8':return 0x00;
        case '9':return ;
        case '-':return ;
    	case 'x':return 0xff;// 灯全灭
    }
}
// 只显示一位
bool strdp[8]={0,0,0,0,0,0,1,0};
void DisplayBit(unsgined char dat,unsgined char pos,bool dp){
    P2 = Y6C;// 片选
    P0 = 0x01 << pos;
    P2 = Y7C;// 段选
    P0 = dp?(dat|0x80):(dat);// dat是单字节
}
// 🎃思考关于如何显示小数点的问题
// dat = dp=1->(dat|0x80) 或 dp=0->dat|0x00;

// 死循环刷新
void Display(){
	/*下面的数组放到外部，通过定时器定时修改数组的数值实现数据刷新*/
    // char str[]="2018xx12";
    unsigned char i = 0;
    while(1){
        DisplayBit(str2hex(str[i]),i);i++；
        delay();
        if(i>7) i=0;
    }
}
void main(){
	Display();
}
```

...

## 独立按键基操及扩展

...

任务目标：***

> 独立按键：J5(CON3)跳接2,3。使用S7~S4。

```c
#include <STC15F2K60S2.H>

void main(){
    
}
```

...

## 矩阵键盘扫描原理

...

任务目标：***

```c
#include <STC15F2K60S2.H>
unsigned char KeyScan(){
    unsigned char ret[16]={
        11,12,13,14,
        21,22,23,24,
        31,32,33,34,
        41,42,43,44,
    }
    unsigned char L = 0;
    for(L=0;L<4;++L){
        P2 = (P2 & 0xf0) | (~(0x01 << L) & 0x0f);// 行扫描
        /*  */
        if(){
			
        }
    }
}
int main(){
    KeyScan();
}
```

...

## 中断系统及外部中断

...

任务目标：*** 

```c
#include <STC15F2K60S2.H>
void Service() interrupt 1{
    
}
void InitIpt0(){
    IT0 = 1;
    EX0 = 1;
    EA = 1;
}
void mainWorking(){
    
}
int main(){
    InitIpt0();// 初始化中断
    while(1){
		mainWorking();
    }
}
```

...

## 定时器基操

...

```c
#include <STC15F2K60S2.H>
unsigned int Tim0Count = 0;

void TIM0_Init(){
    TMOD = 0x01;
    TH0 = (65535-50000) / 256;
    TL0 = (65535-50000) % 256;
}

void TIM0_Service()interrupt 0{
    TH0 = (65535-50000) / 256;
    TL0 = (65535-50000) % 256;
	++Tim0Count;
    // 定时器100次溢出才执行下面操作
    if(Tim0Count == 100){
		Tim0Count = 0;
        /* 中断服务操作 */
    }
}
void main(){
    TIM0_Init();
}
```

...

## 定时器进阶

... 

```c
#include <STC15F2K60S2.H>
#define Y6C ((P2 & 0x1f) | 0xc0)// 片选
#define Y7C ((P2 & 0x1f) | 0xe0)// 段选
sbit S4 = P3^2;
sbit S5 = P3^3;
char str[8]="00-00-00"; //数码管数据
bool strdp[8]={0,0,0,0,0,0,0,0};
/* 时间参数 */
unsigned char cnt = 0;
unsigned char h = 13;
unsigned char m = 0;
unsigned char s = 0;
void delay(){
	unsigned int i = 3000;
	while(i--);
}
// 按键扫描************************************************
unsigned char KeyScan(){
    if(S4 == 0){
        return 4;
    }
	if(S5 == 0){
        return 5;
    }
	return 0;
}
// 数码管显示***********************************************
// 字库映射
char str2hex(char str){
    // 共阳数码管
    switch(str){
        case '0':return ~0x3f;
        case '1':return ~0x06;
        case '2':return ~0x5b;
        case '3':return ~0x4f;
        case '4':return ~0x66;
        case '5':return ~0x6d;
        case '6':return ~0x7d;
        case '7':return ~0x07;
        case '8':return ~0x7f;
        case '9':return ~0x6f;
        case '-':return ~0x40;
    	case 'x':return ~0x00;// 灯全灭
    }
	return 0xff;
}

// 只显示一位
void DisplayBit(unsgined char dat,unsgined char pos,bool dp){
    P2 = Y6C;// 片选
    P0 = 0x01 << pos;
    P2 = Y7C;// 段选
    P0 = dp?(dat|0x80):(dat);// dat是单字节
}
// 🎃思考关于如何显示小数点的问题
// dat = dp=1->(dat|0x80) 或 dp=0->dat|0x00;


// 显示数据
void Display(){
	/*下面的数组放到外部，通过定时器定时修改数组的数值实现数据刷新*/
    // char str[]="2022xx12";
    unsigned char i = 0;
    while(i < 8){
        DisplayBit(str[i],i);
		i++;
        delay();
    }
}
//TIM******************************************************
void TIM0_Init(){
    TMOD = 0x01;
    TH0 = (65535-50000) / 256;
    TL0 = (65535-50000) % 256;
    ET0 = 1;
    EA = 1;
    TR0 = 1;
}
// 定时刷新str和strdp数组数据
void TIM0_Service()interrupt 1{
    TH0 = (65535-50000) / 256;
    TL0 = (65535-50000) % 256;
	++cnt;
    // 定时器20次溢出(1s)刷新一次数据
    if(cnt == 20){
		cnt = 0;
        if(++s == 60){
            s = 0;
            if(++m == 60){
                m = 0;
                if(++h == 24){
                    h = 0;
                }
            }
        }
        str[0] = ('0' +(h/10));
        str[1] = ('0' +(h%10));
        str[3] = ('0' +(m/10));
        str[4] = ('0' +(m%10));
        str[6] = ('0' +(s/10));
        str[7] = ('0' +(s%10));
        strdp[] = {0};
        strdp[6] = 1;
    }
	KeyScan();
}

int main(){
    TIM0_Init();
    while(1){
        Display();
    }
}
```

...

## PWM信号发生与控制

...

```c
// 利用PWM控制LED亮度
#include <STC15F2K60S2.H>
#define Y4C_ENABLE() (P2 = (P2 & 0x1f) | 0x80)
sbit L1 = P0^0;
sbit S7 = P3^0;
unsigned char cnt = 0;
unsigned char KEY_STATE = 0;
unsigned char PWM_DUTY = 0;//占空比，最大100

void delay(unsigned int i){
	while(i--);
}
void KeyScan(){
	if(S7 == 0){
		delay();
		if(S7 == 0){
			switch(KEY_STATE){
				case 0:
					L1 = 0;
					TR0 = 1;
					PWM_DUTY = 10;
					KEY_STATE = 1;
					break;
				case 1:
					PWM_DUTY = 50;
					KEY_STATE = 2;
					break;
				case 2:
					PWM_DUTY = 90;
					KEY_STATE = 3;
					break;
				case 3:
					L1 = 1;TR0 = 0;
					KEY_STATE = 0;
					break;
			}
		}
		while(S7 != 1);// 等待按键弹起再进行下一次判断，防止按键抖动误判
	}
}
void TIM0_Init(){
    TMOD = 0x01;
    TH0 = (65535-100) / 256;
    TL0 = (65535-100) % 256;
    ET0 = 1;
    EA = 1;
}
void TIM0_Service()interrupt 1{
    TH0 = (65535-100) / 256;
    TL0 = (65535-100) % 256;
	++cnt;
    // 定时器100次溢出(10us)刷新一次数据
    if(cnt == PWM_DUTY){
    	L1 = 1;
    }
    if(cnt == 100){
		cnt = 0;
		L1 = 0;
    }
}
int main(){
	Y4C_ENABLE();
	TIM0_Init();
	while(1){
		KeyScan();
	}
	return 0;
}
```

...

## 串口通信基操

...

```c
#include <STC15F2K60S2.H>
sfr AUXR = 0x8e;
unsigned char rebyte;
void UART_Init(){
    TMOD = 0x20;//8位自动重装
    TH1 = 0xfd;
    TL1 = 0xfd; // 波特率相关
    TR1 = 1;
    SCON = 0x50;
    AUXR = 0x8e;
    // AUXR &= 0x01;
    EA = 1;
    ES = 1;
}
void UART_Service()interrupt 4{
	if(RI == 1){
        RI = 0;
        UART_Rebyte = SBUF;
        UART_SendByte(rebyte+2);
    }
}
void UART_SendByte(unsigned char dat){
    SBUF = dat;
    while(TI != 1);// TI发送完成标志,此语句为等待发送完成。
    TI = 0;
}

int main(){
    UART_Init();
    UART_SendByte(0x01);
    UART_SendByte(0x32);
    return 0;
}

```

...

## 串口通信进阶

...

任务目标：***

```c
#include <STC15F2K60S2.H>

int main(){
    
}
```

...

## IO扩展及存储器映射MM

...

任务目标：***

```c
#include <STC15F2K60S2.H>
#include <absacc.h>
XBYTE[0x8000] = 0xff;// 指示灯
XBYTE[0xa000] = 0xff;// 蜂鸣器/继电器
XBYTE[0xc000] = 0xff;// 位选
XBYTE[0xe000] = 0xff;// 段选
void main(){
    while(1);
}
```

...

## 基操综合

...

任务目标：***

```c
#include <STC15F2K60S2.H>

int main(){
    
}
```

...

## DS18B20

...

<img src="image\DS18B20.png" style="zoom: 67%;" />

<img src="image\DS18B20-2.png" alt="image-20220312202150464" style="zoom:67%;" />

...

```c
#include <STC15F2K60S2.H>
#define Y6C_ENABLE() (P2 = (P2 & 0x1f) | 0xc0)// 片选
#define Y7C_ENABLE() (P2 = (P2 & 0x1f) | 0xe0)// 段选
char str[8]="xxxxx000"; //数码管数据
/* 时间参数 */
unsigned char cnt = 0;

void delay(){
    unsigned int i = 3000;
    while(i--);
}

// 数码管显示***********************************************
// 字库映射
char str2hex(char str){
    // 共阳数码管
    switch(str){
        case '0':return 0xc0;
        case '1':return ~0x06;
        case '2':return ~0x5b;
        case '3':return ~0x4f;
        case '4':return ~0x66;
        case '5':return ~0x6d;
        case '6':return ~0x7d;
        case '7':return ~0x07;
        case '8':return ~0x7f;
        case '9':return ~0x6f;
        case '-':return ~0x40;
        case 'x':return 0xff;   // 灯全灭
    }
    return 0xff;
}

// 显示一位
void DisplayBit(unsigned char str,unsigned char pos){
    Y7C_ENABLE();
    P0 = 0xff;      // 消影
    Y6C_ENABLE();   // 片选
    P0 = 0x01 << pos;
    Y7C_ENABLE();   // 段选
    P0 = str2hex(str);
}
// 显示数据
void Display(){
    unsigned char i = 0;
    while(i < 8){
        DisplayBit(str[i],i);
        i++;
        delay();
    }
	// 小数点显示放在了DisplayBit函数中，具体代码再= 
    /* 下面语句显示小数点. */
    /*Y6C_ENABLE();   // 片选
    P0 = 0x40;
    Y7C_ENABLE();   // 段选
    P0 = (str2hex(str[6]) | 0x80);
    delay();*/
}
//TIM定时器用于更新数据******************************************************
void TIM0_Init(){
    TMOD = 0x01;
    TH0 = (65535-50000) / 256;
    TL0 = (65535-50000) % 256;
    ET0 = 1;
    EA = 1;
    TR0 = 1;
}
void TIM0_Service()interrupt 1{
    TH0 = (65535-50000) / 256;
    TL0 = (65535-50000) % 256;
    ++cnt;
    // 定时器20次溢出(1s)刷新一次数据
    if(cnt == 20){
        cnt = 0;
        str[5] = ('0' +(m%10));
        str[6] = ('0' +(s/10));
        str[7] = ('0' +(s%10));
    }
    KeyScan();
}
void Read_DS18B20(){
    unsigned char MSB,LSB;
    init_ds18b20();
    Write_DS18B20(0xcc);
    Write_DS18B20(0x44);
    delay();
    init_ds18b20();
    Write_DS18B20(0xcc);
    Write_DS18B20(0xbe);
    MSB = Read_DS18B20();
    LSB = Read_DS18B20();

}
int main(){
    while(1){
        
    }
}
```

...

## 模块化设计

...

任务目标：***

```c
#include <STC15F2K60S2.H>

int main(){
    
}
```

...

## DS1302

...

任务目标：***

```c
#include <STC15F2K60S2.H>

int main(){
    
}
```

...

## 555定时器及频率测量

...

任务目标：***

```c
#include <STC15F2K60S2.H>

int main(){
    
}
```

...



## 受不了极其简化的助记符

```c
/* 定时器 */
#define TIM_CONFIG TCON
#define TIM_MODEL TMOD
// 定时器0
#define TIM_HIGH_BIT_0 TH0
#define TIM_LOW_BIT_0 TL0
#define TIM_FLAG_0 TF0
// 定时器1
#define TIM_HIGH_BIT_1 TH1
#define TIM_LOW_BIT_1 TL1
```

