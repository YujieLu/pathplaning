# Shortest Path Problems with Resource Constraints (SPPRC)

## 定义与概念

列表生成法(colum generation)能够非常好的解决SPPRC问题

* SPPRC 就是解决查找从一个起点到终点的，满足一系列基于资源的限制条件的最短路径。 
* 资源是在一条路径基于某种函数（即rescource extention functions REFs)上变化的变量，可以是时间、负载。
* REF提供了到达某个节点的资源消耗量的增加。
* 资源限制条件（也就是资源窗），形式为区间，限制了到达某一节点的资源范围

实例：  
![image](https://github.com/YujieLu/pathplaning/blob/master/literature/Figures/fig1.png)

如图，
路径上的两个值分别代表时间消耗和评分
节点上的两个值代表资源窗，（这里正好也就是时间窗）
REF： f<sub>ij</sub>(T<sub>i</sub>)=T<sub>i</sub>+t<sub>ij</sub>
那么，  
P<sub>1</sub>=(s,1,t) 最终的时间和消耗为[12,10]  
P<sub>2</sub>=(s,2,t) 最终的时间和消耗为[11,11]  
P<sub>3</sub>=(s,2,t) 最终的不可通行  

> P<sub>1</sub>和P<sub>2</sub>不可比较，因为一个时间快，一个消耗低

* 2个资源的SPPRC 一班就叫做基于时间窗的最短路径问题（SPPTW）