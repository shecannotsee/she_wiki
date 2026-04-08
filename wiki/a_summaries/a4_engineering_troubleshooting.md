# 工程排障类

## 包含的知识提取页

- [企业网络与代理排障](../b_extractions/b3_engineering_troubleshooting/b31_enterprise_network_and_proxy_troubleshooting.md)
- [Linux输入法与PyQt](../b_extractions/b3_engineering_troubleshooting/b32_linux_ime_and_pyqt.md)
- [强前端反爬内容获取](../b_extractions/b3_engineering_troubleshooting/b33_frontend_anti_bot_content_acquisition.md)
- [终端长任务与会话脱离](../b_extractions/b3_engineering_troubleshooting/b34_terminal_job_detachment.md)
- [OpenCV CUDA 源码构建排障顺序](../b_extractions/b3_engineering_troubleshooting/b35_opencv_cuda_source_build.md)

## 这一类的共同点

- 都强调先定位真正出问题的层
- 都不能只看表面现象
- 都要求对比不同路径或不同执行面
- 都要求先分清“程序问题”和“运行环境问题”
- 也包括多依赖源码构建场景里，如何先守住环境一致性再谈功能开关

## 核心总结

- 排障最重要的不是“知道更多命令”，而是知道如何把问题压缩到某一层
- 常见高价值动作是：对比、隔离、验证
- 长任务保活问题要先区分 shell、终端、会话和主机生命周期这四层
- 源码构建问题要先查版本、路径、依赖和安装残留是否一致，再查业务代码
