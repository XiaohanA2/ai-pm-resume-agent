# AI PM Resume Agent

一个本地优先的 AI 产品经理求职工作流：把真实项目事实、JD 分析、简历定制、面试准备和投递追踪串成可审计的链路。

本仓库是**公开工作流模板**。它不包含任何候选人的简历、项目材料、JD、联系人、内部指标或输出文件；这些内容全部在本机私有目录中维护，并被 `.gitignore` 排除。

## 它解决什么问题

- 面向国内 AI PM / 大模型 / Agent / AI 应用 / 搜索推荐等岗位，先做结构化 JD 分析，再决定是否投递。
- 把每个 JD 关键词映射到真实项目证据、简历优先级和面试风险，避免“关键词硬贴”。
- 简历定制只改变表达和项目排序，不改变事实内核；`inferred` 和 `hypothesis` 不可进入最终简历。
- 对投递和 HR 消息保留人工确认：工作流不自动投递、不绕验证码、不做反检测。

## 公开模板与私有数据的边界

| 公开提交 | 仅本地保存 |
| --- | --- |
| schema、提示词、校验/导出/看板脚本、模板 CSS | 简历、项目事实与介绍、原始证据、JD、匹配结果、投递记录、仪表盘、PDF/JSON 输出 |

`config/profile.yml`、`config/target.yml`、`profiles/`、`projects/`、`jd/`、`evidence/`、`trackers/`、`dashboards/`、`outputs/`、`market/` 和 `vendor/` 都是本地内容，不会被版本控制。

## 快速开始

```bash
git clone <your-repository-url>
cd ai-pm-resume-agent
scripts/init_private_workspace
scripts/validate_contracts
```

初始化脚本只会从 `templates/private-inputs/` 复制缺失的占位文件，**绝不覆盖**已有私有内容。随后用真实信息替换占位内容，并为每个项目建立 `projects/<project-id>/01-facts.yml`。

当私有素材、至少一条 JD 和追踪文件准备好后，运行严格校验：

```bash
scripts/validate_contracts --require-private-inputs
```

## 日常执行顺序

1. 先确认项目事实：阅读 `schemas/enums.yml` 与 `schemas/project_facts.schema.yml`，每项事实标明来源等级和是否允许写进简历。
2. 把真实 JD 放到 `jd/inbox/`，按 `prompts/02_analyze_jd.md` 规范化岗位字段与风险。
3. 按 `prompts/03_score_match.md` 输出匹配分、硬门槛、关键词—证据—风险表和建议动作。
4. 只对 shortlisted JD 按 `prompts/04_tailor_resume.md` 生成定制简历、change log、bullet provenance 与面试风险。
5. 用 `prompts/05_review_resume.md` 审核；通过后再导入 Oh My CV 预览/导出，并手工确认投递。
6. 需要投递看板时运行 `scripts/build_trackers`，它会从本地 `trackers/applications.yml` 生成表格和看板。

## 核心命令

```bash
scripts/init_private_workspace
scripts/validate_contracts
scripts/validate_contracts --require-private-inputs
scripts/export_for_ohmycv profiles/base_resume.md
scripts/start_ohmycv
scripts/build_trackers
```

`scripts/export_for_ohmycv` 仅生成可导入的本地 JSON；`scripts/start_ohmycv` 启动本地 Oh My CV 页面以预览和导出 PDF。详情见 [OHMYCV_ADAPTER.md](OHMYCV_ADAPTER.md)。

## 安全和真实性约束

- 不虚构公司、岗位、项目、学历、指标、上线、获奖或负责人身份。
- 不把产品策略描述成模型训练或算法工程实现，除非私有事实层明确支持。
- 不把联系方式、HR 信息、JD 来源、内部指标和原始证据提交到公开仓库。
- 不自动投递或自动发送消息；任何外部申请均须人工确认。

更完整的执行约束见 [AGENTS.md](AGENTS.md)，字段约束见 `schemas/`，具体作业书见 `prompts/`。
