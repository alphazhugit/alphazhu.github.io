---
layout:     post
title:      "Unity3d固定写法Shader设置透明"
date:       2017-04-14 16:19:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

关于unity3d固定写法的shader确实研究的不多，群里有朋友问起如何使用自定义的变量进行设置透明，经过本人研究一番，实在没找到办法传入自定义的变量，貌似之所以被称之为固定写法就是因为所有的写法都是固定的关键词，只要我写入自定义的变量就会报错，最后实在没办法，传入了这个固定的颜色变量*Constant*，利用这个变量的z值或者说这个颜色的a值来进行控制整体的透明度。

```
Shader "ZX/Mobile ParticlesAlpha Blended" {
Properties {
	_MainTex ("Particle Texture", 2D) = "white" {}
	_Constant("constantColor",Color) = (1,1,1,1)
}

Category {
	Tags { "Queue"="Transparent" "IgnoreProjector"="True" "RenderType"="Transparent" }
	Blend SrcAlpha OneMinusSrcAlpha
	Cull Off Lighting Off ZWrite Off Fog { Color (0,0,0,0) }
	
	BindChannels {
		Bind "Color", color
		Bind "Vertex", vertex
		Bind "TexCoord", texcoord
	}
	
	SubShader {
		Pass {
			SetTexture [_MainTex] {
			constantColor[_Constant]
				combine texture,texture * constant
			}
		}
	}
}
}


```

[一些参数说明可以参考本博客的另一片文章](http://alphazhu.top/2016/03/02/Unity3d-Shader_%E5%9B%BA%E5%AE%9A%E5%86%99%E6%B3%95/)

[另外链接一下一位开发者博客](http://www.bbsmax.com/A/Gkz1RpRGJR/)



