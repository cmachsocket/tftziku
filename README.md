这是一个通过单片机在各种屏幕上显示中文的解决方案：

你可能要在TFT屏幕进行显示，也可能要在点阵屏进行显示；无论哪一种都涉及到对于中文的处理，从个人经验上来说，各种常规方法，要么繁琐，要么无法满足各种文字通讯的显示，所以提供软字库的方案就有点必要。



==================== ======================================== 


| 文件名                  | 说明  |    
|:------------------|:-----------------------------------------------------------------------|
| font.rst                | 整体对于解决方案的说明  |
| src/getunicode.py       | 使用Python 创建字库  |
| src/main.c              | 在esp32中调用字库并显示到tft屏幕  |
| fileo.h fileo.c         | 暂时保留在项目说供以后使用  |
 


性能指标：

GB2312 字符集创建的字库16号字体，tft_espi 大约1M，本方案507k; tft_espi装载并显示中文需要2秒以上，本方案大约0.3秒显示完。


提请注意：这只是一个小型的方案，提供思路和代码参考，并且经过验证可行；如果你要使用是在有些地方是需要配套修改的