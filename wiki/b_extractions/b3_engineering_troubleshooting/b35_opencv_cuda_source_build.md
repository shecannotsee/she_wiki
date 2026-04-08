# OpenCV CUDA 源码构建排障顺序

## 提取结果

- 从源码构建带 CUDA / cuDNN 的 OpenCV 时，最容易出错的不是单条命令，而是环境中同时存在旧安装、版本不对齐、Python 路径不一致和 GPU 架构参数错误
- 构建是否真正成功，不能只看 `cmake` 或 `make` 是否结束，还要看 build summary 里 CUDA、cuDNN、Python 安装路径是否符合预期
- 这类问题本质上是“构建链路一致性”问题：
  - 依赖一致
  - OpenCV / Contrib 版本一致
  - GPU 架构配置一致
  - 安装路径与 Python 搜索路径一致

## 稳定排障顺序

1. 先清理已有 OpenCV 安装，尤其是系统包和 Python 包的冲突版本
2. 再补齐构建、媒体、GUI、Python 和并行相关依赖
3. 获取 `opencv` 与 `opencv_contrib`，并确保两者版本严格一致
4. 在 CMake 中明确设置 Python 路径、CUDA / cuDNN 开关和 `CUDA_ARCH_BIN`
5. 配置后先检查 build summary，确认 `CUDA: YES`、`cuDNN: YES` 和 Python install path 正确
6. 编译安装后，再到 Python 里验证 `cv2` 是否可导入、版本是否正确、build information 是否包含预期能力
7. 如果 Python 找不到 `cv2`，最后再回到 `site-packages` 与 `PYTHONPATH` 层修复

## 可直接复用

- 遇到 OpenCV + CUDA 构建失败时，先怀疑“环境里已经有别的 OpenCV”，不要先怀疑源码本身
- `opencv` 和 `opencv_contrib` 版本不一致，是一类高频但隐蔽的问题
- `CUDA_ARCH_BIN` 必须按实际 GPU 算力填写，不能盲抄别人的示例值
- 对这类源码构建，最重要的检查点不是日志里有没有 `error`，而是 summary 里关键能力是否真的被启用
- 安装成功但 `import cv2` 失败时，优先检查 Python 的 `site` 路径，而不是马上重编译
- 涉及 `/usr/local` 安装时，卸载和残留清理必须做完整，否则下一次构建很容易被旧文件污染

## 来源

- [Gist：从源码构建带 CUDA 和 cuDNN 的 OpenCV](../../c_sources/c24_opencv_cuda_build_guide_gist.md)
