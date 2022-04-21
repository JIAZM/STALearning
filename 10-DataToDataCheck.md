# Data-to-Data Check

> 什么是Data-to-Data Check
>
> 检查两个pin之间相互的时序关系，并且这两个pin之间没有clock，例如数据信号与使能等控制信号
>
> 分别将两个pin定义为***Constrained pin***和***Related pin***，检查两个pin之间相对的时序约束

与基于Flip-flop的时序检查的不同点：

- 建立时间检查（Setup Check）时Launch和Capture在同一个边沿

  > 因此这种***Data-to-Data Check***又被称为***zero-cycle check***或***same-cycle check***

  ```tcl
  set_data_check -from SDA -to SCTRL -setup 2.1
  set_data_check -from SDA -to SCTRL -hold 1.5
  ```

  ![image-20220420152300602](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420152300602.png)

  ![image-20220420152328584](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420152328584.png)

  ![image-20220420152347965](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420152347965.png)

<div STYLE="page-break-after: always;"></div>

> 一个例子
>
> <img src="C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420152543418.png" alt="image-20220420152543418" style="zoom:67%;" />
>
> ![image-20220420152701422](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420152701422.png)
>
> 建立时间检查报告指定了数据检查，因此报告中出现了***data check setup time***参数，根据时序约束确定为1.8
>
> - 对于Hold Check
>
>   ***<u>默认情况下Hold check在setup check的前一个周期，Data-to-Data Check中 Capture网络向前推移了一个周期（由于Setup Check中Capture与Launch在同一个边沿，Hold Check中Capture路径早于Launch路径一个周期），需要使用一个时钟信号进行假定</u>***
>
>   ![image-20220420153153163](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420153153163.png)
>
>   > 假定了一个时钟，周期为10，占空比50%
>   >
>   > Launch路径的时间点从1-cycle（10ns）开始，Capture路径的时间点从0-cycle（0ns）开始，***Data-to-Data Check的保持时间检查 Launch路径和 Capture路径时间点不在同一边沿，Launch路径时间点滞后于Capture路径一个周期***
>
> - **当需要在同一个边沿进行Hold check时**
>
>   ![image-20220420160950964](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420160950964.png)
>
>   ***set_multicycle_path***命令，使用***-1（负一）***参数将***默认的Capture路径早于Launch路径一个周期的时间点向后推一个周期，移至同一个时间点***，数序报告：
>
>   ![image-20220420161744844](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420161744844.png)

<u>***Constrained Pin和Related Pin的定义并不是固定的***，约束中***-from***和***-to***选项后的参数是可以互换的</u>



- **Data-to-Data Check中也可以不针对单独某个边沿进行检查，可以使用*-rise_from*和*-fall_from*选项分别对指定边沿进行建立时间检查和保持时间检查**

   ![image-20220420162201370](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220420162201370.png)





