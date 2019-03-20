# a shortest path problem with time window

## 定义与假设

* 地图可以分解为一系列节点(node)以及连接这些节点的连线（Arc）
* 每个连线有一个权重(w)，即为运行时间（长度/平均速度）
* 从节点A到节点B有一条连线，则节点A即为节点B的源节点（upstream），而B节点为A节点的尾节点(downstream)，同样，如果连线A，到连线B指直接路过节点，则连线A是连线B的源连线。
* G 矩阵： 如果连线i是连线j的源连线，则为g<sub>ij为1

