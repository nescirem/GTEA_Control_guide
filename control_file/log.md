---
description: 用于输出GTEA程序运行信息，便于调试记录。
---

# 日志输出

{% hint style="info" %}
暂无开发计划。
{% endhint %}

### LogObject

```javascript
{
  "logPath": "文件路径",
  "logLevel": "warning"
}
```

> `logPath`: string

保存日志的文件的目录，其值是一个合法的路径，如`"D:/Temp/GTEA/`（Windows）。当此项不指定或为空值时，表示将日志输出至程序运行路径，GTEA运行时的相对路径由input.name决定，我们约定称相对路径为 `stdout`。日志保存名称由由`logLevel`决定。

> `logLevel`: "debug" \| "info" \| "warning" \| "error" \| "none"

错误日志的级别。默认值为`"warning"`。

* `"debug"`: 只有GTEA开发人员能看懂的信息，影响GTEA运算速度。同时包含所有`"info"`内容。
* `"info"`: GTEA在运行时的状态，基本不影响正常运算速度。同时包含所有`"warning"`内容。
* `"warning"`: GTEA遇到了一些问题，通常是外部问题，不影响正常运算速度。同时包含所有`"error"`内容。
* `"error"`: GTEA 遇到了无法正常运行的问题，需要立即解决。
* `"none"`: 不记录任何内容。



