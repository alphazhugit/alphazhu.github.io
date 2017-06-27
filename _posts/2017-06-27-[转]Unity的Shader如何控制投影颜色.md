---
layout:     post
title:      "[转]Unity的Shader如何控制投影颜色"
date:       2017-06-27 19:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

细节慢慢补充，有几个需要注意的地方，必须要有接收投影的pass也就是Name是ShadowCollector的，必须添加#pragma multi_compile_fwdbase，物体的着色器必须有Tag 里面的 {"LightMode" = "ForwardBase"}。处理投影要在接收投影的物体shader里。

随后是三个宏。

SHADOW_COORDS这个宏后面的参数是指第几个通道，不要和其他的出现冲突，也就是要改变投影的颜色话必须要占用一个通道。

在顶点处理器里调用TRANSFER_SHADOW。

然后像素着色器调用SHADOW_ATTENUATION，返回的就是这个像素是否存在阴影中。

可改变投影颜色的范例如下。

```
Shader "Example/ShadowColor" 
{
    Properties 
    {
        _MainColor("MainColor",color) = (0.5,0.5,0.5,1)
        _ShadowColor("ShadowColor",color) = (1,1,0,1)
    }
    SubShader 
    {
        Tags {"RenderType"="Opaque"}
        Pass 
        {
                Tags {"LightMode" = "ForwardBase"}
                
            CGPROGRAM
            #pragma multi_compile_fwdbase
            #include "UnityCG.cginc"
            #include "Lighting.cginc"
            #include "AutoLight.cginc"
            
            #pragma vertex vertexMain
            #pragma fragment fragmentMain
            
            fixed4 _MainColor;
            fixed4 _ShadowColor;

            struct VertexToFragment 
            {
                float4 pos         : SV_POSITION;
                half4 uv          : TEXCOORD0;
                SHADOW_COORDS(1)
            };
            
            VertexToFragment vertexMain (appdata_full v) 
            {
                VertexToFragment result = (VertexToFragment)0;
                result.pos = mul(UNITY_MATRIX_MVP, v.vertex);
                result.uv.xy =  v.texcoord.xy;
                TRANSFER_SHADOW(result)
                return result;
            }
            
            fixed4 fragmentMain (VertexToFragment i) : COLOR0 
            {
                float attenuation = SHADOW_ATTENUATION(i);
                return lerp(_MainColor*_ShadowColor,_MainColor,attenuation);
            }

            ENDCG
        }
        
        Pass
        {
            Name "ShadowCollector"
            Tags { "LightMode" = "ShadowCollector" }
            
            Fog {Mode Off}
            ZWrite On ZTest LEqual

            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #pragma multi_compile_shadowcollector

            #define SHADOW_COLLECTOR_PASS
            #include "UnityCG.cginc"

            struct appdata { float4 vertex : POSITION;};
            struct v2f { V2F_SHADOW_COLLECTOR;};

            v2f vert (appdata v)
            {
                v2f o;
                TRANSFER_SHADOW_COLLECTOR(o)
                return o;
            }

            fixed4 frag (v2f i) : SV_Target { SHADOW_COLLECTOR_FRAGMENT(i)}
            ENDCG
        }
   
    } 

}

```

[原文链接](http://www.cnblogs.com/sitt/p/4699369.html)
