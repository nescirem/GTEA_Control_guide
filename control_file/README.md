---
description: 本节为采用JSON规范化GTEA控制文件提供建议，尽可能使得控制文件格式统一、结构清晰、易读、可拓展。
---

# 控制文件

GTEA读入的数据并不以数据结构树的形式存在。换言之，GTEA目前CTL格式的控制文件的可扩展性较差，于是考虑采用JSON统一GTEA读入的数据。

**建议在编写JSON解析器前先将当前CTL控制文件改写为JSON格式的文件**——也就是本节将要讨论的内容，改写完成之后详细程序编写范例可参见[APP. A](../how-to-use-fson.md)。

{% hint style="info" %}
GTEA使用基于类BSD协议开源的json-fortran作为应用程序接口编写json解析器（以后称为“GTEA-JsonControl解析器”或“GTEA-JC”），该解析器支持读写含有Fortran风格注释的JSON，这意味着用户可以使用“! 注释内容 ”来进行单行注释。虽然这在某些语法检查器中可能被显示为“格式错误”，但实际上GTEA-JC中的是可以正常读入并使用的。
{% endhint %}

## JSON 数据类型

以下列举JSON常用的数据类型，在之后的配置中会用到。

> `boolean`: true \| false

布尔值，只可取`true`或`false`，不带引号。

> `number`

数字，在 GTEA 的使用中可以是整数，也可以是实数，如`1.25e-6`、`-1` 、`0`、`1.254`…… 数字在 JSON 格式中不带引号。

> `string`

字符串，由引号包含的一串字符，如无特殊说明，字符的内容不限。

> `array`: \[\]

数组，由方括号包含的一组元素，如字符串数组表示为`[string]`。

> `maps`: {}

映射，由花括号包含的一组或多组元素，其元素基本结构为值值对。映射作为一种特殊的结构，不被通常的json解析器所接受，但考虑到采用映射可以大幅简化GTEA控制文件，我们决定引入这种结构。但同时对GTEA-json控制文件中的映射作出如下限制：

1. 映射中的第一个值（作为索引）只接受字符串。例如：`"1234":1234`、`"3124":{“a”:1}`、`"4123":false` 等都是可以接受的映射结构；而`1234:"abcd"`、`false:{null}` 等是不被接受的。
2. 映射中的第一个值在当前结构下的所有映射中只能出现一次（唯一性），且满足相应的对应的关系。

> `object`: {}

对象，一组键值对。样例见文章[起始节](../#wei-shen-me-shi-yong-json)示例。

{% hint style="info" %}
通常一个键值对的后面需要有一个逗号","，但如果这个键值对后面紧跟一个大括号"｝"的话，则一定不能有逗号。
{% endhint %}

## GTEA 常用数据类型

> `map`: object {string:string}

一组字符类型的键值对，其类型在括号内指出。每一个键和值的类型对应相同。

> `gridInfo`: string

字符串，表示网格文件名，形如：`"Karman_vortex_street.msh"`或 `"porous_barrier.cgns"`

> `gridInfo`: array

由对象组成的数组，每个对象包含一套网格文件的控制信息，GTEA某些情况下可能需要一次读入多个网格文件。

## GTEA中以数组形式读入的数据

### 网格文件信息

通常情况下，一个算例只需要一个网格文件，这时用户可以任意选择用对象或者数组读入网格文件。而在某些特殊情况下，一个算例需要一次读入多个网格文件，这时我们允许用户已数组形式给定网格文件信息。

```javascript
{
  "grid": [
    {
      "fileName": "gridfile1.cgns",
      "type": "cgns",
      "cgnsLibrary": "v2.3",
      "settings": []
    },
    {
      "fileName": "gridfile2.msh",
      "type": "cgns",
      "cgnsLibrary": "v2.3",
      "settings": []
    },
    {
      "fileName": "grifile3.grid",
      "type": "msh",
      "settings": []
    }
  ]
}
```

## GTEA中以映射形式读入的数据

### 材料信息

```javascript
{
  "material": {
    "311": {"rho": 1.226, "acousticVelocity": 340},
    "312": {
      "name": "porous","rho": 1.226, "acousticVelocity": 340,"structureConstant": 3.0, 
	  "flowResistance": 2000.0,"porousity": 0.3,"heatCapacityRatio": 1.4,
      "ambientPressure": 101215.0 !//,"isMultiphase":false
	}
}
```

### 域信息

网格文件信息中的域信息稍稍有些特殊，需要针对每个网格文件单独列出，映射索引不能重复。

```javascript
{
  "grid":[
    {
      "zone": {
        "11":{"name": "区域名称1"},
        "12":{"name": "区域名称2"},
      }
    }
    {
      "zone": {
        "13":{"name": "区域名称3"},
    }
  ]
}
```

求解器域设置信息与网格文件信息中的域信息具有对应关系，这里以ASI声固耦合求解器为例

```javascript
{
  "zone": {
    "11":{
      "problem":"acoustic",
      "waveFlag":5,
      "numWave":4,
      "haveMeanFlow":false,
      "isPorous":false
    },
    "12":{
      "problem":"acoustic",
      "waveFlag":5,
      "numWave":4,
      "haveMeanFlow":false,
      "isPorous":true
    },
    "13":{
      "problem":"acoustic",
      "waveFlag":5,
      "numWave":4,
      "haveMeanFlow":false,
      "isPorous":false
    }
  }
}
```

求解离散策略中的域信息

```javascript
{
  "zone": {
    "11": {"gradient": "linear","timeFormulation": "explicit", "deltaT": 1e-6},
    "12": {"gradient": "least squares","timeFormulation": "implicit", "deltaT": 1e-6}
  }
}
```

### 边界信息

```javascript
{
  "boundary": {
    "5011": {
      "name": "in","type": "acousticSource",
      "acousticSourceSetting": {
        "type": "gauss pulse", 
        "amplitude": 1, 
        "frequency": 8000
      }
    },
    "5012": {"name": "wall_a","type": "hard wall"},
    "5013": {
      "name": "inf_a",
      "type": "interface",
      "correspondingFace":"inf_p",
      "isCorrespondenceNode": true
    },
    "5014": {"name": "inf_p","type": "interface",
             "correspondingFace":"inf_a",
             "isCorrespondenceNode": true
    },
    "5015": {"name": "wall_p","type": "hard wall"},
    "5016":	{"name": "out","type": "transmissive"}
  }
}
```

### 其他

域与材料的映射信息

```javascript
{
  "zone": {
    "11": {"material":"311","isHomogeneous": true},
    "12": {"material":"312","isHomogeneous": true}
  }
},
```



