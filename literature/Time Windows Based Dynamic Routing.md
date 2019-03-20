# a shortest path problem with time window

## 定义与假设

* 地图可以分解为一系列节点(node)以及连接这些节点的路径（Arc）
* 每个路径有一个权重(w)，即为运行时间（长度/平均速度）
* 从节点A到节点B有一条路径，则节点A即为节点B的源节点（upstream），而B节点为A节点的尾节点(downstream)，同样，如果路径A，到路径B指直接路过节点，则路径A是路径B的源路径。


## 符号定义
* 车辆：r<sub>i</sub>
* 路径：a<sub>i</sub>
* 任务：m<sub>i</sub>
* 第i个任务的起始路径：o<sub>i</sub>
* 第i个任务的终止路径：d<sub>i</sub>
* 第i个任务的中间路径：![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn1.gif)
* 第i个任务优先级：优先级P<sub>i</sub>(t)
* 第i个任务在第j条路径上的时间窗：w<sub>ij</sub>=


## 规则
* G 矩阵： 如果路径i是路径j的源路径，则为g<sub>ij</sub>为1
* 优先级P<sub>i</sub>,值越小优先级越高
	![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Equations/Eqn2.png)
* 当第i个任务完成后，没有新的任务分配给AGV r<sub>i</sub>,回到最近的停车库
* ** 同一时刻，有且仅有一个AGV在有且只有一条路径上 

