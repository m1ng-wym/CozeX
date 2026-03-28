# CozeX

**One Prompt to Deploy, Zero Mouse Clicks.**

An AI-Driven Coze Workflow Engine Powered by Multi-Agent Orchestration. With Playwright as the hands and LLM as the brain, this pipeline autonomously builds, tests, and deploys Coze workflows. From blueprint to production without touching your mouse.

## 是什么

CozeX 是一个基于 AI Agent 的 Coze 工作流自动化流水线。

传统的 Coze 工作流开发需要人工在浏览器中完成：添加节点、连接节点、配置变量、测试、发布。整个过程繁琐重复。

CozeX 通过引入多 Agent 协作架构，将整个流程自动化：

- **planner** 分析用户需求，制定执行计划
- **executor** 通过 Playwright MCP 控制浏览器，按照计划逐步执行
- **validator** 在每个关键步骤验证正确性，确保质量
- **retrospect** 开发完成后复盘总结，优化流程

用户只需描述需求，CozeX 就能自动完成从想法到上线的一切。

## 工作流列表

### cn_to_en_translator

中文到英文翻译工作流，实现精准翻译。适用于国际化内容处理、跨语言沟通等场景。

## 技术架构

### Agent 系统

| Agent | 描述 |
|-------|------|
| coze-planner | 需求分析专家。读取用户需求，参考扣子官方文档，设计工作流节点架构，制定详细的执行计划（节点类型、连接顺序、变量配置、测试用例）。 |
| coze-executor | 执行专家。严格遵循 planner 的计划，通过 Playwright MCP 控制浏览器完成：导航、添加节点、连接节点、配置变量、运行测试、发布工作流。每个阶段完成后调用 validator 验证。 |
| coze-validator | QA 专家。对每个关键步骤进行验证：节点是否正确添加、连接是否成功、变量配置是否正确、测试输出是否符合预期。如有问题，提供诊断和修复建议。 |
| coze-retrospect | 复盘专家。开发完成后回顾整个过程，总结经验教训，识别文档和流程中的不足，提出优化建议并执行文档更新。 |

### Skill 系统

本项目还维护了一套完整的 Skill 文档：

- **coze-workflow-builder**: 扣子平台低代码工作流搭建的完整指南，包含节点类型详解、操作步骤、代码模板、常见问题与解决方案。

### 工具链

| 工具 | 用途 |
|------|------|
| Playwright MCP | 浏览器自动化控制 |
| Claude Code | AI Agent 编排框架 |
| Coze 平台 | 低代码工作流托管 |

## 相关资源

- [Coze 官方文档](https://docs.coze.cn/)
- [低代码工作流指南](https://docs.coze.cn/guides/workflow)
- [扣子平台](https://www.coze.cn/)
