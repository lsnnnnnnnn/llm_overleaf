# 资料来源与阅读重点

核查日期：2026-10-22。本提案只研究 LLM API 持续监控。下列来源中的既有方法与本项目的拟议实验分别说明。

## 用户指定的三项资料

### 模型抽取检测：问题定义、实验和局限

Shuze Liu、Qianwen Guo、Yushun Dong，*An Embarrassingly Simple Detector for Model Extraction Attacks in Large Language Model API Traffic*，arXiv:2606.05725，v3，2026-08-29。

[全文 v3](https://arxiv.org/html/2606.05725v3) · [元数据](https://arxiv.org/abs/2606.05725) · [官方代码](https://github.com/LabRAI/mmd-llm-mea-detection/tree/af9420da12a03bf321a2218870a68f7b38f5787c)

重点读第 2 节的问题定义、第 4.1 节实验设置、第 4.6 节污染分析和 Limitations。它从查询内容出发，以固定编码器表示和 MMD 比较查询窗口与正常参考。正常流量用于校准；论文中的混合流量模型允许窗口中只有部分抽取查询。

实验采用 BGE-small-en-v1.5 表示和大小为 1,500 的窗口。本提案保留这一配置作为受限复现入口，不将原文离线窗口上的低误报结果改写为连续运行的误报保证。论文已讨论稀释攻击、自适应攻击及参考污染，因此这些现象本身不能直接作为新贡献。

Limitations 明确讨论正常流量随时间变化、参考陈旧及跨窗口累积。本提案拟比较完整顺序报警和更新策略的整段误报与及时检出能力。是否存在尚未解决的问题，需要与已有顺序方法比较后判断。

### LangSkills：生成、来源记录和验证边界

[官方项目](https://github.com/LabRAI/LangSkills) · [核查版本 039f8b4](https://github.com/LabRAI/LangSkills/tree/039f8b421237452bd70b3df44ac908f8720fb99e)

项目从提取的来源文本生成技能，保存模型、提示词哈希、生成时间与来源引用。来源档案记录 URL、抓取时间和文本指纹，发布记录保留运行与来源 lineage。

核查入口：[生成代码](https://github.com/LabRAI/LangSkills/blob/039f8b421237452bd70b3df44ac908f8720fb99e/core/skills/generate.py)、[来源档案](https://github.com/LabRAI/LangSkills/blob/039f8b421237452bd70b3df44ac908f8720fb99e/core/sources/artifacts.py)、[发布记录](https://github.com/LabRAI/LangSkills/blob/039f8b421237452bd70b3df44ac908f8720fb99e/core/skills/publish.py)。

[当前 strict validator](https://github.com/LabRAI/LangSkills/blob/039f8b421237452bd70b3df44ac908f8720fb99e/core/scripts/validate_skills/core_checks.py) 主要检查结构、来源 URL 和许可证。Verification 缺失或没有可操作命令会产生警告；该检查本身不执行验证命令。[可选 SkillGate](https://github.com/LabRAI/LangSkills/blob/039f8b421237452bd70b3df44ac908f8720fb99e/core/skills/gate.py) 默认关闭，其判断也不能等同于科学有效性证明。

本提案只借鉴来源可追溯的记录方式，用于记录参考查询为何进入参考集。来源齐全不表示查询已获正常性确认，也不表示更新保持报警误差性质。

### HazardWeaver：科学适用条件的提取与判断

Wangshu Zhu、Xueqi Cheng、Liang Wu、Yushun Dong，*HazardWeaver: Scientific Route Selection for Hazard Analysis Agents*，arXiv:2610.03591，v1，2026-10-02。

[全文 v1](https://arxiv.org/html/2610.03591v1) · [元数据](https://arxiv.org/abs/2610.03591)

重点读第 3.1 节和 Appendix J。条件提取保留语义角色、适用范围、逻辑方向和支持证据，内容可包括必要观察、假设、有效范围及失败情形。判断区分 satisfied、violated 和 unresolved；必要条件全部满足才能适用，条件违反会否决，证据不足时保留未定并限制立即执行。

其结论依赖固定证据库、能力目录与所研究任务的条件。新证据需要重新整理和复核。本提案只借鉴显式记录假设和证据状态的原则，例如记录校准支持的正常变化范围、验证来源及尚未确认的部署条件。对 API 监控是否有效仍需本项目验证。

## 顺序监控必须比较的已有工作

- Thomas Flynn、Shinjae Yoo，*Change Detection with the Kernel Cumulative Sum Algorithm*，CDC 2019。[论文及版本](https://arxiv.org/abs/1903.01661)。KCUSUM 用基于 MMD 的统计量累积变化证据，已研究发现延迟和误报前平均运行长度；MMD 加累积本身不是新方法贡献。
- Oliver Cobb、Arnaud Van Looveren、Janis Klaise，*Sequential Multivariate Change Detection with Calibrated and Memoryless False Detection Rates*，AISTATS 2022，PMLR 151:226–239。[正式页面](https://proceedings.mlr.press/v151/cobb22a.html)。CALM 用模拟设置随时间变化的阈值。其无变化假设下的性质不能直接延伸到任意正常漂移。

## 证据状态

本仓库包含研究提案和实验设计，没有新的检测性能结果。公开项目的来源追踪和适用条件方法只提供设计参考。真实生产时间、账号历史、额外验证信息及数据实际可访问性需要在执行研究时核实和记录。
