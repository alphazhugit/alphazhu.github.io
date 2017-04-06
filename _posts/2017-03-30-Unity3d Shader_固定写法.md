```
Shader "ZX/fx1" {  
    properties
    {
        _Color("Color",Color) = (0,0,0,1)
        _Ambient("Ambient",Color) = (0.2,0.2,0.2,1)
        _Specular("specular",Color) = (1,1,1,1)
        _Shininess("Shininess",range(0,0.5)) = 0.2
        _Emission("emission",Color) = (1,1,1,1)
        _MainTex("maintex",2D) = "white"{}
        _SecTex("SexTex",2D) = "white"{}
        _Constant("constantColor",Color) = (1,1,1,0.3)

//         _MainTex("maintex",2D) = ""
//        _SecTex("SexTex",2D) = ""
//        _Constant("constantColor",Color) = (1,1,1,0.3)

//这样的方式是不能正确运行的,如果想正确运行，需要把   _Constant("constantColor",Color) = (1,1,1,0.3) 放到那两句上面//
//         _Constant("constantColor",Color) = (1,1,1,0.3)
//         _MainTex("maintex",2D) = ""
//        _SecTex("SexTex",2D) = ""
//        


    }

        SubShader 
        {
       		Tags{"Queue" = "Transparent"}
            pass
            {
            	blend srcalpha oneminussrcalpha
//            	
//                color(1,0,0,1)
//                color[_Color]//直接输出一个可自定义的颜色//
                material
                {
                    diffuse[_Color]//漫反射//
                    ambient[_Ambient]//环境光//
                    specular[_Specular]//高光//
                    shininess[_Shininess]//高光光斑的大小//
                    emission[_Emission]//自发光//
                }

                lighting on//打开光照系统//
                separatespecular on//开启镜面高光//

                settexture[_MainTex]//一个settexture只能带一个参数//
                {
                    combine texture * primary double//combine命令是用当前的纹理乘以之前的顶点光照信息,primary代表之前输出的所有颜色值,double是2倍，quad是4倍//
                }

                settexture[_SecTex]
                {
                	constantColor[_Constant]//命令为"constantColor"，下面在使用的时候直接用"constant"进行相乘//
                    combine texture * previous double,texture * constant//逗号后面的参数“texture为取alpha的值”,代表取当前的alpha值，之前的alpha都失效//
                }
            }

        }
}

```