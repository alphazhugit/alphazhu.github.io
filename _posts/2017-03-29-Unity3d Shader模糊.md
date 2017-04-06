![blur.png](http://imgchr.com/images/blur.png)

```
Shader "Custom/ObjectBlur" {
        Properties {
            _MainTex ("Base (RGB)", 2D) = "white" {}
            _uvOffset("uvOffset",Range(0,0.01)) = 0
        }
        SubShader
        {
             Tags{"Queue"="Transparent"}
             blend srcalpha oneminussrcalpha
         
            pass
            {
                CGPROGRAM
                #pragma vertex vert
                #pragma fragment frag
               
                #include "UnityCG.cginc"
                sampler2D _MainTex;
                float4 _MainTex_ST;
                float _uvOffset;
               
                struct v2f {
                    float4  pos : SV_POSITION;
                    float2  uv : TEXCOORD0;
                } ;
                v2f vert (appdata_base v)
                {
                    v2f o;
                    o.pos = mul(UNITY_MATRIX_MVP,v.vertex);
                    o.uv =  TRANSFORM_TEX(v.texcoord,_MainTex);
                    return o;
                }
                float4 frag (v2f i) : COLOR
                {
                    fixed4 s1 = tex2D(_MainTex,i.uv + fixed2(_uvOffset,0));
                    fixed4 s2 = tex2D(_MainTex,i.uv + fixed2(-_uvOffset,0));
                    float4 s3 = tex2D(_MainTex,i.uv + fixed2(0,_uvOffset));
                    float4 s4 = tex2D(_MainTex,i.uv + fixed2(0,-_uvOffset));
                   
                    float4 texCol = tex2D(_MainTex,i.uv);
                    float4 outp;
                   
                    float pct=0.2;
                    outp = texCol* (1- pct*4) + s1* pct + s2* pct+ s3* pct + s4* pct;
                    return outp;
                }
                ENDCG
            }
        }
    }
```
转载自<a href="http://blog.sina.com.cn/s/blog_471132920101gued.html" target="_blank">风宇冲博客</a>