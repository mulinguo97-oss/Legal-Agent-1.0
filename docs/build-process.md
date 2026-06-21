# 法律小助手严谨制作流程

## 1. 文档目的

本文档用于规定“法律小助手”从规划到 MVP 的制作顺序。

它解决的问题是：不能在前置条件没完成时直接进入后续开发。例如不能在没有项目结构、环境变量、数据库、文件解析、Schema 校验的情况下，直接开始写 Agent 审查逻辑。

本文档强调：

- 每一步都有前置条件。
- 每一步都有明确产出。
- 每一步都有验收标准。
- 每一步完成后再进入下一步。
- 后续扩展能力要预留位置，但不能提前塞进 MVP。

## 2. 借鉴来源

本流程参考了以下开源项目的落地顺序和工程经验：

| 项目 | 借鉴点 |
|---|---|
| [Azure-Samples/rag_workshop](https://github.com/Azure-Samples/rag_workshop) | 采用索引、检索、增强生成、评估、Demo 的顺序，适合作为 RAG 学习路径。 |
| [GiovanniPasq/agentic-rag-for-dummies](https://github.com/GiovanniPasq/agentic-rag-for-dummies) | 先提供学习路径，再提供模块化项目；LLM、Embedding、PDF 转换、Agent workflow 都可替换。 |
| [Fan-Luo/Legal-RAG](https://github.com/Fan-Luo/Legal-RAG) | 法律 RAG 项目应拆分 ingest、retrieval、routing、pipeline、prompts、schemas 等模块。 |
| [hasnaintypes/lawbotics](https://github.com/hasnaintypes/lawbotics) | 合同分析类产品适合用 monorepo，把应用端和 AI 能力分开组织。 |
| [shuldeshoff/legalflow-ai](https://github.com/shuldeshoff/legalflow-ai) | 法律助手平台应把前端、后端、Docker、测试和文档独立管理。 |
| [hillaryke/contract-qa-high-precision-rag](https://github.com/hillaryke/contract-qa-high-precision-rag) | 合同问答流程应单独设计，并预留 RAG 质量评估。 |
| [superwise-ai/Legal_Document_Analyzer_AI](https://github.com/superwise-ai/Legal_Document_Analyzer_AI) | 法律文档分析要重视条款提取、风险识别、缺失条款、隐私和安全护栏。 |
| [jashankish/local-legal-ai](https://github.com/jashankish/local-legal-ai) | 后续可扩展私有化部署、审计日志、访问控制和自动化入库，但 MVP 不提前实现。 |
| [microsoft/langchainjs-for-beginners](https://github.com/microsoft/langchainjs-for-beginners/blob/main/08-agentic-rag-systems/README.md) | Agentic RAG 的核心是让 Agent 判断何时检索、何时直接回答，并正确处理上下文和引用。 |

## 3. 总体原则

### 3.1 不跳步原则

每一阶段必须完成本阶段验收后，才能进入下一阶段。

例如：

- 没有 `.env.example`，不接模型 API。
- 没有数据库模型，不写保存审查结果。
- 没有合同解析，不写合同审查。
- 没有 Schema，不让大模型输出直接入库。
- 没有 RAG 入库，不要求回答引用法律知识库。
- 没有基础 Web 演示端，不直接做小程序。

### 3.2 小步提交原则

每个阶段完成后都要：

- 更新相关文档。
- 更新 `tasks.md`。
- 本地运行可执行检查。
- 提交 Git。
- 尽量推送 GitHub。

提交信息必须符合项目规范，例如：

```text
docs: 编写制作流程

change-id: 20260621-build-process

- 新增严谨制作流程文档。
- 明确每个阶段的前置条件、产出和验收标准。
```

### 3.3 先学习后扩展原则

本项目主要目标是学习，所以第一版应优先理解核心机制：

- 文件上传怎么进入系统。
- 合同文本怎么解析和保存。
- RAG 怎么切片、检索、引用。
- Agent 怎么判断任务类型和选择技能。
- 大模型输出怎么被 Schema 校验。
- 法律回答怎么降低误导风险。

LangGraph、手机端、小程序、管理后台、多模型、复杂 RAG 都要预留，但不抢 MVP 主线。

## 4. 阶段 0：仓库与规划基线

当前状态：已完成。

目标：

- 让项目可以被 Git 管理，并有基础规划文档。

已完成产出：

- `README.md`
- `.gitignore`
- `docs/mvp-spec.md`
- `docs/agent-project-plan.md`
- `docs/reference-projects.md`
- `docs/architecture.md`
- `tasks.md`
- GitHub 仓库：`https://github.com/mulinguo97-oss/Legal-Agent-1.0.git`

验收标准：

- 本地 Git 仓库存在。
- GitHub 远程仓库可推送。
- 规划文档能说明项目定位、Agent 项目结构、参考项目和总体架构。

下一步入口：

- 编写 Agent 运行流程和阶段验收清单。

## 5. 阶段 1：流程文档与验收标准

目标：

- 在写代码前，把执行顺序、Agent 流程和验收标准写清。

要做：

1. 编写 `docs/build-process.md`。
2. 编写 `docs/agent-flow.md`。
3. 编写 `docs/acceptance.md`。
4. 更新 `tasks.md`。

不做：

- 不初始化 `apps/api`。
- 不初始化 `apps/web`。
- 不写 Agent 代码。
- 不接入数据库和模型 API。

产出：

- 清楚的制作流程。
- 清楚的 Agent 运行步骤。
- 清楚的每阶段验收标准。

验收标准：

- 能回答“下一步为什么是这个，而不是直接写 Agent”。
- 能回答“每个阶段完成到什么程度才算结束”。
- 能回答“哪些能力是未来扩展，哪些是 MVP 必须做”。

不能跳过的理由：

- 如果没有流程和验收，后续容易把脚手架、RAG、Agent、前端、小程序混在一起做，导致项目失控。

## 6. 阶段 2：工程脚手架

前置条件：

- 阶段 1 完成。
- `docs/architecture.md`、`docs/build-process.md`、`docs/agent-flow.md`、`docs/acceptance.md` 已存在。

目标：

- 建立一个空但规范的 TypeScript monorepo。

要做：

1. 初始化根 `package.json`。
2. 选择包管理器，建议 `pnpm`。
3. 配置 workspace。
4. 创建 `apps/api`。
5. 创建 `apps/web`。
6. 创建 `packages/agent-core`。
7. 创建 `packages/legal-skills`。
8. 创建 `packages/legal-rag`。
9. 创建 `packages/legal-prompts`。
10. 创建 `packages/legal-schemas`。
11. 创建 `packages/model-client`。
12. 创建 `packages/shared`。
13. 配置 TypeScript 基础配置。
14. 配置格式化和 lint。

不做：

- 不写业务接口。
- 不接数据库。
- 不调用大模型。
- 不写合同审查逻辑。

产出：

```text
apps/
packages/
package.json
pnpm-workspace.yaml
tsconfig.base.json
```

验收标准：

- 能安装依赖。
- 能运行基础 lint 或 typecheck。
- 每个包有最小 `package.json` 或入口占位。
- 目录结构与 `docs/architecture.md` 一致。

不能跳过的理由：

- 没有统一脚手架，后续模块会散落，Agent、RAG、Prompt、Schema 很容易混在一起。

## 7. 阶段 3：基础设施与环境变量

前置条件：

- 阶段 2 完成。
- Monorepo 可安装、可检查。

目标：

- 建立本地开发所需的数据库、缓存、对象存储和向量库。

要做：

1. 创建 `infra/docker-compose.yml`。
2. 添加 PostgreSQL。
3. 添加 Redis。
4. 添加 MinIO。
5. 添加 Qdrant。
6. 创建 `.env.example`。
7. 在 `apps/api` 中添加配置读取模块。
8. 添加 `/health` 健康检查接口。

不做：

- 不建复杂业务表。
- 不写合同上传。
- 不接大模型。

产出：

```text
infra/docker-compose.yml
.env.example
apps/api/src/modules/health
```

验收标准：

- `docker compose up` 能启动基础服务。
- `/health` 能返回服务状态。
- `.env.example` 包含必要变量且不包含真实密钥。
- `.env` 被 `.gitignore` 忽略。

不能跳过的理由：

- 没有基础设施，后续文件、任务、向量和业务数据都没有稳定落点。

## 8. 阶段 4：数据库模型与迁移

前置条件：

- 阶段 3 完成。
- PostgreSQL 可连接。

目标：

- 定义最小业务数据模型，先让系统有“记忆”和“状态”。

要做：

1. 接入 Prisma。
2. 定义 `users`。
3. 定义 `contracts`。
4. 定义 `contract_texts`。
5. 定义 `review_tasks`。
6. 定义 `review_results`。
7. 定义 `chat_sessions`。
8. 定义 `chat_messages`。
9. 定义 `knowledge_documents`。
10. 定义 `knowledge_chunks`。
11. 定义 `reports`。
12. 执行第一版 migration。

不做：

- 不过早设计企业级权限。
- 不做复杂多租户。
- 不做计费系统。

产出：

```text
apps/api/prisma/schema.prisma
apps/api/prisma/migrations/
```

验收标准：

- Prisma migration 能成功执行。
- API 可以连接数据库。
- 至少能创建一条测试合同记录。

不能跳过的理由：

- 没有数据模型，后续合同、任务、问答、审查结果无法追踪。

## 9. 阶段 5：文件上传与合同解析

前置条件：

- 阶段 4 完成。
- MinIO 和 PostgreSQL 可用。

目标：

- 先让合同能进入系统，并解析出正文。

要做：

1. 实现合同上传接口。
2. 将原文件保存到 MinIO。
3. 创建 `contracts` 记录。
4. 支持 TXT 解析。
5. 支持 DOCX 解析。
6. 支持可复制文本 PDF 解析。
7. 保存 `contract_texts`。
8. 添加文件类型、大小、解析失败状态。

不做：

- 不做 OCR。
- 不做合同审查。
- 不接大模型。
- 不做复杂前端。

产出：

- 合同上传 API。
- 合同解析服务。
- 原文件和正文保存能力。

验收标准：

- 上传 TXT 后能保存并读取正文。
- 上传 DOCX 后能解析正文。
- 上传可复制文本 PDF 后能解析正文。
- 解析失败时有明确错误状态。

不能跳过的理由：

- 合同审查和合同问答的输入是合同正文，没有正文就不能审查。

## 10. 阶段 6：结构化 Schema、Prompt 与模型客户端

前置条件：

- 阶段 5 完成。
- 系统能拿到合同正文。

目标：

- 在接入 Agent 前，先定义模型输入输出边界。

要做：

1. 在 `legal-schemas` 定义风险点 Schema。
2. 定义问答结果 Schema。
3. 定义引用依据 Schema。
4. 定义律师咨询提示 Schema。
5. 在 `legal-prompts` 定义基础系统 Prompt。
6. 定义合同审查 Prompt。
7. 定义合同问答 Prompt。
8. 定义创业法律问答 Prompt。
9. 在 `model-client` 封装 `generateText`、`generateJson`、`embedText`。
10. 支持模型配置、超时、错误处理。

不做：

- 不写完整 Agent。
- 不做 RAG 入库。
- 不让未校验的模型输出直接入库。

产出：

```text
packages/legal-schemas
packages/legal-prompts
packages/model-client
```

验收标准：

- 模型返回 JSON 能被 Schema 校验。
- 校验失败能得到明确错误。
- Prompt 明确包含法律免责声明、引用要求和不确定性处理。

不能跳过的理由：

- 如果没有 Schema 和 Prompt 边界，模型输出会不可控，后续入库和展示都会混乱。

## 11. 阶段 7：轻量 Agent Core

前置条件：

- 阶段 6 完成。
- Skill 所需的 Prompt、Schema、模型客户端已有最小版本。

目标：

- 实现轻量自研 Agent 执行框架，先理解 Agent 的核心机制。

要做：

1. 定义 `AgentContext`。
2. 定义 `AgentIntent`。
3. 定义 `AgentPlan`。
4. 实现意图识别。
5. 实现 Skill Router。
6. 实现执行步骤记录。
7. 实现错误处理。
8. 预留 LangGraph 适配器接口。

不做：

- 不引入 LangGraph。
- 不引入 CrewAI。
- 不引入 AutoGen。
- 不做复杂多 Agent。

产出：

```text
packages/agent-core
```

验收标准：

- 输入合同审查请求，能路由到 `contract-review`。
- 输入合同相关问题，能路由到 `contract-qa`。
- 输入创业经营问题，能路由到 `startup-legal-qa`。
- 执行过程能记录步骤和错误。

不能跳过的理由：

- 没有 Agent Core，合同审查、合同问答、创业问答会各写各的流程，后续难以扩展。

## 12. 阶段 8：合同审查最小闭环

前置条件：

- 阶段 7 完成。
- 合同正文可读取。
- 模型客户端和 Schema 可用。

目标：

- 先实现不依赖完整 RAG 的合同审查最小闭环。

要做：

1. 实现 `legal-skills/contract-review`。
2. 从合同正文中构建审查上下文。
3. 调用模型生成结构化风险点。
4. 校验风险点 Schema。
5. 保存 `review_tasks` 和 `review_results`。
6. 返回审查结果。

不做：

- 不要求引用法规知识库。
- 不做报告导出。
- 不做复杂规则引擎。

产出：

- 合同审查 Skill。
- 审查任务 API。
- 审查结果保存和查询能力。

验收标准：

- 样例合同能输出至少 5 个风险点。
- 每个风险点包含风险等级、原文片段、问题说明、修改建议。
- 高风险点能提示咨询律师。

不能跳过的理由：

- 这是 MVP 的第一条核心业务链路，必须先跑通再增强 RAG。

## 13. 阶段 9：Web 演示端最小闭环

前置条件：

- 阶段 8 完成。
- 后端已有上传、解析、审查和结果查询接口。

目标：

- 让用户可以通过页面完成合同审查流程。

要做：

1. 初始化 `apps/web` 页面结构。
2. 实现合同上传页面。
3. 实现审查任务状态展示。
4. 实现风险结果列表。
5. 实现风险详情展示。
6. 展示免责声明。

不做：

- 不做小程序。
- 不做手机端 App。
- 不做复杂后台管理。
- 不做支付和登录。

产出：

- 可运行 Web 演示端。

验收标准：

- 用户可以上传合同。
- 页面可以看到审查结果。
- 风险点展示清楚。
- 页面展示免责声明。

不能跳过的理由：

- 没有演示端，就无法真实验证用户流程是否顺畅。

## 14. 阶段 10：合同问答最小闭环

前置条件：

- 阶段 9 完成。
- 合同审查和合同正文查询可用。

目标：

- 支持用户围绕已上传合同继续追问。

要做：

1. 实现 `legal-skills/contract-qa`。
2. 创建合同问答会话。
3. 保存用户消息。
4. 基于合同正文检索或截取相关上下文。
5. 调用模型生成回答。
6. 保存助手消息。
7. 前端展示问答消息。

不做：

- 不做复杂多轮记忆压缩。
- 不做 WebSocket。
- 不做多合同对比问答。

产出：

- 合同问答 API。
- 合同问答页面。

验收标准：

- 用户可以围绕合同追问。
- 回答能引用合同原文片段。
- 不确定问题能提示需要补充信息。

不能跳过的理由：

- 合同问答是 MVP 的第二条核心价值链路。

## 15. 阶段 11：法律知识库 RAG 最小闭环

前置条件：

- 阶段 10 完成。
- Qdrant 可用。
- `legal_corpus/` 已有初步资料。

目标：

- 把法律资料变成可检索、可引用的知识库。

要做：

1. 解析 `legal_corpus/` 的文档。
2. 整理文档元数据。
3. 切片。
4. 生成 embedding。
5. 写入 Qdrant。
6. 写入 `knowledge_documents` 和 `knowledge_chunks`。
7. 实现相似内容检索。
8. 将检索结果接入合同审查和合同问答。

不做：

- 不做全量法规库。
- 不做 GraphRAG。
- 不做 BM25 + RRF。
- 不做 RAGAS 自动评估。

产出：

- 知识库入库脚本或任务。
- 知识库检索服务。
- 审查和问答中的法律依据引用。

验收标准：

- 给定一个合同付款风险问题，能检索到合同通用或民法典相关片段。
- 回答中能展示法律资料来源。
- 检索不到时能说明依据不足。

不能跳过的理由：

- 没有 RAG，法律回答只依赖模型记忆，风险太高。

## 16. 阶段 12：创业法律决策问答

前置条件：

- 阶段 11 完成。
- 法律知识库检索可用。
- Agent Core 可路由到 `startup-legal-qa`。

目标：

- 支持用户不上传合同，直接描述经营问题并获得风险提示。

要做：

1. 实现 `legal-skills/startup-legal-qa`。
2. 设计场景识别：合伙、劳动、欠款、知识产权、公司基础事务。
3. 检索相关法律知识。
4. 输出风险清单、建议动作、需准备材料、律师咨询提示。
5. 前端增加创业法律问答入口。

不做：

- 不做正式法律意见。
- 不做复杂案件分析。
- 不做诉讼预测。

产出：

- 创业法律问答 API。
- 创业法律问答页面。

验收标准：

- 用户能输入经营问题。
- 回答能区分可自行处理和建议找律师处理。
- 回答能列出下一步材料。

不能跳过的理由：

- 这是 MVP 的第三条核心价值链路，但必须建立在 Agent、Prompt、Schema、RAG 基础之上。

## 17. 阶段 13：报告与验收

前置条件：

- 合同审查、合同问答、创业法律问答都已跑通。

目标：

- 形成可演示、可验收的 MVP。

要做：

1. 实现审查结果详情页。
2. 实现简版审查报告。
3. 添加统一免责声明。
4. 准备测试合同样例。
5. 验证合同审查流程。
6. 验证合同问答流程。
7. 验证创业法律问答流程。
8. 补充基础测试。

不做：

- 不做正式法律意见书。
- 不做 Word 红线。
- 不做电子签章。

产出：

- MVP 演示版本。
- 验收记录。
- 测试样例。

验收标准：

- MVP 满足 `docs/mvp-spec.md` 中的核心验收标准。
- 所有核心流程有测试或人工验收记录。
- 风险提示和免责声明完整。

不能跳过的理由：

- 没有验收，项目只是一堆功能，不能证明学习目标和 MVP 目标达成。

## 18. 阶段 14：未来扩展预留

这些内容不是第一版 MVP，但架构要预留位置。

### 18.1 手机端和小程序

预留位置：

```text
apps/
├─ web/
├─ mobile/          # React Native / Expo，可后续添加
└─ miniprogram/     # Taro 微信小程序，可后续添加
```

进入条件：

- Web 演示端已经跑通。
- API 边界稳定。
- 核心流程不再频繁变化。

### 18.2 LangGraph

预留位置：

```text
packages/agent-core/
├─ runtime/
├─ planners/
├─ routers/
└─ adapters/
   └─ langgraph/
```

进入条件：

- 当前轻量 Agent Core 的意图识别、路由、执行日志已经清楚。
- 出现多步骤循环、自我纠错、多工具编排等真实需求。

### 18.3 高级 RAG

预留位置：

```text
packages/legal-rag/
├─ retrievers/
├─ rankers/
├─ evaluators/
└─ adapters/
```

后续能力：

- BM25。
- RRF。
- 多查询。
- 多跳检索。
- RAGAS 评估。
- GraphRAG。

进入条件：

- 第一版向量检索效果不够。
- 已有真实样例问题和标准答案。

### 18.4 管理后台

预留位置：

```text
apps/admin/
```

后续能力：

- 审查任务管理。
- 知识库管理。
- 用户反馈管理。
- 模型调用日志。
- 风险规则管理。

进入条件：

- Web 用户端已有稳定流程。
- 知识库和任务数据需要可视化维护。

### 18.5 私有化部署和审计

后续能力：

- 本地大模型。
- 审计日志。
- 权限角色。
- 合同脱敏。
- 私有对象存储。

进入条件：

- MVP 跑通。
- 有真实数据安全需求。

## 19. 当前下一步

当前已经完成：

- 阶段 0：仓库与规划基线。
- 阶段 1 的一部分：`docs/build-process.md`。

严格下一步：

1. 编写 `docs/agent-flow.md`。
2. 编写 `docs/acceptance.md`。
3. 更新 `tasks.md`，把流程拆成阶段任务。
4. 再进入阶段 2：工程脚手架。

现阶段不应该直接做：

- 不直接初始化 NestJS。
- 不直接写合同审查接口。
- 不直接接大模型。
- 不直接做小程序。

