# 终端关闭后长任务保活资料整理

## 记录时间

- 2026-04-08 CST

## 原始讨论对象

- 知乎问题：《linux 在终端打开程序后关闭终端，程序也跟着关闭了怎么办？》
- 目标回答链接：https://www.zhihu.com/question/442188249/answer/1709501320
- 可访问正文页：https://www.zhihu.com/tardis/bd/ans/1709501320

## 相关资料

- GNU Bash Manual:
  - https://www.gnu.org/software/bash/manual/bash.html
- GNU Coreutils `nohup`:
  - https://www.gnu.org/software/coreutils/nohup
- tmux Getting Started:
  - https://github.com/tmux/tmux/wiki/Getting-Started
- GNU Screen Manual:
  - https://www.gnu.org/software/screen/manual/screen.html

## 原始信息

- 知乎回答最有价值的主线，不是单独介绍 `nohup` 或 `tmux`，而是区分几种“让任务脱离当前终端”的路径：
  1. 任务已经启动，才发现会跑很久：
     - 可用 `Ctrl-Z -> bg -> disown`
     - 本质是先把前台任务转成后台 job，再让 shell 不再把它当成自己的 job
  2. 一开始就知道任务不需要交互：
     - 可直接用 `nohup COMMAND > log 2>&1 &`
     - `nohup` 的核心作用是忽略 `SIGHUP`
  3. 任务需要保留交互、输出或重连能力：
     - 更适合 `tmux` / `screen`
     - 这类工具的核心不是“忽略信号”，而是把任务运行环境放进一个可分离、可重新连接的会话
- Bash 手册给出的关键边界：
  - shell 退出时默认会向 jobs 发送 `SIGHUP`
  - `disown` 可以把 job 从 shell 的 job table 移除，或用 `disown -h` 标记为不接收 `SIGHUP`
  - `huponexit` 选项会影响 interactive login shell 退出时是否向 jobs 发送 `SIGHUP`
- `nohup` 文档给出的关键边界：
  - `nohup` 只负责让命令忽略 hangup signal
  - 如果 stdout/stderr 仍接着终端，默认会重定向到 `nohup.out` 或用户主目录下的 `nohup.out`
  - 它不解决“重新连回会话继续交互”的问题
- tmux / screen 相关资料共同指向的关键点：
  - 会话可以 detach 后继续存在
  - 网络断开或主动 detach 后，可以重新 attach
  - 这类工具适合需要持续观察输出、保留终端上下文、后续继续交互的任务
- 知乎回答里还有几个对工程现场很重要的提醒：
  - 如果程序后续还要交互，就不适合简单 `disown`
  - 如果本地机器关机或休眠，任务仍会停止或暂停，这和“只是关终端”不是一回事
  - `at now` 也能把任务提交到独立执行环境，但环境和当前 shell 可能不同，更适合作为补充手段而不是默认方案

## 原始结论

- 这个问题的本质，不是“记住某个命令”，而是先分清任务到底依赖的是：
  - 当前 shell 的 job 控制
  - 当前终端的挂断信号
  - 当前会话的交互上下文
  - 当前主机是否持续运行
- 只有把依赖层分清，才能判断该用 `disown`、`nohup`，还是 `tmux` / `screen`
- 对长任务来说，最稳定的经验不是“背一个万能命令”，而是先明确：
  - 任务是否已启动
  - 后续是否还需要交互
  - 是否必须保留输出
  - 是否要跨断线重连

## 相关页面

- [终端长任务与会话脱离](../b_extractions/b3_engineering_troubleshooting/b34_terminal_job_detachment.md)
