---
layout:     post
title:      "Unity3d_Shader_RimEdgeGlow"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

![](http://storage1.imgchr.com/Co4MR.png)
```
// Upgrade NOTE: replaced '_World2Object' with 'unity_WorldToObject'

Shader "ZX/RimEdgeGlow" {  
    Properties {  
    	_MainColor("Rim Color",COLOR) = (1,1,1,1)
    	_Scale("Scale",range(1,8)) =2
    	_Outer("Outer",range(0,1)) = 0.2
    }  
    SubShader {  
        Tags { "RenderType"="Opaque" "queue" = "Transparent"}  
        LOD 300  

        //=================卡通渲染PASS======================
        Pass {  
            blend srcalpha one
            zwrite off

            CGPROGRAM  
              
            #pragma vertex vert  
            #pragma fragment frag  
              
            #include "UnityCG.cginc"  
             //只有点乘在frag做

             float4 _MainColor;
             float _Scale;
             float _Outer;

            struct a2v {  
                float4 vertex : POSITION;  
                float3 normal : NORMAL;  
                float4 texcoord : TEXCOORD0;  
            };  
              
            struct v2f {  
                float4 pos : POSITION; 
                float3 normal:TEXCOORD0;
                float4 vertex:TEXCOORD1; 
            };  
              
            v2f vert(a2v v) {  

            	v.vertex.xyz += v.normal*_Outer;

                v2f o;
                o.pos = mul(UNITY_MATRIX_MVP, v.vertex);  

                o.vertex = v.vertex;
                o.normal = v.normal;
                return o;  
            }  
              
            float4 frag(v2f i) : COLOR { 
            	//float3 N = normalize( mul( i.normal ,(float3x3)_World2Object));
              	float3 N =  UnityObjectToWorldNormal(i.normal);
                float3 V =  normalize( WorldSpaceViewDir(i.vertex));
                float bright =pow(saturate(dot(N,V)),_Scale);

                _MainColor.a *= bright;

                return _MainColor;  

            }  
              
            ENDCG  
        }  

        //========================================
        Pass {  
            Tags { "LightMode" = "ForwardBase" }  
              
//            Cull Back  
//            Lighting On  

            blend one one // 当前都不渲染
//            blend srcalpha oneminussrcalpha
            zwrite off

            CGPROGRAM  
              
            #pragma vertex vert  
            #pragma fragment frag  
              
            #include "UnityCG.cginc"  
             //只有点乘在frag做

             float4 _MainColor;
             float _Scale;

            struct a2v {  
                float4 vertex : POSITION;  
                float3 normal : NORMAL;  
                float4 texcoord : TEXCOORD0;  
            };  
              
            struct v2f {  
                float4 pos : POSITION; 
                float3 normal:TEXCOORD0;
                float4 vertex:TEXCOORD1; 
            };  
              
            v2f vert(a2v v) {  
                v2f o;
                o.pos = mul(UNITY_MATRIX_MVP, v.vertex);  

                o.vertex = v.vertex;
                o.normal = v.normal;
                return o;  
            }  
              
            float4 frag(v2f i) : COLOR { 
            	//float3 N = normalize( mul( i.normal ,(float3x3)_World2Object));
              	float3 N =  UnityObjectToWorldNormal(i.normal);
                float3 V =  normalize( WorldSpaceViewDir(i.vertex));
                float bright =pow( 1.0 - saturate(dot(N,V)),_Scale);

                return _MainColor*  bright;  
            }  
              
            ENDCG  
        }  
    }   
    FallBack "Diffuse"  
}  
```