# games106
games106 现代图形绘制流水线原理与实践，作业框架。

作业框架实践部分，选择了使用了AMD的[Cauldron](https://github.com/GPUOpen-LibrariesAndSDKs/Cauldron)绘制框架。使用这个渲染库有考虑到两点：1、它有一些图形对象必要的封装（应该没有人会想要裸写Vulkan API的代码）。2、它的封装程度相对来说比较低，适合初学者。因为这个渲染库的选择，学习者最好使用Window。同时Sample使用了Win32 APP没有命令行终端，需要使用OutputDebugStringA函数，需要在Visual Studio的Output页面，选择Debug查看调试信息。

这里不推荐MacOS用户来使用作业框架或者学习Vulkan，因为没有官方自己提供的SDK，以及调试工具。

因为作者不太熟悉Linux（主要是我懒~~~笑），同时Cauldron也没有提供Linux的支持，学习者有兴趣，可以自己提供Linux的适配，提供贡献到Cauldron。可以参考Cauldron中的issue[Linux and GCC support by Zakhrov · Pull Request #3 · GPUOpen-LibrariesAndSDKs/Cauldron (github.com)](https://github.com/GPUOpen-LibrariesAndSDKs/Cauldron/pull/3)

# Build

## how to get code

框架代码引入了 submodule 和 git lfs，确保安装git的时候已经

通过下方的命令获取代码

```powershell
git lfs install
git clone https://github.com/dodoleon/games106.git
cd games106
git submodule update --init
```

## Prerequisites

来自Cauldron

- [Git](https://git-scm.com/)
- [CMake 3.24](https://cmake.org/download/)
- [Visual Studio 2019 or 2022](https://visualstudio.microsoft.com/downloads/)
- [Windows SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk) 在安装VS的时候，勾选上最新版本的SDK
- [Vulkan SDK](https://www.lunarg.com/vulkan-sdk/) 直接下载最新的Vulkan SDK，同时更新显卡驱动到最新的版本

## how to build

```powershell
cd games106
mkdir out
cd out
cmake ..
./games106.sln
```



# homework

代码样例会有一个拷贝数据的过程，程序的工作路径在bin文件夹下，cmake会生成一个生成的后处理任务来拷贝数据到bin文件夹下（所以不要直接修改bin文件夹下的数据，项目被编译一次就会被覆盖掉，幸苦的作业就都没有了，笑~~~）。同样如果想要增加一些新的文件，需要在对应的cmake文件里面增加，可以搜索copyTargetCommand函数。查看如何使用。

## homework0

作业0 是一个hello triangle的样例。代码在HelloTriangle项目下

期望得到的渲染结果是一个青色背景的三角形

![homework0_0](./img/homework0_0.png)

直接运行会得到一个黑色背景的三角形。

![homework0_1](./img/homework0_1.png)

作业0就是修复这个渲染错误。可以使用下面推荐的调试工具以及方法。

## homework1

作业1 gltf渲染框架。这个框架是基于AMD的[glTFSample](https://github.com/GPUOpen-LibrariesAndSDKs/glTFSample)。后续很多的作业可以在这个基础上改动。代码在gltfLoad项目下。

## homework2

作业2 profile，gltfLoad项目里面有各个阶段的时间统计。这里推荐使用NV的Nsight Graphic工具来做性能分析。

## homework3

作业3 细分着色器实现。代码在PNTriangle里面。

## homework4

作业4 压缩纹理的压缩和实现

## homework5

作业5 几何处理，生成好简化模型的gltf文件，通过gltfLoad项目导入显示。

## homework6

作业6 压缩纹理的压缩和实现

## homework7

作业7 LOD系统

# Debug & Profile

Vulkan的调试有两类

- Vulkan API调用的相关问题，使用```vkCreateDebugReportCallbackEXT```设置回调函数，这个需要VK_EXT_debug_report扩展支持。回调函数定义在在```ExtValidation.cpp```中的```MyDebugReportCallback```函数。这里使用了OutputDebugStringA，需要在Visual Studio的Output页面，选择Debug查看调试信息。
- Vulkan 图形渲染的相关问题，可以在Vulkan的[相关页面](https://www.vulkan.org/tools#profilers-and-debuggers)中寻找和学习者显卡厂商相关的工具，这里比较推荐[RenderDoc](https://renderdoc.org/)和[Nvidia Nsight Graphics](https://developer.nvidia.com/nsight-graphics)

Vulkan 的Profile 推荐使用显卡厂商做的工具，同样可以在Vulkan的[相关页面](https://www.vulkan.org/tools#profilers-and-debuggers)中寻找和学习者显卡厂商相关的工具。这里不推荐使用RenderDoc做性能分析，它给的结果不太准确。

