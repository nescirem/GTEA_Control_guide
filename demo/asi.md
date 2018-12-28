---
description: 这是一ASI求解器JSON控制文件的实例展示。
---

# ASI

{% code-tabs %}
{% code-tabs-item title="solid\_acoustic\_porous\_interactions.json" %}
```javascript
{
    "log": {
        "logPath": "./",
        "logLevel": "warning"
    },
    "gridInfo": [
        {
            "fileName": "solid.cgns",
            "settings": 
            [
                {"name": "solid", "type": "damping",  "id": 3}
            ]
        },
        {
            "fileName": "fluid.cgns",
            "settings": 
            [
                {"name": "air",   "type": "acoustic", "id": 7},
                {"name": "poro",  "type": "acoustic", "id":10}
            ]
        }
    ],
    "additionalGridInfo": {
        "space": "3D",
        "scale": 1.0,
        "numPhases": 1, /*这里的pahse针对流体项，所以是1*/
        "homogeneous": true,
        "interiorFaceInOneCgns": false,
        "numInteriorFacePair": 2
    },
    "parallel": {},
    "material": [
        {    
            "name": "air",    "id": 1,
            /**************************/
            "rho": 1.225,
            "acousticVelocity": 340
        },
        {    
            "name": "porous", "id": 2,
            /**************************/
            "acousticVelocity": 340,
            "structureConstant": 1.95,
            "flowResistance": 2000,
            "porosity": 0.3
        },
        {    
            "name": "steel",  "id": 3,
            /**************************/
            "acousticVelocity": 2700,
            "E": 7e10,
            "upsilon": 0.33
        }
    ],
    "slover": {
        "problemType": "asi",
        "asiSettings": {
            "zones":[
                {"name": "air",   "id": 7, "porous": false, "solid": false},
                {"name": "poro",  "id":10, "porous": true , "solid": false},
                {"name": "solid", "id": 3, "porous": false, "solid": true }
            ]
        }
    },
    "strategy": {
        "time": {
            "formulation": "explicit",
            "settings": {
                "timeStart": 0.0,
                "timeEnd": 1.0
            }
        },
        "gradient": "gaussian",
        "zones": [
            {
                "name": "steel", "id": 1,
                "time": {
                    "settings":{"deltaT": 1e-6}
                }
            },
            {
                "name": "air",   "id": 2,
                "time": {
                    "settings":{"deltaT": 2.5e-6}
                }
            },
            {
                "name": "porous","id": 3,
                "time": {
                    "settings":{"deltaT": 2.5e-6}
                }
            }
        ]
    },
    "boundary": [
        {
            "name": "wall_s",    "id": 1,
            "type": "域或边界类型"
        },
        {
            "name": "inf_sa",    "id": 2,
            "type": "interface",
            "isCorrespondenceNode": true
        },
        {
            "name": "steel",     "id": 3,
            "type": "interior",
            "material": [{"name":"steel", "id":3}]
        },
        {
            "name": "inf_as",    "id": 4,
            "type": "interface",
            "isCorrespondenceNode": true
        },
        {
            "name": "wall_a",    "id": 5,
            "type": "wall"
        },
        {
            "name": "inf_ap",    "id": 6,
            "type": "interface",
            "isCorrespondenceNode": true
        },
        {
            "name": "air",       "id": 7,
            "type": "interior",
            "material": [{"name":"air", "id":1}]
        },
        {
            "name": "inf_pa",    "id": 8,
            "type": "interface",
            "isCorrespondenceNode": true
        },
        {
            "name": "wall_p",    "id": 9,
            "type": "wall"
        },
        {
            "name": "poro",     "id": 10,
            "type": "interior",
            "material": [{"name":"porous", "id":2}],
            "condition": {"p": 101325}
        }
    ],
    "additionalBoundary": [
        {
            "name": "wall_s",    "id": 1,
            "type": "free"
        },

        {
            "name": "wall_a",    "id": 5,
            "type": "hard"
        },
        {
            "name": "air",       "id": 7,
            "type": "acoustic source",
            "acousticSource": [
                "type": "gauss pulse",
                "amplitude": 1,
                "frequency": 8000,
                "position": [
                    [0,1.414, 1.414],[0,-1.414, 1.414],
                    [0,1.414,-1.414],[0,-1.414,-1.414]
                ]
            ]
        },
        {
            "name": "wall_p",    "id": 9,
            "type": "hard"
        }
    ],
    "initialize": ["u": 0.0,"v": 0.0,"w": 0.0,"p": 0.0,
    "vf": 0.0,"enthalpy": 293.15,"k": 0.6,
    "epsilon": 0.6,"y": 0，],
    "output": {}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

