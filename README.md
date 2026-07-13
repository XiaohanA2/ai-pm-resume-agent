# AI PM Resume Agent

一个本地优先的 AI 产品经理求职工作流。它把真实经历整理成项目证据，结合 JD 做匹配和简历定制，并生成面试准备与投递追踪材料。

所有中文生成内容会先遵守 [中文写作风格规范](prompts/00_chinese_writing_style.md)：优先具体事实和自然表达，减少模板化转折与无证据的抽象收尾；事实和来源审核仍以项目证据规则为准。

适合 AI 产品经理、大模型产品经理、Agent 产品经理、AI 应用产品经理和搜索推荐产品经理等岗位。默认面向国内招聘渠道，但不自动投递或发送消息。

## 如何运行

需要 Git；导出/预览简历时还需要 Node.js，启动 Oh My CV 时需要 pnpm。

```bash
git clone https://github.com/XiaohanA2/ai-pm-resume-agent.git
cd ai-pm-resume-agent

# 创建只保存在本机的简历、配置和投递记录模板；不会覆盖已有文件
scripts/init_private_workspace

# 检查公开工作流和已存在的私有输入是否符合约定
scripts/validate_contracts
```

完成自己的简历、项目事实和至少一条 JD 后，运行严格检查：

```bash
scripts/validate_contracts --require-private-inputs
```

常用命令：

```bash
# 根据本地基础简历生成 Oh My CV 导入文件
scripts/export_for_ohmycv profiles/base_resume.md

# 启动本地 Oh My CV 预览与 PDF 导出页面
scripts/start_ohmycv

# 从本地投递记录生成 CSV 和看板
scripts/build_trackers
```

## 工作流

| 模块 | 作用 | 主要位置 |
| --- | --- | --- |
| 1. 项目事实库 | 为每段真实经历记录职责、证据、指标、AI 边界和风险；未确认事实不能写入简历。 | `projects/`、`schemas/project_facts.schema.yml`、`prompts/01_extract_project.md` |
| 2. JD 解析 | 把岗位拆为领域、用户、任务、AI 要求、指标、硬门槛和风险。 | `jd/inbox/`、`schemas/jd.schema.yml`、`prompts/02_analyze_jd.md` |
| 3. JD 匹配 | 将 JD 关键词映射到项目证据，输出匹配分、项目排序、缺口和建议动作。 | `trackers/match_scores.yml`、`prompts/03_score_match.md` |
| 4. 简历定制 | 仅对值得投递的岗位重排项目与 bullet；保留来源记录和变更日志。 | `prompts/04_tailor_resume.md`、`outputs/` |
| 5. 简历审核 | 检查首屏表达、事实来源和 AI claim 是否经得起面试追问。 | `prompts/05_review_resume.md` |
| 6. 面试准备 | 围绕主故事、备选故事、风险问题和追问生成面试材料。 | `prompts/06_interview_prep.md` |
| 7. 投递追踪 | 记录投递状态、下一步动作、简历版本和面试材料。 | `trackers/`、`scripts/build_trackers` |

推荐顺序：**确认项目事实 → 导入 JD → 匹配评分 → 定制简历 → 审核与 PDF 导出 → 人工投递 → 追踪复盘**。

## 隐私与真实性

本仓库只发布流程、schema、提示词和脚本。你的简历、项目材料、JD、联系人、证据、投递记录和输出文件都在本机保存，且已被 `.gitignore` 排除。

- 不虚构项目、职责、指标、学历或上线结果。
- `inferred` 和 `hypothesis` 不进入最终简历。
- 不把产品工作包装成模型训练或算法开发。
- 不自动投递、不绕过验证码、不自动发送 HR 消息；任何外部操作均需人工确认。

更完整的工作规则见 [AGENTS.md](AGENTS.md)，Oh My CV 本地导出说明见 [OHMYCV_ADAPTER.md](OHMYCV_ADAPTER.md)。
