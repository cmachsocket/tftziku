这是一个通过单片机在各种屏幕上显示中文的解决方案：

你可能要在TFT屏幕进行显示，也可能要在点阵屏进行显示；无论哪一种都涉及到对于中文的处理，从个人经验上来说，各种常规方法，要么繁琐，要么无法满足各种文字通讯的显示，所以提供软字库的方案就有点必要。



注意：

2024.03.26 做了一些对比测试 ：在esp8266下，tft_espi的RAM占用要优于arduino_gfx,但是磁盘占用要稍微多一点，在esp32下两者相差不大；其二，这些显示驱动中不相干的驱动对于ram和flash空间基本没有任何影响，相差只有0.1%；

详情可以访问 ：  https://blog.csdn.net/platform/article/details/137037441

2024.03.21 同时支持了两个屏幕驱动库tft_eSPI和arduino_gfx ,新增arduino_gfx是因为这个驱动除了支持tft外还支持led。

2024.01.18 修改核心文件命名，避免相关冲突 
 
2024.01.16 字库生成后自动上传到单片机；

2024.01.16 字库的创建和单片机中调用都升级到了64位；

2024.01.15 目前字库的生成和单片机调用都支持了32进制编码；未来是否支持64进制编码待定。


==================== ======================================== 


| 文件名                  | 说明  |    
|:------------------|:-----------------------------------------------------------------------|
| font.rst                | 整体对于解决方案的说明  |
| src/getunicode.py       | 使用Python 创建字库  |
| src/getunicodeV2.py       | 使用Python 创建字库 (目前在用版本) |
| src/getunicodeV3.py       | 对创建字库换成了32进制版本，压缩18%的体积，(目前在用版本)|
| src/main.c              | 在esp32中调用字库并显示到tft屏幕  |
| lib/        | 宋体不同编码和字号的字库  | 


性能指标：

GB2312 字符集创建的字库16号字体，tft_espi 的.vlw格式大约1M，本方案从507k升级到了360k; tft_espi装载并显示中文需要2秒以上，本方案大约0.3秒显示完。

示例使用的是12号字体64进制编码.



| 字号                  |   16进制(KB)   |    32进制(KB)       |   64进制(KB)   | 
|:------------------|:-------------|:------------|:---------------------|
| 12                    | 302        | 257  |   213     |
| 14                    | 404        | 331  |   287  |
| 16                    | 507        | 419  |   360  |


提请注意：这只是一个小型的方案，提供思路和代码参考，并且经过验证可行；如果你要使用是在有些地方是需要配套修改的

使用说明：

字体的创建
 
 - 直接运行 getuncodeV3.py 创建相应的GB2312 完整字符的字库文件
 - 如果只打算创建部分字符的字库，那么就对createFont( FONT_ALL,"P", "simsun.ttc", 12,64) 中的变量FONT_ALL进行赋值，然后运行生产字库；
 - getuncodeV3.py 创建的字库为了管理方便，命名使用"font/x_f"+str(fontSize)+"_b"+str(binType)+".font" 方式进行存储；
 - 改名字库文件为x.font，放置到arduino项目的data目录下上传到单片机就可以调用了；
 
 写到上面时，我去改了一下代码，实现了字库创建后自动上传到esp32中，牛逼如我，哈哈哈哈

 相较于使用PCtoLCD2018对字体进行编码过程，本方案实际使用起来更简单方便，要想更方便还可以实现字体生成后自动上传到单片机。

esp32上调用

 - 包含文件xfont.h；
 - 创建 TFT_eSPI tft = TFT_eSPI();
 - 调用 DrawStr2(tft,10, 2, "空气温度：", TFT_GREEN); 在屏幕上输出文字
 - 请参考Main.cpp文件


