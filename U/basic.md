##### 概念

csharp 类比 java      .NET 类比 JDK      Mono和IL2CPP(unity自研) 类比 JVM

unity像一个大型IDEA 另外它还自己扩展了.NET工具库(如UnityEngine)

##### using

csharp采用了和c++不同的项目级引用 在c++需要在每个文件里include时 csharp可以支持事先在.csproj文件里定义项目所需库 在单个.cs编写时可直接using而不用include  如在.csproj中声明项目引用UnityEngine库

```
<Reference Include="UnityEngine">
	  //项目使用到的所有库都源于unity_editor下即源于unity编辑器
      <HintPath>D:\Unity_Editor\2022.3.62f3c1\Editor\Data\Managed\UnityEngine\UnityEngine.dll</HintPath>
      <Private>False</Private>
</Reference>
```

##### .NET编译器

###### 传统csc.exe (.NET Framework时期 仅win上可跑(不使用Mone等) 编译对象为单个cs文件)

```
C:\Windows\Microsoft.NET\Framework\v4.0.30319  //一般路径
```

###### 现代dotnet.exe (.NET Core时期 多平台 编译对象为整个项目)

```
dotnet --list-sdks  //命令行查看位置
C:\Program Files\dotnet\...  //一般路径
```

