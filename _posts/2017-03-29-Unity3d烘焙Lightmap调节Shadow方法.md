> 众所周知在Unity3d中调节Shadow的方法就是在灯光的属性参数下面调节

![shdow1.png](http://storage1.imgchr.com/images/shdow1.png)

---

> 然而在烘焙Lightmap的情况下，Shadow的强度却不**受灯光这个参数的变化而变化**，在Bake Lightmap的时候投影的参数默认就是“1”

>在Shadow Strength为“0.05”和“1”的情况下，Bake出来的阴影的强度是一样的

![shdow2.png](http://storage1.imgchr.com/images/shdow2.png)

---

>另外,在烘焙完Light map之后需要关闭**场景中的灯光**和**系统默认灯光**，分别在以下位置：

![shdow3.png](http://storage1.imgchr.com/images/shdow3.png)

---

>对于调节Bake Lightmap下Shadow的方法就是调节**Ambient Color**

![shdow4.png](http://storage1.imgchr.com/images/shdow4.png)

>这里可能有个问题，就是在将环境光的颜色调亮之后，模型本身的颜色也会随之变量，这个时候可以调节模型本身的Shader的颜色的RGB值即可