---
title: 传输中文的乱码的原因及解决方式（两次encodeURI()）转码
date: 2020-06-02 17:59:26
tags:
---
.encodeURL函数主要是来对URI来做转码，它默认是采用的UTF-8的编码.
. UTF-8编码的格式:一个汉字来三个字节构成，每一个字节会转换成16进制的编码，同时添加上%号.

假设页面端输入的中文是一个 <font color="#bf0000">“中”</font>，按照下面步骤进行解码
<!-- more -->
1.第一次encodeURI，**按照utf-8方式获取字节数组变成<font color="#bf0000">[-28,-72-83]</font>，对字节码数组进行遍历，把每个字节转化成对应的16进制数，这样就变成了<font color="#bf0000">[E4,B8,AD]</font>,最后变成<font color="#bf0000">[%E4,%B8,%AD]</font>  此时已经没有了多字节字符，全部是单字节字符。**

2、第二次encodeURI，进行编码，**会把%看成一个转义字符，并不编码%以后字符，会把%编码成%25.把数组最后变成<font color="#bf0000">[%25E4,%25B8,%25AD]</font>然后就把处理后的数据<font color="#bf0000">[%25E4,%25B8,%25AD]</font>发往服务器端，**

当应用服务器调用**getParameter**方法，**getParameter**方法会去向应用服务器请求参数

应用服务器最初获得的就是发送来的<font color="#bf0000">[%25E4,%25B8,%25AD]</font>，应用服务器会对这个数据进行URLdecode操作，应用服务器进行解码的这一次，不管是按照UTF-8，还是GBK，还是ISO-8859，,都能得到<font color="#bf0000">[%E4,%B8,%AD]</font>，因为都会把**%25解析成%**.并把这个值返回给**getParameter方法**;

3、再用UTF-8解码一次，就得到"中"了。

想想看，如果不编码两次，当服务器自动解码的时候，假如是按照ISO-8859去解码UTF-8编码的东西，就是会出现乱码。

前端：
```
//javascript:
var roleName = "张三"：
document.authorityForm.action = basePath3+"User_viewUser.do?id="+id+"&roleName="+encodeURI(encodeURI(roleName))+"&roleType="+roleType;

```
后台：
```
roleName = java.net.URLDecoder.decode(getRequest().getParameter("roleName"),"UTF-8");
```
