# Reliable Online Monitoring of LLM API Traffic

一套只围绕 proposed project 的英文 Beamer 研究提案：研究正常使用需求变化时，如何持续监控 LLM API 中的模型抽取流量。

**8 页正文 + 3 页附页，共 11 页。** 演示稿直接从项目开始，保持原稿的 16:9 蓝白主题。所有新实验、对照和贡献判断均为提议，目前没有新检测结果。

## 内容

正文依次介绍公开 MMD 检测器、持续监控问题、受控顺序重放、顺序检测基线、整段误报与及时检出指标、参考集更新、贡献成立条件及预期产出。

附页说明数据划分与信息边界、已有顺序检测工作，以及 LangSkills 和 HazardWeaver 对来源追溯和适用条件记录的启发。两项工作仅提供方法设计上的参考，当前提案的研究对象仍是 LLM API 持续监控。

## 文件

| 文件 | 用途 |
|---|---|
| `main.tex` | 主入口、文档元数据与附页设置 |
| `theme.tex` | Metropolis 主题、字体、配色与排版辅助命令 |
| `slides.tex` | 8 页项目正文 |
| `appendix.tex` | 3 页项目附页 |
| `figures/*.tex` | 可编辑的检测流程、查询顺序与参考更新示意图 |
| `main.pdf` | 编译后的完整演示稿 |
| `speaker_notes.md` | 与当前页码一致的英文讲稿和中文提示 |
| `START_HERE.md` | 数据核查、复现入口与实验协议 |
| `READING_LIST.md` | 资料阅读重点、来源版本与结论边界 |
| `references.bib` | 所用论文与公开项目的元数据 |
| `build.sh` | 本地编译入口 |

## Overleaf

把仓库中的源文件上传到 Overleaf，主文件设置为 `main.tex`，编译器选择 **pdfLaTeX**。也可以将本仓库作为 Overleaf 的 GitHub 导入源。

项目使用 Latin Modern 字体，无需下载外部图片或启用 shell-escape。演示稿在页脚直接链接文献，当前构建不需要 BibTeX / Biber；`references.bib` 保留供后续扩写使用。

## 本地编译

需要含 Beamer、Metropolis、TikZ/PGFPlots、Latin Modern 和 appendixnumberbeamer 的 TeX 环境，以及 latexmk。

```bash
bash build.sh
```

中间文件保存在 `build/`，最新输出复制到项目根目录的 `main.pdf`。

## 研究边界

公开数据首先用于受控顺序重放。查询条数是发现延迟的单位，现有资料不能证明包含真实生产时间戳或账号历史。原论文已经研究混合流量、自适应攻击与参考污染；本提案关注完整连续报警流程在正常变化下的行为。

共同校准目标需要与独立测试中实际达到的误报率一起报告。参考更新所用的额外验证信息必须明确来源和预算。是否需要新机制，取决于已有顺序方法和公平校准后是否仍有稳定的残余问题。

作者：Sheng'en Li。资料核查日期：2026-10-22。
