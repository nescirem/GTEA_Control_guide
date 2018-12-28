---
description: 求解问题类型/求解器的选择。
---

# 求解模块列表

### SloverObject

```javascript
{
    "type": "求解器选择",
    "sloverSettings": {}
}
```

> `type`: "ASI" \| "CHT" \| "CAA" \| "MPF" \| "None"

GTEA求解器选择，默认为`"None"`即不求解任何问题。

* `"ASI"`: GTEA-ASI 声固耦合求解器。
* `"CAA"`: GTEA-CAA 气动声学求解器。
* `"CHT"`: GTEA-CHT 传热与流体求解器。
* `"MPF"`: GTEA-MPF 多相流求解器。
* `""`: GTEA- 燃烧及化学反应动力学求解器。
* `""`: GTEA- 结构热应力求解器。
* `"None"`: 不求解任何内容。

{% hint style="info" %}
需要进行补充或细化。
{% endhint %}

> `sloverSettings`: SloverSettingsObject

具体输入内容由[SloverObject](./#sloverobject)中的"problem"决定，各个求解器的详细输入参见：

{% page-ref page="asi.md" %}

{% page-ref page="caa.md" %}

{% page-ref page="cht.md" %}

{% page-ref page="mpf.md" %}



