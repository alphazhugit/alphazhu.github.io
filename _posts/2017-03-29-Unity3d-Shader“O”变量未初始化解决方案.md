== variable 'o' used without having been completely initialized Compiling Vertex program ==
Unity3d项目从4.6升级到5.0之后 部分shader会报告以上的错误，解决方案就是添加一个初始化的语句

```
UNITY_INITIALIZE_OUTPUT(v2f, o);

```

来源于[Unity官方论坛](https://forum.unity3d.com/threads/variable-o-used-without-having-been-completely-initialized-compiling-vertex-program.307017/)

这个情况在Unity3d 5.0.2上发现，然而在Unity3d 5.0.4上并未发现。