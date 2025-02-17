# ncmdump

使用本程序可将下载的网易云音乐缓存文件（ncm）转换为 mp3 或 flac 格式

## 简介

该版本为最早的 C++ 版本，也是作者开发的市面上第一个支持 ncm 转换的程序

源码复刻自 anonymous5l/ncmdump，感谢前辈的付出！
做了 Windows 下的移植，修复了一些编译问题

## 便捷式传送门

2021年10月6日，原作者已经删库

## 依赖库及不同平台编译方法

### 依赖库

* taglib

### Linux / macOS

```shell
# CentOS
yum install taglib-devel
make linux

# Ubuntu / Debian
apt install libtag1-dev
make linux

# macOS
brew install taglib
make macos
```

注意：Linux / macOS 为动态库支持，Linux 下静态编译需要手动编译 taglib 静态库，macOS 原生库不支持静态链接

### Windows

#### MinGW

~~因为部分代码（例如常量引用等）在 msvc 下面编译不通过，本项目需要使用 MinGW 编译~~
已经修改部分代码使得支持 msvc 编译，详见下面的 Visual Studio 部分

下载 mingw-w64 的 Windows 版本，这里推荐从 [winlibs](https://winlibs.com/) 这个网站下载，编译器版本始终跟随上游保持最新

将 mingw64/bin 添加到系统环境变量

taglib 在 Windows 下已经编译好放在项目内，直接执行

```shell
mingw32-make win32
```

生成的二进制程序为静态链接版本，可脱离运行库运行在裸机上

#### msys2

```shell
pacman -Syu
git clone https://github.com/taurusxin/ncmdump && cd ncmdump
make win32
```

#### Visual Studio 编译调试

##### 创建项目

- visual studio 2017 从已有代码创建项目，项目类型：“控制台应用程序项目”

##### 项目属性

- Windows SDK 版本，选中可用的（如 win
- `C/C++` -> 附加包含目录，增加`$(ProjectDir)\taglib\include`
- `链接器`
  - -> 常规 -> 附加库目录，增加 `$(ProjectDir)\taglib\lib`
  - -> 输入-> 附加依赖项，增加 `tag.lib`

##### 代码改造

- 数组使用 new 改造，【todo】释放 new 的内存


##### taglib 库编译
- 下载代码：https://github.com/taglib/taglib

- visual studio 2017，
  文件 -> 打开 -> cmake，选中项目的 CMakeList.txt

  工具栏会显示默认的配置，下拉点击`管理配置`，会弹框，可将配置添加到 CMakeSettings，可以选择 x86/x64 的 Debug/Release

  CMake 菜单 -> 仅生成 -> tag.lib；安装 -> tag.lib

  注意 ncmdump 项目 debug 时需要 taglib 的 debug 的库，生成 release 可执行文件时需要 release 的 taglib 库

## 使用

1. 命令行下使用 `ncmdump [files]...`
2. 直接 ncm 拖拽文件到二进制文件上
