# PrimeTime概述

![image-20220411184708277](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411184708277.png)

<img src="C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411185404957.png" alt="image-20220411185404957" style="zoom:67%;" />

## PrimeTime 提供了两种环境

- pt_shell (Command Line & script-execution environment based on Synopsys Tcl)
- GUI

# STA基本概念

- Timing Arc

  描述两个节点延时的信息（走线延时、逻辑单元延时）

![image-20220411192510822](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411192510822.png)

![image-20220411192606046](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411192606046.png)

![image-20220411192651693](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411192651693.png)

![image-20220411193006699](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411193006699.png)

![image-20220411193138416](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411193138416.png)

***<font color=blue size=5>时序路径起点：触发器cell时钟pin & 输入port</font>***

![image-20220411193338457](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411193338457.png)

***<font color=blue size=5>时序路径终点：触发器cell输入pin & 输出port</font>***

- 时钟域 - Clock Domains

  > 现在的SoC芯片	***全局异步，局部同步***
  >
  > <u>**一般的分析软件对异步电路是无能为力的**</u>
  >
  > ---***对于跨时钟域的电路需要做约束，使EDA对其不做时序分析***---

![image-20220411193716625](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411193716625.png)

- 操作条件

  ![image-20220411194205085](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411194205085.png)![image-20220411194302375](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411194302375.png)

  ![image-20220411194559297](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411194559297.png)

  > 三种操作等级（高温低电压(Worst) -> 一般条件(Typical) -> 低温高电压(Best_case)）

  ![image-20220411194726036](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411194726036.png)



# 线性延时模型

![image-20220411203101667](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411203101667.png)

![image-20220411203115828](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411203115828.png)

![image-20220411203044077](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411203044077.png)

# 非线性延时模型（NLDM，Non-Linear Delay Model）

![image-20220411203147881](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411203147881.png)

> ***Pin(INP1) -> Pin(OUT) 的延时***
>
> 分为两种：
>
> 1. cell_rise
> 2. cell_fall
>
> <u>通过输入转换***(Input transition)***和输出负载电容***(Output capacitance)***对其进行查找</u>

![image-20220411203221893](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220411203221893.png)



