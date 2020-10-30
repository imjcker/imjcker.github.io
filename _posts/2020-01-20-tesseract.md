---
layout: post
title: Tesseract 使用记录
category: tools
tags: OCR tesseract
bg: tesseract.png
---

# 2020-01-20-tesseract

对tesseract的使用源于项目开发中要将PDF文件转换为双层PDF，然后提取文件内容，存入ES供文档检索使用。本文粗略地记录了Tesseract的搭建和使用。

PDF --&gt; tiff --&gt; 双层PDF

1. imagemagick 转PDF为TIFF
2. tesseract 转TIFF为双层PDF
3. 安装tesseract

```text
sudo apt install tesseract-ocr
sudo apt install libtesseract-dev
```

1. 操作命令operation

```text
# PDF转TIFF
magick convert test.pdf -density 300 -depth 8 -strip -background white -alpha off image.tiff
# TIFF转双层PDF
tesseract -l chi_sim test.tiff test pdf 

magick convert E:\test\data\test.png -density 300 -depth 8 -strip -background white -alpha off E:\test\data\test.png.tiff 

magick convert -geometry 3600x3600 -density 300 -quality 100 -depth 8 -strip -background white -alpha off test.pdf test.tiff

tesseract -l chi_sim com.cyvation.exp0.tif com.cyvation.exp0  batch.nochop makebox

tesseract cyvation.stsong.exp0.tif cyvation.stsong.exp0 nobatch box.train
```

1. Java dependency
2. pdfbox 读取双层PDF文本
3. tesseract-platform TIFF转双层PDF
4. im4java/jmagick PDF转TIFF
5. 训练

工具:

jTessBoxEditor

imagemagick

步骤:

1. 转tif格式图片

   命名规范\[lang\].\[fontname\].exp\[num\].tif

   ```text
   magick convert -geometry 3600x3600 -density 300 -quality 100 lang.font.exp0.pdf lang.font.exp0.tif
   ```

2. 生成box文件

```text
tesseract -l chi_sim lang.font.exp0.tif lang.font.exp0 batch.nochop makebox

# 用jTEssBoxEditor编辑box文件
```

1. 生成tr训练数据

```text
tesseract lang.font.exp0.tif lang.font.exp0 nobatch box.train
```

1. 生成unicharset

```text
unicharset_extractor lang.font.exp0.box
```

1. 生成字库文件lang.font\_properties

```text
echo "font 0 0 0 0 0" > lang.font_properties
```

1. mftraining

```text
mftraining -F lang.font_properties -U unicharset -O lang.unicharset lang.font.exp0.tr
```

1. cntraining

```text
cntraining lang.font.exp0.tr
```

1. 更改文件名

```text
mv inttemp lang.inttemp
mv pffmtable lang.pffmtable
mv normproto lang.normproto
mv shapetable lang.shapetable
```

1. 生成tessdata文件

```text
combine_tessdata lang.
```

1. 拷贝训练好的数据到tessdata文件目录
2. 同时使用多个字体库提取文本

```text
# 使用+链接语言,没有空格
tesseract -l chi_sim+por aaaa.tif out
```

1. Tess4j

设置环境变量

TESSDATA\_PREFIX C:\jTessBoxEditorFX\tesseract-ocr\tessdata

