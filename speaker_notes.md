# Speaker notes: proposed research project

正文 8 页，附页 3 页。英文段落可直接用于讲解，中文提示说明每页应保留的限定。新实验均为提议，没有新性能结果。

## 1. Proposed project: continuous LLM API monitoring

> I propose a project on continuous monitoring of model extraction in LLM API traffic. The released detector compares a query window with a benign reference through a fixed sentence encoder and MMD. Its data and code provide a concrete experimental starting point. The paper already studies mixed traffic, adaptive attacks, and reference contamination. My focus is the behavior of a complete sequential monitor as normal usage changes.

中文提示：直接进入 proposed project。区分公开检测器的既有实验与本项目的连续监控问题。数据入口存在不等于已经下载或复现。

## 2. The research question is sustained monitoring

> The research question is how early extraction can be detected under changing normal usage at a comparable false-alarm budget. A window-level error rate does not determine the probability of an alarm during an entire run. A fixed reference can become stale, while unverified adaptation may absorb attack traffic. The query-order schematic illustrates controlled replay conditions. It does not show measured detector outputs.

中文提示：横轴是查询序号。重叠窗口存在依赖，不能用独立窗口假设直接推导整体误报。正常变化与跨窗口累积也是原论文明确讨论的局限。

## 3. Controlled replay under benign traffic changes

> I would compare stable and changing benign backgrounds, with and without sparse extraction traffic. The initial dataset pair is model_leeching, with Meaeq_SST-2 used to check whether the pattern persists. Before constructing windows, duplicates and common source items should be grouped and assigned to separate reference, development, calibration, and test pools. Since real production timing and account histories are not established by the released records, detection delay is measured in query counts.

中文提示：先根据唯一记录数量决定实验规模。话题变化的构造规则须固定，聚类只用开发数据。对同一语料的多次重排不能解释为独立部署样本。

## 4. Alarm rules and sequential baselines

> The original MMD configuration provides a restricted reproduction reference. For the new comparison, I would keep the representation fixed and compare a window rule calibrated for the whole run with an accumulator. KCUSUM and CALM are established baselines for kernel sequential detection and false-detection calibration. They should use common input pools, replay instances, decision opportunities, and calibration budgets, while retaining their native update and calibration mechanisms.

中文提示：固定分数的两规则对照用于分离报警机制的影响。KCUSUM 与 CALM 不必共享完全相同的中间统计量。不能把已知方法组合直接包装成新贡献。

## 5. Timely detection at a matched false-alarm target

> For benign runs, I would measure the probability of at least one alarm within a fixed query horizon. For attack runs, I would measure detection within a declared query budget after attack onset. The protocol stops at the first alarm. An alarm before the attack is a false alarm and does not count as successful detection. Main calibration includes the declared family of benign changes, and testing uses held-out queries and replay designs. Unknown drift after stable-only calibration is a separate stress test.

中文提示：共同校准目标与实际测试误报率都要报告。未检出记为删失，不能算零延迟；只汇报成功样本平均延迟会掩盖漏检。有限样本中的实际误报率不必恰好相同。

## 6. Reference updates under explicit information assumptions

> I would compare a frozen reference, direct admission of recent queries, and admission based on additional benign verification. Adaptation may reduce alerts from benign change, but could also hide persistent extraction. The source and cost of verification must be explicit, and methods with verified updates must share that information budget. Updates that use hidden test labels are oracle diagnostics. The comparison must calibrate the complete policy, including its updates.

中文提示：原论文已经测试参考污染，也建议谨慎更新。本项目要评价完整运行中的策略行为，不声称首次提出污染问题或参考更新。有标签诊断不自动构成性能上界。

## 7. Evidence required for a research contribution

> A method contribution should follow a reproducible failure that survives comparable calibration and established sequential baselines. Reference updates could reveal a focused issue if the information assumptions are clear. If ordinary recalibration or existing methods resolve the difficulty, that result should narrow the project. Any broader claim should also survive another dataset pair and a different source split.

中文提示：对照实验决定贡献范围。保留负面结果的解释，不承诺尚无证据支持的性能改善或创新。

## 8. Study scope and expected outputs

> The study evaluates query-only continuous monitoring under a declared family of benign changes. Its outputs are reproducible replay configurations, comparisons of false alarms and timely detection, and documented failures. Reference provenance and verification assumptions are part of the protocol. A new mechanism is justified only if a remaining failure supports a precise research question.

中文提示：结束时聚焦研究范围与产出。需要明确真实正常变化的业务含义、数据来源和组内相关工作边界。

# Backup slides

## A1. Replay protocol and information boundaries

> Source groups must remain separate, and the replay design must be fixed before testing. Reordering a fixed corpus measures sensitivity to the replay design. It does not add independent queries. The full alarm and update policy must be calibrated. If benign and extraction processes have the same observable query distribution, query-only monitoring cannot distinguish their intentions. Additional signals therefore change the information assumptions of the study.

中文提示：未来若加入账号、响应或人工验证信息，要明确每个方法能访问什么。

## A2. Related work and the proposed increment

> The MMD work provides the extraction setting and released starting point. KCUSUM already covers kernel sequential accumulation, and CALM studies sequential false-detection calibration. The proposed increment must be supported by a failure in continuous API replay or reference adaptation under explicit assumptions. Experiments should establish whether such a failure remains before a new method is designed.

中文提示：已有工作提供实质基线，不能只作背景引用。引用详情见阅读清单和 bibliography。

## A3. Provenance and applicability of reference updates

> LangSkills records source references and generation metadata for evidence-backed skill artifacts. Its structural checks do not establish scientific validity. HazardWeaver links method applicability conditions to evidence and distinguishes satisfied, violated, and unresolved conditions. For this monitoring proposal, these works motivate explicit records of reference provenance, verification evidence, and the benign-change regime supported by calibration. Their results do not establish the effectiveness of this documentation principle for API monitoring.

中文提示：LangSkills 的现有 validator 不会实际执行所有 Verification 命令。HazardWeaver 的科学适用条件判断受其证据库和任务范围限制。两者只支持设计原则，本项目仍需独立验证。

# Likely questions

**What is the method contribution?**

> It depends on whether a reproducible failure remains after fair calibration and established sequential comparisons. The proposed study is designed to identify that failure and its assumptions.

**Is this a production deployment study?**

> The current design uses controlled sequential replay and query-count delay. Production timing, account behavior, and extraction damage require additional data and a broader information model.

**What would a negative result mean?**

> If ordinary calibration or established methods resolve the difficulty, the conclusion should state that and narrow the research question. Such evidence is still useful for deciding what requires a new mechanism.
