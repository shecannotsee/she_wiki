# 分类总览

本页是 `she_wiki` 的总入口。
页面内容用于说明仓库结构、阅读顺序和现有知识分布。

## 入口说明

- [分类总览](./a1_overview.md) 承担仓库总入口的作用，用于建立整体认识和快速浏览全部主题
- [方法论类](./a2_methods.md) 汇总仓库的阅读方法、学习方法和知识组织方法
- [she_wiki 文章归档方法](../b_extractions/b1_methods/b12_she_wiki_article_ingestion.md) 说明新文章进入仓库时的归档流程
- [she-wiki-maintainer](../../skills/she-wiki-maintainer/SKILL.md) 记录维护本仓库时使用的结构规则和编辑约束

## 维护检查清单

新文章、链接或整理结果进入 `she_wiki` 后，通常需要检查以下位置：

1. `c_sources` 是否已经新增来源页，或补充到已有来源页
2. `b_extractions` 是否已经并入已有知识页，或新增到正确分类
3. 对应分类页是否已经在 `a_summaries` 中出现新条目
4. [分类总览](./a1_overview.md) 的全量清单、分类计数和分类分布是否已经同步
5. 新增页、被合并页和 summary 页之间的相对链接是否完整

## 三层结构

1. `a_summaries`：分类与总览
2. `b_extractions`：稳定知识
3. `c_sources`：原始上下文

阅读顺序：

- 先用 `a_summaries` 判断主题和阅读入口
- 再进入 `b_extractions` 读稳定结论
- 只有在需要追来源、上下文和原始材料时，再回到 `c_sources`

## 快速总览

当前共有 5 个分类，47 篇知识提取页。

### 方法论类（18）

适合处理“怎么思考、怎么学习、怎么表达、怎么组织知识”这类问题。

- 分类页：[方法论类](./a2_methods.md)
- [苏格拉底式推理](../b_extractions/b1_methods/b11_socratic_reasoning.md)
- [she_wiki 文章归档方法](../b_extractions/b1_methods/b12_she_wiki_article_ingestion.md)
- [让平淡材料产生叙事张力](../b_extractions/b1_methods/b13_engaging_narrative_writing.md)
- [高性能计算优化的稳定顺序](../b_extractions/b1_methods/b14_hpc_optimization_order.md)
- [技能成长与刻意练习](../b_extractions/b1_methods/b15_skill_growth_and_deliberate_practice.md)
- [事实优先与宽容共存](../b_extractions/b1_methods/b16_fact_first_and_tolerant_coexistence.md)
- [按认知任务分层的软件文档](../b_extractions/b1_methods/b17_divio_documentation_system.md)
- [软件架构图的 C4 分层方法](../b_extractions/b1_methods/b18_c4_software_architecture_diagramming.md)
- [纯科学优先与科研标准](../b_extractions/b1_methods/b19_pure_science_and_scientific_standards.md)
- [LLM pretrain 工程的稳定顺序](../b_extractions/b1_methods/b110_llm_pretrain_engineering_order.md)
- [数学证明学习的数感优先顺序](../b_extractions/b1_methods/b111_math_proof_learning_number_sense_order.md)
- [技能训练应尽早嵌入真实任务与表达](../b_extractions/b1_methods/b112_skill_training_inside_real_tasks.md)
- [CUDA 学习的硬件优先与 profiling 顺序](../b_extractions/b1_methods/b113_cuda_learning_order.md)
- [自由、责任与反自欺](../b_extractions/b1_methods/b114_freedom_responsibility_and_bad_faith.md)
- [分布式存储工程师的学习顺序](../b_extractions/b1_methods/b115_distributed_storage_learning_order.md)
- [乐器认知与常见误解检查表](../b_extractions/b1_methods/b116_instrument_cognition_and_misconception_check.md)
- [跨语言中介帮助理解艰深文本](../b_extractions/b1_methods/b117_cross_language_bridge_for_text_understanding.md)
- [科研思想培养中的问题先于方法与平台先于论文](../b_extractions/b1_methods/b118_research_thought_training_and_mentor_boundaries.md)

### 技术原理类（10）

适合先建立底层机制、分层关系和运行骨架。

- 分类页：[技术原理类](./a3_technical_fundamentals.md)
- [gzip 与 DEFLATE 的分层关系](../b_extractions/b2_technical_fundamentals/b21_gzip_and_deflate_layering.md)
- [数字视频压缩的基础骨架](../b_extractions/b2_technical_fundamentals/b22_digital_video_compression_fundamentals.md)
- [低延时系统的端到端成本链路](../b_extractions/b2_technical_fundamentals/b23_low_latency_system_cost_path.md)
- [C++ 运行时多态的虚表骨架](../b_extractions/b2_technical_fundamentals/b24_cpp_runtime_polymorphism_vtable.md)
- [CUDA/NVIDIA 算子性能优化骨架](../b_extractions/b2_technical_fundamentals/b25_cuda_operator_performance_optimization_skeleton.md)
- [解释器、编译器与虚拟机的求值骨架](../b_extractions/b2_technical_fundamentals/b26_interpreter_compiler_vm_eval_apply_skeleton.md)
- [AST 作为源码结构抽象层的职责](../b_extractions/b2_technical_fundamentals/b27_ast_as_source_structure_abstraction.md)
- [区块链的可信记账与链外存储分工](../b_extractions/b2_technical_fundamentals/b28_blockchain_trust_and_storage_principles.md)
- [动态规划的状态、转移与依赖压缩骨架](../b_extractions/b2_technical_fundamentals/b29_dynamic_programming_state_transition_skeleton.md)
- [群体行为的局部规则、拓扑邻居与生物启发算法](../b_extractions/b2_technical_fundamentals/b210_swarm_behavior_and_bio_inspired_algorithms.md)

### 工程排障类（6）

适合排查环境、依赖、运行时和工程链路里的具体问题。

- 分类页：[工程排障类](./a4_engineering_troubleshooting.md)
- [企业网络与代理排障](../b_extractions/b3_engineering_troubleshooting/b31_enterprise_network_and_proxy_troubleshooting.md)
- [Linux输入法与PyQt](../b_extractions/b3_engineering_troubleshooting/b32_linux_ime_and_pyqt.md)
- [强前端反爬内容获取](../b_extractions/b3_engineering_troubleshooting/b33_frontend_anti_bot_content_acquisition.md)
- [终端长任务与会话脱离](../b_extractions/b3_engineering_troubleshooting/b34_terminal_job_detachment.md)
- [OpenCV CUDA 源码构建排障顺序](../b_extractions/b3_engineering_troubleshooting/b35_opencv_cuda_source_build.md)
- [Electron Renderer 依赖接入与启动链路排障顺序](../b_extractions/b3_engineering_troubleshooting/b36_electron_renderer_dependency_startup_troubleshooting.md)

### 系统设计类（6）

适合看系统骨架、状态边界、反馈回路和架构演进。

- 分类页：[系统设计类](./a5_system_design.md)
- [多Agent系统工程化](../b_extractions/b4_system_design/b41_multi_agent_system_engineering.md)
- [反馈链路可靠性](../b_extractions/b4_system_design/b42_feedback_loop_reliability.md)
- [Agent 长期记忆系统设计](../b_extractions/b4_system_design/b43_agent_long_term_memory_systems.md)
- [线程池的最小系统骨架](../b_extractions/b4_system_design/b44_thread_pool_minimal_architecture.md)
- [DDIA 分布式系统设计基础框架](../b_extractions/b4_system_design/b45_ddia_distributed_system_design_foundations.md)
- [游戏服务端架构的类型分化与演进](../b_extractions/b4_system_design/b46_game_server_architecture_evolution.md)

### 技术判断类（7）

适合处理“看起来很强，但到底值不值得、稳不稳、判断是否失真”的问题。

- 分类页：[技术判断类](./a6_technical_judgment.md)
- [技术热潮判断](../b_extractions/b5_technical_judgment/b51_technology_hype_evaluation.md)
- [量化机器学习风险](../b_extractions/b5_technical_judgment/b52_quant_ml_risks.md)
- [AI 进入数学后的判断顺序](../b_extractions/b5_technical_judgment/b53_ai_in_mathematics_judgment_order.md)
- [技术范式要服从问题与抽象质量](../b_extractions/b5_technical_judgment/b54_problem_first_abstraction_over_paradigm.md)
- [性能优化要优先处理 IO、布局与减法空间](../b_extractions/b5_technical_judgment/b55_performance_optimization_judgment_order.md)
- [GPU Kernel Engineer 岗位的长期价值与护城河](../b_extractions/b5_technical_judgment/b56_gpu_kernel_engineer_role_and_moat.md)
- [市场涨跌判断要先看订单失衡再看消息叙事](../b_extractions/b5_technical_judgment/b57_market_move_judgment_order_imbalance_over_news.md)

## 当前分类

- [方法论类](./a2_methods.md)
- [技术原理类](./a3_technical_fundamentals.md)
- [工程排障类](./a4_engineering_troubleshooting.md)
- [系统设计类](./a5_system_design.md)
- [技术判断类](./a6_technical_judgment.md)

## 页面定位

- [方法论类](./a2_methods.md) 负责说明如何思考、如何学习、如何表达和如何组织知识
- [she_wiki 文章归档方法](../b_extractions/b1_methods/b12_she_wiki_article_ingestion.md) 负责说明新文章的归档步骤和落点规则
- [she-wiki-maintainer](../../skills/she-wiki-maintainer/SKILL.md) 负责说明维护操作中的结构约束和编辑边界

## 分类原则

- 先看知识提取页的主要用途
- 再决定它属于方法、原理、排障、系统设计还是判断

## 编号策略

- 先按字母前缀保证排序顺序
- 再用数字后缀表示顺序
- 数字直接按自然数往后增长
