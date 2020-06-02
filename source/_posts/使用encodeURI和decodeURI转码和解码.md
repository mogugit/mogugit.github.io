---
title: 使用encodeURI和decodeURI转码和解码
date: 2020-06-02 18:00:49
tags:
---
**encodeURI() 函数可把字符串作为 URI 进行编码。**

**语法:**
encodeURI(URIstring)

| 参数 | 描述 |
| ------------- |:-------------:|
|URIstring |必需。一个字符串，含有 URI 或其他要编码的文本。|
<!-- more -->
**返回：**
URIstring 的副本，其中的某些字符将被十六进制的转义序列进行替换。

**说明**
该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码： - _ . ! ~ * ' ( ) 。

该方法的目的是对 URI 进行完整的编码，因此对以下在 URI 中具有特殊含义的 ASCII 标点符号，encodeURI() 函数是不会进行转义的：;/?:@&=+$,#  **可以[使用encodeURIComponent()和decodeURIComponent()](https://blog.csdn.net/qq_39712029/article/details/81003518)来进行转义**

```
var a ="张三";

encodeURI(a)// "%E5%BC%A0%E4%B8%89"
```

**decodeURI() 函数可对 encodeURI() 函数编码过的 URI 进行解码。**

**语法**
decodeURI(URIstring)

**描述**
| 参数 | 描述 |
| ------------- |:-------------:|
|URIstring |必需。一个字符串，含有 URI 或其他要解码的文本。|

**返回值**
URIstring 的副本，其中的十六进制转义序列将被它们表示的字符替换。

```
var b = encodeURI("张三")；//%E5%BC%A0%E4%B8%89
decodeURI(b) //张三
```
