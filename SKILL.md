---
name: technical-solution-writer
description: Generate complete backend-oriented technical solution documents in Markdown by analyzing project requirements, source code, modules, interfaces, estimated file changes, workload, testing scope, and confirmed or pending issues. Use when the user asks for a technical solution, technical design, development plan, refactoring plan, implementation assessment, backend change analysis, 技术方案、技术设计、研发方案、开发方案、后端方案、后端改造方案、工作量评估、需求开发分析 or a Markdown研发方案 before coding.
---

# Technical Solution Writer

根据用户需求和实际项目代码，生成可用于研发评审、开发落地和技术复盘的后端技术方案。先分析项目，再写方案；不要只套用模板，也不要修改业务代码。

## Workflow

1. 确定需求、项目根目录和输出路径。
   - 用户明确指定的项目路径和输出路径优先。
   - 未指定项目路径时使用当前工作目录。
   - 未指定输出路径时，使用当前项目目录下的 `technical-solution.md`，并在回复中说明。
2. 阅读需求上下文，识别业务目标、核心问题、涉及的功能和约束。
3. 检查项目结构、技术栈、后端入口、相关模块、接口、数据模型、配置和已有测试。
4. 将需求关键词映射到实际代码，确认可能受影响的文件、类、函数、接口和模块。
5. 推导实现方案、数据流、调用链、兼容性影响和异常处理方式。
6. 仅从后端范围估算工作量，列出具体文件和模块；不要把测试代码或前端工作量计入工作量估算。
7. 识别已确认问题和待确认问题。无法从需求或代码确认的内容必须进入待确认问题，不得自行补全为确定结论。
8. 按 `references/output-template.md` 生成完整 Markdown，并遵守 `references/technical-solution-rules.md`。
9. 生成前检查固定章节是否齐全、文件清单是否具体、方案是否与代码一致。
10. 将 Markdown 文件保存到目标路径，最后报告输出文件路径、分析范围和仍需确认的事项。

## Required Sections

输出必须完整包含以下一级章节，不得删除、合并或省略：

1. 需求概述
2. 整体设计思路
3. 接口/功能变更说明
4. 工作量预估
5. 测试方案
6. 已/待确认问题
7. 注意事项

## Analysis Rules

- 使用实际项目文件支持技术判断；找不到相关代码时明确说明搜索范围和缺口。
- 不虚构文件名、接口、数据库字段、业务结论、代码行数或已确认事项。
- 工作量预估必须尽可能落到文件、模块和修改内容；代码量使用合理区间，不要伪造精确值。
- 明确区分新增、修改、废弃和保持不变的接口或功能。
- 没有接口或功能变更时，明确写出“无变更”及判断依据。
- 方案应覆盖正常流程、异常流程、边界条件、兼容性和回滚关注点。
- 测试方案仍然必须生成，但测试代码不计入工作量预估。
- 只统计后端内容；前端、客户端、运营和测试执行工作不计入后端工作量。
- 对无法确认的业务规则，说明问题背景、影响、建议方案、建议理由以及是否需要业务确认。
- 生成标准 Markdown，保持标题层级、列表、表格和代码块格式正确。
- 除非用户明确要求，不直接编辑、创建或删除业务源代码。

## References

- 需要固定章节、输出字段和表格格式时，读取 `references/output-template.md`。
- 需要判断内容完整性、技术方案质量和禁止事项时，读取 `references/technical-solution-rules.md`。
