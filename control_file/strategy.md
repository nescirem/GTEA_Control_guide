# 求解策略

### StrategyObject

{% hint style="info" %}
或许应该将空间项重命名为"spatial"
{% endhint %}

```javascript
{
    "time": {},
    "gradient": "梯度项求解策略",
    "zones": []
}
```

> `time`: [TimeStrategyObject](strategy.md#timestrategyobject)

时间项相关（全局设置），详见[时间项策略](strategy.md#timestrategyobject)。

> `gradient`: "least squre" \| "gaussian"

梯度项相关（全局设置）。

> `zones`: \[[StrategyZonesObject](strategy.md#strategyzonesobject)\]

对各个域的空间项以及时间项进行单独设置（如果有需要的话）。



#### TimeStrategyObject

```javascript
{
    "formulation": "稳态，显式或隐式",
    "settings": {},
}
```

`formulation`: "steady" \| "explicit" \| "implicit"

若求解稳态问题，则需要设置迭代次数等参数；若求解瞬态问题，则需要另外设置时间项离散规则。

* "steady": 稳态问题求解。
* ”explicit“: 显格式推进求解。
* "implicit": 隐格式迭代求解。

{% hint style="info" %}
注意：GTEA-ASI求解器只求解瞬态问题。
{% endhint %}

> `settings`: [TimeStrategySettingsObject](strategy.md#timestrategysettingsobject)

时间项显格式与隐格式相关设置，详见[时间项策略](strategy.md#timestrategysettingsobject)。

#### TimeStrategySettingsObject

```javascript
{
    "minIter": 10,
    "maxIter": 200,
    "timeStart": 0.0, /*s*/
    "timeEnd": 1.0, /*s*/
    "deltaT": 1e-6, /*s*/
    "ifSatisfyConvergence": true,
    "convergenceSet": "收敛判据类型"
    "convergenceCriterion": {}
}
```

> `minIter`: number

若采用隐格式离散时间项或求解稳态问题，则需要设置单个时间步的迭代次数；`maxIter`同理。

> `timeStart`: number

若求解瞬态问题，则需要设置起止时刻，一般为正实数。

> `deltaT`: number

若求解瞬态问题，则需要设置时间步长，一般为正实数。

> `ifSatisfyConvergence`: “true” \| "false"

是否设置瞬态场稳定后终值计算的判定规则。

> `convergenceSet`: "absolute" \| "order reduction"

判定规则设为绝对值或两个时间步的差值。

#### ConvergenceCriterion

```javascript
{
    "u": 1.0e-10,
    "v": 1.0e-10,
    "w": 1.0e-10,
    "p": 1.0e-6,
    "vf": 1.0e-8,
    "enthalpy": 1.0e-8,
    "k": 1.0e-8,
    "epsilon": 1.0e-8,
    "y": 1.0e-5
}
```

#### StrategyZonesObject

```javascript
{
    "name": "域名称",
    "id": 1,
    "time": {},
    "gradient": {}
}
```

> `name`: string

域名称。

> `id`: number

域id。

> `time`: [TimeStrategyObject](strategy.md#timestrategyobject)

如果由需要的话，可以时间项的区域设置单独设置，注意不能与全局设置冲突。

> `gradient`: "least squre" \| "gaussian"

梯度项相关，优先级高于全局设置。

