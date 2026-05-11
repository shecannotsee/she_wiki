# 知乎专栏：从头训练大模型——超小型模型（1b以下模型）开源项目汇总

## 记录时间

- 2026-05-11 CST

## 原始讨论对象

- 知乎专栏：《从头训练大模型——超小型模型（1b以下模型）开源项目汇总》
- 目标作者：`周博士`
- 来源链接：https://zhuanlan.zhihu.com/p/2026598044943041953
- 文章 ID：`2026598044943041953`

## 正文获取情况

- 当前环境直接请求知乎专栏原页会拿到反爬壳页面。
- 当前环境直接请求知乎专栏 API 会返回 `10003`，不能依赖公开接口直接获取全文。
- `Chrome headless` 渲染后读取正文仍会命中 `40362` 风控。
- 本次按 she_wiki 的“强前端反爬内容获取”流程，改用真实浏览器渲染页面，再通过 CDP `Runtime.evaluate` 读取 `.Post-RichText` 正文节点。
- 目标文章正文已完整拿到：
  - 标题正确
  - 正文节点长度约 `1900`
  - 未命中 `40362`
  - 页面内未出现额外“阅读全文”折叠按钮
- 本地提取文件：
  - `/tmp/zh_2026598044943041953.json`
  - `/tmp/zh_2026598044943041953.txt`

## 原始结构

- `1. MiniMind`
- `2. nanoGPT`
- `3. minGPT`
- `4. TinyLlama`
- `5. MobileLLM`
- `6. mini_qwen`
- `7. LLMs-learning`
- `8. baby-llama2-chinese`
- `9. tiny-llm-zh`
- `10. Chinese Tiny LLM`
- `11. min-LLM`
- `12. llama2.c`
- `13. LiteLlama`
- `14. 从0到1手搓mini LLM`
- `对比总结`
- `学习路径建议`

## 原始信息

- 文章本质上是一份“超小型模型从零训练项目盘点”，核心不是解释单一算法，而是帮助读者快速找到适合入门、复现和扩展的项目起点。
- 文章把 `MiniMind` 放在最推荐位置，强调的不是参数规模本身，而是：
  - 模型足够小，试错成本低
  - 训练成本极低
  - 训练链路完整，覆盖 `pretrain -> SFT -> LoRA -> DPO`
  - 支持 `MoE`
  - 还有视频教程
- 文章对 `nanoGPT` 和 `minGPT` 的定位很清楚：
  - 不是最完整的工程栈
  - 而是最适合先建立 GPT 架构直觉和实现直觉
- 文章把 `TinyLlama`、`MobileLLM`、`mini_qwen` 放在更接近 `1B` 量级的位置，强调：
  - 可以继续往更真实的参数规模走
  - 可以接触更完整的训练工程和更贴近实际部署的设计
- 文章单独列出中文专项路线，说明作者默认认为：
  - 语言目标会影响项目选择
  - 中文训练并不只是把英文路径原样缩小
- 文末给出的学习路径很明确：
  - 先用 `nanoGPT` 理解 GPT 核心架构
  - 再用 `MiniMind` 跑通从头训练的小模型全流程
  - 再参考 `TinyLlama` 或 `MobileLLM` 进入更大规模训练
  - 如果目标偏中文，再结合 `baby-llama2-chinese` 一类项目

## 关键链接

- `MiniMind`：https://github.com/jingyaogong/minimind
- `nanoGPT`：https://github.com/karpathy/nanogpt
- `minGPT`：https://github.com/karpathy/minGPT
- `TinyLlama`：https://github.com/jzhang38/TinyLlama
- `MobileLLM`：https://github.com/facebookresearch/MobileLLM
- `mini_qwen`：https://github.com/qiufengqijun/mini_qwen
- `baby-llama2-chinese`：https://github.com/DLLXW/baby-llama2-chinese
- `tiny-llm-zh`：https://github.com/wdndev/tiny-llm-zh
- `Chinese Tiny LLM`：https://github.com/Chinese-Tiny-LLM/Chinese-Tiny-LLM
- `min-LLM`：https://github.com/SeanNaren/min-LLM
- `llama2.c`：https://github.com/karpathy/llama2.c

## 补充信息

- 用户额外指出：`MiniMind` 不只是公开了训练流程，也提供了各阶段训练数据。
- 这一点会显著提高它作为学习样本的价值，因为它把“代码能跑”进一步推进到“数据、流程和结果都能对照复核”。
- 这条补充信息来自本次整理要求，不是本页正文节点里直接可见的显式句子。

## 原始结论

- 这篇文章最稳定的价值，不是它列出的 14 个项目名单本身，而是它隐含的项目选择标准：
  - 先选低成本、可反复试错的项目
  - 先选能暴露完整训练链路的项目
  - 再按语言目标和规模目标扩展
- `MiniMind` 在这篇盘点里的特殊地位，来自“低成本 + 完整流程 + 可扩展到更真实训练问题”的组合，而不只是“小”。
- 结合用户补充信息后，可以进一步把它看成“连各阶段训练数据也较完整暴露出来的学习项目”，因此更适合作为从零训练小模型的第一站。

## 相关页面

- [超小型模型从零训练的项目选择与学习路径](../b_extractions/b1_methods/b119_small_llm_training_project_selection.md)
- [LLM pretrain 工程的稳定顺序](../b_extractions/b1_methods/b110_llm_pretrain_engineering_order.md)
- [强前端反爬内容获取](../b_extractions/b3_engineering_troubleshooting/b33_frontend_anti_bot_content_acquisition.md)
- [知乎全文获取流程](./c9_zhihu_full_content_retrieval.md)
