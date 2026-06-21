# 法律小助手

法律小助手是一款面向微小企业、个体经营者、自由职业者、初创公司和普通个人的 AI 法律风险辅助工具。

第一版目标是验证三个核心能力：

- 合同审查：上传合同后输出结构化风险点。
- 合同问答：围绕已上传合同继续追问，并引用合同原文片段。
- 创业法律决策问答：围绕常见经营和创业场景输出法律风险提示、建议动作和律师咨询提示。

本项目不替代律师，不出具正式法律意见书。AI 输出仅作为初步风险提示和决策参考，复杂、高金额、高争议事项应咨询专业律师。

## 当前状态

当前仓库处于 MVP 规划阶段，已有内容：

- `docs/mvp-spec.md`：MVP 规格说明。
- `docs/agent-project-plan.md`：Agent 项目结构与实施计划。
- `legal_corpus/`：按业务场景整理的法律知识库原始资料。
- `tasks.md`：任务清单。

## 推荐技术方向

```text
前端：Taro + React + TypeScript
后端：NestJS + TypeScript
数据库：PostgreSQL
ORM：Prisma
对象存储：MinIO
缓存与任务队列：Redis + BullMQ
向量数据库：Qdrant
AI 能力：大模型 API + RAG
部署：Docker Compose
```

## 下一步

优先补充 `docs/architecture.md`，明确整体架构、模块边界、核心数据流和第一版不做的内容。
