# CHT传热与流体

### SloverSettingsObject

```javascript
{
    "problemType": "求解问题类型选择",
    "chtSettings": {}
}
```

> `problemType`: \[[AsiSettingsZonesObject](asi.md#asisettingsobject)\]

声固耦合求解器针对特定求解问题的控制问题的相关附加参数。

#### ChtSettingsObject

```javascript
{
    "equation":{}
    "relaxationCoefficient": []
}
```



#### ChtSettingsEquationObject

```javascript
{
    "u": true,
    "v": true,
    "w": true,
    "p": true,
    "vf": false,
    "enthalpy": false,
    "k": true,
    "epsilon": true,
    "y": false,
    "rho": true,
    "viscosity": true,
    "pp": true,
    "tempreture": false
}
```

#### ChtSettingsRelaxationCoefficientObject

```javascript
{
    "phaseName": "相名称",
    "phaseId": 1,
    "relaxationFactor":{}
    "convergenceCriterion":{}
}
```

> `convergenceCriterion`: [ChtSettingsConvergenceCriterionObject](cht.md#chtsettingsconvergencecriterionobject)

传热与流体求解器内迭代的收敛判据。

#### ChtSettingsRelaxationFactorObject

```javascript
{
    "u": 0.8,
    "v": 0.8,
    "w": 0.8,
    "p": 1.0,
    "vf": 0.5,
    "enthalpy": 0.9,
    "k": 4,
    "epsilon": 4,
    "y": 0.6,
    "rho": 1.0,
    "viscosity": 1.0,
    "pp": 0.8,
    "tempreture": 0.8
}
```

#### ChtSettingsConvergenceCriterionObject

```javascript
{
    "u": 1.0e-10,
    "v": 1.0e-10,
    "w": 1.0e-3,
    "p": 1.0e-6,
    "vf": 1.0,
    "enthalpy": 1.0,
    "k": 1.0，
    "epsilon": 1.0,
    "y": 1.0e-5
}
```



