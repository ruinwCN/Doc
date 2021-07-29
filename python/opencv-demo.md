



一个简单的图片+文字

lib

> ```
> opencv-python
> ```

opencv-python 

函数`cv.imread()`来打开图片

第二个参数是一个标志位，用来指定打开的方式。

cv.IMREAD_COLOR: 加载彩色图片。图片的透明度会被忽略，默认
cv.IMREAD_GRAYSCALE: 用灰度模式
cv.IMREAD_UNCHANGED:包含alpha通道的方式加载图片

默认的font 不多

```
FONT_HERSHEY_COMPLEX = 3

FONT_HERSHEY_COMPLEX_SMALL = 5

FONT_HERSHEY_DUPLEX = 2
FONT_HERSHEY_PLAIN = 1

FONT_HERSHEY_SCRIPT_COMPLEX = 7
FONT_HERSHEY_SCRIPT_SIMPLEX = 6

FONT_HERSHEY_SIMPLEX = 0
FONT_HERSHEY_TRIPLEX = 4

FONT_ITALIC = 16
```

putText

```
.   @param img Image.
.   @param text Text string to be drawn.
.   @param org Bottom-left corner of the text string in the image.
.   @param fontFace Font type, see #HersheyFonts.
.   @param fontScale Font scale factor that is multiplied by the font-specific base size.
.   @param color Text color.
.   @param thickness Thickness of the lines used to draw a text.
.   @param lineType Line type. See #LineTypes
.   @param bottomLeftOrigin When true, the image data origin is at the bottom-left corner. Otherwise,
```

cv.LINE_AA 抗锯齿。



write

```
cv.CV_IMWRITE_JPEG_QUALITY  设置图片格式为.jpeg或者.jpg的图片质量，其值为0---100（数值越大质量越高），默认95
cv.CV_IMWRITE_WEBP_QUALITY  设置图片的格式为.webp格式的图片质量，值为0--100
cv.CV_IMWRITE_PNG_COMPRESSION  设置.png格式的压缩比，其值为0--9（数值越大，压缩比越大），默认为3
```

code

```
def pic():
    src = cv.imread('bg.jpg')

    text = "Hi python"
    context = src.copy()
    numText = "{}{:0>4d}".format("No.", 1)
    cv.putText(context, text, (230, 360), cv.FONT_HERSHEY_SIMPLEX, 1.1, (255, 255, 255), 2, cv.LINE_AA)
    cv.putText(context, numText, (620, 1115), cv.FONT_HERSHEY_SIMPLEX, 1.1, (255, 255, 255), 3, cv.LINE_AA)

    cv.imwrite('1.jpg', context, [int(cv.IMWRITE_JPEG_QUALITY), 70])
```

