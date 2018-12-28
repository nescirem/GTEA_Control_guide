---
description: 本节为采用JSON规范化GTEA控制文件提供建议，尽可能使得控制文件格式统一、结构清晰、易读、可拓展。
---

# 控制文件

GTEA读入的数据并不以数据结构树的形式存在。换言之，GTEA目前CTL格式的控制文件的可扩展性较差，于是考虑采用JSON统一GTEA读入的数据。

**建议在编写JSON解析器前先将当前CTL控制文件改写为JSON格式的文件**——也就是本节将要讨论的内容，改写完成之后详细程序编写范例可参见[APP. A](../how-to-use-fson.md)。

{% hint style="info" %}
GTEA使用开源的fson编写json解析器，该解析器支持读写含有C风格注释的JSON，这意味着用户可以使用“/\* 注释内容\*/”来进行单行或多行注释。虽然这在某些语法检查器中可能被显示为“格式错误”，但实际上GTEA-fson是可以正常读入并使用的。
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

```javascript
{
  "gridInfo": [
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

### 域信息

网格信息中的域信息稍微有些特殊，针对每个网格文件单独列出，因此是个二维数组。

```javascript
{
  "log": {},
  "gridInfo": [{
    "fileName": "网格文件名.网格文件名后缀",
    "zones": [
      {
        "name": "区域名称",
        "type": "区域问题类型",
        "id": 1
      },
      {
        "name": "区域名称",
        "type": "区域问题类型",
        "id": 2
      },
      {
        "name": "区域名称",
        "type": "区域问题类型",
        "id": 3
      },
    ]
  }]
}
```

求解模块中的域信息则为一维数组，这里以ASI声固耦合求解器为例

```javascript
{
  "slover": {
    "type": "ASI",
    "asiSettings": {
      "zones":[
        {
          "name": "air",
          "id": 1,
          "porous": false,
          "solid": false
        },
        {
          "name": "porous",
          "id": 2,
          "porous": true,
          "solid": false
        },
        {
          "name": "solid",
          "id": 3,
          "porous": false,
          "solid": true
        }
      ]
    }
  }
}
```

求解策略中的域信息





