---
layout: post
title: Tesseract 使用记录
category: tools
tags: OCR tesseract
bg: tesseract.png
---
对tesseract的使用源于项目开发中要将PDF文件转换为双层PDF，然后提取文件内容，存入ES供文档检索使用。本文粗略地记录了Tesseract的搭建和使用。

PDF --> tiff --> 双层PDF

1. imagemagick 转PDF为TIFF
2. tesseract 转TIFF为双层PDF

3. 安装tesseract

```shell
sudo apt install tesseract-ocr
sudo apt install libtesseract-dev
```

2. 操作命令operation

```shell
# PDF转TIFF
magick convert test.pdf -density 300 -depth 8 -strip -background white -alpha off image.tiff
# TIFF转双层PDF
tesseract -l chi_sim test.tiff test pdf 

magick convert E:\test\data\test.png -density 300 -depth 8 -strip -background white -alpha off E:\test\data\test.png.tiff 

magick convert -geometry 3600x3600 -density 300 -quality 100 -depth 8 -strip -background white -alpha off test.pdf test.tiff

tesseract -l chi_sim com.cyvation.exp0.tif com.cyvation.exp0  batch.nochop makebox

tesseract cyvation.stsong.exp0.tif cyvation.stsong.exp0 nobatch box.train

```

3. Java dependency

4. pdfbox 读取双层PDF文本
5. tesseract-platform TIFF转双层PDF
6. im4java/jmagick PDF转TIFF

7. 训练

工具:

jTessBoxEditor 

imagemagick

步骤:

1. 转tif格式图片

   命名规范[lang].[fontname].exp[num].tif  

   ```shell
   magick convert -geometry 3600x3600 -density 300 -quality 100 lang.font.exp0.pdf lang.font.exp0.tif
   ```

2. 生成box文件

```shell
tesseract -l chi_sim lang.font.exp0.tif lang.font.exp0 batch.nochop makebox

# 用jTEssBoxEditor编辑box文件
```

3. 生成tr训练数据

```shell
tesseract lang.font.exp0.tif lang.font.exp0 nobatch box.train
```

4. 生成unicharset

```shell
unicharset_extractor lang.font.exp0.box
```

5. 生成字库文件lang.font_properties

```shell
echo "font 0 0 0 0 0" > lang.font_properties
```

6. mftraining

```shell
mftraining -F lang.font_properties -U unicharset -O lang.unicharset lang.font.exp0.tr
```

7. cntraining

```shell
cntraining lang.font.exp0.tr

```

8. 更改文件名

```shell
mv inttemp lang.inttemp
mv pffmtable lang.pffmtable
mv normproto lang.normproto
mv shapetable lang.shapetable

```

9. 生成tessdata文件

```shell
combine_tessdata lang.

```

10. 拷贝训练好的数据到tessdata文件目录

11. 同时使用多个字体库提取文本

```shell
# 使用+链接语言,没有空格
tesseract -l chi_sim+por aaaa.tif out 

```

12. Tess4j

设置环境变量

TESSDATA_PREFIX	C:\jTessBoxEditorFX\tesseract-ocr\tessdata

