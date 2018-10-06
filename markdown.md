 &nbsp; &nbsp; [一、首行缩进的两种方法](#1)  
&nbsp; &nbsp;  [二、字体颜色及大小由内嵌HTML的方法来实现](#2)  
 &nbsp; &nbsp; [三、表情](#3)  
  &nbsp; &nbsp; [四、换行](#4)  

<h3 id='1'>一、首行缩进的两种方法</h3>

1. 这里实用空格去替代缩进的字符，下面讲的替代包括分号   
2. 把输入法由半角改为全角。 两次空格之后就能够有两个汉字的缩进。

> 半方大的空白用\&ensp;或\&#8194;  
> 全方大的空白用\&emsp;或\&#8195;  
> 不断行的空白格用\&nbsp;或\&#160;  
> 示例：
> 略略略\&#8195;\&#8195;略略略略略略略略略略略略略略略略略略略略略略略略略\&#160;\&#160;略略略略略略略略略略略略略  

**效果：**  
略略略&#8195;&#8195;略略略略略略略略略略略略略略略略略略略略略略略略略&#160;&#160;略略略略略略略略略略略略略

---

<h3 id='2'>二、字体颜色及大小由内嵌HTML的方法来实现</h3>

```
<font face="黑体">我是黑体字</font>  
<font face="微软雅黑">我是微软雅黑</font>  
<font face="STCAIYUN">我是华文彩云</font>   
<font color=#0099ff size=4 face="黑体">color=#0099ff size=4 face="黑体"</font>  
<font color=#00ffff size=4>color=#00ffff</font>  
<font color=gray size=4>color=gray</font>  
size:由1~7之间的数字
```
**上面代码若直接写入编辑框，经由makdown显示效果如下**:  

<font face="黑体">我是黑体字</font>  
<font face="微软雅黑">我是微软雅黑</font>  
<font face="STCAIYUN">我是华文彩云</font>   
<font color=#0099ff size=4 face="黑体">color=#0099ff size=4 face="黑体"</font>  
<font color=#00ffff size=4>color=#00ffff</font>  
<font color=gray size=4>color=gray</font> 

```
<font color=red size=4>4号红色字体
<font color=blue size=4>4号蓝色字体
<font color=gren size=4>4号绿色字体
<font color=gray size=4>4号黑色字体
<font color=gold size=4>4号蓝色字体
<font color=indigo size=4>4号什么色字体
<font color=Crimson size=4>4号什么色字体
<font color=BurlyWood size=4>4号什么色字体
<font color=SlateGray size=4>4号什么色字体
<font color=Wheat size=4>4号什么色字体
<font color=MediumVioletRed size=4>4号什么色字体
```
**显示效果如下:**  

<font color=red size=4>4号红色字体</font>   
<font color=blue size=4>4号蓝色字体</font>   
<font color=gren size=4>4号绿色字体</font>   
<font color=gray size=4>4号黑色字体</font>   
<font color=gold size=4>4号蓝色字体</font>   
<font color=indigo size=4>4号什么色字体</font>   
<font color=Crimson size=4>4号什么色字体</font>   
<font color=BurlyWood size=4>4号什么色字体</font>   
<font color=SlateGray size=4>4号什么色字体</font>   
<font color=Wheat size=4>4号什么色字体</font>   
<font color=MediumVioletRed size=4>4号什么色字体</font> 

<h3 id='3'>三、表情</h3>
&#8195;&#8195;github上找到一篇收录了很多表情的文：  https://github.com/guodongxiaren/README/blob/master/emoji.md 

<h3 id='4'>四、换行</h3>
&#8195;&#8195;直接在要换行的语句最后打上2个空格。
https://github.com/i5ting/tocmd.gem

