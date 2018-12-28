---
description: ASI声固耦合求解器用于求解：液体或气体中近场声传播问题、固体振动问题、声固耦合问题、多孔介质声传播问题。
---

# ASI声固耦合

### SloverSettingsObject

```javascript
{
    "problemType": "求解问题类型选择",
    "asiSettings": {}
}
```

> `problemType`: ”gas acoustic“ \| ”fluid acoustic“ \| "structure damping" \| "asi"

GTEA-ASI声固耦合求解器求解问题类型选择，默认为`"gas acoustic"`。

* `"gas acoustic"`: 气体中声学方程求解，可用于求解：无流经典波动方程、平均流近场声传播、多孔介质声传播。
* `"fluid acoustic"`: 用于求解液体中的近场声传播。
* `"structure damping"`: 用于求解结构振动问题。

> `asiSettings`: [AsiSettingsObject](asi.md#asisettingsobject)

声固耦合求解器针对特定求解问题的控制问题的相关附加参数。

#### AsiSettingsObject

```javascript
{
    "zones":[]
}
```

> `zones`: \[[AsiSettingsZonesObject](asi.md#asisettingsobject)\]

声固耦合求解器针对特定求解问题的控制问题的相关附加参数。

#### AsiSettingsZonesObject

```javascript
{
    "name": "域名称",
    "id": 2,
    "porous": true,
    "solid": false
}
```

> `name`: string

域名称。

> `id`: number

域id。

> `porous`: "true" \| "false"

是否为多孔介质域。

> `solid`: "true" \| "false"

是否为固体域。



{% hint style="info" %}
声固耦合求解器具体边界条件（包括zone的物性、点声源等）参见[AdditionalBoundaryConditionObject](../boundarycondition.md#abco)。
{% endhint %}

