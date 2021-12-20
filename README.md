# 计算机图形学 Final Project
## 简介
尝试使用OptiX在GPU上实现简单、基于微表面模型的路径追踪以及环境光照
## 实验环境
* 系统：Win10 64位
* GPU： GeForce GTX 1660Ti
* OptiX 6.0.0
* CMAKE 3.22.1
* CUDA 10.0
* Microsoft Visual Studio 2015 Community(CUDA 10的SDK仅支持到vs2015，OptiX的有些示例在更高版本中编译会不通过，因此官方论坛推荐vs2015)
* OpenGL
## 实验内容
* 使用CMAKE编译OptiX示例
  * 官方示例代码在x:\...\OptiX6.0.0\SDK中
  * 将文件夹中的CMAKElist.txt拖入CMAKE界面，点击Configure，编译器选择vs2015，平台选择X64
  * Configure完成->Generate->Open Project，选择vs2015
  * 在vs2015打开后即可编译生成可执行文件，位置在\cmake输出目录\bin\Debug
  * 运行结果如图<br>
  ![](https://github.com/shijiu9/shijiu9/blob/main/sample.jpg)
* Phong光照模型
  * 代码在OptiXPhong.rar中，下载解压后用vs2015打开
  * 配置工程环境：
    * 菜单->项目->属性，配置->VC++目录->包含目录中添加CUDA和OPTIX的包含目录
    * 链接器->输入->附加依赖项,添加<br>
      C:\ProgramData\NVIDIA Corporation\OptiX SDK 6.0.0\lib64\optix.6.0.0.lib;C:\ProgramData\NVIDIA Corporation\OptiX SDK 6.0.0\SDK\support\freeglut\win64\Release\freeglut.lib;C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\lib\x64\nvrtc.lib ; 
    * 编译环境修改为X64
  * 原理：
  phong光照效果=全局光+方向光源的漫反射+方向光源的镜面反射<br>
  * 代码在OptiX中构建了一个球，并实现Phong光照，效果如图：<br>
    ![](https://github.com/shijiu9/shijiu9/blob/main/phong.jpg)
* 基于微表面模型的路径追踪
  * 代码在OptixPathtracing.rar中，下载解压后用vs2015打开
  * 配置工程环境：同上
  * 原理
    * 微表面模型<br>
    普通的着色模型假设着色的区域是一个平滑的表面，表面的方向可以用一个单一的法线向量来定义来定义。而微表面模型则认为:<br>
    1、着色的区域是一个有无数比入射光线覆盖范围更小的微小表面组成的粗糙区域。<br>
    2、所有这些微小表面都是光滑镜面反射的表面。<br>
    因为这种模型认为着色区域的表面是由无数方向不同的小表面组成的，所以在 Microfacet 模型中，着色区域并不能用一个法线向量来表示表面的方向，只能用一个概率分布函数D来计算任意方向的微小表面在着色区域中存在的概率。
    * 代码中所用模型<br>
    代码参考自https://freesouth.blog.csdn.net/article/details/91511970 <br>
    使用了基于随机微平面的理论，我们认为光线击到任何一处，它的反射方向是随机的，因为这一处可以划分为无穷小，现实世界的某一点也是吸收了几乎可以认为是随机方向的各种光的反射效果<br>
    在代码中的体现为：反射光线完全由随机函数生成
    运行结果如图：<br>
    ![](https://github.com/shijiu9/shijiu9/blob/main/micro.jpg)<br>
    尝试过把模型修改为Cook Torrance模型，但失败
    
    
