---
layout:     post
title:      "Unity3d_Vertex&Fragment_BaseShadow"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

```
Shader "ZX/BaseVertFragmentShadow"
{
    Properties
    {
        _DiffuseTexture ("Diffuse Texture", 2D) = "white" {}
        _DiffuseTint ( "Diffuse Tint", Color) = (1, 1, 1, 1)
    }
 
    SubShader
    {
        Tags { "RenderType"="Opaque" }
 
        pass
        {     
            Tags { "LightMode"="ForwardBase"} //第一步//
 
            CGPROGRAM
 
            #pragma vertex vertShadow
            #pragma fragment fragShadow
            #pragma multi_compile_fwdbase//第二步//
 
            #include "UnityCG.cginc"
            #include "AutoLight.cginc" //第三步//
 
            sampler2D _DiffuseTexture;
            float4 _DiffuseTint;
            float4 _LightColor0; //第四步//
 
            struct v2f
            {
                float4 pos : SV_POSITION;
                float2 uv : TEXCOORD2;
                LIGHTING_COORDS(0, 1) //第五步//
            };
 
            v2f vertShadow(appdata_base v)
            {
                v2f o;
                o.pos = mul(UNITY_MATRIX_MVP, v.vertex);
                o.uv = v.texcoord;
                TRANSFER_VERTEX_TO_FRAGMENT(o); //第六步//
                return o;
            }
 
            float4 fragShadow(v2f i) : COLOR
            {       
                       
				float4 diffuse = tex2D(_DiffuseTexture, i.uv);
                float4 diffuseTerm = _LightColor0 * LIGHT_ATTENUATION(i); //第七步//
                float4 finalColor = diffuseTerm * diffuse * _DiffuseTint;
                return finalColor;

            }
 
            ENDCG
        }      
 
    }
    FallBack "Diffuse"
}
```