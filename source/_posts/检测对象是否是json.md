---
title: 检测对象是否是json
date: 2020-06-02 11:23:58
tags:
  - util
---
检测一个对象是否是一个对象的方法：

```
if(typeof(obj) == "object" && Object.prototype.toString.call(obj).toLowerCase() == "[object object]" && !obj.length){
	alert('是JSON对象');
}
```

<!-- more -->
