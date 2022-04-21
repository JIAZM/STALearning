# Tcl在EDA工具中的扩展与应用

## Synopsys TCL

- ***sizeof_collection[all_alocks]***	

  ```tcl
  all_clocks # 获取所有时钟 输出数据类型为collection
  sizeof_collection # 对collection类型取大小
  ```

  ![image-20220417232954485](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220417232954485.png)

- get_ports portsName

  返回Design中对应的ports object

  portsName 可以使用正则

  ```tcl
  # 查看design中与没有一个port叫做CLK
  % get_ports CLK
  # 查看design中所有的port
  % get_ports *
  ```

  ![image-20220417233009217](C:\Users\Jiazm556\AppData\Roaming\Typora\typora-user-images\image-20220417233009217.png)

- get_cells cellsName

  返回design中对应的cell的instance object name

  ```tcl
  # reference name 与 instance name
  reference name - 模块名
  instance name - reference例化出来的实例名
  ```

  ***<font color=red size=5>get_cells 命令查看的是instance name</font>***

- get_nets netsName

  返回design中的net object name

  ```tcl
  # 查看design中有多少net
  % llength [get_object_name[get_nets *]]	# Tcl基本语法
  # get_object_name 指令 - 将get_nets指令的输出值转为list类型
  % sizeof_collection [get_nets *]	# Synopsys扩展指令
  ```

- get_pins pinsName

  返回design中的pins object name

  ***design中的pins - 指的是cell中的pins***

## 数据类型 object（对象）与其“属性”

- object是对于tcl脚本的一个重要扩展
- 常见的object有四种 ***cell、net、port、pin***
- 美中object都有其属性

> ***object的常见属性***
>
> - 任意一个属性都可以用 ***get_attribute***命令得到
> - ***list_attribute -class \****可以得到所有object的属性
> - 部分属性可以使用***set_attribute***命令来设置

1. Cell object

   > 属性：ref_name - 用来保存其map到的reference cell的名称
   >
   > ```tcl
   > % get attribute [get_cells -h U3] ref_name
   > {INV}
   > ```

2. Pin object

   > 属性：owner_net - 用来保存与之相连的net的名称
   >
   > ```tcl
   > % get_attribute [get_pins U2/A] owner_net
   > {BUS0}	# 得到与 pin{U2/A} 相连的net名称为 BUS0
   > ```

3. Port object

   > 属性：direction - 用来保存port的方向
   >
   > ```tcl
   > % get_attribute [get_ports A] direction
   > {in}
   > ```

4. Net object

   > 属性：full_name - 用来保存net的名称
   >
   > ```tcl
   > % get attribute [get_nets INV0] full_name
   > % get_onject_name [get_nets INV0]
   > ```

- get_* 指令的属性过滤选项

  ***-f*** 使用"<attribute>==<option>"过滤属性，得到想要的object

  ```tcl
  % get_ports * -f "direction==in"
  # 得到design中所有方向为输入的port
  % get_pins * -f "direction==in"
  # 得到所有方向是输入的pin
  % get_cells * -f "ref_name==INV"
  # 得到所有ref_name为INV的cell - INV例化了多少个cell
  ```

  ***-of*** 得到与指定object相连接的object

  ```tcl
  # --port object <-> net object
  % get_nets -of [get_ports A]
  # --net object  <-> port object
  % get_ports -of [get_nets BUS0]
  # --pin object  <-> net object
  % get_nets -of [get_pins U2/A]
  # --cell object <-> pin object
  % get_pins -of [get_cells U4]
  ```

  