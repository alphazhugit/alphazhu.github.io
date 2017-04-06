**运行环境：Unity5.4.0f3**


我们都知道在unity3d中烘焙lightmap的作用及方法，可以将实时灯光烘焙在一张或者多张lightmap上，成为静态光照，好处就是可以提高运行效率，但是坏处就是，针对场景中的动态物体，物体与灯光之间没有任何的交互，比如某一片区域被照射为红色的光，这时候一个运动的"Cube"进入红色光区域，自身没有任何变化。
![](http://storage1.imgchr.com/images/light1.png)
![](http://storage1.imgchr.com/images/light22.png)

可以看到上图中，实时灯光，Cube会被灯光照亮，这毫无疑问，烘焙过后Cube是有颜色了，但是cube移动到别的地方依然是保持当时烘焙的位置的效果，说白了，就是烘焙完，不能动，不然就穿帮。
这个时候就需要用到**Light Probe**，下面说一下，Light Probe具体该怎么操作。

>首先需要搭建一个简单的场景,场景中拥有两盏灯（红蓝），中间有一个cube

![](http://storage1.imgchr.com/images/light3.png)


>然后将所有的场景中的所有的场景模型，== 除了那个运动的Cube ==,都标记为**Lightmap Static**

![](http://storage1.imgchr.com/images/light4.png)

>创建Light Probe组件，将Light Probe组件复制，覆盖整个场景

[![](http://storage1.imgchr.com/images/light5.png)](http://imgchr.com/image/zCW)

![](http://storage1.imgchr.com/images/light6.png)

>完毕之后，进行烘焙

![](http://storage1.imgchr.com/images/light71.png)

>烘焙完成之后，将场景中的两站点光源关闭，移动cube查看cube本身的颜色变化

![](http://storage1.imgchr.com/images/light8.gif)