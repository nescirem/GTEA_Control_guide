# MPF多相流

{% hint style="info" %}
这只是一份草稿，需要重写编写。
{% endhint %}

### SloverSettings

```javascript
{
    "model": "多相流求解模型"
    "mpfSettings": {}
}
```

> `model`: "eulerian" \| "vof" \| "mixture"

GTEA-MPF多相流求解模型选择，默认为`"eulerian"`。

* `"eulerian"`: 欧拉模型，分别建立各相的动量方程和连续性方程并求解。可求解气泡流，颗粒悬浮和流化床等问题。
* `"vof"`: 用于求解：分层流、自由面流等存在气-液分界面的稳态或瞬态问题，可以追踪分界面。
* `"mixture"`: 认为各相分布均匀且连续，求解等效混合流体的动量方程，可求解低负载的粒子流、气泡流、沉降等问题。

> `mpfSettings`: [MpfSettingsObject](mpf.md#mpfsettingsobject)

多相流求解控制参数，内容由不同求解模型决定。

#### MpfSettingsObject



