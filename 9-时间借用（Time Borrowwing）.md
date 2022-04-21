# Time Borrowing

> 什么是***时间借用***
>
> 时序逻辑的两种基本组成——DFF和Latch
>
> DFF依靠时钟边沿进行采样
>
> Latch根据电平进行触发，在有效电平期间Latch透明，输出随输入改变，在无效电平期间输出被锁存
>
> ***在对Latch组成的时序逻辑电路进行静态时序分析时需要考虑 Time Borrowing***

- **Time Borrowing (Cycle stealing)**

  > 在Latch组成的时序逻辑电路中产生

  - 在Latch中，使其透明的时钟边沿称作***开启边沿（Opening edge）***
  - 在接下来的边沿中，使其所存的时钟边沿称作***关闭边沿（Closing edge）***

  DFF中存在建立时间与保持时间，Latch中也存在

  - 要求Latch的数据端信号在开启边沿到来之前稳定
  - 实际电路中Data不一定非要在Opening edge之前到来，也***可以在Opening edge之后一段时间到来，只要在Closing edge之前数据稳定即可***
  - ***数据滞后于Opening edge时，相当于Latch向后一个周期借用一部分时间 ——称作 Time Borrowing***

![image-20220419211247483](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419211247483.png)

<u>*前面电路向下一周期借用时间，相当压缩了后级电路计算需要的时间*</u>

![image-20220419211742624](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419211742624.png)

![image-20220419211800143](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419211800143.png)

- 按照数据到来时间点可以分为三种

   ![image-20220419212046298](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419212046298.png)

- **对特定电路进行分析**

   > 在逻辑电路中，门控时钟的设计中为了消除毛刺，可以使用电平相反的Latch

   ![image-20220419212212779](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419212212779.png)

   ***时序报告：***

   > Launch网络：起点DFF -> 重点 Latch
   >
   > ***data arrival time = 4.65ns < 5ns*** , 故为***positive slack***

   ![image-20220419212527714](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419212527714.png)

   > Capture网络：由于数据到达类别为***positive slack*** ，所以 ***time borrowed from endpoing***为***0*** 

   ![image-20220419212621047](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419212621047.png)

   ***当<u>data arrival time</u>分别处于5到10、大于10，使用 zero slack、negedge slack进行分析，时序分析中不同之处体现在Capture路径***

   - **zero slack**

     > 依然能够满足时序
     >
     > ***此时由 Latch->DFF的时序路径也依然能够满足时序要求，时序分析报告只展示Capture路径***

     ![image-20220419213327392](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419213327392.png)

     ![image-20220419214016218](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220419214016218.png)

   - **negedge slack**

     ***数据到来的非常晚，无法满足时序要求***

   ***<u><font color=blue size=4>只要数据到达时间满足 positive slack或 zero slack，即可满足时序要求，不发生违例</font>></u>***

   
