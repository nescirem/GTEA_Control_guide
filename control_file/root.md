# 基本结构

## 根结构树 <a id="&#x914D;&#x7F6E;&#x6587;&#x4EF6;&#x683C;&#x5F0F;"></a>

GTEA 的配置文件基础结构如下，共有10个子结构：

* 日志输出
* 待读入网格信息
* 附加网格信息（由读入网格类型以及求解问题类型决定）
* 并行计算
* 材料
* 求解问题类型
* 离散求解策略
* 边界条件
* 附加边界条件（由求解问题类型决定）
* 计算域初始化
* 计算结果输出策略

```javascript
{
  "log": {},
  "gridInfo": [],
  "additionalGridInfo": {},
  "parallel": {},
  "material": [],
  "slover": {},
  "strategy": {},
  "boundary": [],
  "additionalBoundary": [],
  "initialize": {},
  "output": {}
}
```

##  各项子结构树

> `log`: [LogObject](log.md#logobject)

日志配置，表示 GTEA 如何输出日志，若此项不存在则GTEA不输出日志到任何文件，详见[日志输出](log.md)。

> `gridInfo`: \[[GridInfoObject](gridinfo.md#gridinfo)\]

网格信息**数组**，其数组对象一般包括：网格文件位置、网格文件类型、网格各zone待求解问题的类型，详见[网格信息](gridinfo.md)。

> `additionalGridInfo`: [AdditionalGridInfoObject](gridinfo.md#adi)

附加网格信息，主要包括了网格维度、网格缩放系数、计算域相数（某些求解器需要用到），详见[网格信息](gridinfo.md)。

> `parallel`: [ParallelObject](mpi.md#parallelobject)

并行计算相关，详见[并行计算](mpi.md)。

> `material`: \[[MaterialObject](material.md#parallelobject)\]

材料设置相关**数组**，详见[材料设置](material.md#parallelobject)。

> `slover`: [SloverObject](slover/#sloverobject)

与待求解问题相关，详见[求解模块列表](slover/)。

> `strategy`: [StrategyObject](strategy.md#strategyobject)

求解策略相关，主要包括时间项（稳态or瞬态问题的计算起止时刻，时间步长，迭代步数等）；空间项（梯度求解规则等），详见[求解策略](strategy.md)。

> `boundary`: \[[BoundaryConditionObject](boundarycondition.md#bco)\]

边界条件相关，详见[边界条件](boundarycondition.md)。

> `additionalBoundary`: \[[AdditionalBoundaryConditionObject](gridinfo.md#agio)\]

附加边界条件，部分求解问题需要附加边界条件信息，见[边界条件](boundarycondition.md)。

> `initialize`: [InitializeObject](initialize.md#bco)

计算域初始化，详见[计算域初始化](initialize.md)。

> `output`: [OutputObject](output.md#bco)

计算结果输出策略，详见[输出策略](output.md)。

