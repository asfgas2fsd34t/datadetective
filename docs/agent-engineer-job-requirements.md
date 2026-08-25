# Agent 开发工程师招聘技术要求

## 1. 调研说明

本文根据 2026-08-25 在 BOSS 直聘读取的 4 份 Agent/AI 应用相关岗位整理：

- [Agent 研发工程师（中后台方向）](https://www.zhipin.com/job_detail/c8d0fb51c69fbde30nJ80960ElJZ.html)
- [Agent 开发工程师](https://www.zhipin.com/job_detail/22d51cd6b220623a0nBy0924FlFV.html)
- [Agent 后端高级工程师](https://www.zhipin.com/job_detail/bca46a277b64c7ce0nd82d60GFNS.html)
- [AI 后端工程师](https://www.zhipin.com/job_detail/86ba57b383d506e60nF62Nu6F1JU.html)

样本数量有限，频率仅用于判断方向，不代表完整市场统计。本文重点提取这些岗位的共同技术要求，并用于反向设计求职项目。

## 2. 高频能力概览

| 能力 | 样本出现情况 | 优先级 |
|---|---:|---:|
| 业务 Agent 设计与落地 | 4/4 | 必须 |
| Tool Calling、工作流和模型接入 | 4/4 | 必须 |
| Python、Java 或 Go 后端工程 | 4/4 | 必须 |
| 稳定性、性能和异步任务 | 4/4 | 必须 |
| RAG、上下文或记忆 | 3/4 | 高 |
| Multi-Agent | 2/4 | 中 |
| MCP | 1/4 | 加分 |
| Harness Engineering | 1/4 | 加分 |

## 3. Agent 核心能力

企业需要的不是一次模型调用，而是能够完成多步骤任务的状态化 Agent：

- Agent 工作流设计
- ReAct
- Plan-and-Execute
- Tool Calling / Function Calling
- Agent 任务编排
- 会话管理
- 消息流转
- Agent 状态管理
- 模型服务接入
- Agent 中断、恢复和重试
- Human-in-the-loop

## 4. RAG 与知识库

- 企业知识库构建
- 文档解析与切分
- Embedding
- 向量检索
- 混合检索
- Rerank
- Query Rewrite
- 检索结果过滤
- 引用与事实溯源
- RAG 效果调优
- 权限控制下的知识检索

重点不只是接入向量数据库，还要能够区分并定位召回错误、排序错误和模型生成错误。

## 5. 上下文工程

- Context Engineering
- 上下文窗口管理
- 相关信息筛选
- 上下文压缩
- 对话摘要
- 长任务上下文维护
- 工具返回结果裁剪
- Prompt 缓存
- 不同 Agent 之间的上下文传递

上下文工程的重要性高于单纯调整 Prompt 文案。

## 6. 记忆系统

- 短期会话记忆
- 长期用户记忆
- Episodic Memory
- Semantic Memory
- 记忆提取
- 记忆检索
- 记忆更新与遗忘
- 不同用户和会话的记忆隔离
- Agent 状态持久化

## 7. Multi-Agent

- Multi-Agent 架构设计
- Supervisor / Worker 模式
- Agent 分工
- Agent 间消息传递
- 任务委派
- 结果聚合
- 冲突处理
- 多 Agent 上下文管理

Multi-Agent 不是所有岗位的必要条件。只有角色确实需要不同工具、权限或上下文时才应拆分，不能为了关键词让多个 Agent 无约束讨论。

## 8. Agent 框架

岗位中直接出现：

- LangChain
- LangGraph
- AutoGen
- CrewAI

建议掌握顺序：

```text
LangGraph > LangChain 基础 > 了解 AutoGen / CrewAI
```

LangGraph 更适合展示状态图、中断恢复、人工审批和持久化。

## 9. MCP 与业务工具集成

- MCP Client / Server
- HTTP API
- 企业内部接口
- 第三方系统集成
- Tool Schema
- 工具权限
- 工具调用超时与重试
- 工具结果校验
- 高风险操作审批
- 服务化工具封装

MCP 在样本中不是最高频要求，但已经是明确的加分项。

## 10. Prompt Engineering

- System Prompt 设计
- Few-shot 示例
- 结构化输出
- Prompt 模板化
- Prompt 版本管理
- Prompt 调优
- 指令冲突处理
- Prompt Injection 防护
- 不同模型的 Prompt 适配

Prompt 优化需要有评测依据，不能只依赖人工感觉。

## 11. Agent 评测

- 效果评估
- 链路优化
- Harness Engineering
- Agent 回归测试
- LLM-as-a-Judge
- 确定性规则评测
- 工具调用正确率
- 任务完成率
- RAG 准确率
- Token、成本和延迟
- Trace 与失败分析

Harness 是 Agent 开发能力的一部分，不能单独替代业务 Agent。

## 12. 编程语言

- Python：Agent Runtime、大模型应用、RAG 和评测
- Java：企业后端、业务系统和 Agent 平台
- Go：高并发服务、IoT 和基础设施场景

推荐的 Java + Python 分工：

```text
Java
- 用户、权限和业务领域模型
- 状态、消息、数据库和第三方业务系统
- 高并发、实时通信和企业服务治理

Python
- Agent Runtime
- LangGraph 工作流
- RAG、模型调用和上下文管理
- Agent 评测与实验
```

## 13. Java 后端工程

- Spring Boot
- Spring Cloud
- MyBatis
- Java 集合和并发
- 线程池
- I/O
- JVM 原理与调优
- 微服务
- API Gateway
- Nacos
- 服务稳定性
- 高并发系统设计

企业并不只招聘会调用模型的人，还要求完整的后端工程能力。

## 14. 数据库、缓存和消息系统

- MySQL
- PostgreSQL
- Redis
- Elasticsearch
- Kafka
- RabbitMQ
- RocketMQ
- NATS
- MQTT
- SQL 优化
- 索引优化
- 慢查询分析
- 消息堆积处理
- 异步任务

这些能力用于长时间运行的 Agent、消息流转、状态持久化和可靠执行。

## 15. 实时与流式通信

- SSE
- WebSocket
- 流式模型输出
- 异步任务
- 实时数据流
- Agent 运行进度
- 人工确认链路
- 断线恢复

Agent 后端通常不是普通的同步请求响应接口。

## 16. 工程化与稳定性

- 可用性
- 稳定性
- 响应效率
- 性能优化
- 高并发
- 可扩展性
- 超时与重试
- 幂等
- 限流
- 缓存策略
- 故障定位
- 日志、指标和 Trace
- 成本控制

## 17. 安全与治理

- 工具权限控制
- 高风险操作审批
- 用户数据隔离
- Agent 操作审计
- Prompt Injection 防护
- 敏感信息脱敏
- Secret 管理
- 多租户
- 访问控制
- Agent 行为可追溯

## 18. 业务与系统集成

- 企业内部业务应用
- 广告投放和数据平台
- 金融、电商和客服垂直场景
- IoT 和智能硬件
- 知识库和工作流引擎
- MCP / HTTP API
- 模型服务和第三方系统

企业更看重 Agent 能否进入真实业务流程，而不是单独展示聊天能力。

## 19. 能力优先级

```text
第一层：Python/Java 后端 + Agent 工作流 + Tool Calling
第二层：RAG + 上下文工程 + 记忆
第三层：业务系统集成 + MCP + 异步状态管理
第四层：评测、Trace、稳定性和成本
第五层：Multi-Agent、平台化和复杂基础设施
```

## 20. 求职项目最低验收清单

一个用于投递 Agent 开发工程师岗位的项目，至少应完整展示：

- [ ] 一个真实、明确且需要 Agent 的业务任务
- [ ] LangGraph 或等价状态工作流
- [ ] Tool Calling
- [ ] 至少一个 MCP 集成
- [ ] RAG 与带来源的结果
- [ ] 上下文压缩或上下文选择
- [ ] 短期状态与长期记忆
- [ ] Java 和 Python 服务协作
- [ ] 异步任务与流式进度
- [ ] 关键操作人工审批
- [ ] 超时、重试、幂等和失败恢复
- [ ] Trace、成本和延迟记录
- [ ] 确定性评测与 LLM Judge
- [ ] 可运行测试和公开评测结果
- [ ] 可公开访问的产品
- [ ] 完整 README、架构说明和失败案例

## 21. 项目评审原则

后续评估项目创意时，优先检查：

1. 不使用 Agent 时，这个任务是否明显更难完成？
2. 是否存在多步骤规划、工具调用和状态变化？
3. 是否能使用真实数据，而不是预制演示脚本？
4. 是否能产生可验证的结果？
5. 是否能让普通用户或企业用户直接体验？
6. 是否覆盖招聘要求，而不是机械堆叠技术关键词？
7. 是否有明确边界，避免退化成万能助手或通用中台？
