>**效果图**

![yuanjiao.png](http://storage1.imgchr.com/images/yuanjiao.png)

>**代码**

```
Shader "ZX/Yuanjiao" {
    Properties {
        _MainTex ("Base (RGB)", 2D) = "white" {}
        _Range ("range",Range(0,0.24)) = 0.1
    }
    SubShader {
        Tags { "RenderType"="Opaque" }

         Blend SrcAlpha OneMinusSrcAlpha

        pass
        {
	        CGPROGRAM
	        #pragma vertex vert
	        #pragma fragment frag
	        #include "UnityCG.cginc"

	        sampler2D _MainTex;
	        float _Range;

	        struct Input 
	        {
                fixed4 vertex:POSITION;
                fixed2 texcoord :TEXCOORD0;
	        };
	        
	        struct Output 
	        {
	            fixed4 pos:POSITION;
	            fixed2 uv :TEXCOORD0;
	        };

	        Output vert(Input v)
	        {
	            Output o;
	            o.pos = mul(UNITY_MATRIX_MVP,v.vertex);
	            o.uv = v.texcoord;
	            return o;
	        }
	        
	        fixed4 frag(Output i):COLOR
	        {
	            fixed4 c = tex2D(_MainTex,i.uv);
	            float2 uv = i.uv.xy - float2(0.5,0.5);//UV 坐标默认是(0,0)左下角,减0(.5，0.5) 是把UV的中心点移到(0 0) 这样纹理坐标的范围就是 (-0.5,-0.5)~(0.5,0.5)
	            float rx = fmod(uv.x, 0.5 - _Range); //fmod 求余函数,从原点开始 矩形的顶点坐标为0.4
	            float ry = fmod(uv.y, 0.5 - _Range);     
	            float mx = step(0.5 - _Range, abs(uv.x)); // if 0.4>abs(uv.x)?0:1
	            float my = step(0.5 - _Range, abs(uv.y)); 
	            float filletAlpha = 1 - mx * my * step(_Range, length(half2(rx,ry)));  //如果在圆角矩形内，alpha值为1 否则为0。 和0.1做比较 因为圆角半径为0.1 
	            c = fixed4(c.r,c.g,c.b,filletAlpha);
	            return c;
	        }
	        ENDCG
        }
    } 
}

```
**==我在[原文](http://www.cnblogs.com/U-tansuo/p/yuanjiao.html)的基础上加上了圆角的强度==**
