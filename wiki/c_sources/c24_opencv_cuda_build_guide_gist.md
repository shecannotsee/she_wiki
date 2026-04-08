# Gist：从源码构建带 CUDA 和 cuDNN 的 OpenCV

## 记录时间

- 2026-04-08 CST

## 原始来源

- 标题：`Guide to build OpenCV from source with GPU support (CUDA and cuDNN)`
- 作者：`minhhieutruong0705`
- Gist 页面：https://gist.github.com/minhhieutruong0705/8f0ec70c400420e0007c15c98510f133
- 原始文件：`OpenCV_Build-Guide.md`
- 原始 Raw：https://gist.github.com/minhhieutruong0705/8f0ec70c400420e0007c15c98510f133/raw/cd3a66e320ddf10e3a621545217810c3d2b23b42/OpenCV_Build-Guide.md
- 文档时间：2022-02-22

## 原始环境

- Operating system: `Ubuntu 20.04 x84_64 (64-bit)`
- Architecture: `amd64`
- GPU: `NVIDIA GeForce RTX 3090`
- Python: `3.8`
- 示例成功安装版本：
  - OpenCV `4.5.5`
  - CUDA `11.6`
  - cuDNN `8.3.2`

## 原始结构

- System specification
- Install dependencies and recommended packages
- Clone OpenCV and OpenCV Contrib
- Config OpenCV
- Build OpenCV
- Install OpenCV
- Verify OpenCV Installation
- Uninstall built OpenCV
- My Installation

## 原始信息

### 安装前的冲突清理

- 如果系统里已经存在其他 OpenCV 安装，作者建议先清理
- 文中特别提醒要移除可能与源码安装冲突的 Python 包或系统包：
  - `opencv-python-headless`
  - `python3-opencv`
- 还提到一个可选做法：
  - 如果不移除 `python3-opencv`，可以考虑在 CMake 中加入 `-D HAVE_opencv_python3=ON`

### 依赖准备

- 文中给出了一组较完整的依赖安装清单，覆盖：
  - 基础构建工具
  - Python 开发依赖
  - 图像 I/O
  - 视频音频相关依赖
  - Camera / V4L
  - GTK / HighGUI
  - TBB
  - Atlas / gfortran
  - protobuf、glog、gflags、eigen、hdf5、doxygen 等可选项
- 其中有一个环境性修补动作：
  - 在 `/usr/include/linux` 下为 `videodev.h` 建符号链接，指向 `../libv4l1-videodev.h`

### OpenCV 与 OpenCV Contrib 的版本对齐

- 文中要求同时获取：
  - `opencv`
  - `opencv_contrib`
- 无论使用 `git clone` 还是压缩包下载，都强调二者版本号必须一致
- 如果使用 Git，则分别对两个仓库 `git checkout <opencv-version>`

### CMake 配置的关键点

- 文中建议先创建单独的构建目录 `opencv_build`
- 它特别强调两个关键点：
  - 必须根据实际 GPU 查询并填写 `CUDA_ARCH_BIN`
  - 不要为了让 CMake 识别 OpenGL 而引入 `libgtkglext1` 与 `libgtkglext1-dev`，因为会在 OpenCV 4.x 编译时报错
- 示例配置中包含的重点开关有：
  - `OPENCV_EXTRA_MODULES_PATH`
  - 一整组 `PYTHON3_*` 路径
  - `OPENCV_GENERATE_PKGCONFIG`
  - `WITH_CUDA`
  - `WITH_CUDNN`
  - `OPENCV_DNN_CUDA`
  - `CUDA_ARCH_BIN`
  - `ENABLE_FAST_MATH`
  - `CUDA_FAST_MATH`
  - `WITH_CUFFT`
  - `WITH_CUBLAS`
  - `WITH_V4L`
  - `WITH_OPENCL`
  - `WITH_OPENGL`
  - `WITH_GSTREAMER`
  - `WITH_TBB`

### 关键验证点

- 文中并不把 `cmake` 成功当作真正成功，而是要求检查 build summary
- 重点检查项包括：
  - `NVIDIA CUDA: YES`
  - `cuDNN: YES`
  - `NVIDIA GPU arch`
  - Python 解释器、库、numpy include 路径、install path
- 作者贴出了一份完整 build summary 样例，用来对照：
  - OpenCV modules
  - GUI / Video I/O
  - CUDA / cuDNN
  - Python 3
  - Install path

### 编译、安装与验证

- 编译前先用 `nproc` 查看 CPU 核数
- 再用 `make -j<number of processors>` 编译
- 编译后先检查构建目录中是否生成：
  - `bin`
  - `lib`
  - `OpenCVConfig*.cmake`
  - `OpenCVModules.cmake`
- 安装步骤包括：
  - `sudo make install`
  - 将 `/usr/local/lib` 写入 `opencv.conf`
  - 执行 `ldconfig`
- Python 侧验证包括：
  - `import cv2`
  - `cv2.__version__`
  - `cv2.getBuildInformation()`

### 常见问题与修复

- 如果 Python 中报 `ModuleNotFoundError: No module named 'cv2'`
- 文中建议先运行 `python3 -m site` 检查 `/usr/local/lib/python3.8/site-packages` 是否在搜索路径中
- 如果不在，则通过 `PYTHONPATH` 补入该路径
- 并建议写入：
  - `~/.bashrc`
  - 或 `/etc/profile`

### 卸载与彻底清理

- 文中提供了源码构建版本的卸载方式：
  - 在 `opencv_build` 下执行 `sudo make uninstall`
  - 删除 `opencv_build`
- 同时提醒要人工检查并清理残留文件，重点目录包括：
  - `/usr/local`
  - `/usr/local/bin`
  - `/usr/local/lib`
  - `/usr/local/cmake/opencv4`
  - `/usr/local/include/opencv4`
  - `/usr/local/share/opencv4`
  - `/usr/local/lib/python3.8/dist-packages/cv2`
  - `/usr/local/lib/python3.8/site-packages/cv2`
- 作者还给出两个 `find` 命令来搜索 `opencv` 和 `cv2` 相关残留

## 原始结论

- 这篇指南的主要价值，不是给出某个固定版本号，而是把 OpenCV + CUDA/cuDNN 的源码构建过程拆成了一条较完整的可执行链路
- 成功关键不在“执行一串命令”，而在于同时满足几件事：
  - 清理已有冲突安装
  - 对齐 `opencv` 与 `opencv_contrib` 版本
  - 正确设置 Python 与 CUDA 相关路径
  - 核对 CMake build summary 中 CUDA / cuDNN / Python 的状态
  - 处理 `/usr/local` 与 Python 搜索路径的安装后问题
- 这篇指南明显强依赖特定环境：
  - Ubuntu 20.04
  - Python 3.8
  - RTX 3090
- 因此更适合作为“来源上下文”和“经验样例”，不应把其中具体路径和版本号直接视为普遍真理

## 相关页面

- [OpenCV CUDA 源码构建排障顺序](../b_extractions/b3_engineering_troubleshooting/b35_opencv_cuda_source_build.md)
