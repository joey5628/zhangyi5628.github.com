---
title: input type=file accept中可以限制可选的文件类型
date: 2016-06-07 16:35:55
categories: Html
tags: [Html, input, file, upload]
---
在上传文件的时候，通过accept属性可以限制可选文件的类型，当设置改属性是，可以让用户在浏览上传文件时，只能选择指定的文件类型。那么不同的文件类型，accept值应该如何设置了？
见下表：

| 文件类型 | MIME(accept属性值) |
| ------------ | ------------- |
| 图片（.png,.jpg等所有格式的图片） | image/* |
| *.doc | application/msword |
| *.docx | application/vnd.openxmlformats-officedocument.wordprocessingml.document |
| *.xls | application/vnd.ms-excel |
| *.xlsx | application/vnd.openxmlformats-officedocument.spreadsheetml.sheet |
| *.txt | text/plain |
| *.html | text/html |
| Video(.avi, .mpg, .mpeg, .mp4) | video/* |
| Audio(.mp3, .wav, etc) | audio/* |
| *.pdf | application/pdf |
| *.csv | text/csv |
| *.css | text/css |
| *.gif | image/gif |
