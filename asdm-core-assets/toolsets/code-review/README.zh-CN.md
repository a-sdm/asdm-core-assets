# ASDM 工具集 - 代码审查 (Code Review)

toolset-id: code-review
toolset-name: Code Review
version: 0.0.2
updated-date: 2026-03-16
toolset-description: 基于 Pull Request 的 AI 辅助代码审查综合工具集。

## 概述

Code Review（工具集 ID：`code-review`）是一个基于 Pull Request 的 AI 辅助代码审查综合工具集。它提供了丰富的工具用于分析代码变更、检测潜在问题并生成审查报告。

本工具集帮助团队在开发流程早期维护代码质量、识别安全漏洞并及时发现问题。

## 功能特性

Code Review 的主要功能：

- **PR Diff 分析**：获取并分析 Pull Request 的代码变更
- **PR 摘要**：生成 PR 变更的综合摘要
- **全面代码审查**：从多个维度进行深入的代码审查
- **安全专项审查**：识别安全漏洞和风险
- **快速审查**：对常见问题提供快速反馈
- **多维度分析**：从安全性、性能、可维护性和可读性等多个角度分析代码
- **最佳实践检查**：验证代码是否符合语言特定的最佳实践和编码规范
- **审查报告生成**：生成带有严重程度分级结构化审查报告

## 工具集安装流程

`INSTALL.md` 将通过以下步骤安装工具集：

- 检测当前的 `Agentic Engine` 提供商（如 Claude Code、GitHub Copilot、腾讯 CodeBuddy）
- 在提供商的入口点创建 Code Review 的快捷命令（如 `.claude/commands`、`.github/prompts`、`.codebuddy/commands`）

## 工具集工作流

安装 `Code Review` 后，用户可以使用以下命令：

- `/asdm-pr-review`：对 PR 执行全面代码审查
- `/asdm-pr-security-review`：专注于安全漏洞和问题
- `/asdm-pr-quick-review`：快速审查常见问题并获取快速反馈
- `/asdm-pr-diff`：获取 PR 变更差异
- `/asdm-pr-summary`：生成 PR 变更摘要

## 工具集结构

工具集的目录结构如下：

```
.asdm/toolsets/code-review/
├── INSTALL.md                              ## 工具集安装说明
├── README.md                               ## 当前文档
├── actions                                 ## Code Review 指令文件
│   ├── asdm-pr-review.md                   ## 全面 PR 代码审查指令
│   ├── asdm-pr-security-review.md          ## 安全专项代码审查指令
│   ├── asdm-pr-quick-review.md             ## 快速轻量级代码审查指令
│   ├── asdm-pr-diff.md                     ## 获取 PR Diff 指令
│   └── asdm-pr-summary.md                  ## 生成 PR 摘要指令
└── specs                                   ## 模板与规范
    ├── pr-review-spec.md                   ## PR 审查流程规范
    ├── pr-analysis-spec.md                 ## PR 分析规范
    ├── security-review-spec.md             ## 安全审查规范
    └── review-report-template.md           ## 审查报告生成模板
```

## 可用指令

### 1. PR 代码审查 (`asdm-pr-review.md`)

对 Pull Request 执行全面的代码审查：

- 分析代码变更中的质量问题
- 检查安全漏洞
- 识别潜在的 Bug 和代码异味
- 审查性能相关考量
- 生成结构化审查报告

### 2. PR 安全审查 (`asdm-pr-security-review.md`)

对 Pull Request 执行安全专项代码审查：

- 识别安全漏洞和风险
- 验证认证和授权实现
- 检查常见攻击向量
- 确保安全的数据处理实践
- 提供安全修复指导

### 3. PR 快速审查 (`asdm-pr-quick-review.md`)

执行快速、轻量级的代码审查以获取即时反馈：

- 检查基本代码质量和风格
- 识别明显的 Bug 和错误
- 验证基本最佳实践
- 支持快速迭代周期

### 4. PR Diff (`asdm-pr-diff.md`)

获取并格式化 PR 变更：

- 获取 PR Diff
- 按文件组织变更
- 提供结构化输出用于分析

### 5. PR 摘要 (`asdm-pr-summary.md`)

生成全面的 PR 摘要：

- 概括 PR 中的所有变更
- 识别受影响的组件
- 列出关键修改
- 提供高层次概览

## 审查维度

Code Review 工具集从多个维度分析代码：

1. **代码质量**：可维护性、可读性及代码异味检测
2. **安全性**：漏洞检测、注入风险、认证问题
3. **性能**：效率问题、内存泄漏、优化机会
4. **最佳实践**：语言特定的编码规范和设计模式
5. **测试**：测试覆盖率、测试质量及缺失的测试
6. **文档**：缺失的文档、不清晰的注释

## 严重程度分级

审查发现按严重程度分类：

| 级别 | 描述 | 所需操作 |
|------|------|----------|
| **严重 (Critical)** | 安全漏洞、严重 Bug、数据丢失风险 | 合并前必须修复 |
| **高 (High)** | 可能导致问题的重大缺陷 | 合并前应当修复 |
| **中 (Medium)** | 代码质量问题、可维护性隐患 | 建议修复 |
| **低 (Low)** | 轻微改进、风格偏好 | 可选修复 |
| **信息 (Info)** | 提示信息、建议 | 无需操作 |

## 使用示例

### 对 PR 执行全面代码审查

```shell
# 使用斜杠命令
/asdm-pr-review 123

# 或遵循指令文件
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-review.md
```

### 执行安全专项审查

```shell
# 使用斜杠命令
/asdm-pr-security-review 123

# 或遵循指令文件
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-security-review.md
```

### 执行快速审查

```shell
# 使用斜杠命令
/asdm-pr-quick-review 123

# 或遵循指令文件
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-quick-review.md
```

### 获取 PR Diff

```shell
# 使用斜杠命令
/asdm-pr-diff 123

# 或遵循指令文件
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-diff.md
```

### 生成 PR 摘要

```shell
# 使用斜杠命令
/asdm-pr-summary 123

# 或遵循指令文件
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-summary.md
```

## 前置条件

- 可访问 Pull Request 系统（GitHub、GitLab、Bitbucket 等）
- 具有完整提交历史的 Git 仓库
- 了解项目的编码标准和规范

## 与其他工具集的集成

Code Review 可以独立使用，也可以与其他工具集集成：

- **基础工具 (Basic Tools)**：配合提交工作流使用
- **上下文构建器 (Context Builder)**：利用项目上下文获取更深入的审查洞察
- **CI/CD 流水线**：作为 Pull Request 工作流的一部分实现自动化审查

## 获取帮助

如遇到 Code Review 工具集的问题，请参考：

- [ASDM 文档](https://asdm.ai/docs)
- 工具集 README：`.asdm/toolsets/code-review/README.md`
- 规范文档：`.asdm/toolsets/code-review/specs/`

## 版权与许可

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

基于 PROPRIETARY SOFTWARE LICENSE 授权。详见项目根目录中的 [LICENSE](LICENSE)。
