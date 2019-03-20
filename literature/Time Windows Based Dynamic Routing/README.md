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


## 定义与假设

* 地图可以分解为一系列节点(node)以及连接这些节点的路径（Arc）
* 每个路径有一个权重(w)，即为运行时间（长度/平均速度）
* 从节点A到节点B有一条路径，则节点A即为节点B的源节点（upstream），而B节点为A节点的尾节点(downstream)，同样，如果路径A，到路径B指直接路过节点，则路径A是路径B的源路径。


## 规则
* G 矩阵： 如果路径i是路径j的源路径，则为g<sub>ij</sub>为1
* 对于不需要路过a<sub>j</sub>的任务，对应的w<sub>j</sub>=0,<sup>in</sup>t<sub>j</sub>=<sup>out</sup>t<sub>j</sub>=无穷大，
* 优先级P<sub>i</sub>,值越小优先级越高
	![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn2.png)
* 优先级仅在有新任务下发和当前路径不可通行时更新
* 当第i个任务完成后，没有新的任务分配给AGV r<sub>i</sub>,回到最近的停车库
* **同一时刻，有且仅有一个AGV在有且只有一条路径上**
> 此规则应该改成，同一时刻，一个路径最多有一个AGV占有
* 使用Dijkstra 或者string algebra 或者Floyd-Warshal查找最短路径
> 不能走重复路径

* 其他
> 进入任意路径，需要有和他干涉的路径的权限
> 路径应截短至2米以下（2米这个是车身长度+激光区域+缓冲区域的大概数值）

## 流程


## TODO
* 碰撞检测
