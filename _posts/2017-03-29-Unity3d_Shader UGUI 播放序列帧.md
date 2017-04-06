![ScreenGif91715.gif](http://storage1.imgchr.com/images/ScreenGif91715.gif)
```
//感谢Dr.Dog提供序列算法//
Shader "ZX/UIFlow"
 {
     Properties
     {
        _Color ("Tint", Color) = (1,1,1,1)
        _MainTex ("Sprite Texture", 2D) = "white" {}
        _SpriteRowCount("RowCounts",float) = 0
		_SpriteColumnCount("ColumnCounts",float) = 0
		_Speed("AnimationSpeed",Range(0.01,10)) = 4

     }
 
     SubShader
     {
         Tags
         { 
             "Queue"="Transparent"  "IgnoreProjector"="True"  "RenderType"="Transparent" 
         }
          Cull Off
         Lighting Off
         Blend SrcAlpha OneMinusSrcAlpha

         Pass
         {
         CGPROGRAM
             #pragma vertex vert
             #pragma fragment frag
 
             #include "UnityCG.cginc"
             #include "UnityUI.cginc"
 

            fixed4 _Color;
			uniform sampler2D _MainTex;
            uniform half _SpriteRowCount;
			uniform half _SpriteColumnCount;
			uniform half _Speed;

             struct appdata_t
             {
                 half4 vertex   : POSITION;
                 half4 color    : COLOR;
                 fixed2 texcoord : TEXCOORD0;
             };
 
             struct v2f
             {
                 half4 vertex   : SV_POSITION;
                 fixed4 color    : COLOR;
                 fixed2 texcoord  : TEXCOORD0;
                 float4 worldPosition : TEXCOORD2;
             };
             

             v2f vert(appdata_t IN)
             {
                 v2f OUT;
                 OUT.worldPosition = IN.vertex;
                 OUT.vertex = mul(UNITY_MATRIX_MVP, OUT.worldPosition);
                 OUT.texcoord = IN.texcoord;
                 return OUT;
             }
 
             fixed4 frag(v2f IN) : SV_Target
             {
          
             		fixed totalSpriteCount = _SpriteRowCount * _SpriteColumnCount;
					fixed rowAvgPercent = 1 / _SpriteColumnCount;
					fixed columnAvgPercent = 1 / _SpriteRowCount;
					fixed SpriteIndex = fmod(_Time.y * _Speed,totalSpriteCount);
					SpriteIndex = floor(SpriteIndex);
					fixed columnIndex = fmod(SpriteIndex,_SpriteColumnCount);
					fixed rowIndex = SpriteIndex / _SpriteColumnCount;
					rowIndex = _SpriteRowCount - 1 - floor(rowIndex);
					fixed2 spriteUV = IN.texcoord;
					spriteUV.x = (spriteUV.x + columnIndex) * rowAvgPercent;
					spriteUV.y = (spriteUV.y + rowIndex) * columnAvgPercent;
					fixed4 col = tex2D(_MainTex,spriteUV) * _Color;
					return col;

             }
         ENDCG
         }
     }
 }

```