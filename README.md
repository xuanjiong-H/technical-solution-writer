# Technical Solution Writer

一个面向后端研发场景的 Codex Skill。它会根据用户需求和实际项目代码，生成可用于研发评审、开发落地和技术复盘的 Markdown 技术方案。

## 功能特性

- 分析项目结构、技术栈、后端入口、模块、接口、数据模型、配置和测试。
- 将需求关键词映射到实际代码，识别可能受影响的文件、类、方法和模块。
- 生成整体设计、调用链、异常处理、兼容性和回滚方案。
- 输出具体的接口、功能、数据、配置和依赖变更说明。
- 仅估算后端实现工作量，并列出文件级修改范围。
- 生成覆盖正常流程、边界条件、异常处理和回归范围的测试方案。
- 区分已确认问题与待确认问题，避免把推测写成事实。

## 适用场景

适用于以下类型的任务：

- 技术方案和技术设计
- 研发方案和开发计划
- 后端改造方案
- 需求开发分析
- 工作量评估
- 重构方案和实现评估

## 安装

Codex Skill 本质上是放在 Codex Skills 目录下的一个文件夹。安装时需要保留项目目录结构，只复制 Skill 所需文件，不需要复制 Git 仓库元数据。

### Windows PowerShell

将本项目复制到用户级 Skills 目录：

```powershell
$skillsDir = if ($env:CODEX_HOME) {
    Join-Path $env:CODEX_HOME "skills"
} else {
    Join-Path $HOME ".codex\skills"
}

$target = Join-Path $skillsDir "technical-solution-writer"
New-Item -ItemType Directory -Force $target | Out-Null

Get-ChildItem -Force . |
    Where-Object { $_.Name -ne ".git" } |
    ForEach-Object {
        Copy-Item -LiteralPath $_.FullName -Destination $target -Recurse -Force
    }
```

如果当前目录不是本项目目录，请先进入项目目录，或将 `.` 替换为本项目的完整路径。

### macOS / Linux

```bash
SKILLS_DIR="${CODEX_HOME:-$HOME/.codex}/skills"
TARGET="$SKILLS_DIR/technical-solution-writer"

mkdir -p "$TARGET"
find . -mindepth 1 -maxdepth 1 ! -name ".git" -exec cp -R {} "$TARGET/" \;
```

安装完成后，重新打开 Codex，或重新加载 Skills，使新 Skill 生效。

## 使用方法

### 显式调用

在 Codex 中使用以下方式调用：

```text
$technical-solution-writer
```

也可以直接描述需求，例如：

```text
请使用 $technical-solution-writer，分析当前项目并生成用户登录改造的后端技术方案。
```

### 自动触发

当请求中包含以下意图时，Skill 也可以被自动匹配：

- 技术方案
- 技术设计
- 研发方案
- 开发方案
- 后端方案
- 后端改造方案
- 工作量评估
- 需求开发分析

建议在请求中同时提供：

1. 需求背景和目标。
2. 项目路径，或说明使用当前工作目录。
3. 期望的输出路径，或说明使用默认路径。
4. 已知的业务约束、接口约束和兼容性要求。

## 默认行为

- 项目路径未指定时，使用当前工作目录。
- 输出路径未指定时，在项目根目录生成 `technical-solution.md`。
- 默认只分析后端实现。
- 测试方案会被生成，但测试代码和测试执行不计入后端工作量。
- 找不到相关代码或无法确认的业务规则时，会在方案中明确记录缺口或待确认问题。
- 除非用户明确要求，不会修改业务源代码。

## 输出结构

生成的技术方案必须包含以下一级章节：

1. 需求概述
2. 整体设计思路
3. 接口/功能变更说明
4. 工作量预估
5. 测试方案
6. 已/待确认问题
7. 注意事项

默认输出模板位于 [`references/output-template.md`](references/output-template.md)，内容规则位于 [`references/technical-solution-rules.md`](references/technical-solution-rules.md)。

## 项目结构

```text
technical-solution-writer/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    ├── output-template.md
    └── technical-solution-rules.md
```

## 开发与维护

修改 Skill 行为时，优先更新 [`SKILL.md`](SKILL.md)；修改输出格式时，更新 [`references/output-template.md`](references/output-template.md)；修改内容质量要求时，更新 [`references/technical-solution-rules.md`](references/technical-solution-rules.md)。

提交前建议检查：

```powershell
Get-Content -Raw -Encoding UTF8 .\SKILL.md
Get-Content -Raw -Encoding UTF8 .\README.md
```

## License

当前仓库未声明开源许可证。如需公开发布到 GitHub，建议根据实际授权意愿补充 `LICENSE` 文件。
