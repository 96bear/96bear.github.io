+++
title = 'Net容器内缺少libSkiaSharp'
date = 2024-01-08T11:35:36+08:00
draft = false

+++

### 情况

ABP项目中引入了aspose.words，Win下正常运行，通过DockerFile构造并运行的容器报错缺少libSkiaSharp，完整报错请看文章末。

### 解决方法

在打包模块.csproj中新增引入：

~~~
    <ItemGroup>
        <PackageReference Include="SkiaSharp" Version="2.88.6" />
        <PackageReference Include="SkiaSharp.NativeAssets.Linux.NoDependencies" Version="2.88.6" />
    </ItemGroup>
~~~

2.88.0版本存在任意代码执行漏洞，所以使用2.88.6版本。

### 完整报错

~~~
~# docker logs 4a82 --since 2m

Unhandled exception. System.TypeInitializationException: The type initializer for 'SkiaSharp.SKObject' threw an exception.

---> System.DllNotFoundException: Unable to load shared library 'libSkiaSharp' or one of its dependencies. In order to help diagnose loading problems, consider using a tool like strace. If you're using glibc, consider setting the LD_DEBUG environment variable:

/app/runtimes/linux-x64/native/libSkiaSharp.so: cannot open shared object file: No such file or directory

/usr/share/dotnet/shared/Microsoft.NETCore.App/7.0.14/libSkiaSharp.so: cannot open shared object file: No such file or directory

/app/libSkiaSharp.so: cannot open shared object file: No such file or directory

/app/runtimes/linux-x64/native/liblibSkiaSharp.so: cannot open shared object file: No such file or directory

/usr/share/dotnet/shared/Microsoft.NETCore.App/7.0.14/liblibSkiaSharp.so: cannot open shared object file: No such file or directory

/app/liblibSkiaSharp.so: cannot open shared object file: No such file or directory

/app/runtimes/linux-x64/native/libSkiaSharp: cannot open shared object file: No such file or directory

/usr/share/dotnet/shared/Microsoft.NETCore.App/7.0.14/libSkiaSharp: cannot open shared object file: No such file or directory

/app/libSkiaSharp: cannot open shared object file: No such file or directory

/app/runtimes/linux-x64/native/liblibSkiaSharp: cannot open shared object file: No such file or directory

/usr/share/dotnet/shared/Microsoft.NETCore.App/7.0.14/liblibSkiaSharp: cannot open shared object file: No such file or directory

/app/liblibSkiaSharp: cannot open shared object file: No such file or directory

at SkiaSharp.SkiaApi.sk_version_get_milestone()

at SkiaSharp.SkiaSharpVersion.get_Native()

at SkiaSharp.SkiaSharpVersion.CheckNativeLibraryCompatible(Boolean throwIfIncompatible)

at SkiaSharp.SKObject..cctor()

--- End of inner exception stack trace ---

at SkiaSharp.SKObject.DeregisterHandle(IntPtr handle, SKObject instance)

at SkiaSharp.SKObject.set_Handle(IntPtr value)

at SkiaSharp.SKNativeObject.Dispose(Boolean disposing)

at SkiaSharp.SKObject.Dispose(Boolean disposing)

at SkiaSharp.SKBitmap.Dispose(Boolean disposing)

at SkiaSharp.SKNativeObject.Finalize()
~~~

