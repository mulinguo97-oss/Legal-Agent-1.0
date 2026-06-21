# GitHub 相似开源项目调研

## 1. 调研目标

本文件用于记录“法律小助手”在设计总体架构前可参考的 GitHub 开源项目。

本次调研不是为了复制代码，而是为了学习相似项目在以下方面的设计：

- 法律 AI 助手如何组织 RAG、Agent、Prompt、Schema 和工具调用。
- 合同审查、合同问答、文档分析类产品如何设计用户流程。
- 前端、后端、知识库、向量检索和模型调用如何分层。
- 哪些能力适合第一版 MVP，哪些能力应暂缓。

调研日期：2026-06-21。

## 2. 筛选维度

优先选择与本项目相似度高的项目：

- 法律 AI 助手、法律问答、合同审查、合同问答、法律 RAG。
- 包含真实目录结构，而不是只有单个 Notebook。
- 有可学习的工程分层、Prompt 管理、检索策略、文档处理或前后端结构。
- 与当前推荐技术栈有部分重合，例如 TypeScript、React、NestJS、FastAPI、Docker、向量数据库、LangChain 等。

注意：部分项目没有明确开源许可证。没有许可证的项目只能学习架构思想和产品思路，不能复制代码。

## 3. 重点参考项目

### 3.1 Fan-Luo/Legal-RAG

仓库地址：[https://github.com/Fan-Luo/Legal-RAG](https://github.com/Fan-Luo/Legal-RAG)

项目定位：面向法律场景的 RAG 系统，包含法律检索、任务路由、LLM 调用、Prompt、Schema、API、索引和文档处理等模块。

主要技术与结构：

- 主要语言：Python。
- 包含 `Dockerfile`、`docker-compose.yml`、`pyproject.toml`、`requirements.txt`。
- 核心目录 `legalrag/` 下包含：
  - `agents/`
  - `api/`
  - `index/`
  - `ingest/`
  - `llm/`
  - `pdf/`
  - `pipeline/`
  - `prompts/`
  - `retrieval/`
  - `routing/`
  - `schemas.py`
  - `services/`
  - `utils/`
- `docs/` 中包含架构图相关文件。

可借鉴点：

- 法律 RAG 不应只写成一个“大函数”，而应拆成入库、索引、检索、路由、生成、Schema 校验等模块。
- `routing/`、`retrieval/`、`pipeline/`、`prompts/`、`schemas` 的拆分方式值得参考。
- 可以学习它把法律检索和任务路由分离的思路。
- 文档里保留架构图，有助于后续维护和学习。

不适合照搬的点：

- 它是 Python 项目，而本项目第一版倾向 TypeScript / NestJS。
- 其中图检索、复杂路由和高级 RAG 能力不适合第一版一次性全部实现。
- 我们的第一版重点是合同审查和创业法律问答，不需要一开始做成通用法律检索平台。

对本项目的启发：

- `packages/legal-rag` 可以参考它的 `ingest`、`retrieval`、`pipeline` 分层。
- `packages/agent-core` 可以参考它的 `routing` 思路，但先做轻量版。
- `packages/legal-schemas` 应独立出来，避免结构化输出散落在业务代码中。

### 3.2 hasnaintypes/lawbotics

仓库地址：[https://github.com/hasnaintypes/lawbotics](https://github.com/hasnaintypes/lawbotics)

项目定位：AI 驱动的法律合同分析平台，侧重合同文档审查、条款提取、机器学习和现代 Web 技术。

主要技术与结构：

- 主要语言：TypeScript。
- 采用 monorepo 思路。
- 顶层目录包含：
  - `apps/`
  - `ai-model/`
  - `docs/`
- `apps/` 下包含：
  - `admin/`
  - `web/`
- 项目主题包含 Next.js、TypeScript、Tailwind、shadcn、LangChain、CUAD dataset、contract analysis 等。

可借鉴点：

- 和本项目一样偏合同分析，产品方向接近。
- `apps/admin`、`apps/web` 的组织方式适合学习前端应用拆分。
- 合同条款提取、合同分析、风险呈现等产品思路值得参考。
- TypeScript monorepo 对我们后续 `apps/` + `packages/` 的设计有参考意义。

不适合照搬的点：

- 它包含机器学习、数据集、认证、复杂前端栈等内容，第一版不应全部引入。
- 我们当前更需要先跑通合同上传、解析、审查、问答闭环。
- 如果直接照搬前端和 AI 模型结构，容易让 MVP 范围失控。

对本项目的启发：

- 本项目可以保留 `apps/web`，后续再考虑 `apps/admin`。
- 合同审查结果页可以借鉴“条款提取 + 风险说明 + 建议动作”的呈现方式。
- 暂不引入模型训练或微调，先使用大模型 API + RAG。

### 3.3 shuldeshoff/legalflow-ai

仓库地址：[https://github.com/shuldeshoff/legalflow-ai](https://github.com/shuldeshoff/legalflow-ai)

项目定位：AI 法律助手平台，包含 LLM 聊天、文档分析和合同生成，使用 FastAPI + React + OpenAI。

主要技术与结构：

- 后端：FastAPI。
- 前端：React。
- 顶层目录包含：
  - `backend/`
  - `frontend/`
  - `docs/`
  - `docker-compose.yml`
- `backend/` 中包含：
  - `app/`
  - `tests/`
  - `alembic/`
  - `Dockerfile`
  - `requirements.txt`
  - `requirements-ai.txt`
- `frontend/` 中包含：
  - `src/`
  - `package.json`
  - `vite.config.ts`
  - `tailwind.config.js`

可借鉴点：

- 前后端分离清晰，适合学习完整平台结构。
- 后端同时保留应用代码、数据库迁移、测试和 Docker 配置，工程完整度较好。
- 适合参考“文档分析 + 聊天 + 合同生成”这类法律助手平台的模块边界。

不适合照搬的点：

- 它使用 FastAPI，而本项目推荐 NestJS。
- 合同生成不是本项目第一版重点，暂时不做。
- 如果第一版同时做聊天、审查、生成、管理后台，范围会过大。

对本项目的启发：

- `apps/api` 应从一开始就保留 `modules`、`services`、`controllers`、`schemas` 或 DTO 的边界。
- `infra/docker-compose.yml` 应尽早建立，避免后续数据库、Redis、对象存储、向量库零散配置。
- 测试目录应跟随业务模块逐步补齐。

### 3.4 hillaryke/contract-qa-high-precision-rag

仓库地址：[https://github.com/hillaryke/contract-qa-high-precision-rag](https://github.com/hillaryke/contract-qa-high-precision-rag)

项目定位：面向合同问答的高精度 RAG 系统，支持 React 前端、FastAPI 后端、RAG Pipeline、AutoGen Agents、WebSocket 和 RAGAS 评估。

主要技术与结构：

- 后端：FastAPI。
- 前端：React。
- 包含：
  - `backend/`
  - `frontend/`
  - `src/`
  - `notebooks/`
  - `tests/`
  - `docker-compose.yaml`
  - `Makefile`
  - `requirements.txt`

可借鉴点：

- 与本项目“合同问答”能力高度相似。
- 可以学习用户上传合同后，围绕合同继续问答的链路。
- RAGAS 评估思路适合后续评估回答质量。
- WebSocket 可以作为后续流式回答或任务状态推送的参考。

不适合照搬的点：

- AutoGen Agents 和 WebSocket 对第一版来说偏复杂。
- 第一版可以先使用普通 HTTP 接口和异步任务状态查询。
- RAGAS 可以作为后续质量评估阶段，不必在脚手架阶段引入。

对本项目的启发：

- 合同问答应独立成 `legal-skills/contract-qa`，不要混在合同审查逻辑里。
- 每个回答都应尽量引用合同片段，而不是只给泛泛法律建议。
- 后续可以单独设计 `tests/evaluation` 或 `evals/` 来做 RAG 质量评估。

### 3.5 sougaaat/RAG-based-Legal-Assistant

仓库地址：[https://github.com/sougaaat/RAG-based-Legal-Assistant](https://github.com/sougaaat/RAG-based-Legal-Assistant)

项目定位：基于 LangChain 的上下文感知法律聊天机器人，结合 BM25 关键词检索、语义检索、多跳检索、多查询检索和 RRF 融合。

主要技术与结构：

- 主要语言：Python。
- 顶层目录包含：
  - `modules/`
  - `prompts/`
  - `data/`
  - `outputs/`
  - `RAGAS-dataset/`
  - `app.py`
  - `ragas_eval.py`
  - `ragas_score.py`
  - `requirements.txt`
- `modules/` 中包含：
  - `bm25_retriever.py`
  - `semantic_retriever.py`
  - `multi_hop_retriever.py`
  - `multi_query_retriever.py`
  - `rrf_score.py`
  - `conversation_history.py`
  - `chatbot_response.py`
  - `decide_query_complexity.py`

可借鉴点：

- BM25 + 语义检索 + RRF 融合是很实用的 RAG 检索策略。
- 对复杂问题进行 query complexity 判断，再决定是否多跳检索，思路值得学习。
- 单独管理 conversation history，有助于法律问答保持上下文。
- RAGAS 评估文件说明它关注回答质量评估。

不适合照搬的点：

- 许可证不明确，不能复制代码。
- 第一版不宜同时实现 BM25、多跳、多查询、RRF、RAGAS 全套能力。
- 对中文法律文本，还需要额外考虑分词、法规条文结构和引用格式。

对本项目的启发：

- 第一版 RAG 可以先做“向量检索 + 元数据过滤”。
- 第二阶段再补 BM25、RRF、多查询。
- `packages/legal-rag` 内部可以预留 `retrievers/`、`rankers/`、`evaluations/` 目录。

### 3.6 mrdaliselmi/9anounGPT

仓库地址：[https://github.com/mrdaliselmi/9anounGPT](https://github.com/mrdaliselmi/9anounGPT)

项目定位：面向突尼斯法律的 AI 律师助手平台，包含 RAG 聊天、OAuth 登录和社区论坛。

主要技术与结构：

- 项目主题包含 Flask、LLMs、microservices、NestJS、RAG、React。
- 顶层目录包含：
  - `chatbot-backend`
  - `forum-backend`
  - `frontend`
  - `docker-compose.prod.yml`
  - `.gitmodules`

可借鉴点：

- 它说明法律 AI 助手后续可能扩展为多服务架构。
- React + NestJS + RAG 与本项目推荐方向有部分重合。
- 可以参考它把聊天后端、论坛后端、前端分开的边界。

不适合照搬的点：

- 论坛、OAuth、多服务生产部署都不是本项目 MVP 必需。
- 第一版做微服务会增加部署、调试和数据一致性成本。
- 本项目应先做模块化单体，后续再拆服务。

对本项目的启发：

- `apps/api` 先作为模块化单体，不急着拆成多个后端服务。
- 用户、社区、论坛、OAuth 登录可以放到后续版本。
- Docker Compose 可以先服务本地开发，不急于生产化。

### 3.7 lawglance/lawglance

仓库地址：[https://github.com/lawglance/lawglance](https://github.com/lawglance/lawglance)

项目定位：免费开源的 RAG 法律 AI 助手。

主要技术与结构：

- 项目主题包含 AI、ChromaDB、CrewAI、Django、LangChain、legal AI。
- 顶层目录包含：
  - `src/`
  - `config/`
  - `docs/`
  - `examples/`
  - `prompts.py`
  - `chains.py`
  - `cache.py`
  - `app.py`
  - `lawglance_main.py`
  - `requirements.txt`
  - `pyproject.toml`

可借鉴点：

- 适合学习一个较简单的法律 RAG 助手如何把 Prompt、Chain、缓存和应用入口拆开。
- 示例和文档目录有利于学习与演示。
- ChromaDB 和 LangChain 的组合可以作为理解 RAG 快速原型的参考。

不适合照搬的点：

- 顶层包含 `.env`，这对我们的仓库管理是反面提醒，密钥和环境变量不能提交。
- Python + LangChain 快速原型结构，不完全适合我们的 TypeScript / NestJS 目标。
- 第一版不需要引入 CrewAI。

对本项目的启发：

- `legal-prompts` 应独立管理。
- 模型调用链路、缓存、Prompt 不应散落在控制器里。
- `.env.example` 可以提交，真实 `.env` 必须忽略。

### 3.8 arulkumarann/legalRAG

仓库地址：[https://github.com/arulkumarann/legalRAG](https://github.com/arulkumarann/legalRAG)

项目定位：基于 FastAPI 的法律文档分析 RAG 系统，使用 Pinecone、LangChain 和 Llama。

主要技术与结构：

- 主要语言：Python。
- 主要技术：FastAPI、LangChain、Pinecone、Llama。
- 顶层目录包含：
  - `src/`
  - `data/`
  - `app.py`
  - `document_uploader.py`
  - `requirements.txt`
  - `processed_state.txt`

可借鉴点：

- 文档上传、处理状态、RAG 问答的最小闭环值得学习。
- 可以参考它把文档上传处理和问答入口拆开的思路。

不适合照搬的点：

- 它使用 Pinecone，而本项目计划使用 Qdrant。
- `processed_state.txt` 这类本地状态文件不适合作为正式状态管理方式。
- 对本项目来说，状态应进入数据库或任务队列。

对本项目的启发：

- 第一版合同解析状态要入库，不能只保存在本地文件。
- `contract_texts`、`review_tasks`、`knowledge_chunks` 等表应支撑后续追踪。

### 3.9 AbdulRehmanMehar/AI-Legal-Document-Analyzer-POC

仓库地址：[https://github.com/AbdulRehmanMehar/AI-Legal-Document-Analyzer-POC](https://github.com/AbdulRehmanMehar/AI-Legal-Document-Analyzer-POC)

项目定位：AI 法律文档分析 POC，支持上传 PDF / DOCX 合同，进行摘要、关键条款提取和智能分析。

主要技术与结构：

- 主要语言：TypeScript。
- 主要技术：Next.js、OpenAI。
- 顶层目录包含：
  - `app/`
  - `lib/`
  - `public/`
  - `package.json`
  - `next.config.ts`
  - `tsconfig.json`
  - `.env.example`

可借鉴点：

- 与“上传法律文档后做摘要和分析”的用户流程接近。
- Next.js 单体项目适合学习快速 POC 的交互方式。
- `lib/` 中集中放模型调用、文档处理等能力的思路可以参考。

不适合照搬的点：

- 单体 Next.js POC 对长期后端、队列、对象存储和 RAG 扩展不够清晰。
- 我们的项目后续需要任务队列、数据库、对象存储和知识库，不能只做单应用。

对本项目的启发：

- 第一版演示端要简单直接，优先让用户完成“上传合同 → 查看风险 → 继续追问”。
- 可以先做 Web 演示端，再考虑小程序端。

## 4. 横向对比

| 项目 | 更像本项目的哪一部分 | 主要学习价值 | 第一版是否直接采用 |
|---|---|---|---|
| `Fan-Luo/Legal-RAG` | 法律 RAG 和 Agent 底层 | RAG 模块拆分、路由、Schema、文档架构 | 部分采用 |
| `hasnaintypes/lawbotics` | 合同分析产品 | TypeScript monorepo、合同分析体验 | 部分采用 |
| `shuldeshoff/legalflow-ai` | 完整法律助手平台 | 前后端分离、Docker、测试、服务分层 | 部分采用 |
| `hillaryke/contract-qa-high-precision-rag` | 合同问答 | 合同 QA 流程、RAG 评估 | 后续采用 |
| `sougaaat/RAG-based-Legal-Assistant` | 法律问答检索策略 | BM25、语义检索、多跳、多查询、RRF | 后续采用 |
| `mrdaliselmi/9anounGPT` | 法律助手平台扩展版 | NestJS、React、多服务边界 | 暂不采用 |
| `lawglance/lawglance` | 法律 RAG 原型 | Prompt、Chain、缓存、示例 | 只借鉴思想 |
| `arulkumarann/legalRAG` | 最小 RAG API | 文档上传和处理闭环 | 部分采用 |
| `AI-Legal-Document-Analyzer-POC` | 文档分析 POC | 简洁上传分析体验 | 只借鉴交互 |

## 5. 对法律小助手的架构启发

综合以上项目，本项目不应照搬任何一个仓库，而应组合吸收：

- 总体结构采用 `apps/` + `packages/` + `infra/` 的 monorepo 方式。
- `apps/api` 采用模块化后端，先做模块化单体，暂不拆微服务。
- `apps/web` 先做 Web 演示端，用于学习和验证核心流程。
- `packages/agent-core` 负责规划、路由、执行上下文、状态和日志。
- `packages/legal-skills` 负责合同审查、合同问答、创业法律问答。
- `packages/legal-rag` 负责文档解析、切片、向量化、检索和引用依据。
- `packages/legal-prompts` 负责系统 Prompt、任务 Prompt 和输出模板。
- `packages/legal-schemas` 负责风险点、报告、问答、知识库元数据等结构化格式。
- `packages/model-client` 负责封装大模型供应商。
- `infra/` 负责 PostgreSQL、Redis、MinIO、Qdrant 等基础服务。

## 6. 第一版应暂缓的能力

这些能力虽然在参考项目中出现，但不适合第一版立即实现：

- 微服务拆分。
- OAuth、论坛、社区。
- 模型微调和训练数据集管线。
- GraphRAG。
- CrewAI / AutoGen 等复杂多 Agent 框架。
- WebSocket 流式任务推送。
- RAGAS 自动评估体系。
- BM25 + RRF + 多跳 + 多查询全量检索策略。
- 合同自动生成。
- Word 红线修订。
- OCR 扫描件识别。

## 7. 建议学习顺序

建议按以下顺序学习和落地：

1. 学 `shuldeshoff/legalflow-ai` 和 `hasnaintypes/lawbotics`：理解完整法律助手平台和合同分析产品结构。
2. 学 `Fan-Luo/Legal-RAG`：理解法律 RAG、路由、检索、Prompt、Schema 的模块拆分。
3. 学 `hillaryke/contract-qa-high-precision-rag`：理解合同问答流程和后续 RAG 评估。
4. 学 `sougaaat/RAG-based-Legal-Assistant`：理解更高级的检索策略，但先不实现。
5. 最后再参考 `9anounGPT`：等 MVP 跑通后，再考虑认证、多服务和社区能力。

## 8. 下一步

下一步可以基于本调研文档编写 `docs/architecture.md`。

`docs/architecture.md` 应重点回答：

- 法律小助手整体采用什么工程结构。
- 前端、后端、Agent、RAG、数据库、队列、对象存储和向量库如何协作。
- 每个模块负责什么，不负责什么。
- 第一版 MVP 的核心数据流是什么。
- 哪些参考项目的设计被吸收进来，哪些被明确排除。

