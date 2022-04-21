# Clock Gating Checks

<!-- <div STYLE="page-break-after: always;"></div> -->

![image-20220420162520766](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420162520766.png)

> 门控时钟
>
> ![image-20220421094344767](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421094344767.png)
>
> 有利于动态功耗的控制和优化

**只使用一个逻辑门进行门控时会产生毛刺*（glitch）***

一般情况下使用集成的***ICG（IP）***进行时钟门控设计

![image-20220421094831090](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421094831090.png)



- **门控时钟时序检查**

  > 门控时钟检查发生在 - 在一个逻辑单元上***（Logic Cell）***当一个门控信号***（Gating Signal）***可以控制时钟信号***（Clock Signal）***时

  - 门控时钟检查的条件

    1. 经过cell的时钟处于时钟下游<u>***(Clock Downstream)***</u>. ***若时钟信号不是在门控单元（Gating Cell）后被使用，则将不会被推断为门控时钟检查（Clock Gating Check）***
    2. 检查的条件取决于门控信号，检查中门控引脚***（Gating pin）***的信号不应该是一个时钟信号，或者，***若门控引脚的信号是一个时钟信号，则其不应该在时钟下游使用***

    ![image-20220421100422109](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421100422109.png)

  -   门控时钟检查方法

    <u>***检查参考的是有效门控信号的逻辑状态   High和 Low指的是时钟能够传到下一级的时候门控信号的值***</u>

    1. ***Active-high Clock Gating Check***

       > 发生在：门控单元使用与门***（and）***或 与非门***（nand）***时

       有时门控时钟结构相对复杂，使用***多路选择器(Multiplexer)***或***异或门(xor)***，使用命令***set_clock_gating_check***可将时钟门控的结构识别出来进行下一步静态时序分析

       ![image-20220421102054061](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421102054061.png)

       > 门控结构***UAND0***中Pin(A)为门控信号，Pin(B)为时钟信号
    >
       > 假定电路中存在的两个时钟信号CLKA、CLKB相同
       >
       > ![image-20220421102901132](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421102901132.png)
  
       - 建立时间检查：
    
          ![image-20220421102357609](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421102357609.png)
    
          ![image-20220421102438300](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421102438300.png)
    
       - 保持时间检查：
    
          ![image-20220421102624644](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421102624644.png)
    
          ![image-20220421102656381](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421102656381.png)
    
       ***将门控时钟约束至半周期时序路径，能够完成想要的效果***
    
    2. ***Active-low Clock Gating Check***
    
       > 发生在：门控单元使用或门***（or）*** 或或非门***（nor）***时
       
       ![image-20220421103426043](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421103426043.png)
       
       ***Active-Low结构的门控时钟SetupCheck和HoldCheck都满足需求，不需要像 Active-High一样约束至半周期***
    
  - 其他结构的门控时钟
  
    ![image-20220421104325193](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421104325193.png)
  
    使用单独的约束命令
  
    ***set_clock_gating_check -high [get_cells UMUX0]***
  
    ***set_disable_clock_gating_check UMUX0/I1***
  
    ```tcl
    # set_clock_gating_check 命令
    # -high 选项指定检查的时间点处于时钟的高电平期间——即使用 Active-High方式进行检查
    # -low 选项指定使用 Active-Low方式进行检查
    ```
  
    > ***Setup Check Report***
    >
    > ![image-20220421104452895](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421104452895.png)
    >
    > ![image-20220421104511138](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421104511138.png)
    
    > ***Hold Check Report***
    >
    > ![image-20220421104542741](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421104542741.png)
    >
    > ![image-20220421104555773](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421104555773.png)
  
- **带有时钟反转的门控时钟**<u>（也是比较常见的）</u>

  ![image-20220421104702863](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421104702863.png)

  > ***对于这种Active-High的门控结构，通常需要让 Gating Signal落在 Clock Signal的低电平区间（负半周期），以确保 Clock Signal能够正确的传播到下一级电路***
  >
  > <u>默认的时序报告</u>
  >
  > - SetupCheck
  >
  >    ![image-20220421105352458](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421105352458.png)
  >    
  >    ![image-20220421105431521](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421105431521.png)
>    
  > - HoldCheck
  >
  >    ![image-20220421105452941](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421105452941.png)
  >
  >    ![image-20220421105510288](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220421105510288.png)
  >
  >    <u>***保持检查验证数据（门控信号）是否在 MCLK 下降沿之前 10ns 发生变化***</u>
  >
  
  ***加入约束，引入具体的值***
  
  ```tcl
  # 指定在 Cell(U0/UXOR1)进行门控时钟检查
  set_clock_gating_check -setup 2.4 -hold 0.8 [get_cells U0/UXOR1]
  # -setup | -hold 指定时钟的建立时间和保持时间
  ```
