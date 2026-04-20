# 技术判断类

## 包含的知识提取页

- [技术热潮判断](../b_extractions/b5_technical_judgment/b51_technology_hype_evaluation.md)
- [量化机器学习风险](../b_extractions/b5_technical_judgment/b52_quant_ml_risks.md)
- [AI 进入数学后的判断顺序](../b_extractions/b5_technical_judgment/b53_ai_in_mathematics_judgment_order.md)
- [技术范式要服从问题与抽象质量](../b_extractions/b5_technical_judgment/b54_problem_first_abstraction_over_paradigm.md)
- [性能优化要优先处理 IO、布局与减法空间](../b_extractions/b5_technical_judgment/b55_performance_optimization_judgment_order.md)

## 这一类的共同点

- 都在处理“表面上很强，但实际上很容易判断失真”的问题
- 都要求把炫目的结果拆回真实前提
- 都要求区分“技术上做得到”与“是否值得、是否稳妥、是否真对人有益”
- 都要求把“语言、范式、模式”降回手段层，而不是把它们当成设计本身
- 都要求把“优化得很炫”拆回数据搬运、内存布局、近似权衡和流程删减这些真实代价层

## 核心总结

- 先问真实问题是什么
- 先问结论赖以成立的条件是否成立
- 先问成本、风险和可验证性，再问效果有多漂亮
- 先问抽象是否真的减少复杂度，再问它听起来是否高级
- 先问瓶颈是不是 IO、布局或多余流程，再问某个热点函数还能不能继续抠常数
