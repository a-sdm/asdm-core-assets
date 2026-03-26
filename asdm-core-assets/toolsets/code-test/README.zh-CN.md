# ASDM 工具集 - 代码测试 (CodeTest)

toolset-id: code-test
toolset-name: CodeTest
version: 0.0.2
updated-date: 2026-03-16
toolset-description: 面向自动化测试用例生成、API 测试、UI 测试和测试报告的综合测试工具集。

## 概述

CodeTest 工具集提供了一个完整的测试解决方案，与 AI 编码工具集成以实现测试生命周期的自动化。它支持多种测试范式，包括单元测试、集成测试、API 测试和 UI 测试。

## 功能特性

### 1. 测试用例生成

基于代码分析和规范自动生成全面的测试用例：

- 函数和方法级别的单元测试生成
- 集成测试场景覆盖
- 边界条件检测与覆盖
- 测试数据自动生成

### 2. API 测试

生成并执行带有完整请求/响应验证的 API 测试：

- RESTful API 测试
- GraphQL API 支持
- 认证授权测试
- 性能基准测试
- Schema 验证

### 3. UI 测试

面向 Web 应用的自动化 UI 测试：

- 端到端测试工作流
- 视觉回归测试
- 跨浏览器兼容性测试
- 可访问性测试
- 响应式设计验证

### 4. 测试报告

全面的测试报告与分析：

- 测试执行摘要
- 覆盖率分析
- 趋势可视化
- CI/CD 集成
- 多格式导出（HTML、JSON、JUnit XML）

## 组件说明

### 指令 (Actions)

指令可转换为 AI 编码工具的斜杠命令：

| 指令 | 命令 | 说明 |
|------|------|------|
| `asdm-test-generate-cases` | `/asdm-test-generate-cases` | 从代码生成测试用例 |
| `asdm-test-api` | `/asdm-test-api` | 创建并执行 API 测试 |
| `asdm-test-ui` | `/asdm-test-ui` | 执行 UI 测试 |
| `asdm-test-report` | `/asdm-test-report` | 生成测试报告 |

### 规范 (Specifications)

每个指令的详细规范位于 `specs/` 目录：

- [测试用例生成规范](specs/specs4asdm-test-generate-cases.md)
- [API 测试规范](specs/specs4asdm-test-api.md)
- [UI 测试规范](specs/specs4asdm-test-ui.md)
- [测试报告规范](specs/specs4asdm-test-report.md)

### 工具 (Tools)

CLI 环境下的实用工具位于 `tools/` 目录：

- `test-generator/` — 测试用例生成引擎
- `api-tester/` — API 测试框架
- `ui-tester/` — UI 测试自动化
- `report-generator/` — 报告生成工具

## 工具集结构

```
.asdm/toolsets/code-test/
├── INSTALL.md                              ## 工具集安装说明
├── README.md                               ## 当前文档
├── actions                                 ## CodeTest 指令文件
│   ├── asdm-test-generate-cases.md         ## 测试用例生成指令
│   ├── asdm-test-api.md                    ## API 测试指令
│   ├── asdm-test-ui.md                     ## UI 测试指令
│   └── asdm-test-report.md                 ## 测试报告生成指令
├── specs                                   ## 规范文档
│   ├── specs4asdm-test-generate-cases.md   ## 测试用例生成规范
│   ├── specs4asdm-test-api.md              ## API 测试规范
│   ├── specs4asdm-test-ui.md               ## UI 测试规范
│   └── specs4asdm-test-report.md           ## 测试报告规范
└── tools                                   ## 实用工具
    ├── test-generator/                     ## 测试用例生成引擎
    ├── api-tester/                         ## API 测试框架
    ├── ui-tester/                          ## UI 测试自动化
    └── report-generator/                   ## 报告生成工具
```

## 工具集安装流程

`INSTALL.md` 将通过以下步骤安装工具集：

- 检测当前的 `Agentic Engine` 提供商（如 Claude Code、GitHub Copilot、腾讯 CodeBuddy）
- 在提供商的入口点创建 CodeTest 的快捷命令（如 `.claude/commands`、`.github/prompts`、`.codebuddy/commands`）

## 快速开始

### 1. 为文件生成测试用例

```shell
/asdm-test-generate-cases --file src/utils.js --type unit
```

### 2. 运行 API 测试

```shell
/asdm-test-api run --collection api-tests.json --env staging
```

### 3. 执行 UI 测试

```shell
/asdm-test-ui run --suite e2e --browser chrome
```

### 4. 生成测试报告

```shell
/asdm-test-report generate --format html --output reports/
```

## 安装

详见 [INSTALL.md](INSTALL.md) 获取详细的安装说明。

## 支持的技术栈

### 测试框架
- Jest、Mocha、Vitest（JavaScript/TypeScript）
- PyTest、unittest（Python）
- JUnit、TestNG（Java）
- xUnit、NUnit（.NET）

### API 测试
- REST 客户端
- GraphQL
- OpenAPI/Swagger
- Postman 集合

### UI 测试
- Playwright
- Selenium
- Cypress
- Puppeteer

### 报告生成
- Allure
- Mochawesome
- HTML 报告
- JUnit XML

## 最佳实践

1. **测试覆盖率**：目标至少 80% 的代码覆盖率
2. **测试隔离**：每个测试用例应相互独立
3. **描述性命名**：使用清晰的测试用例命名
4. **断言**：包含有意义的断言
5. **清理**：正确清理测试资源

## 配置

配置文件位于 `~/.asdm/toolsets/codetest-toolset/`：
- `config.json` — 主配置文件
- `templates/` — 测试模板
- `environments/` — 环境配置

## CI/CD 集成

工具集支持与主流 CI/CD 平台的集成：
- GitHub Actions
- GitLab CI
- Jenkins
- Azure DevOps
- CircleCI

## 与其他工具集的集成

CodeTest 可以独立使用，也可以与其他工具集集成以支持完整的开发工作流：

- **基础工具 (Basic Tools)**：配合提交工作流使用
- **上下文构建器 (Context Builder)**：利用项目上下文优化测试生成
- **代码审查 (Code Review)**：将测试结果集成到代码审查流程中
- **CI/CD 流水线**：作为部署工作流的一部分实现自动化测试

## 获取帮助

如遇到 CodeTest 工具集的问题，请参考：

- [ASDM 文档](https://asdm.ai/docs)
- 工具集 README：`.asdm/toolsets/code-test/README.md`
- 规范文档：`.asdm/toolsets/code-test/specs/`

## 版权与许可

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

基于 PROPRIETARY SOFTWARE LICENSE 授权。详见项目根目录中的 [LICENSE](LICENSE)。
