#### GPIO

7组IO一组16个外加GPIOH0，GPIOH1

##### 输入

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 135455.png)

浮空输入模式：

没有输入信号时，上拉电阻和下拉电阻都不起作用

上拉电阻拉高引脚默认电平为1，下拉电阻拉低引脚默认电平为0

上拉输入时上拉电阻接通，拉高电平

下拉输入时下拉电阻接通，拉低电平

模拟输入：此时施密特触发器关闭，信号经模拟通道传输

##### 输出

复用输出由复用功能外设控制

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 103010.png)

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-10-30 125929.png)

推挽输出：

P-MOS接通，IO口输出高电平，N-MOS关闭；N-MOS接通，P-MOS关闭，IO口输出低电平。

开漏输出：

P-MOS一直处于断开状态，只有N-MOS工作

N-MOS断开，IO口输出高电平；N-MOS接通，IO口输出低电平

##### 寄存器

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 103457.png)

端口模式寄存器（GPIOx_MODER）

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 103841.png)

端口输出类型寄存器（GPIOx_OTYPER）

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 103947.png)

端口输出速度寄存器（GPIOx_OSPEEDR）

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 104249.png)

端口上/下拉寄存器（GPIOx_PUPDR）

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 105231.png)

端口输入数据寄存器（GPIOx_IDR）

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 105638.png)

接收到1，即该端口接收到高电平，接收到0，即该端口接收到低电平

端口置位复位寄存器（GPIOx_BSRR）

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 105858.png)

低16位置位输出高电平，高16位复位输出低电平

可以不干涉其它位的情况下配置目标位，比ODR较方便

输出数据寄存器（GPIOx_ODR）

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 110707.png)

端口设置为1即输出高电平，端口设置为0即输出低电平

端口位复用功能寄存器

复用器

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 111831.png)

#### 跑马灯实验

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 112433.png)

##### FWLIB中的重要文件

misc.c

stm32f4xx_rcc.c 时钟的使能

stm32f4xx_gpio.c IO口操作相关函数

对声明函数的实现（对寄存器的操作）

stm32f4xx_usart.c 串口

stm32f4xx_rcc.h 宏定义以及函数声明（函数用于寄存器的配置）

##### 重要函数

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 113622.png)

![ ](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 113917.png)

使能IO口时钟

调用函数RCC_AHB1PeriphClockCmd()

(在使用GPIO前要先初始化GPIO的时钟，因为许多外设都被设计成时序逻辑电路，所以必须要为外设提供时钟源，否则外设的电路状态就不会改变)

初始化IO口模式

调用函数GPIO_Init()

操作IO口

GPIO_Set/ResetBits()

##### 编程注意事项

![](C:\Users\LENOVO\Pictures\Screenshots\屏幕截图 2024-11-06 124544.png)

GPIO_InitTypeDef GPIO_InitStructure

定义结构体类型的变量

在GPIOF9,F10的初始化设置中，若初始化设置全相同且在同一组，可用逻辑符号“|”“或”合并简化代码

在编程过程中可使用go to definition 来查看代码定义以辅助编写

assert_param 有效性判断，对此处使用go to definition以查找相应目标函数

delay_init(168) 延时函数，此处填写168是因为该模版中CPU主时钟频率默认为168MHz

GPIO_SetBits控制IO口输出高电平

GPIO_ResetBits控制IO口输出低电平

delay_ms()设置延时时间

代码最后一行要回车，不然会报错

