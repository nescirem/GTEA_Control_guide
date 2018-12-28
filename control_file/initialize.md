# 计算域初始化

### InitializeObject <a id="bco"></a>

```javascript
{
    "u": 0.0,
    "v": 0.0,
    "w": 0.0,
    "p": 0.0,
    "vf": 0.0,
    "enthalpy": 293.15,
    "k": 0.6,
    "epsilon": 0.6,
    "y": 0，
    "zone": [],
    "fromFile": []
}
```

这里的`u`,`v`,`w`,`p`,`vf`,`enthapy`,`k`,`epsilon`,`y`均为全局初始化，优先级没有zone中的区域初始化来得高。

> `zone`: \[[InitializeZoneObject](initialize.md#initializezoneobject)\]

对各个域进行单独初始化，详见[区域初始化](initialize.md#initializezoneobject)。

> `fromFile`: \[[ImportConditionsFromFileObject](boundarycondition.md#importconditionsfromfileobject)\]

从外部文件导入环境条件，可以导入流速、压力、密度场等，详见[外部文件变量场读入](boundarycondition.md#importconditionsfromfileobject)。

#### InitializeZoneObject

```javascript
{
    "name": "域名称",
    "id": 1,
    "u": 0.0,
    "v": 0.0,
    "w": 0.0,
    "p": 0.0,
    "vf": 0.0,
    "enthalpy": 293.15,
    "k": 0.6,
    "epsilon": 0.6,
    "y": 0，
}
```

这里的`u`,`v`,`w`,`p`,`vf`,`enthapy`,`k`,`epsilon`,`y`为单个域的初始化，优先级高于全局初始化。



