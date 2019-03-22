# Time Windows Based Dynamic Routing in Multi-AGV Systems

## 符号定义
* 车辆：r<sub>i</sub>
* 路径：a<sub>i</sub>
* 任务：m<sub>i</sub>
* 第i个任务的起始路径：o<sub>i</sub>
* 第i个任务的终止路径：d<sub>i</sub>
* 第i个任务的中间路径：![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn1.gif)
* 第i个任务优先级：优先级P<sub>i</sub>(t)
* 第i个任务在第j条路径上的时间窗：w<sub>ij</sub>=<sup>out</sup>t<sub>ij</sub> - <sup>in</sup>t<sub>ij</sub>
* 第j条路径上的时间窗 
	**w<sub>j</sub>**=[w<sub>ij</sub>]
	**<sup>in</sup>t<sub>j</sub>**=[<sup>in</sup>t<sub>ij</sub>]
	**<sup>out</sup>t<sub>j</sub>**=[<sup>out</sup>t<sub>ij</sub>]
* 排序：<**x**>=[x<sub>i</sub>]
* 安全时间：epsilon<sub>m<sub>j</sub></sub>

## 定义与假设

* 地图可以分解为一系列节点(node)以及连接这些节点的路径（Arc）
* 每个路径有一个权重(w)，即为运行时间（长度/平均速度）
* 从节点A到节点B有一条路径，则节点A即为节点B的源节点（upstream），而B节点为A节点的尾节点(downstream)，同样，如果路径A，到路径B指直接路过节点，则路径A是路径B的源路径。

## 规则
* G 矩阵： 如果路径i是路径j的源路径，则为g<sub>ij</sub>为1，否则为0
* 对于不需要路过a<sub>j</sub>的任务，对应的w<sub>j</sub>=0,<sup>in</sup>t<sub>j</sub>=<sup>out</sup>t<sub>j</sub>=无穷大，
* 优先级P<sub>i</sub>,值越小优先级越高

	![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn2.png)

> 无法完成的任务优先级提到最高

* 优先级仅在有新任务下发和当前路径不可通行时更新
* 当第i个任务完成后，没有新的任务分配给AGV r<sub>i</sub>,回到最近的停车库
* **同一时刻，有且仅有一个AGV在有且只有一条路径上**
> 此规则应该改成，同一时刻，一个路径最多有一个AGV占有
* 使用Dijkstra 或者string algebra 或者Floyd-Warshal查找最短路径
> 不能走重复路径

* 每次release某一段路径，就重新优化一遍，因为之前不行的任务可能可以了。

* 其他
> 进入任意路径，需要有和他干涉的路径的权限

> 停车库的权限也要预先申请

> 路径应截短至2米以下（2米这个是车身长度+激光区域+缓冲区域的大概数值，直线上相连三个路径不应该相互限制）

> 多个停车位，以增加可调度性

> 停车位终点加一小段路径，作为终点路径，这个路径不能和主路干涉

## 流程
### 初始化时间窗
> 当新接收到一个任务时触发
* 已经完成的<sup>in</sup>t<sub>ij</sub>，<sup>out</sup>t<sub>ij</sub>设置为无穷大(应该是便于排序)，w<sub>ij</sub>设置为0
* 优先级低的任务按上述设置
* 正在运行的w就按比例设置

### 时间窗插入
* 通过下列公式检查是否能插入：

![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn3.png)

或

![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn4.png)

* 通过下列公式插入：

![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn5.png)

或

![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn6.png)

### 时间窗检查

* 拉长 w<sub>m<sub>j</sub></sub>使得<sup>out</sup>t<sub>m<sub>j</sub></sub>=<sup>in</sup>t<sub>m<sub>i</sub></sub>
> 我们的版本需要拉长所有冲突路径

* 检查overlap，如果

![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn7.png)

如果不成立，则重新插入overlap的那条路径

直到：

a) 完成插入，它的权重为完成时的时间

或者

b) 第一条路径overlap，它的权重为无穷大

如果是b则代表无法运行

* 优先级低于m的任务规划方式相同


感觉两种方式都可以考虑：
1. 不断刷新，如果不能完成就提高优先级
> 这个有待验证
2. 先到能走优先级越高，不更新优先级，走不通的任务不下发
> 此种方法比较简单，也可以根据实际情况尝试更新行走方式，如果新的规划比旧的规划快，就采取新规划


## 需要考虑的问题
* 我们系统不支持取消任务
* 网络问题，不能整段下发路径的，要一批一批的申请
* 异常车辆需要占用路径！！！
* 异常车辆需要将对应任务重新分配？？

* 进入任何一条路径之前，需要确定这条路径以及这条路径会碰撞的路径没有被预约


## 基于此方案之后与原系统差别
### 优点
* 所有路径已经预先处理过，可通行，并且在优先级高的任务确定的情况下时间最短
* 不需要画管制区域，人工工作较少
* 

### 缺点
* 会一直等待优先级较高的任务
* 运算量较原方法大


## 小结
* 如果按照此方法来做，最大优势是无需画管制区域，plan方面不一定有多好
* 其他方式还可以按照现有方式来，

## TODO
* 碰撞检测
* 单机反馈走到的路径

## 碰撞检测
* 每一对车需要一个碰撞检测表
* 我们需要存一个列表：
	这个列表是预先分配的路径，每种车会有一套一模一样的表
* 假设有N条路径(2000-8000)（每条路径不长于2m，每10cm设一个点，每条路径最多20个点），M种车，每种车K个点（暂时仅仅使用4个点）则需要检测M*M*N*N个碰撞情况（2-3种车）	
> 先对M * N * K * 20个点进行处理，得到M * N * 4的运行方框(10000 * 3 * 4 * 3),预估1ms,或考虑做凸多边形

> 再对M * (M-1)/2进行两点初步碰撞滤除，预估150ms

用2个点进行初步滤除，距离大于>2个*对角线+2个路径长度就可以不考虑了
再使用8个点进行滤除

> 在对M * 100 * N * N * 8进行进一步滤除，预估200ms

> 精细碰撞检测 M * 50 * N * N * 80 * 2，预估4s 可忽略



