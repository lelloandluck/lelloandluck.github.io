---
title: How to use Python PIL in graphic processing"
date: 2018-11-07 17:56:45
tags:
  - python
  - PIL
categories:
  - python
  - PIL
---
PIL(Python Image Library)是python中用来处理图形的专用模块，PIL提供的方法很全面，尽管没有专用图形软件如PS，Croeldraw，Adobe那么强大，但应付日常的图片操作还是足够的，并且借助Python脚本来处理，能极大地提高图形编辑效率，对于喜欢自动化的人来说，怎能缺少对这一块的认识？

PIL的常用操作不外乎这几种 -
+ 新建图片
+ 编辑图片（大小裁剪，几何变换，像素处理，颜色转换，图像增强，类型转换，复制粘贴等）
+ 分裂和合并
+ 点运算
+ 读序列
等等， 这里我就不一一介绍了，直接用几个实际例子来加以说明，这些代码在我本地是能跑通的，如有兴趣，读者可将此代码逐一调试，这样才能建立起深刻的认识，一起来看下 -

<!--More-->

<b><font color='red'/>PIL图像读取和保存
```
from PIL import Image
import tkinter as tk

# picPath = r'C:\Users\Luck_Lello\Pictures\release1.jpg'
picPath = r'C:\Users\Luck_Lello\Desktop\IMG_0072.JPG'
im = Image.open(picPath)
im.save(r'C:\Users\Luck_Lello\Desktop\original.JPG') # 保存图片
width, height = im.size

# 宽高
print "输入出该图片的尺寸 - ", im.size, width, height
# 格式，以及格式的详细描述
print "该图片的格式 - ", im.format, "及其图片描述 - ", im.format_description
```

<b><font color='red'/>PIL新建图像
```
'''
新建图像
'''
newIm = Image.new('RGB', (100, 100), 'red')
newIm.save(r'C:\Users\Luck_Lello\Desktop\tempFolder\001.png')

blcakIm = Image.new('RGB', (200, 100), 'red')
blcakIm.save(r'C:\Users\Luck_Lello\Desktop\tempFolder\002.png')

blcakIm = Image.new('RGBA', (300, 100), '#A0a010')
blcakIm.save(r'C:\Users\Luck_Lello\Desktop\tempFolder\003.png')

blcakIm = Image.new('CMYK', (400, 100), (255, 0, 0, 255))
blcakIm.save(r'C:\Users\Luck_Lello\Desktop\tempFolder\004.jpg')
```

<b><font color='red'/>PIL裁剪图像
```
'''
裁剪图像
'''
picPath2 = r'C:\Users\Luck_Lello\Desktop\avatar1.JPG'
im = Image.open(picPath2)
cropedIm = im.crop((300, 100, 700, 540)) # left, upper, right, below - 后两个值必须比前面两个值要大
cropedIm.save(r'C:\Users\Luck_Lello\Desktop\tempFolder\cropedAvatar1.jpg')
```

<b><font color='red'/>PIL粘贴及几何转换
```
'''
复制与粘贴图像到另一个图像
'''
im = Image.open(picPath2)
cropedIm = im.crop((270, 160, 440, 300)) # left, upper, right, below - 后两个值必须比前面两个值要大
cropedIm = cropedIm.transpose(Image.ROTATE_180)
cropedIm = cropedIm.transpose(Image.FLIP_TOP_BOTTOM)

# cropedIm = 'C:\Users\Luck_Lello\Desktop\tempFolder\cropedAvatar1.jpg'  # paste 方法中第一个参数必须是用4个数字定义的区域 use 4-item box, 不能是一个文件路径，否则类型不对
im.paste(cropedIm, (474, 163)) # paste 方法中第一个参数必须是用4个数字定义的区域 use 4-item box, 不能是一个文件路径，否则类型不对
# im.show() # 查看图片显示效果
```

<b><font color='red'/>PIL图片叠加
```
''''
两张图片相加
'''
picPath3 = r'C:\Users\Luck_Lello\Desktop\tempFolder\001.png'
picPath4 = r'C:\Users\Luck_Lello\Desktop\tempFolder\002.png'
img1 = Image.open(picPath3)
img1 = img1.convert('RGBA')
# img2 = Image.open(r"C:\Users\Luck_Lello\Desktop\tempFolder\lelloAvatar1.jpg")
img2 = Image.open(picPath4)
img2 = img2.convert('RGBA')
img = Image.blend(img2, img2, 0.4) # 貌似要两张一样的图片
# img.show()
```

<b><font color='red'/>PIL图片整合
```
'''
将当前文件夹中的图片拼凑成一整张图片
'''
from os import listdir; from PIL import Image

# 获取当前文件夹中的所有后缀为.jpg 的图片
path = "C:\Users\Luck_Lello\Pictures"
imgs = listdir(path) # 列出目标文件夹里所包含的所有文件名（无论文件还是文件夹）
objImgs = []

# 将目标文件夹里所有后缀为jpg的图片，装入objImgs列表
for fn in imgs:
	if fn.endswith(".jpg"):
		objImgs.append(fn)
print "打印jpg文件列表", objImgs

# 单幅图片尺寸
im = Image.open(path + "\\" + objImgs[0])
mode = im.mode
print "mode is ", mode

width, height = im.size
print "width and height are ", width, height

# 创建空白长图，长度为objImage列表中所有图片的总长
result = Image.new(mode, (width, (height+5) * len(objImgs)), "red")


# 拼接objImgs列表中所有的图片
# for i, im in enumerate(objImgs): # enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中
# 	result.paste(Image.open(path + "\\" + objImgs[i]), box=(0, (i * height)+23))
# 	# result.paste((path + "\\" + objImgs[i]), box=(0, i * height)) # 这样写不行的，提示can't determine region size, use 4-item box, 所以只能讲图片打开后才能识别其region区域

for i in range(1, 10):
	result.paste(Image.open(path + "\\" + objImgs[i]), box=(0, (i * height) + 23))


# 保存拼接长图
result.save(path + "\\" + "result.jpg")
```
