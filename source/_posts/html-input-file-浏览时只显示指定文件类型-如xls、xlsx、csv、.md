---
title: html input='file' 浏览时只显示指定文件类型 如xls、xlsx、csv、
date: 2020-06-02 15:02:00
tags:
---
html:
```html
<input id="fileSelect" type="file" accept=".csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel" />
```
Valid Accept Types:
对于 CSV 文件 (.csv), 使用:
```html
<input type="file" accept=".csv" />
```
对于Excel 2003-2007 (.xls)文件, 使用:
```html
<input type="file" accept="application/vnd.ms-excel" />
```
 对于Excel 2010 (.xlsx)文件, 使用:
```html
<input type="file" accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" />
```
对于文本 (.txt)文件, 使用:
```html
<input type="file" accept="text/plain" />
```
<!-- more -->
对于图片(.png/.jpg/etc)文件, 使用:
```html
<input type="file" accept="image/*" />
```
对于 HTML (.htm,.html)文件, 使用:
```html
<input type="file" accept="text/html" />
```
对于视频Video (.avi, .mpg, .mpeg, .mp4)文件, 使用:
```html
<input type="file" accept="video/*" />
```
对于音频Audio (.mp3, .wav, etc)文件, 使用:
```html
<input type="file" accept="audio/*" />
```
对于 PDF 文件, 使用:
```html
<input type="file" accept=".pdf" />
```


