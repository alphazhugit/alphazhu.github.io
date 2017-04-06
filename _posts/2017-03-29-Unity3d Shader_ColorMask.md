---
layout:     post
title:      "Unity3d Shader_ColorMask"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

>**ColorMask RGB | A | 0 | any combination of R, G, B, A
Set color writing mask. Writing ColorMask 0 turns off rendering to all color channels.
      设置颜色写遮罩。设置为0将关闭所有颜色通道的渲染**

**一个最简单的ColorMask的shader**

```
Shader "Depth Mask" {
 
SubShader{
 
ColorMask 0
 
Pass {}
 
}
 
}
```
==如果用了多维材质,你需要像下面这样写==

```
Shader "Depth Mask Complex"
 
{
 
SubShader
 
{
 
Tags {"Queue" = "Background"}
 
Blend SrcAlpha OneMinusSrcAlpha
 
Lighting Off
 
ZWrite On
 
ZTest Always
 
Pass
 
{
 
Color(0,0,0,0)
 
}
 
}
 
}
```

==效果图==

![colormask.png](http://storage1.imgchr.com/images/colormask.png)

代码转载自[BobbyKim](http://www.manew.com/thread-46946-1-1.html?_dsign=e84c1d51)