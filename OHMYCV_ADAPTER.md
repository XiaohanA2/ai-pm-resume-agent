# Oh My CV Adapter

本系统已拉取并适配 GitHub 项目 [Renovamen/oh-my-cv](https://github.com/Renovamen/oh-my-cv)。

结论：它不是一个 `oh-my-cv input.md -o output.pdf` 形式的命令行渲染器，而是一个 Nuxt/Vue 本地 Web 应用。正确接入方式是把系统生成的简历 Markdown 打包成 Oh My CV 支持导入的 JSON，然后在本地 Oh My CV 里预览、微调并导出 PDF。

## 第一次使用

当前工作区已经完成过依赖安装；如果你就在这台机器上继续用，可以直接跑 `scripts/start_ohmycv`。只有换机器、重新克隆或删除 `node_modules` 后，才需要带 `--install`。

```bash
cd /Users/chenzhuoyang/repo/Workspace/interview/resume-agent
scripts/export_for_ohmycv profiles/base_resume.md
scripts/start_ohmycv
```

新环境第一次安装时再用：

```bash
scripts/start_ohmycv --install
```

打开终端里 Nuxt 打印的 localhost 地址，在 Oh My CV 页面点击 Import，选择：

```text
outputs/ohmycv_import/base_resume.json
```

导入后进入编辑器预览，再用页面里的 PDF/Print 导出。

本次验证通过的地址是：

```text
http://localhost:3000/
```

## 日常生成简历

```bash
cd /Users/chenzhuoyang/repo/Workspace/interview/resume-agent
scripts/validate_contracts
scripts/export_for_ohmycv outputs/resumes_md/<company-position-date>.md
scripts/start_ohmycv
```

如果只是检查基础简历：

```bash
scripts/export_for_ohmycv profiles/base_resume.md
```

## Troubleshooting

如果启动后看到类似报错：

```text
[plugin:vite:import-analysis] Failed to resolve entry for package "@renovamen/utils"
```

原因是 Oh My CV monorepo 的内部 workspace packages 还没生成 `dist/index.mjs`。当前 `scripts/start_ohmycv` 已经会自动检测并运行：

```bash
pnpm --dir vendor/oh-my-cv build-fast:pkg
```

如果你想手动修复，也可以直接运行上面这条命令，然后再执行：

```bash
scripts/start_ohmycv
```

## 隐私说明

优先使用本地 `scripts/start_ohmycv`，不要把私人简历、JD、联系方式、HR 记录上传到在线站点。Oh My CV 官方在线站点也支持本地浏览器存储，但本工作流默认按 `private-local` 处理。

## 开源协议提醒

`Renovamen/oh-my-cv` 使用 GPL-3.0。作为本地私人工具使用没有问题；如果未来要公开分发一个包含其代码的衍生系统，需要单独处理 GPL-3.0 的开源合规。

## 已排除的同名仓库

`vendor/oh-my-cv-frontend` 是另一个同名/相近仓库，实测只是普通简历表单前端，不具备 Markdown 到 PDF 的 Oh My CV 渲染能力。本系统不使用它。
