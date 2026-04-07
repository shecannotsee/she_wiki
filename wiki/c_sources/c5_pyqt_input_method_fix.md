# PyQt输入法修复

## 记录时间

- 2026-03-04

## 原始背景

Ubuntu 22.04 运行 CodexDesk 时，输入框无法使用搜狗输入法输入中文。

## 原始事实

- 环境变量：`QT_IM_MODULE=fcitx`
- PyQt5 插件目录：`/home/shecannotsee/.local/lib/python3.10/site-packages/PyQt5/Qt5/plugins`
- 缺失插件：
  - `platforminputcontexts/libfcitxplatforminputcontextplugin.so`
- 系统插件路径：
  - `/usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so`

## 原始修复动作

1. 创建目标目录：
   - `~/.local/lib/python3.10/site-packages/PyQt5/Qt5/plugins/platforminputcontexts`
2. 复制插件：
   - `cp /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so <目标目录>/`
3. 清理变量：
   - `unset QT_PLUGIN_PATH`
4. 启动验证：
   - `CODEXDESK_DEBUG_IM=1 ./start.sh`

## 原始工程加固

- 输入框改为 IME 友好子类
- 启动脚本统一设置 `QT_IM_MODULE`、`GTK_IM_MODULE`、`XMODIFIERS`
- 启动脚本加入自动补插件逻辑
