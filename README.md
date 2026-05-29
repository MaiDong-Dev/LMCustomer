# LMCustomer - 智能客服对话系统

基于 LLM 驱动的智能客服对话系统，采用模块化架构设计，支持多通道接入和流程定制。

## 功能特性

- **对话管理**：基于 `DialogueStateTracker` 管理对话状态、槽位和历史
- **Flow 流程引擎**：支持定义和执行结构化对话流程
- **命令生成与处理**：通过 LLM 将用户自然语言转换为系统命令
- **多策略支持**：FlowPolicy、EnterpriseSearchPolicy 等策略集成
- **多通道接入**：REST API、SocketIO、Console
- **检索增强**：支持向量检索和 GraphRAG 知识图谱检索

## 技术栈

| 分类 | 技术 |
|------|------|
| LLM 集成 | LangChain, LangGraph |
| Web 框架 | FastAPI |
| 数据验证 | Pydantic |
| LLM 提供商 | OpenAI, Anthropic, 通义千问 |
| 检索 | sentence-transformers, FAISS, Neo4j |

## 项目结构

```
atguigu_ai/
├── agent/              # 对话代理
├── api/                # Web API 服务
├── channels/            # 对话通道
├── cli/                 # 命令行工具
├── core/                # 核心模块
├── dialogue_understanding/  # 对话理解
├── nlg/                 # 自然语言生成
├── policies/            # 策略系统
├── retrieval/           # 检索增强
├── shared/              # 共享工具
└── training/            # 训练模块

ecs_demo/               # 电商客服示例
├── actions/            # 自定义动作
├── domain/              # 领域定义
├── flows/               # 流程定义
└── addons/              # 扩展功能
```

## 快速开始

### 安装依赖

```bash
pip install -r requirements-atguigu.txt
pip install -e .
```

### 配置

编辑 `ecs_demo/endpoints.yml` 配置 LLM API：

```yaml
models:
  default:
    type: openai
    model: gpt-4o-mini
    api_key: your-api-key
```

### 启动服务

```bash
atguigu run ./ecs_demo
```

### 命令行交互

```bash
atguigu inspect ./ecs_demo
```

## 开发指南

### 创建自定义 Action

```python
from atguigu_ai.agent.actions import Action, register_action

class ActionOrderQuery(Action):
    def __init__(self):
        super().__init__("action_order_query")

    async def run(self, tracker, domain, **kwargs):
        order_id = tracker.get_slot("order_id")
        return {"response": f"查询订单 {order_id}"}

register_action(ActionOrderQuery())
```

### 定义 Flow

在 `flows/` 目录下创建 YAML 文件：

```yaml
id: order_query
steps:
  - id: ask_order_id
    collect: order_id
    action: utter_ask_order_id
  - id: query
    action: action_order_query
  - id: end
    action: utter_goodbye
```

## License

MIT
