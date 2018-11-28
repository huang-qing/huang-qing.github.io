---
layout:     post
title:      HTML5 Input
subtitle:   accept 属性值详解
date:       2018-08-10
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML]
tags:
    - HTML
---

# `<input type="file">`的`accept`属性值详解

`accept`可以限制文件的上传类型，比如只上传图片文件、视频文件、音频文件.如下:

```
audio/*	接受所有的声音文件。
video/*	接受所有的视频文件。
image/*	接受所有的图像文件。
```

```html
<input type="file" name="pic" id="pic" accept="image/gif, image/jpeg" />
```


详细类型限制如下所示：

```
*.3gpp	audio/3gpp, video/3gpp	3GPP Audio/Video
*.ac3	audio/ac3	AC3 Audio
*.asf	allpication/vnd.ms-asf	Advanced Streaming Format
*.au	audio/basic	AU Audio
*.css	text/css	Cascading Style Sheets
*.csv	text/csv	Comma Separated Values
*.doc	application/msword	MS Word Document
*.dot	application/msword	MS Word Template
*.dtd	application/xml-dtd	Document Type Definition
*.dwg	image/vnd.dwg	AutoCAD Drawing Database
*.dxf	image/vnd.dxf	AutoCAD Drawing Interchange Format
*.gif	image/gif	Graphic Interchange Format
*.htm	text/html	HyperText Markup Language
*.html	text/html	HyperText Markup Language
*.jp2	image/jp2	JPEG-2000
*.jpe	image/jpeg	JPEG
*.jpeg	image/jpeg	JPEG
*.jpg	image/jpeg	JPEG
*.js	text/javascript, application/javascript	JavaScript
*.json	application/json	JavaScript Object Notation
*.mp2	audio/mpeg, video/mpeg	MPEG Audio/Video Stream, Layer II
*.mp3	audio/mpeg	MPEG Audio Stream, Layer III
*.mp4	audio/mp4, video/mp4	MPEG-4 Audio/Video
*.mpeg	video/mpeg	MPEG Video Stream, Layer II
*.mpg	video/mpeg	MPEG Video Stream, Layer II
*.mpp	application/vnd.ms-project	MS Project Project
*.ogg	application/ogg, audio/ogg	Ogg Vorbis
*.pdf	application/pdf	Portable Document Format
*.png	image/png	Portable Network Graphics
*.pot	application/vnd.ms-powerpoint	MS PowerPoint Template
*.pps	application/vnd.ms-powerpoint	MS PowerPoint Slideshow
*.ppt	application/vnd.ms-powerpoint	MS PowerPoint Presentation
*.rtf	application/rtf, text/rtf	Rich Text Format
*.svf	image/vnd.svf	Simple Vector Format
*.tif	image/tiff	Tagged Image Format File
*.tiff	image/tiff	Tagged Image Format File
*.txt	text/plain	Plain Text
*.wdb	application/vnd.ms-works	MS Works Database
*.wps	application/vnd.ms-works	Works Text Document
*.xhtml	application/xhtml+xml	Extensible HyperText Markup Language
*.xlc	application/vnd.ms-excel	MS Excel Chart
*.xlm	application/vnd.ms-excel	MS Excel Macro
*.xls	application/vnd.ms-excel	MS Excel Spreadsheet
*.xlt	application/vnd.ms-excel	MS Excel Template
*.xlw	application/vnd.ms-excel	MS Excel Workspace
*.xml	text/xml, application/xml	Extensible Markup Language
*.zip	aplication/zip	Compressed Archive
```