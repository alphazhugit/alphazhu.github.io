---
layout:     post
title:      "Unity3d Shader剔除图片白色部分"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

![tichubaise.png](http://imgchr.com/images/tichubaise.png)

    Shader "ZX_AlphaWhite" {
    Properties {
	    _Color ("Main Color", Color) = (1,1,1,1)
	    _MainTex ("Base (RGB) Trans (A)", 2D) = "white" {}
	    _WhiteRange ("WhiteRange",float) = 0.7
    }

    SubShader {
	    Tags {"Queue"="Transparent" "IgnoreProjector"="True"      "RenderType"="Transparent"}
	ColorMask 0

    CGPROGRAM
    #pragma surface surf Lambert alpha
    sampler2D _MainTex;
    fixed4 _Color;
    float _WhiteRange;

    struct Input {
	float2 uv_MainTex;
    };

    void surf (Input IN, inout SurfaceOutput o) {
	fixed4 c = tex2D(_MainTex, IN.uv_MainTex);
	o.Albedo = c.rgb;
	if(c.r >= _WhiteRange && c.g >= _WhiteRange && c.b >= _WhiteRange )//判断RGB的值接近白色//
	{
		o.Alpha = 0;
	}
	else
	{
		o.Alpha = 1;
	}
	o.Emission = c.rgb;
    }
    ENDCG
    }

    Fallback "Legacy Shaders/Transparent/VertexLit"
    }
