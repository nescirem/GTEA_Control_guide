# 使用数组简化控制文件

{% hint style="info" %}
需要注意的是：使用数组确实可以简化JSON控制文件的复杂度，但同时也会降低控制文件可读性，届时还请适当补充注释。
{% endhint %}

这里以[计算域初始化InitializeObject](control_file/initialize.md#bco)为例，展示如何使用数组简化控制文件。可以看到这里计算域初始化的大部分变量都为同类型的实数，这很适合转化为数组。

{% code-tabs %}
{% code-tabs-item title="简化前" %}
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
{% endcode-tabs-item %}

{% code-tabs-item title="简化后" %}
```javascript
{
                           /*u, v, w, p, vf, enthalpy, k, apsilon, y*/
    "globalInitialization":[0.0,0.0,0.0,0.0,0.0,293.15, 0.6,0.6,0],
    "zone": [],
    "fromFile": []
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



