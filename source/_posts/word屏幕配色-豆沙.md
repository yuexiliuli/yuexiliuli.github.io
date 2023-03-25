---
title: word屏幕配色-豆沙
urlname: ot93et
date: '2021-05-15 21:28:04 +0800'
tags: [word]
categories: [word]
---

# word 屏幕配色-豆沙

如果只针对 Word 的话，可以尝试在"开发工具"选项卡中新建一个宏，复制下面的内容进行运行：

```visual
Sub Screen_Color_green_bean()
'
' Screen_Color_green_bean 宏
'
'
ActiveDocument.Background.Fill.Visible = msoTrue
ActiveDocument.Background.Fill.ForeColor.RGB = RGB(199, 237, 204)
ActiveDocument.Background.Fill.Solid
ActiveDocument.ActiveWindow.View.DisplayBackgrounds = True

End Sub
```

其中 RGB 部分的三个数字，您可以按照自己的需求进行调整，看下效果。

```c
绿豆沙色能有效的减轻长时间用电脑的用眼疲劳！
色调：85，饱和度：123，亮度：205；
RGB颜色红：199，绿：237，蓝：204；
十六进制颜色：#C7EDCC或用#CCE8CF

其他几种电脑窗口视力保护色：

银河白 #FFFFFF RGB(255, 255, 255)

杏仁黄 #FAF9DE RGB(250, 249, 222)

秋叶褐 #FFF2E2 RGB(255, 242, 226)

胭脂红 #FDE6E0 RGB(253, 230, 224)

青草绿 #E3EDCD RGB(227, 237, 205)

海天蓝 #DCE2F1 RGB(220, 226, 241)

葛巾紫 #E9EBFE RGB(233, 235, 254)

极光灰 #EAEAEF RGB(234, 234, 239)

以上来源：https://www.iteye.com/blog/igogo007-2432004

我们可以设置Dev、VS2017、Quartus II等等软件的背景颜色为护眼色，对缓解视疲劳有好处。
```
