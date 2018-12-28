# 域及边界条件

### BoundaryConditionObject <a id="bco"></a>

```javascript
{
    "name": "域或边界名称",
    "id": 1,
    "type": "域或边界类型",
    "isCorrespondenceNode": true,
    "material": [],
    "condition": {}
}
```

> `name`: string

域或边界的名称。

> `id`: number

域或边界的id。

> `type`: "wall" \| "inlet" \| "outlet" \| "acoustic source" \| "interior" \| "interface" \| “internal face” \| ...

边界类型。

* `"wall"`: 壁面：滑移壁面、无滑移壁面、简支、固支、声学绝对硬、声学绝对软。
* `"inlet"`: 传热与流动入口边界。
* `"outlet"`: 传热与流动出口边界。
* `"internal face"`: 内部面。
* `"interface"`: 网格交界面，网格交界面**一般**成对出现。

域类型

* `"interior"`: 网格内部域（相对于边界域而言）

> `isCorrespondenceNode`: "true" \| "false"

若计算域存在`"interface"`，则需要告知GTEA网格交界面的节点对应关系，若为`"true"`则GTEA自动寻找节点对应关系；若为`"false"`则GTEA会对界面进行插值。

> `material`: \[[MaterialSetObject](boundarycondition.md#materialsetobject)\]

某个域内含有的材料**数组**。

> `condition`: [ConditionSetObject](boundarycondition.md#conditionsetobject)

某个域的状态条件（绝对值）。

#### MaterialSetObject 

```javascript
{
    "name": "材料名称",
    "id": 1
}
```

> `name`: string

域内包含的材料的名称。

> `id`: number

域内包含的材料的编号。

#### ConditionSetObject

```javascript
{
    "u": 0.0, /*m/s*/
    "v": 0.0,
    "w": 0.0,
    "p": 101325, /*Pa*/
    "fromFile": []
}
```

{% hint style="info" %}
ASI求解器中的气体计算域（无多孔介质）可以设置平均流速`u`,`v`,`w`，届时ASI将求解平均流波动方程。其他求解器中则不需要。
{% endhint %}

> `fromFile`: \[[ImportConditionsFromFileObject](boundarycondition.md#importconditionsfromfileobject)\]

从外部文件导入环境条件，可以导入流速、压力、密度场等，详见[外部文件变量场读入](boundarycondition.md#importconditionsfromfileobject)。

#### ImportConditionsFromFileObject

```javascript
{
    "name": "外部文件名称.文件后缀",
    "import": []
}
```

> `name`: string

外部文件名称。

> `import`: VariableObject

域内包含的材料的编号。

## 

### AdditionalBoundaryConditionObject <a id="abco"></a>

```javascript
{
    "name": "域或边界名称",
    "id": 1,
    "type": "域或边界类型",
    "acousticSource": []
}
```

> `type`: "acoustic source" \| "absorption boundary" \| "damping source"

* `"acoustic source"`: 声源边界条件。
* `"damping source"`: 振动源边界条件。
* `"absorption boundary"`: 声学吸收边界条件。
* `"hard"`: 声源绝对硬边界条件。
* `"soft"`: 声学绝对软边界条件。
* `"absorption boundary"`: 声学吸收边界条件。
* `”simply supported“`: 结构简支边界。
* `"fixed"`: 结构固支边界。
* `"free"`: 结构自由边界。

{% hint style="info" %}
在[边界条件](boundarycondition.md#bco)中被设为interior的域不能在[附加边界条件](boundarycondition.md#abco)中被设置为`"absorption boundary"`；若域在[附加边界条件](boundarycondition.md#abco)中被设为`"acoustic source"`或`"damping source"`，则意指该域内含有点声源/振动源，需要在[AcousticSourceObject](boundarycondition.md#acousticsourceobject)中增加声源点坐标或节点编号。
{% endhint %}

> `acousticSource`: \[[AcousticSourceObject](boundarycondition.md#acousticsourceobject)\]

域内点源（声或振动）的相关设置。

#### AcousticSourceSettingsObject

```javascript
{
    "type": "声源类型",
    "amplitude": 1, /*Pa*/
    "frequency": 8000, /*Hz*/
    "sinPulseT": 0,
    "position": [],
    "nodeId": [123，254，53]
}
```

> `type`: "gauss pulse" \| "gauss" \| "sin" \| "white noise"

声源类型设置，可以为高斯脉冲、持续高斯声源、正弦声源、白噪声

* `"gauss pulse"`: 高斯脉冲声源，默认在 $$\frac{3}{f}$$ 后改为透射边界。
* `"gauss"`: 在边界或声源点上持续给定 $$p=Aexp\left(-\frac{(t-3/f)^2}{(3/f)^2}\right)$$ 。
* `"sin"`: 正弦单频声源，默认为持续声源，用户可以通过增加`sinPulseT`项以达到给定特定周期的正弦脉冲的目的。
* `”white noise“`: 高斯白噪声声源。

> `amplitude`: number

高斯脉冲、高斯持续或正弦声源的幅值，一般为正实数。

> `frequency`: number

高斯脉冲、高斯持续声源的分析频率或正弦单频声源的频率，一般为正实数。

> `sinPulseT`: number

设定正弦声源脉冲的给定周期，一般为正实数。默认为`0` 即给定持续单频声源。

{% hint style="info" %}
用户可以以坐标的形式给出点声源的声源坐标，也可以直接给定声源节点编号。
{% endhint %}

> `position`: \[[PositionObject](others.md#positionobject)\]

给定一个坐标（长度为3的一个一维数组），详见[其他信息](others.md)中的[给定点坐标](others.md#positionobject)。

> `nodeId`: \[number\]

给定一个或多个节点编号作为源点。



