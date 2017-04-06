```
Shader "Custom/vf2" 
{
 
	SubShader 
	{
		pass
		{
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag

			void vert(in float2 objPos:POSITION,out float4 pos:POSITION,out float4 col:COLOR)
			{
				pos = float4(objPos,0,1);
			}
			void frag(inout float4 col:COLOR)
			{

			//=======================================switch语句=================================================//
//				int i = 0;
//				switch(i)
//				{
//					case 0:
//					col = fixed4(1,0,0,1);
//					break;
//					case 1:
//					col = fixed4(0,1,0,1);
//					break;
//					default:
//					col = fixed4(0,0,1,1);
//					break;
//				}
				//switch语句在教程上说还不支持，但是我在5.3版本中测试，确实已经起作用了，所以5.3版本已经支持switch语句//


			//=====================================While语句=====================================================//
				int i = 0;
				while(i < 10)
				{
					i++;
				}
				if(i == 10)
					col = float4(0,0,0,1);



			//=====================================do While语句=====================================================//

					i= 0;
					do
					{
						i++;
					}
					while(i<10);
					if(i = 10)
					col = float4(1,1,1,1);

			//=====================================for循环语句=====================================================//

			for(i = 0;i<10000000;i++)
			{

			}
			if(i==10000000)
			col = float4(0.5,0.5,0,1);
			//教程上说只能是循环的次数是有限的，次数要<1024次，在Unity5.3中，给出了10000000依然能被支持//
			}



			ENDCG

			
		}
	}
}
```