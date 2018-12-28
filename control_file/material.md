---
description: 材料物性设置，GTEA并没有fluent那样自建材料物性数据库，需要用户自行输入计算条件下的材料物性。
---

# 材料

### MaterialObject <a id="parallelobject"></a>

```javascript
{    
    "name": "材料名称",
    "id": 1,
    "rho": 1.225,  /*kg/m^3*/
    "viscosity": 1.789e-5, /*Pa s*/
    "acousticVelocity": 343,
    "Cp": 1000,
    /*以下下专为固体*/
    "E": 7e10,
    "upsilon": 0.33,
    /*以下专为多孔介质*/
    "thermalConductivity": 0.026,
    "structureConstant": 1.95,
    "flowResistance": 2000,
    "porosity": 0.3
}
```

> `name`: string

材料名称。

> `id`: number

材料编号，是个正整数。

> `rho`: number

材料密度，是个实数。

> `viscosity`: number

流体材料的动力粘度，是个实数。

> `E`: number

固体材料的杨氏模量，是个实数。

> `upsilon`: number

固体材料的泊松比，是个实数。

> `acousticVelocity`: number

材料中的声速，是个实数，可以不指定。

> `Cp`: number

流体的定压比热，是个实数。

> `thermalConductivity`: number

材料的导热系数，是个实数。

