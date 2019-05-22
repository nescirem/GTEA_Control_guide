---
description: 待读入网格的相关信息。
---

# 网格信息

{% hint style="info" %}
需要补充、修正、完善。
{% endhint %}

### GridObject <a id="gio"></a>

```javascript
{
  "fileName": "网格文件名.网格文件名后缀",
  "fileType": "网格文件类型",
  "cgnsLibrary": "cgns网格lib版本",
  "zone": {}
}
```

> `fileName`: string

GTEA需要读入的网格文件名称，其值是在程序运行路径 `stdout`下的一个带后缀名的网格文件名，如`"case.msh"`。

> `fileType`: "msh" \| "layring msh" \| "pgrid" \| "cgns"

网格文件类型，若不指定则默认文件类型为`"msh"`。

> `cgnsLibrary`: "v2.3" \| “”

若指定网格文件类型为`"cgns"`。则需要增加指定cgns library的版本号。

`zones`: [GridZoneMaps](grid.md#agio)

每个网格文件含有的区域信息**映射**，详见[网格区域信息](grid.md#gso)。



#### GridZoneMaps <a id="gso"></a>

{% hint style="info" %}
与求解模块列表中的区域设置对应，与边界条件以及附加边界条件中的interior物性设置对应。
{% endhint %}

```javascript
"id": {
    "name": "区域名称",
    "type": "区域问题类型",
    "id": "区域编号"
}
```

> `name`: string

某个区域zone的名称，与网格各个区域名称对应。

> `type`:  “acoustic" \| "damping" \| "transient" \| ”aeroacoustics“ \| "multi-phase flow" \|

区域zone待求解的问题类型。

* `"acoustic"`: 声学域。包括：经典声波方程求解、多孔介质声传播、平均流声场、含温度梯度静止声场等。
* `"damping"`: 结构域。包括：结构振动、热应力等。
* `"transient"`: 流体域。离散相的求解区域应当设为流体域（详见燃烧及化学反应动力学）。
* `"aeroacoustics"`: 气动声学域。
* `"multi-phase flow"`: 多相流体域。

> `id`: string

某个域zone的id，与网格各个区域名称对应。

##  <a id="additionalgridinfo"></a>

### GridControlObject <a id="agio"></a>

```javascript
{
    "space": "网格维度信息",
    "axi2dSettings":{}
    "revolveSettings":{}
    "scale": 1.0,
    "numPhases": 1,
    "homogeneous": true,
    "interiorFaceInOneCgns": false,
    "numInteriorFacePair": 1,
    "numCyclePair": 0
}
```

> `space`: “AXI2D" \| "3D" \| "2D" \| "REVOLVE"

计算域的空间维度信息。

> `scale`: number

计算域网格的缩放系数，是一个实数。

> `numPhases`: number

计算域包含的的流体相总数，一般是一个正整数。

> `interiorFaceInOneCgns`: true \| false

是否存在单个CGNS文件内有多个zone的情况（在ASI声固耦合求解器中用到）。

> `homogeneous`: true \| false

介质是否为各向同性（针对全体计算域）。

> `interiorFaceInOneCgns`: number

计算域存在多个zone时的的内部面个数，一般是个正整数。

> `numCyclePair`: number

一般是个正整数。

