# Vanna 2.0 源代码说明（SOURCE.md）

## 1. 源代码整体介绍

本仓库是 **Vanna 2.0**（PyPI 包名 `vanna`，当前 pyproject 版本 **2.0.2**），用自然语言驱动 SQL 查询与可视化，并提供用户感知的 Agent 运行时与 Web 组件。**主要编程语言**是 Python 3.9+（核心库）与 TypeScript 5.9（`frontends/webcomponent`）。**构建与开发工具**包括：flit（打包）、pytest / pytest-asyncio / pytest-mock / pytest-cov（测试）、tox（多环境矩阵）、ruff（lint/format）、mypy（类型，dev extra）、pre-commit、Vite 7 与 Storybook 8（前端）、click（CLI）、Pydantic v2（数据校验）、httpx（HTTP）、pandas / plotly / sqlparse / sqlalchemy（数据与可视化）。版本控制为 GitHub，CI 配置位于 `.github/`。许可证为 MIT。

**规模统计（对本工作区静态扫描，不含 `.git` 对象细节）**：文件 371 个，合计文本行约 87211 行；其中 Python 文件 301 个、Python 行数 43417；Python 类 384 个、模块级函数 219 个、方法 1293 个；TypeScript/JavaScript 文件 29 个；Markdown 6 个。核心实现位于 `src/vanna`，测试位于 `tests`，前端位于 `frontends/webcomponent`，遗留 0.x 代码位于 `src/vanna/legacy`，示例位于 `src/vanna/examples` 与 `examples`。

### 1.1 按扩展名统计

- `.py`：301 个文件，约 43417 行，类型为Python。
- `.ts`：28 个文件，约 11902 行，类型为TypeScript。
- `.png`：19 个文件，约 27235 行，类型为PNG 图像。
- `.md`：6 个文件，约 2099 行，类型为Markdown 文档。
- `（无扩展名）`：3 个文件，约 53 行，类型为无扩展名。
- `.yaml`：2 个文件，约 139 行，类型为YAML。
- `.json`：2 个文件，约 78 行，类型为JSON 配置。
- `.html`：2 个文件，约 606 行，类型为HTML。
- `.ini`：1 个文件，约 243 行，类型为.ini。
- `.cfg`：1 个文件，约 11 行，类型为INI/CFG 配置。
- `.toml`：1 个文件，约 223 行，类型为TOML 配置。
- `.ipynb`：1 个文件，约 170 行，类型为Jupyter Notebook。
- `.txt`：1 个文件，约 9 行，类型为文本。
- `.js`：1 个文件，约 64 行，类型为JavaScript。
- `.typed`：1 个文件，约 0 行，类型为.typed。
- `.svg`：1 个文件，约 962 行，类型为SVG 图像。

### 1.2 目录职责总览

- `./`：11 个文件，约 1916 行。项目支撑文件，用于构建、文档、示例或配置。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `examples/`：2 个文件，约 295 行。可运行示例，覆盖认证、配额、评测、可视化、自定义工具。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `frontends/webcomponent/`：7 个文件，约 2008 行。Lit Web Components 前端，提供 <vanna-chat> 等组件。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `frontends/webcomponent/.storybook/`：3 个文件，约 49 行。Lit Web Components 前端，提供 <vanna-chat> 等组件。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `frontends/webcomponent/scripts/`：1 个文件，约 64 行。Lit Web Components 前端，提供 <vanna-chat> 等组件。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `frontends/webcomponent/src/`：2 个文件，约 43 行。Lit Web Components 前端，提供 <vanna-chat> 等组件。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `frontends/webcomponent/src/components/`：20 个文件，约 9582 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `frontends/webcomponent/src/services/`：1 个文件，约 296 行。Lit Web Components 前端，提供 <vanna-chat> 等组件。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `frontends/webcomponent/src/styles/`：2 个文件，约 1915 行。Lit Web Components 前端，提供 <vanna-chat> 等组件。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `img/`：4 个文件，约 9956 行。项目支撑文件，用于构建、文档、示例或配置。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `notebooks/`：1 个文件，约 170 行。项目支撑文件，用于构建、文档、示例或配置。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `papers/`：1 个文件，约 310 行。项目支撑文件，用于构建、文档、示例或配置。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `papers/img/`：16 个文件，约 18241 行。项目支撑文件，用于构建、文档、示例或配置。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/evals/benchmarks/`：1 个文件，约 173 行。项目支撑文件，用于构建、文档、示例或配置。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/evals/datasets/sql_generation/`：1 个文件，约 119 行。项目支撑文件，用于构建、文档、示例或配置。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/`：2 个文件，约 173 行。Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/agents/`：1 个文件，约 8 行。Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/capabilities/`：1 个文件，约 18 行。Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/capabilities/agent_memory/`：3 个文件，约 180 行。Agent 记忆能力接口，支持向量检索工具用法与文本记忆。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/capabilities/file_system/`：3 个文件，约 113 行。文件系统能力，供 SQL 结果落盘与编码 Agent 读写文件。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/capabilities/sql_runner/`：3 个文件，约 66 行。SQL 执行能力接口，各数据库集成都实现该接口。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/`：2 个文件，约 105 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/rich/`：2 个文件，约 101 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/rich/containers/`：2 个文件，约 29 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/rich/data/`：3 个文件，约 122 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/rich/feedback/`：8 个文件，约 198 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/rich/interactive/`：4 个文件，约 271 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/rich/specialized/`：2 个文件，约 29 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/components/simple/`：4 个文件，约 60 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/`：9 个文件，约 1275 行。Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/agent/`：3 个文件，约 1543 行。Agent 编排内核，负责把用户消息、工具调用、会话存储和可观测性串成一次完整请求。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/audit/`：3 个文件，约 461 行。审计日志抽象，覆盖工具访问、调用、结果和 UI 特性检查。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/enhancer/`：3 个文件，约 226 行。LLM 上下文增强器，默认从 AgentMemory 检索相关记忆写入系统提示。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/enricher/`：2 个文件，约 71 行。工具上下文富集器，可注入租户、数据库方言、文档片段。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/evaluation/`：6 个文件，约 1505 行。评测框架，含轨迹、输出、LLM-as-Judge、效率四类评估器。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/filter/`：2 个文件，约 79 行。会话过滤器，在送入 LLM 前裁剪或脱敏历史消息。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/lifecycle/`：2 个文件，约 95 行。生命周期钩子，可在消息前后、工具前后插入配额、过滤、日志。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/llm/`：3 个文件，约 120 行。大模型通信的请求/响应/流式分片模型与服务接口。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/middleware/`：2 个文件，约 81 行。LLM 中间件，可缓存、改写提示词、记录成本。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/observability/`：3 个文件，约 149 行。Span 与 Metric 抽象，用于追踪一次 Agent 调用的耗时与错误。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/recovery/`：3 个文件，约 130 行。错误恢复策略，决定重试、放弃或改写请求。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/storage/`：3 个文件，约 109 行。会话与消息持久化抽象。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/system_prompt/`：3 个文件，约 209 行。系统提示词构建器。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/tool/`：3 个文件，约 175 行。工具领域模型与抽象基类，定义 LLM 可调用的工具契约。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/user/`：5 个文件，约 188 行。用户身份、分组与请求上下文，是行级安全和 UI 特性开关的根基。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/core/workflow/`：3 个文件，约 1058 行。工作流处理器，可在进入 LLM 循环前短路处理 /help、/status 等命令。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/examples/`：22 个文件，约 4455 行。可运行示例，覆盖认证、配额、评测、可视化、自定义工具。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/`：1 个文件，约 18 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/anthropic/`：2 个文件，约 281 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/azureopenai/`：2 个文件，约 340 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/azuresearch/`：2 个文件，约 422 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/bigquery/`：2 个文件，约 88 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/chromadb/`：2 个文件，约 594 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/clickhouse/`：2 个文件，约 89 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/duckdb/`：2 个文件，约 72 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/faiss/`：2 个文件，约 444 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/google/`：2 个文件，约 381 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/hive/`：2 个文件，约 94 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/local/`：5 个文件，约 640 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/local/agent_memory/`：2 个文件，约 294 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/marqo/`：2 个文件，约 363 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/milvus/`：2 个文件，约 467 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/mock/`：2 个文件，约 76 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/mssql/`：2 个文件，约 73 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/mysql/`：2 个文件，约 99 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/ollama/`：2 个文件，约 261 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/openai/`：3 个文件，约 443 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/opensearch/`：2 个文件，约 420 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/oracle/`：2 个文件，约 82 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/pinecone/`：2 个文件，约 338 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/plotly/`：2 个文件，约 320 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/postgres/`：2 个文件，约 123 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/premium/agent_memory/`：2 个文件，约 195 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/presto/`：2 个文件，约 114 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/qdrant/`：2 个文件，约 475 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/snowflake/`：2 个文件，约 154 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/sqlite/`：2 个文件，约 76 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/integrations/weaviate/`：2 个文件，约 437 行。第三方集成：LLM、数据库、向量库、本地存储、Plotly。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/`：5 个文件，约 1030 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/ZhipuAI/`：3 个文件，约 317 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/advanced/`：1 个文件，约 29 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/anthropic/`：2 个文件，约 83 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/azuresearch/`：2 个文件，约 277 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/base/`：2 个文件，约 2128 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/bedrock/`：2 个文件，约 89 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/chromadb/`：2 个文件，约 262 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/cohere/`：3 个文件，约 181 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/deepseek/`：2 个文件，约 62 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/exceptions/`：1 个文件，约 47 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/faiss/`：2 个文件，约 233 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/flask/`：3 个文件，约 1460 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/google/`：3 个文件，约 365 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/hf/`：2 个文件，约 83 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/marqo/`：2 个文件，约 171 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/milvus/`：2 个文件，约 331 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/mistral/`：2 个文件，约 54 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/mock/`：4 个文件，约 103 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/ollama/`：2 个文件，约 113 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/openai/`：3 个文件，约 175 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/opensearch/`：3 个文件，约 574 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/oracle/`：2 个文件，约 587 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/pgvector/`：2 个文件，约 285 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/pinecone/`：2 个文件，约 280 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/qdrant/`：2 个文件，约 334 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/qianfan/`：3 个文件，约 211 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/qianwen/`：3 个文件，约 183 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/types/`：1 个文件，约 293 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/vannadb/`：2 个文件，约 471 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/vllm/`：2 个文件，约 100 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/weaviate/`：2 个文件，约 197 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/legacy/xinference/`：2 个文件，约 56 行。Vanna 0.x 兼容层与旧向量/聊天实现。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/servers/`：2 个文件，约 26 行。FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/servers/base/`：5 个文件，约 671 行。FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/servers/cli/`：2 个文件，约 213 行。FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/servers/fastapi/`：3 个文件，约 356 行。FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/servers/flask/`：3 个文件，约 279 行。FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/tools/`：6 个文件，约 1830 行。内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/utils/`：1 个文件，约 0 行。Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `src/vanna/web_components/`：1 个文件，约 45 行。简单组件与富组件的服务端数据模型，对应前端 Web Component。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。
- `tests/`：14 个文件，约 5815 行。pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。 该目录是阅读源码时的导航锚点，修改前应先看同目录 `__init__.py` 的导出，以免破坏公开 API。

### 1.3 语言与风格约定

Python 侧目标风格接近 Black（ruff line-length 88）。异步优先：Agent、LLM、工具、存储均为 async。类型用 Pydantic 模型跨越边界，ABC 定义端口。前端为 Lit 自定义元素，事件与属性即文档。测试函数命名 test_* ，集成用 pytest.mark。不要在核心模块导入具体云 SDK，应放 integrations。legacy 仅维护兼容，新功能禁止往 legacy 堆。

## 2. 源代码文件清单（全量）

下表式清单覆盖工作区内每个被扫描文件：路径、目录、语言、行数、主要功能。Python 文件额外展开类与方法摘要，便于对照 1DESIGN.md 的详细设计。

### `.gitattributes`

- **目录**：`.`。**文件名**：`.gitattributes`。**语言/类型**：无扩展名。**行数**：2。**字节**：34。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `.gitignore`

- **目录**：`.`。**文件名**：`.gitignore`。**语言/类型**：无扩展名。**行数**：29。**字节**：402。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `.pre-commit-config.yaml`

- **目录**：`.`。**文件名**：`.pre-commit-config.yaml`。**语言/类型**：YAML。**行数**：20。**字节**：493。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `CONTRIBUTING.md`

- **目录**：`.`。**文件名**：`CONTRIBUTING.md`。**语言/类型**：Markdown 文档。**行数**：486。**字节**：11057。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `LICENSE`

- **目录**：`.`。**文件名**：`LICENSE`。**语言/类型**：无扩展名。**行数**：22。**字节**：1065。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `MIGRATION_GUIDE.md`

- **目录**：`.`。**文件名**：`MIGRATION_GUIDE.md`。**语言/类型**：Markdown 文档。**行数**：297。**字节**：10179。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `README.md`

- **目录**：`.`。**文件名**：`README.md`。**语言/类型**：Markdown 文档。**行数**：312。**字节**：9578。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
- **阅读提示**：对外产品说明与最小 FastAPI 接入示例。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `README_LEGACY.md`

- **目录**：`.`。**文件名**：`README_LEGACY.md`。**语言/类型**：Markdown 文档。**行数**：271。**字节**：10225。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `examples/chromadb_gpu_example.py`

- **目录**：`examples`。**文件名**：`chromadb_gpu_example.py`。**语言/类型**：Python。**行数**：138。**字节**：4142。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example: Using ChromaDB AgentMemory with GPU acceleration

This example demonstrates how to use ChromaAgentMemory with intelligent
device selection for GPU acceleration when available.
- **结构摘要**：类 0 个，模块函数 5 个，方法 0 个。
  - **函数 `example_default_usage`**：完成该模块中的具体处理。无显式位置参数。Example 1: Use default embedding function (no GPU, no sentence-transformers required)行 15–24。
  - **函数 `example_auto_gpu`**：完成该模块中的具体处理。无显式位置参数。Example 2: Automatic GPU detection with SentenceTransformers行 27–44。
  - **函数 `example_explicit_cuda`**：完成该模块中的具体处理。无显式位置参数。Example 3: Explicitly use CUDA行 47–60。
  - **函数 `example_custom_model_gpu`**：完成该模块中的具体处理。无显式位置参数。Example 4: Use a larger model with GPU行 63–78。
  - **函数 `example_manual_chromadb`**：完成该模块中的具体处理。无显式位置参数。Example 5: Manually configure ChromaDB embedding function行 81–100。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `examples/transform_args_example.py`

- **目录**：`examples`。**文件名**：`transform_args_example.py`。**语言/类型**：Python。**行数**：157。**字节**：5250。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example demonstrating how to use ToolRegistry.transform_args for user-specific
argument transformation, such as applying row-level security (RLS) to SQL queries.

This example shows:
1. Creating a custom ToolRegistry subclass that overrides transform_args
2. Applying RLS transformation to SQL queries based on user context
3. Rejecting tool execution when validation fails
- **结构摘要**：类 3 个，模块函数 1 个，方法 5 个。
  - **类 `SQLExecutionArgs`**（Pydantic 数据模型，基类：BaseModel，行 20–22）：职责见方法列表。
  - **类 `SQLExecutionTool`**（抽象或具体服务/工具实现，基类：Tool[SQLExecutionArgs]，行 25–42）：职责见方法列表。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 27–28 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 31–32 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 34–35 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 37–42 行。
  - **类 `RLSToolRegistry`**（抽象或具体服务/工具实现，基类：ToolRegistry，行 45–96）：Custom ToolRegistry that applies row-level security to SQL queries.
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Apply row-level security transformation to SQL queries.位于第 48–96 行。
  - **函数 `async example_usage`**：完成该模块中的具体处理。无显式位置参数。Demonstrate using the RLS-enabled ToolRegistry.行 100–150。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/.storybook/main.ts`

- **目录**：`frontends/webcomponent/.storybook`。**文件名**：`main.ts`。**语言/类型**：TypeScript。**行数**：25。**字节**：685。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/.storybook/preview-head.html`

- **目录**：`frontends/webcomponent/.storybook`。**文件名**：`preview-head.html`。**语言/类型**：HTML。**行数**：7。**字节**：328。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/.storybook/preview.ts`

- **目录**：`frontends/webcomponent/.storybook`。**文件名**：`preview.ts`。**语言/类型**：TypeScript。**行数**：17。**字节**：289。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/TEST_README.md`

- **目录**：`frontends/webcomponent`。**文件名**：`TEST_README.md`。**语言/类型**：Markdown 文档。**行数**：423。**字节**：11658。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
- **阅读提示**：对外产品说明与最小 FastAPI 接入示例。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/package.json`

- **目录**：`frontends/webcomponent`。**文件名**：`package.json`。**语言/类型**：JSON 配置。**行数**：58。**字节**：1495。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/requirements-test.txt`

- **目录**：`frontends/webcomponent`。**文件名**：`requirements-test.txt`。**语言/类型**：文本。**行数**：9。**字节**：255。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/scripts/sync-version.js`

- **目录**：`frontends/webcomponent/scripts`。**文件名**：`sync-version.js`。**语言/类型**：JavaScript。**行数**：64。**字节**：1724。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/button.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`button.stories.ts`。**语言/类型**：TypeScript。**行数**：533。**字节**：15139。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/dataframe-component.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`dataframe-component.stories.ts`。**语言/类型**：TypeScript。**行数**：564。**字节**：19876。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/plotly-chart.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`plotly-chart.stories.ts`。**语言/类型**：TypeScript。**行数**：273。**字节**：6516。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/plotly-chart.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`plotly-chart.ts`。**语言/类型**：TypeScript。**行数**：201。**字节**：5218。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-card.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-card.stories.ts`。**语言/类型**：TypeScript。**行数**：187。**字节**：4921。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-card.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-card.ts`。**语言/类型**：TypeScript。**行数**：309。**字节**：8820。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-component-system.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-component-system.stories.ts`。**语言/类型**：TypeScript。**行数**：354。**字节**：11365。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-component-system.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-component-system.ts`。**语言/类型**：TypeScript。**行数**：2100。**字节**：70943。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-progress-bar.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-progress-bar.stories.ts`。**语言/类型**：TypeScript。**行数**：252。**字节**：6442。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-progress-bar.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-progress-bar.ts`。**语言/类型**：TypeScript。**行数**：202。**字节**：5442。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-task-list.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-task-list.stories.ts`。**语言/类型**：TypeScript。**行数**：270。**字节**：6889。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/rich-task-list.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`rich-task-list.ts`。**语言/类型**：TypeScript。**行数**：272。**字节**：7338。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-chat.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-chat.stories.ts`。**语言/类型**：TypeScript。**行数**：1177。**字节**：43341。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-chat.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-chat.ts`。**语言/类型**：TypeScript。**行数**：1429。**字节**：44436。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-message.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-message.stories.ts`。**语言/类型**：TypeScript。**行数**：95。**字节**：2725。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-message.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-message.ts`。**语言/类型**：TypeScript。**行数**：222。**字节**：6467。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-progress-tracker.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-progress-tracker.stories.ts`。**语言/类型**：TypeScript。**行数**：268。**字节**：9037。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-progress-tracker.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-progress-tracker.ts`。**语言/类型**：TypeScript。**行数**：263。**字节**：7309。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-status-bar.stories.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-status-bar.stories.ts`。**语言/类型**：TypeScript。**行数**：168。**字节**：4420。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/components/vanna-status-bar.ts`

- **目录**：`frontends/webcomponent/src/components`。**文件名**：`vanna-status-bar.ts`。**语言/类型**：TypeScript。**行数**：443。**字节**：13100。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/index.ts`

- **目录**：`frontends/webcomponent/src`。**文件名**：`index.ts`。**语言/类型**：TypeScript。**行数**：38。**字节**：1210。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/services/api-client.ts`

- **目录**：`frontends/webcomponent/src/services`。**文件名**：`api-client.ts`。**语言/类型**：TypeScript。**行数**：296。**字节**：8394。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/styles/rich-component-styles.ts`

- **目录**：`frontends/webcomponent/src/styles`。**文件名**：`rich-component-styles.ts`。**语言/类型**：TypeScript。**行数**：1763。**字节**：41059。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/styles/vanna-design-tokens.ts`

- **目录**：`frontends/webcomponent/src/styles`。**文件名**：`vanna-design-tokens.ts`。**语言/类型**：TypeScript。**行数**：152。**字节**：6049。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/src/vite-env.d.ts`

- **目录**：`frontends/webcomponent/src`。**文件名**：`vite-env.d.ts`。**语言/类型**：TypeScript。**行数**：5。**字节**：118。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
- **阅读提示**：浏览器端渲染与 SSE 客户端，和后端组件 type 契约绑定。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/test-comprehensive.html`

- **目录**：`frontends/webcomponent`。**文件名**：`test-comprehensive.html`。**语言/类型**：HTML。**行数**：599。**字节**：19109。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/test_backend.py`

- **目录**：`frontends/webcomponent`。**文件名**：`test_backend.py`。**语言/类型**：Python。**行数**：875。**字节**：29189。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
- **模块文档字符串**：Comprehensive test backend for vanna-webcomponent validation.

This backend exercises all component types and update patterns to validate
that nothing breaks during webcomponent pruning.

Usage:
    python test_backend.py --mode rapid      # Fast stress test
    python test_backend.py --mode realistic  # Realistic conversation flow
- **结构摘要**：类 2 个，模块函数 23 个，方法 0 个。
  - **类 `ChatRequest`**（Pydantic 数据模型，基类：BaseModel，行 61–66）：Chat request matching vanna API.
  - **类 `UiComponent`**（Pydantic 数据模型，基类：BaseModel，行 69–71）：UI component wrapper.
  - **函数 `async yield_chunk`**：产出流式结果。`component`（该函数的业务参数，详见源码类型标注）、`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）。Convert component to ChatStreamChunk.行 82–90。
  - **函数 `async delay`**：完成该模块中的具体处理。`mode`（该函数的业务参数，详见源码类型标注）、`short`（该函数的业务参数，详见源码类型标注）、`long`（该函数的业务参数，详见源码类型标注）。Add delay based on mode.行 93–98。
  - **函数 `async test_text_component`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test text component with markdown.行 101–146。
  - **函数 `async test_status_card`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test status card with all states.行 149–176。
  - **函数 `async test_progress_display`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test progress display component.行 179–205。
  - **函数 `async test_card_component`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test card component with actions.行 208–254。
  - **函数 `async test_task_list`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test task list component.行 257–293。
  - **函数 `async test_progress_bar`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test progress bar component.行 296–314。
  - **函数 `async test_notification`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test notification component.行 317–327。
  - **函数 `async test_status_indicator`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test status indicator component.行 330–348。
  - **函数 `async test_badge`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test badge component.行 351–359。
  - **函数 `async test_icon_text`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test icon_text component.行 362–370。
  - **函数 `async test_buttons`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test button and button_group components.行 373–395。
  - **函数 `async test_dataframe`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test dataframe component with sample data.行 398–447。
  - **函数 `async test_chart`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test chart component with Plotly data.行 450–509。
  - **函数 `async test_artifact`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test artifact component with HTML/SVG content.行 511–535。
  - **函数 `async test_log_viewer`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test log viewer component.行 538–571。
  - **函数 `async test_ui_state_updates`**：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Test UI state update components.行 573–637。
  - **函数 `async run_comprehensive_test`**：运行。`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`mode`（该函数的业务参数，详见源码类型标注）。Run all component tests.行 640–738。
  - **函数 `async handle_action_message`**：处理请求或事件。`message`（用户自然语言输入）、`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）。Handle button action messages.行 741–759。
  - **函数 `async chat_sse`**：完成该模块中的具体处理。`chat_request`（该函数的业务参数，详见源码类型标注）。SSE endpoint for streaming chat.行 781–830。
  - **函数 `async health`**：完成该模块中的具体处理。无显式位置参数。Health check.行 834–836。
  - **函数 `async root`**：完成该模块中的具体处理。无显式位置参数。Serve test HTML page.行 840–852。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/tsconfig.json`

- **目录**：`frontends/webcomponent`。**文件名**：`tsconfig.json`。**语言/类型**：JSON 配置。**行数**：20。**字节**：520。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `frontends/webcomponent/vite.config.ts`

- **目录**：`frontends/webcomponent`。**文件名**：`vite.config.ts`。**语言/类型**：TypeScript。**行数**：24。**字节**：554。
- **主要功能简介**：Lit Web Components 前端，提供 <vanna-chat> 等组件。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `img/architecture.png`

- **目录**：`img`。**文件名**：`architecture.png`。**语言/类型**：PNG 图像。**行数**：6010。**字节**：844989。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `img/top-10-customers.png`

- **目录**：`img`。**文件名**：`top-10-customers.png`。**语言/类型**：PNG 图像。**行数**：388。**字节**：41929。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `img/vanna-readme-diagram.png`

- **目录**：`img`。**文件名**：`vanna-readme-diagram.png`。**语言/类型**：PNG 图像。**行数**：2596。**字节**：373408。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `img/vanna2.svg`

- **目录**：`img`。**文件名**：`vanna2.svg`。**语言/类型**：SVG 图像。**行数**：962。**字节**：196342。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `notebooks/quickstart.ipynb`

- **目录**：`notebooks`。**文件名**：`quickstart.ipynb`。**语言/类型**：Jupyter Notebook。**行数**：170。**字节**：4824。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/ai-sql-accuracy-2023-08-17.md`

- **目录**：`papers`。**文件名**：`ai-sql-accuracy-2023-08-17.md`。**语言/类型**：Markdown 文档。**行数**：310。**字节**：18860。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/accuracy-by-llm.png`

- **目录**：`papers/img`。**文件名**：`accuracy-by-llm.png`。**语言/类型**：PNG 图像。**行数**：1260。**字节**：160749。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/accuracy-using-contextual-examples.png`

- **目录**：`papers/img`。**文件名**：`accuracy-using-contextual-examples.png`。**语言/类型**：PNG 图像。**行数**：3007。**字节**：219354。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/accuracy-using-schema-only.png`

- **目录**：`papers/img`。**文件名**：`accuracy-using-schema-only.png`。**语言/类型**：PNG 图像。**行数**：796。**字节**：122798。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/accuracy-using-static-examples.png`

- **目录**：`papers/img`。**文件名**：`accuracy-using-static-examples.png`。**语言/类型**：PNG 图像。**行数**：1265。**字节**：177743。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/chat-gpt-question.png`

- **目录**：`papers/img`。**文件名**：`chat-gpt-question.png`。**语言/类型**：PNG 图像。**行数**：438。**字节**：62203。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/chatgpt-results.png`

- **目录**：`papers/img`。**文件名**：`chatgpt-results.png`。**语言/类型**：PNG 图像。**行数**：948。**字节**：164947。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/framework-for-sql-generation.png`

- **目录**：`papers/img`。**文件名**：`framework-for-sql-generation.png`。**语言/类型**：PNG 图像。**行数**：1163。**字节**：108562。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/question-flow.png`

- **目录**：`papers/img`。**文件名**：`question-flow.png`。**语言/类型**：PNG 图像。**行数**：464。**字节**：81260。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/schema-only.png`

- **目录**：`papers/img`。**文件名**：`schema-only.png`。**语言/类型**：PNG 图像。**行数**：1056。**字节**：151833。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/sql-error.png`

- **目录**：`papers/img`。**文件名**：`sql-error.png`。**语言/类型**：PNG 图像。**行数**：308。**字节**：43107。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/summary-table.png`

- **目录**：`papers/img`。**文件名**：`summary-table.png`。**语言/类型**：PNG 图像。**行数**：690。**字节**：104753。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/summary.png`

- **目录**：`papers/img`。**文件名**：`summary.png`。**语言/类型**：PNG 图像。**行数**：1591。**字节**：279799。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/test-architecture.png`

- **目录**：`papers/img`。**文件名**：`test-architecture.png`。**语言/类型**：PNG 图像。**行数**：1119。**字节**：186149。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/test-levers.png`

- **目录**：`papers/img`。**文件名**：`test-levers.png`。**语言/类型**：PNG 图像。**行数**：800。**字节**：111694。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/using-contextually-relevant-examples.png`

- **目录**：`papers/img`。**文件名**：`using-contextually-relevant-examples.png`。**语言/类型**：PNG 图像。**行数**：1881。**字节**：202886。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `papers/img/using-sql-examples.png`

- **目录**：`papers/img`。**文件名**：`using-sql-examples.png`。**语言/类型**：PNG 图像。**行数**：1455。**字节**：187567。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `pyproject.toml`

- **目录**：`.`。**文件名**：`pyproject.toml`。**语言/类型**：TOML 配置。**行数**：223。**字节**：7145。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
- **阅读提示**：定义包元数据、可选依赖 extras、pytest/ruff 配置、CLI 入口 vanna。这是安装面的单一事实来源。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `setup.cfg`

- **目录**：`.`。**文件名**：`setup.cfg`。**语言/类型**：INI/CFG 配置。**行数**：11。**字节**：277。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/evals/benchmarks/llm_comparison.py`

- **目录**：`src/evals/benchmarks`。**文件名**：`llm_comparison.py`。**语言/类型**：Python。**行数**：173。**字节**：4811。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
- **模块文档字符串**：LLM Comparison Benchmark

This script compares different LLMs on SQL generation tasks.
Run from repository root:
    PYTHONPATH=. python evals/benchmarks/llm_comparison.py
- **结构摘要**：类 0 个，模块函数 3 个，方法 0 个。
  - **函数 `get_sql_tools`**：读取并返回。无显式位置参数。Get SQL-related tools for testing.  In a real scenario, this would return actual SQL tools. For this benchmark, we'll use a placeholder.行 27–34。
  - **函数 `async compare_llms`**：完成该模块中的具体处理。无显式位置参数。Compare different LLMs on SQL generation tasks.行 37–156。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the LLM comparison benchmark.行 159–168。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/evals/datasets/sql_generation/basic.yaml`

- **目录**：`src/evals/datasets/sql_generation`。**文件名**：`basic.yaml`。**语言/类型**：YAML。**行数**：119。**字节**：4079。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/__init__.py`

- **目录**：`src/vanna`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：173。**字节**：3683。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Vanna Agents - A modular framework for building LLM agents.

This package provides a flexible framework for creating conversational AI agents
with tool execution, conversation management, and user scoping.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/agents/__init__.py`

- **目录**：`src/vanna/agents`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：116。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Agent implementations.

This package contains agent implementations and utilities.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/__init__.py`

- **目录**：`src/vanna/capabilities`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：18。**字节**：392。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Capabilities module.

This package contains abstractions for tool capabilities - reusable utilities
that tools can compose via dependency injection.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/agent_memory/__init__.py`

- **目录**：`src/vanna/capabilities/agent_memory`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：22。**字节**：350。
- **主要功能简介**：Agent 记忆能力接口，支持向量检索工具用法与文本记忆。
- **模块文档字符串**：Agent memory capability package.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/agent_memory/base.py`

- **目录**：`src/vanna/capabilities/agent_memory`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：104。**字节**：2988。
- **主要功能简介**：Agent 记忆能力接口，支持向量检索工具用法与文本记忆。
- **模块文档字符串**：Agent memory capability interface for tool usage learning.

This module contains the abstract base class for agent memory operations,
following the same pattern as the FileSystem interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 9 个。
  - **类 `AgentMemory`**（抽象或具体服务/工具实现，基类：ABC，行 23–103）：Abstract base class for agent memory operations.
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern for future reference.位于第 27–37 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a free-form text memory.位于第 40–44 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns based on a question.位于第 47–57 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search stored text memories based on a query.位于第 60–69 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories. Returns most recent memories first.位于第 72–76 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Fetch recently stored text memories.位于第 79–83 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID. Returns True if deleted, False if not found.位于第 86–88 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID. Returns True if deleted, False if not found.位于第 91–93 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories (tool or text). Returns number of memories deleted.位于第 96–103 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/agent_memory/models.py`

- **目录**：`src/vanna/capabilities/agent_memory`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：54。**字节**：1125。
- **主要功能简介**：Agent 记忆能力接口，支持向量检索工具用法与文本记忆。
- **模块文档字符串**：Memory storage models and types.
- **结构摘要**：类 5 个，模块函数 0 个，方法 0 个。
  - **类 `ToolMemory`**（Pydantic 数据模型，基类：BaseModel，行 10–19）：Represents a stored tool usage memory.
  - **类 `TextMemory`**（Pydantic 数据模型，基类：BaseModel，行 22–27）：Represents a stored free-form text memory.
  - **类 `ToolMemorySearchResult`**（Pydantic 数据模型，基类：BaseModel，行 30–35）：Represents a search result from tool memory storage.
  - **类 `TextMemorySearchResult`**（Pydantic 数据模型，基类：BaseModel，行 38–43）：Represents a search result from text memory storage.
  - **类 `MemoryStats`**（Pydantic 数据模型，基类：BaseModel，行 46–53）：Memory storage statistics.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/file_system/__init__.py`

- **目录**：`src/vanna/capabilities/file_system`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：15。**字节**：267。
- **主要功能简介**：文件系统能力，供 SQL 结果落盘与编码 Agent 读写文件。
- **模块文档字符串**：File system capability.

This module provides abstractions for file system operations used by tools.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/file_system/base.py`

- **目录**：`src/vanna/capabilities/file_system`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：72。**字节**：1859。
- **主要功能简介**：文件系统能力，供 SQL 结果落盘与编码 Agent 读写文件。
- **模块文档字符串**：File system capability interface.

This module contains the abstract base class for file system operations.
- **结构摘要**：类 1 个，模块函数 0 个，方法 7 个。
  - **类 `FileSystem`**（抽象或具体服务/工具实现，基类：ABC，行 16–71）：Abstract base class for file system operations.
    - `async list_files(directory, context)`：枚举并列出。`directory`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。List files in a directory.位于第 20–22 行。
    - `async read_file(filename, context)`：完成该模块中的具体处理。`filename`（文件名）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Read the contents of a file.位于第 25–27 行。
    - `async write_file(filename, content, context, overwrite)`：完成该模块中的具体处理。`filename`（文件名）、`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`overwrite`（该函数的业务参数，详见源码类型标注）。Write content to a file.位于第 30–38 行。
    - `async exists(path, context)`：完成该模块中的具体处理。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a file or directory exists.位于第 41–43 行。
    - `async is_directory(path, context)`：判断是否满足条件。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a path is a directory.位于第 46–48 行。
    - `async search_files(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for files matching a query within the accessible namespace.位于第 51–60 行。
    - `async run_bash(command, context)`：运行。`command`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute a bash command within the accessible namespace.位于第 63–71 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/file_system/models.py`

- **目录**：`src/vanna/capabilities/file_system`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：26。**字节**：464。
- **主要功能简介**：文件系统能力，供 SQL 结果落盘与编码 Agent 读写文件。
- **模块文档字符串**：File system capability models.

This module contains data models for file system operations.
- **结构摘要**：类 2 个，模块函数 0 个，方法 0 个。
  - **类 `FileSearchMatch`**（核心类型或辅助类，基类：无，行 12–16）：Represents a single search result within a file system.
  - **类 `CommandResult`**（核心类型或辅助类，基类：无，行 20–25）：Represents the result of executing a shell command.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/sql_runner/__init__.py`

- **目录**：`src/vanna/capabilities/sql_runner`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：14。**字节**：217。
- **主要功能简介**：SQL 执行能力接口，各数据库集成都实现该接口。
- **模块文档字符串**：SQL runner capability.

This module provides abstractions for SQL execution used by tools.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/sql_runner/base.py`

- **目录**：`src/vanna/capabilities/sql_runner`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：38。**字节**：826。
- **主要功能简介**：SQL 执行能力接口，各数据库集成都实现该接口。
- **模块文档字符串**：SQL runner capability interface.

This module contains the abstract base class for SQL execution.
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `SqlRunner`**（抽象或具体服务/工具实现，基类：ABC，行 18–37）：Interface for SQL execution with different implementations.
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query and return results as a DataFrame.  Args:     args: SQL query arguments     context: Tool execution co位于第 22–37 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/capabilities/sql_runner/models.py`

- **目录**：`src/vanna/capabilities/sql_runner`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：14。**字节**：261。
- **主要功能简介**：SQL 执行能力接口，各数据库集成都实现该接口。
- **模块文档字符串**：SQL runner capability models.

This module contains data models for SQL execution.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `RunSqlToolArgs`**（Pydantic 数据模型，基类：BaseModel，行 10–13）：Arguments for run_sql tool.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/__init__.py`

- **目录**：`src/vanna/components`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：93。**字节**：2057。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：UI Component system for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/base.py`

- **目录**：`src/vanna/components`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：12。**字节**：341。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：UI components base - re-exports UiComponent from core.

UiComponent lives in core/ because it's a fundamental return type for tools.
This module provides backward compatibility by re-exporting it here.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/__init__.py`

- **目录**：`src/vanna/components/rich`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：84。**字节**：1735。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Rich UI components for the Vanna Agents framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/containers/__init__.py`

- **目录**：`src/vanna/components/rich/containers`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：108。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Container components for layout.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/containers/card.py`

- **目录**：`src/vanna/components/rich/containers`。**文件名**：`card.py`。**语言/类型**：Python。**行数**：21。**字节**：717。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Card component for displaying structured information.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `CardComponent`**（UI 组件模型，基类：RichComponent，行 8–20）：Card component for displaying structured information.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/data/__init__.py`

- **目录**：`src/vanna/components/rich/data`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：10。**字节**：171。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Data display components.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/data/chart.py`

- **目录**：`src/vanna/components/rich/data`。**文件名**：`chart.py`。**语言/类型**：Python。**行数**：18。**字节**：655。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Chart component for data visualization.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `ChartComponent`**（UI 组件模型，基类：RichComponent，行 8–17）：Chart component for data visualization.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/data/dataframe.py`

- **目录**：`src/vanna/components/rich/data`。**文件名**：`dataframe.py`。**语言/类型**：Python。**行数**：94。**字节**：3166。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：DataFrame component for displaying tabular data.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `DataFrameComponent`**（UI 组件模型，基类：RichComponent，行 8–93）：DataFrame component specifically for displaying tabular data from SQL queries and similar sources.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 40–63 行。
    - `from_records(records, title, description)`：从外部结构转换而来。`records`（行记录列表）、`title`（该函数的业务参数，详见源码类型标注）、`description`（该函数的业务参数，详见源码类型标注）。Create a DataFrame component from a list of record dictionaries.位于第 66–93 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/__init__.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：22。**字节**：630。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：User feedback components.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/badge.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`badge.py`。**语言/类型**：Python。**行数**：17。**字节**：514。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Badge component for displaying status or labels.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `BadgeComponent`**（UI 组件模型，基类：RichComponent，行 7–16）：Simple badge/pill component for displaying status or labels.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/icon_text.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`icon_text.py`。**语言/类型**：Python。**行数**：15。**字节**：467。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Icon with text component.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `IconTextComponent`**（UI 组件模型，基类：RichComponent，行 6–14）：Simple component for displaying an icon with text.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/log_viewer.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`log_viewer.py`。**语言/类型**：Python。**行数**：42。**字节**：1332。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Log viewer component.
- **结构摘要**：类 2 个，模块函数 0 个，方法 1 个。
  - **类 `LogEntry`**（Pydantic 数据模型，基类：BaseModel，行 10–16）：Log entry for tool execution.
  - **类 `LogViewerComponent`**（UI 组件模型，基类：RichComponent，行 19–41）：Generic log viewer for displaying timestamped entries.
    - `add_entry(message, level, data)`：完成该模块中的具体处理。`message`（用户自然语言输入）、`level`（该函数的业务参数，详见源码类型标注）、`data`（该函数的业务参数，详见源码类型标注）。Add a new log entry.位于第 30–41 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/notification.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`notification.py`。**语言/类型**：Python。**行数**：20。**字节**：670。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Notification component for alerts and messages.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `NotificationComponent`**（UI 组件模型，基类：RichComponent，行 8–19）：Notification component for alerts and messages.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/progress.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`progress.py`。**语言/类型**：Python。**行数**：38。**字节**：1312。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Progress components for displaying progress indicators.
- **结构摘要**：类 2 个，模块函数 0 个，方法 1 个。
  - **类 `ProgressBarComponent`**（UI 组件模型，基类：RichComponent，行 7–15）：Progress bar with status and value.
  - **类 `ProgressDisplayComponent`**（UI 组件模型，基类：RichComponent，行 18–37）：Generic progress display for any long-running process.
    - `update_progress(value, description)`：更新。`value`（该函数的业务参数，详见源码类型标注）、`description`（该函数的业务参数，详见源码类型标注）。Update progress value and optionally description.位于第 30–37 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/status_card.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`status_card.py`。**语言/类型**：Python。**行数**：29。**字节**：1058。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Status card component for displaying process status.
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `StatusCardComponent`**（UI 组件模型，基类：RichComponent，行 8–28）：Generic status card that can display any process status.
    - `set_status(status, description)`：设置并更新。`status`（该函数的业务参数，详见源码类型标注）、`description`（该函数的业务参数，详见源码类型标注）。Update the status and optionally the description.位于第 21–28 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/feedback/status_indicator.py`

- **目录**：`src/vanna/components/rich/feedback`。**文件名**：`status_indicator.py`。**语言/类型**：Python。**行数**：15。**字节**：425。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Status indicator component.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `StatusIndicatorComponent`**（UI 组件模型，基类：RichComponent，行 7–14）：Status indicator with icon and message.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/interactive/__init__.py`

- **目录**：`src/vanna/components/rich/interactive`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：22。**字节**：495。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Interactive components.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/interactive/button.py`

- **目录**：`src/vanna/components/rich/interactive`。**文件名**：`button.py`。**语言/类型**：Python。**行数**：96。**字节**：2942。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Button component for interactive actions.
- **结构摘要**：类 2 个，模块函数 0 个，方法 2 个。
  - **类 `ButtonComponent`**（UI 组件模型，基类：RichComponent，行 7–54）：Interactive button that sends a message when clicked.  The button renders in the UI and when clicked, sends its action value as a message to the chat input.  Args:     label: Text displayed on the button     action: Mess
    - `__init__(label, action, variant, size, icon, icon_position, disabled)`：提供对象协议或生命周期约定。`label`（该函数的业务参数，详见源码类型标注）、`action`（该函数的业务参数，详见源码类型标注）、`variant`（该函数的业务参数，详见源码类型标注）、`size`（该函数的业务参数，详见源码类型标注）、`icon`（该函数的业务参数，详见源码类型标注）、`icon_position`（该函数的业务参数，详见源码类型标注）、`disabled`（该函数的业务参数，详见源码类型标注）。位于第 31–54 行。
  - **类 `ButtonGroupComponent`**（UI 组件模型，基类：RichComponent，行 57–95）：Group of buttons with consistent styling.  Args:     buttons: List of button data dictionaries     orientation: Layout direction     spacing: Gap between buttons     alignment: Button alignment within group     full_widt
    - `__init__(buttons, orientation, spacing, alignment, full_width)`：提供对象协议或生命周期约定。`buttons`（该函数的业务参数，详见源码类型标注）、`orientation`（该函数的业务参数，详见源码类型标注）、`spacing`（该函数的业务参数，详见源码类型标注）、`alignment`（该函数的业务参数，详见源码类型标注）、`full_width`（该函数的业务参数，详见源码类型标注）。位于第 78–95 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/interactive/task_list.py`

- **目录**：`src/vanna/components/rich/interactive`。**文件名**：`task_list.py`。**语言/类型**：Python。**行数**：59。**字节**：2046。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Task list component for interactive task tracking.
- **结构摘要**：类 2 个，模块函数 0 个，方法 3 个。
  - **类 `Task`**（Pydantic 数据模型，基类：BaseModel，行 10–20）：Individual task in a task list.
  - **类 `TaskListComponent`**（UI 组件模型，基类：RichComponent，行 23–58）：Interactive task list with progress tracking.
    - `add_task(task)`：完成该模块中的具体处理。`task`（该函数的业务参数，详见源码类型标注）。Add a task to the list.位于第 34–37 行。
    - `update_task(task_id)`：更新。`task_id`（该函数的业务参数，详见源码类型标注）。Update a specific task.位于第 39–49 行。
    - `complete_task(task_id)`：完成该模块中的具体处理。`task_id`（该函数的业务参数，详见源码类型标注）。Mark a task as completed.位于第 51–58 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/interactive/ui_state.py`

- **目录**：`src/vanna/components/rich/interactive`。**文件名**：`ui_state.py`。**语言/类型**：Python。**行数**：94。**字节**：3214。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：UI state update components for controlling interface elements.
- **结构摘要**：类 4 个，模块函数 0 个，方法 7 个。
  - **类 `StatusBarUpdateComponent`**（UI 组件模型，基类：RichComponent，行 9–20）：Component for updating the status bar above chat input.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 17–20 行。
  - **类 `TaskOperation`**（核心类型或辅助类，基类：str, Enum，行 23–29）：Operations for task tracker updates.
  - **类 `TaskTrackerUpdateComponent`**（UI 组件模型，基类：RichComponent，行 32–78）：Component for updating the task tracker in the sidebar.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 43–46 行。
    - `add_task(task)`：完成该模块中的具体处理。`task`（该函数的业务参数，详见源码类型标注）。Create a component to add a new task.位于第 49–51 行。
    - `update_task(task_id, status, progress, detail)`：更新。`task_id`（该函数的业务参数，详见源码类型标注）、`status`（该函数的业务参数，详见源码类型标注）、`progress`（该函数的业务参数，详见源码类型标注）、`detail`（该函数的业务参数，详见源码类型标注）。Create a component to update an existing task.位于第 54–68 行。
    - `remove_task(task_id)`：移除。`task_id`（该函数的业务参数，详见源码类型标注）。Create a component to remove a task.位于第 71–73 行。
    - `clear_tasks()`：清空。仅依赖实例或类自身状态，不额外接收调用方参数。Create a component to clear all tasks.位于第 76–78 行。
  - **类 `ChatInputUpdateComponent`**（UI 组件模型，基类：RichComponent，行 81–93）：Component for updating chat input state and appearance.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 90–93 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/specialized/__init__.py`

- **目录**：`src/vanna/components/rich/specialized`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：111。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Specialized components.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/specialized/artifact.py`

- **目录**：`src/vanna/components/rich/specialized`。**文件名**：`artifact.py`。**语言/类型**：Python。**行数**：21。**字节**：752。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Artifact component for interactive content.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `ArtifactComponent`**（UI 组件模型，基类：RichComponent，行 9–20）：Component for displaying interactive artifacts that can be rendered externally.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/rich/text.py`

- **目录**：`src/vanna/components/rich`。**文件名**：`text.py`。**语言/类型**：Python。**行数**：17。**字节**：485。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Rich text component.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `RichTextComponent`**（UI 组件模型，基类：RichComponent，行 7–16）：Rich text component with formatting options.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/simple/__init__.py`

- **目录**：`src/vanna/components/simple`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：16。**字节**：405。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Simple UI components for basic rendering.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/simple/image.py`

- **目录**：`src/vanna/components/simple`。**文件名**：`image.py`。**语言/类型**：Python。**行数**：16。**字节**：487。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Simple image component.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `SimpleImageComponent`**（UI 组件模型，基类：SimpleComponent，行 8–15）：A simple image component.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/simple/link.py`

- **目录**：`src/vanna/components/simple`。**文件名**：`link.py`。**语言/类型**：Python。**行数**：16。**字节**：473。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Simple link component.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `SimpleLinkComponent`**（UI 组件模型，基类：SimpleComponent，行 8–15）：A simple link component.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/components/simple/text.py`

- **目录**：`src/vanna/components/simple`。**文件名**：`text.py`。**语言/类型**：Python。**行数**：12。**字节**：341。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Simple text component.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `SimpleTextComponent`**（UI 组件模型，基类：SimpleComponent，行 7–11）：A simple text component.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/__init__.py`

- **目录**：`src/vanna/core`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：194。**字节**：4776。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Core components of the Vanna Agents framework.

This package contains the fundamental abstractions and implementations
that form the foundation of the agent framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/_compat.py`

- **目录**：`src/vanna/core`。**文件名**：`_compat.py`。**语言/类型**：Python。**行数**：20。**字节**：414。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Compatibility shims for different Python versions.

This module provides compatibility utilities for features that vary across
Python versions.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `StrEnum`**（核心类型或辅助类，基类：str, Enum，行 13–16）：Minimal backport of StrEnum for Python < 3.11.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/agent/__init__.py`

- **目录**：`src/vanna/core/agent`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：11。**字节**：187。
- **主要功能简介**：Agent 编排内核，负责把用户消息、工具调用、会话存储和可观测性串成一次完整请求。
- **模块文档字符串**：Agent module.

This module contains the core Agent implementation and configuration.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/agent/agent.py`

- **目录**：`src/vanna/core/agent`。**文件名**：`agent.py`。**语言/类型**：Python。**行数**：1408。**字节**：61689。
- **主要功能简介**：Agent 编排内核，负责把用户消息、工具调用、会话存储和可观测性串成一次完整请求。
- **阅读提示**：Agent 主循环所在，系统最重要的编排文件，任何行为变化都应有测试。
- **模块文档字符串**：Agent implementation for the Vanna Agents framework.

This module provides the main Agent class that orchestrates the interaction
between LLM services, tools, and conversation storage.
- **结构摘要**：类 1 个，模块函数 0 个，方法 7 个。
  - **类 `Agent`**（核心类型或辅助类，基类：无，行 56–1407）：Main agent implementation.  The Agent class orchestrates LLM interactions, tool execution, and conversation management. It provides 7 extensibility points for customization:  - lifecycle_hooks: Hook into message and tool
    - `__init__(llm_service, tool_registry, user_resolver, agent_memory, conversation_store, config, system_prompt_builder, lifecycle_hooks, llm_middlewares, workflow_handler, error_recovery_strategy, context_enrichers, llm_context_enhancer, conversation_filters, observability_provider, audit_logger)`：提供对象协议或生命周期约定。`llm_service`（该函数的业务参数，详见源码类型标注）、`tool_registry`（该函数的业务参数，详见源码类型标注）、`user_resolver`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）、`conversation_store`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）、`system_prompt_builder`（该函数的业务参数，详见源码类型标注）、`lifecycle_hooks`（该函数的业务参数，详见源码类型标注）、`llm_middlewares`（该函数的业务参数，详见源码类型标注）、`workflow_handler`（该函数的业务参数，详见源码类型标注）、`error_recovery_strategy`（该函数的业务参数，详见源码类型标注）、`context_enrichers`（该函数的业务参数，详见源码类型标注）、`llm_context_enhancer`（该函数的业务参数，详见源码类型标注）、`conversation_filters`（该函数的业务参数，详见源码类型标注）、`observability_provider`（该函数的业务参数，详见源码类型标注）、`audit_logger`（该函数的业务参数，详见源码类型标注）。位于第 82–140 行。
    - `async send_message(request_context, message)`：发送到下游。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）、`message`（用户自然语言输入）。Process a user message and yield UI components with error handling.  Args:     request_context: Request context for user位于第 142–229 行。
    - `async _send_message(request_context, message)`：发送到下游。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）、`message`（用户自然语言输入）。Internal method to process a user message and yield UI components.  Args:     request_context: Request context for user 位于第 231–1154 行。
    - `async get_available_tools(user)`：读取并返回。`user`（当前用户对象，携带 id 与 group_memberships）。Get tools available to the user.位于第 1156–1158 行。
    - `async _build_llm_request(conversation, tool_schemas, user, system_prompt)`：组装并构建。`conversation`（会话聚合根）、`tool_schemas`（对当前用户可见的工具 JSON Schema 列表）、`user`（当前用户对象，携带 id 与 group_memberships）、`system_prompt`（系统提示词）。Build LLM request from conversation and tools.位于第 1160–1239 行。
    - `async _send_llm_request(request)`：发送到下游。`request`（下游请求对象）。Send LLM request with middleware and observability.位于第 1241–1313 行。
    - `async _handle_streaming_response(request)`：处理请求或事件。`request`（下游请求对象）。Handle streaming response from LLM.位于第 1315–1407 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/agent/config.py`

- **目录**：`src/vanna/core/agent`。**文件名**：`config.py`。**语言/类型**：Python。**行数**：124。**字节**：4415。
- **主要功能简介**：Agent 编排内核，负责把用户消息、工具调用、会话存储和可观测性串成一次完整请求。
- **模块文档字符串**：Agent configuration.

This module contains configuration models that control agent behavior.
- **结构摘要**：类 4 个，模块函数 0 个，方法 2 个。
  - **类 `UiFeature`**（核心类型或辅助类，基类：StrEnum，行 17–22）：职责见方法列表。
  - **类 `UiFeatures`**（Pydantic 数据模型，基类：BaseModel，行 35–82）：UI features with group-based access control using the same pattern as tools.  Each field specifies which groups can access that UI feature. Empty list means the feature is accessible to all users. Uses the same intersect
    - `can_user_access_feature(feature_name, user)`：判断是否允许。`feature_name`（UI 特性名）、`user`（当前用户对象，携带 id 与 group_memberships）。Check if user can access a UI feature using same logic as tools.  Args:     feature_name: Name of the UI feature to chec位于第 49–73 行。
    - `register_feature(name, access_groups)`：注册到容器。`name`（该函数的业务参数，详见源码类型标注）、`access_groups`（允许访问的用户组）。Register a custom UI feature with group access control.  Args:     name: Name of the custom feature     access_groups: L位于第 75–82 行。
  - **类 `AuditConfig`**（Pydantic 数据模型，基类：BaseModel，行 85–110）：Configuration for audit logging.
  - **类 `AgentConfig`**（Pydantic 数据模型，基类：BaseModel，行 113–123）：Configuration for agent behavior.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/audit/__init__.py`

- **目录**：`src/vanna/core/audit`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：29。**字节**：623。
- **主要功能简介**：审计日志抽象，覆盖工具访问、调用、结果和 UI 特性检查。
- **模块文档字符串**：Audit logging for the Vanna Agents framework.

This module provides interfaces and models for audit logging, enabling
tracking of user actions, tool invocations, and access control decisions.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/audit/base.py`

- **目录**：`src/vanna/core/audit`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：300。**字节**：9726。
- **主要功能简介**：审计日志抽象，覆盖工具访问、调用、结果和 UI 特性检查。
- **模块文档字符串**：Base audit logger interface.

Audit loggers enable tracking user actions, tool invocations, and access control
decisions for security, compliance, and debugging.
- **结构摘要**：类 1 个，模块函数 0 个，方法 8 个。
  - **类 `AuditLogger`**（抽象或具体服务/工具实现，基类：ABC，行 27–299）：Abstract base class for audit logging implementations.  Implementations can: - Write to files (JSON, CSV, etc.) - Send to databases (Postgres, MongoDB, etc.) - Stream to cloud services (CloudWatch, Datadog, etc.) - Send 
    - `async log_event(event)`：记录审计或日志。`event`（该函数的业务参数，详见源码类型标注）。Log a single audit event.  Args:     event: The audit event to log  Raises:     Exception: If logging fails critically位于第 51–60 行。
    - `async log_tool_access_check(user, tool_name, access_granted, required_groups, context, reason)`：记录审计或日志。`user`（当前用户对象，携带 id 与 group_memberships）、`tool_name`（该函数的业务参数，详见源码类型标注）、`access_granted`（该函数的业务参数，详见源码类型标注）、`required_groups`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`reason`（该函数的业务参数，详见源码类型标注）。Convenience method for logging tool access checks.  Args:     user: User attempting to access the tool     tool_name: Na位于第 62–93 行。
    - `async log_tool_invocation(user, tool_call, ui_features, context, sanitize_parameters)`：记录审计或日志。`user`（当前用户对象，携带 id 与 group_memberships）、`tool_call`（LLM 发出的工具调用）、`ui_features`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`sanitize_parameters`（该函数的业务参数，详见源码类型标注）。Convenience method for logging tool invocations.  Args:     user: User invoking the tool     tool_call: Tool call inform位于第 95–131 行。
    - `async log_tool_result(user, tool_call, result, context)`：记录审计或日志。`user`（当前用户对象，携带 id 与 group_memberships）、`tool_call`（LLM 发出的工具调用）、`result`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Convenience method for logging tool results.  Args:     user: User who invoked the tool     tool_call: Tool call informa位于第 133–169 行。
    - `async log_ui_feature_access(user, feature_name, access_granted, required_groups, conversation_id, request_id)`：记录审计或日志。`user`（当前用户对象，携带 id 与 group_memberships）、`feature_name`（UI 特性名）、`access_granted`（该函数的业务参数，详见源码类型标注）、`required_groups`（该函数的业务参数，详见源码类型标注）、`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）。Convenience method for logging UI feature access checks.  Args:     user: User attempting to access the feature     feat位于第 171–201 行。
    - `async log_ai_response(user, conversation_id, request_id, response_text, tool_calls, model_info, include_full_text)`：记录审计或日志。`user`（当前用户对象，携带 id 与 group_memberships）、`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）、`response_text`（该函数的业务参数，详见源码类型标注）、`tool_calls`（该函数的业务参数，详见源码类型标注）、`model_info`（该函数的业务参数，详见源码类型标注）、`include_full_text`（该函数的业务参数，详见源码类型标注）。Convenience method for logging AI responses.  Args:     user: User receiving the response     conversation_id: Conversat位于第 203–241 行。
    - `async query_events(filters, start_time, end_time, limit)`：完成该模块中的具体处理。`filters`（该函数的业务参数，详见源码类型标注）、`start_time`（该函数的业务参数，详见源码类型标注）、`end_time`（该函数的业务参数，详见源码类型标注）、`limit`（返回条数上限）。Query audit events (optional, for implementations that support it).  Args:     filters: Filter criteria (user_id, event_位于第 243–264 行。
    - `_sanitize_parameters(parameters)`：完成该模块中的具体处理。`parameters`（该函数的业务参数，详见源码类型标注）。Sanitize sensitive data from parameters.  Args:     parameters: Raw parameters dict  Returns:     Tuple of (sanitized_pa位于第 266–299 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/audit/models.py`

- **目录**：`src/vanna/core/audit`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：132。**字节**：3614。
- **主要功能简介**：审计日志抽象，覆盖工具访问、调用、结果和 UI 特性检查。
- **模块文档字符串**：Audit event models.

This module contains data models for audit logging events.
- **结构摘要**：类 7 个，模块函数 0 个，方法 0 个。
  - **类 `AuditEventType`**（核心类型或辅助类，基类：StrEnum，行 16–34）：Types of audit events.
  - **类 `AuditEvent`**（Pydantic 数据模型，基类：BaseModel，行 37–60）：Base audit event with common fields.
  - **类 `ToolAccessCheckEvent`**（核心类型或辅助类，基类：AuditEvent，行 63–70）：Audit event for tool access permission checks.
  - **类 `ToolInvocationEvent`**（核心类型或辅助类，基类：AuditEvent，行 73–85）：Audit event for actual tool executions.
  - **类 `ToolResultEvent`**（核心类型或辅助类，基类：AuditEvent，行 88–100）：Audit event for tool execution results.
  - **类 `UiFeatureAccessCheckEvent`**（核心类型或辅助类，基类：AuditEvent，行 103–109）：Audit event for UI feature access checks.
  - **类 `AiResponseEvent`**（核心类型或辅助类，基类：AuditEvent，行 112–131）：Audit event for AI-generated responses.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/component_manager.py`

- **目录**：`src/vanna/core`。**文件名**：`component_manager.py`。**语言/类型**：Python。**行数**：330。**字节**：11311。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Component state management and update protocol for rich components.
- **结构摘要**：类 6 个，模块函数 0 个，方法 21 个。
  - **类 `UpdateOperation`**（核心类型或辅助类，基类：str, Enum，行 15–23）：Types of component update operations.
  - **类 `Position`**（Pydantic 数据模型，基类：BaseModel，行 26–31）：Position specification for component placement.
  - **类 `ComponentUpdate`**（Pydantic 数据模型，基类：BaseModel，行 34–55）：Represents a change to the component tree.
    - `serialize_for_frontend()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Return update payload with nested components normalized.位于第 45–55 行。
  - **类 `ComponentNode`**（Pydantic 数据模型，基类：BaseModel，行 58–90）：Node in the component tree.
    - `find_child(component_id)`：完成该模块中的具体处理。`component_id`（该函数的业务参数，详见源码类型标注）。Find a child node by component ID.位于第 65–73 行。
    - `remove_child(component_id)`：移除。`component_id`（该函数的业务参数，详见源码类型标注）。Remove a child component by ID.位于第 75–83 行。
    - `get_all_ids()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get all component IDs in this subtree.位于第 85–90 行。
  - **类 `ComponentTree`**（Pydantic 数据模型，基类：BaseModel，行 93–208）：Hierarchical structure for managing component layout.
    - `add_component(component, position)`：完成该模块中的具体处理。`component`（该函数的业务参数，详见源码类型标注）、`position`（该函数的业务参数，详见源码类型标注）。Add a component to the tree.位于第 99–119 行。
    - `update_component(component_id, updates)`：更新。`component_id`（该函数的业务参数，详见源码类型标注）、`updates`（该函数的业务参数，详见源码类型标注）。Update a component's properties.位于第 121–143 行。
    - `replace_component(old_id, new_component)`：完成该模块中的具体处理。`old_id`（该函数的业务参数，详见源码类型标注）、`new_component`（该函数的业务参数，详见源码类型标注）。Replace one component with another.位于第 145–162 行。
    - `remove_component(component_id)`：移除。`component_id`（该函数的业务参数，详见源码类型标注）。Remove a component and its children.位于第 164–182 行。
    - `get_component(component_id)`：读取并返回。`component_id`（该函数的业务参数，详见源码类型标注）。Get a component by ID.位于第 184–187 行。
    - `_find_parent(position)`：完成该模块中的具体处理。`position`（该函数的业务参数，详见源码类型标注）。Find the parent node for a new component.位于第 189–208 行。
  - **类 `ComponentManager`**（UI 组件模型，基类：无，行 211–329）：Manages component lifecycle and state updates.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 214–218 行。
    - `emit(component)`：完成该模块中的具体处理。`component`（该函数的业务参数，详见源码类型标注）。Emit a component with smart lifecycle management.位于第 220–247 行。
    - `update_component(component_id)`：更新。`component_id`（该函数的业务参数，详见源码类型标注）。Update specific fields of an existing component.位于第 249–261 行。
    - `replace_component(old_id, new_component)`：完成该模块中的具体处理。`old_id`（该函数的业务参数，详见源码类型标注）、`new_component`（该函数的业务参数，详见源码类型标注）。Replace one component with another.位于第 263–276 行。
    - `remove_component(component_id)`：移除。`component_id`（该函数的业务参数，详见源码类型标注）。Remove a component and handle cleanup.位于第 278–288 行。
    - `get_component(component_id)`：读取并返回。`component_id`（该函数的业务参数，详见源码类型标注）。Get a component by ID.位于第 290–292 行。
    - `get_all_components()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get all components in the manager.位于第 294–296 行。
    - `start_batch()`：启动。仅依赖实例或类自身状态，不额外接收调用方参数。Start a batch of related updates.位于第 298–301 行。
    - `end_batch()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。End the current batch.位于第 303–307 行。
    - `get_updates_since(timestamp)`：读取并返回。`timestamp`（该函数的业务参数，详见源码类型标注）。Get all updates since a given timestamp.位于第 309–325 行。
    - `clear_history()`：清空。仅依赖实例或类自身状态，不额外接收调用方参数。Clear the update history.位于第 327–329 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/components.py`

- **目录**：`src/vanna/core`。**文件名**：`components.py`。**语言/类型**：Python。**行数**：54。**字节**：1868。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：UI component base class.

This module defines the UiComponent class which is the return type for tool executions.
It's placed in core/ because it's a fundamental type that tools return, not just a UI concern.
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `UiComponent`**（Pydantic 数据模型，基类：BaseModel，行 14–53）：Base class for UI components streamed to client.  This wraps both rich and simple component representations, allowing tools to return structured UI updates.  Note: We use Any for component types to avoid circular depende
    - `validate_components()`：校验合法性。仅依赖实例或类自身状态，不额外接收调用方参数。Validate that components are the correct types at runtime.位于第 33–51 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/enhancer/__init__.py`

- **目录**：`src/vanna/core/enhancer`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：12。**字节**：392。
- **主要功能简介**：LLM 上下文增强器，默认从 AgentMemory 检索相关记忆写入系统提示。
- **模块文档字符串**：LLM context enhancement system for adding context to prompts and messages.

This module provides interfaces for enriching LLM system prompts and messages
with additional context before LLM calls (e.g., from memory, RAG, documentation).
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/enhancer/base.py`

- **目录**：`src/vanna/core/enhancer`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：95。**字节**：3094。
- **主要功能简介**：LLM 上下文增强器，默认从 AgentMemory 检索相关记忆写入系统提示。
- **模块文档字符串**：LLM context enhancer interface.

LLM context enhancers allow you to add additional context to the system prompt
and user messages before LLM calls.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `LlmContextEnhancer`**（抽象或具体服务/工具实现，基类：ABC，行 16–94）：Enhancer for adding context to LLM prompts and messages.  Subclass this to create custom enhancers that can: - Add relevant context to the system prompt based on the user's initial message - Enrich user messages with add
    - `async enhance_system_prompt(system_prompt, user_message, user)`：增强提示或消息。`system_prompt`（系统提示词）、`user_message`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）。Enhance the system prompt with additional context.  This method is called before the first LLM request with the initial 位于第 54–73 行。
    - `async enhance_user_messages(messages, user)`：增强提示或消息。`messages`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）。Enhance user messages with additional context.  This method is called to potentially modify or add context to user messa位于第 75–94 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/enhancer/default.py`

- **目录**：`src/vanna/core/enhancer`。**文件名**：`default.py`。**语言/类型**：Python。**行数**：119。**字节**：3988。
- **主要功能简介**：LLM 上下文增强器，默认从 AgentMemory 检索相关记忆写入系统提示。
- **模块文档字符串**：Default LLM context enhancer implementation using AgentMemory.

This implementation enriches the system prompt with relevant memories
based on the user's initial message.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `DefaultLlmContextEnhancer`**（可插拔扩展点实现，基类：LlmContextEnhancer，行 17–118）：Default enhancer that uses AgentMemory to add relevant context.  This enhancer searches the agent's memory for relevant examples and tool use patterns based on the user's message, and adds them to the system prompt.  Exa
    - `__init__(agent_memory)`：提供对象协议或生命周期约定。`agent_memory`（该函数的业务参数，详见源码类型标注）。Initialize with optional agent memory.  Args:     agent_memory: Optional AgentMemory instance. If not provided,         位于第 32–39 行。
    - `async enhance_system_prompt(system_prompt, user_message, user)`：增强提示或消息。`system_prompt`（系统提示词）、`user_message`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）。Enhance system prompt with relevant memories.  Searches agent memory for relevant text memories based on the user's mess位于第 41–101 行。
    - `async enhance_user_messages(messages, user)`：增强提示或消息。`messages`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）。Enhance user messages.  The default implementation doesn't modify user messages. Override this to add context to user me位于第 103–118 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/enricher/__init__.py`

- **目录**：`src/vanna/core/enricher`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：11。**字节**：254。
- **主要功能简介**：工具上下文富集器，可注入租户、数据库方言、文档片段。
- **模块文档字符串**：Context enrichment system for adding data to tool execution context.

This module provides interfaces for enriching ToolContext with additional
data before tool execution.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/enricher/base.py`

- **目录**：`src/vanna/core/enricher`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：60。**字节**：1759。
- **主要功能简介**：工具上下文富集器，可注入租户、数据库方言、文档片段。
- **模块文档字符串**：Base context enricher interface.

Context enrichers allow you to add additional data to the ToolContext
before tools are executed.
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `ToolContextEnricher`**（抽象或具体服务/工具实现，基类：ABC，行 15–59）：Enricher for adding data to ToolContext.  Subclass this to create custom enrichers that can: - Add user preferences from database - Inject session state - Add temporal context (timezone, current date) - Include user hist
    - `async enrich_context(context)`：富集上下文。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Enrich the tool execution context with additional data.  Args:     context: The tool context to enrich  Returns:     Enr位于第 46–59 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/errors.py`

- **目录**：`src/vanna/core`。**文件名**：`errors.py`。**语言/类型**：Python。**行数**：48。**字节**：751。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Exception classes for the Vanna Agents framework.

This module defines all custom exceptions used throughout the framework.
- **结构摘要**：类 7 个，模块函数 0 个，方法 0 个。
  - **类 `AgentError`**（异常类型，基类：Exception，行 8–11）：Base exception for agent framework.
  - **类 `ToolExecutionError`**（异常类型，基类：AgentError，行 14–17）：Error during tool execution.
  - **类 `ToolNotFoundError`**（异常类型，基类：AgentError，行 20–23）：Tool not found in registry.
  - **类 `PermissionError`**（异常类型，基类：AgentError，行 26–29）：User lacks required permissions.
  - **类 `ConversationNotFoundError`**（异常类型，基类：AgentError，行 32–35）：Conversation not found.
  - **类 `LlmServiceError`**（异常类型，基类：AgentError，行 38–41）：Error communicating with LLM service.
  - **类 `ValidationError`**（异常类型，基类：AgentError，行 44–47）：Data validation error.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/evaluation/__init__.py`

- **目录**：`src/vanna/core/evaluation`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：82。**字节**：2053。
- **主要功能简介**：评测框架，含轨迹、输出、LLM-as-Judge、效率四类评估器。
- **模块文档字符串**：Evaluation framework for Vanna Agents.

This module provides a complete evaluation system for testing and comparing
agent variants, with special focus on LLM comparison use cases.

Key Features:
- Parallel execution for efficient I/O-bound operations
- Multiple built-in evaluators (trajectory, output, LLM-as-judge, efficiency)
- Rich reporting (HTML, CSV, console)
- Dataset loaders (YAML, JSON)
- 
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/evaluation/base.py`

- **目录**：`src/vanna/core/evaluation`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：187。**字节**：6041。
- **主要功能简介**：评测框架，含轨迹、输出、LLM-as-Judge、效率四类评估器。
- **模块文档字符串**：Core evaluation abstractions for the Vanna Agents framework.

This module provides the base classes and models for evaluating agent behavior,
including test cases, expected outcomes, and evaluation results.
- **结构摘要**：类 7 个，模块函数 0 个，方法 6 个。
  - **类 `ExpectedOutcome`**（Pydantic 数据模型，基类：BaseModel，行 17–37）：Defines what we expect from the agent for a test case.  Provides multiple ways to specify expectations: - tools_called: List of tool names that should be called - tools_not_called: List of tool names that should NOT be c
  - **类 `TestCase`**（Pydantic 数据模型，基类：BaseModel，行 40–57）：A single evaluation test case.  Attributes:     id: Unique identifier for the test case     user: User context for the test     message: The message to send to the agent     conversation_id: Optional conversation ID for 
  - **类 `AgentResult`**（核心类型或辅助类，基类：无，行 61–94）：The result of running an agent on a test case.  Captures everything that happened during agent execution for later evaluation.
    - `get_final_answer()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Extract the final answer from components.位于第 77–90 行。
    - `get_tool_names_called()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get list of tool names that were called.位于第 92–94 行。
  - **类 `EvaluationResult`**（Pydantic 数据模型，基类：BaseModel，行 97–116）：Result of evaluating a single test case.  Attributes:     test_case_id: ID of the test case evaluated     evaluator_name: Name of the evaluator that produced this result     passed: Whether the test case passed     score
  - **类 `TestCaseResult`**（pytest 测试类，基类：无，行 120–136）：Complete result for a single test case including all evaluations.
    - `overall_passed()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Check if all evaluations passed.位于第 128–130 行。
    - `overall_score()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Calculate average score across all evaluations.位于第 132–136 行。
  - **类 `AgentVariant`**（核心类型或辅助类，基类：无，行 140–154）：A variant of an agent to evaluate (different LLM, config, etc).  Used for comparing different agent configurations, especially different LLMs or model versions.  Attributes:     name: Human-readable name for this variant
  - **类 `Evaluator`**（抽象或具体服务/工具实现，基类：ABC，行 157–186）：Base class for evaluating agent behavior.  Evaluators examine the agent's execution and determine if it met expectations. Multiple evaluators can be composed to check different aspects (trajectory, output quality, effici
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Name of this evaluator.位于第 167–169 行。
    - `async evaluate(test_case, agent_result)`：完成该模块中的具体处理。`test_case`（评测用例）、`agent_result`（一次 Agent 运行的采集结果）。Evaluate a single test case execution.  Args:     test_case: The test case that was executed     agent_result: The resul位于第 172–186 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/evaluation/dataset.py`

- **目录**：`src/vanna/core/evaluation`。**文件名**：`dataset.py`。**语言/类型**：Python。**行数**：255。**字节**：8074。
- **主要功能简介**：评测框架，含轨迹、输出、LLM-as-Judge、效率四类评估器。
- **模块文档字符串**：Dataset loaders for evaluation test cases.

This module provides utilities for loading test case datasets from
YAML and JSON files.
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `EvaluationDataset`**（核心类型或辅助类，基类：无，行 17–254）：Collection of test cases with metadata.  Example YAML format:     dataset:       name: "SQL Generation Tasks"       description: "Test cases for SQL generation"       test_cases:         - id: "sql_001"           user_id
    - `__init__(name, test_cases, description)`：提供对象协议或生命周期约定。`name`（该函数的业务参数，详见源码类型标注）、`test_cases`（该函数的业务参数，详见源码类型标注）、`description`（该函数的业务参数，详见源码类型标注）。Initialize evaluation dataset.  Args:     name: Name of the dataset     test_cases: List of test cases     description: 位于第 33–43 行。
    - `from_yaml(path)`：从外部结构转换而来。`path`（该函数的业务参数，详见源码类型标注）。Load dataset from YAML file.  Args:     path: Path to YAML file  Returns:     EvaluationDataset instance位于第 46–58 行。
    - `from_json(path)`：从外部结构转换而来。`path`（该函数的业务参数，详见源码类型标注）。Load dataset from JSON file.  Args:     path: Path to JSON file  Returns:     EvaluationDataset instance位于第 61–73 行。
    - `_from_dict(data)`：从外部结构转换而来。`data`（该函数的业务参数，详见源码类型标注）。Create dataset from dictionary.  Args:     data: Dictionary with dataset structure  Returns:     EvaluationDataset insta位于第 76–94 行。
    - `_parse_test_case(data)`：解析输入。`data`（该函数的业务参数，详见源码类型标注）。Parse a single test case from dictionary.  Args:     data: Test case dictionary  Returns:     TestCase instance位于第 97–137 行。
    - `save_yaml(path)`：持久化保存。`path`（该函数的业务参数，详见源码类型标注）。Save dataset to YAML file.  Args:     path: Path to save YAML file位于第 139–147 行。
    - `save_json(path)`：持久化保存。`path`（该函数的业务参数，详见源码类型标注）。Save dataset to JSON file.  Args:     path: Path to save JSON file位于第 149–157 行。
    - `_to_dict()`：转换为目标结构。仅依赖实例或类自身状态，不额外接收调用方参数。Convert dataset to dictionary.  Returns:     Dictionary representation位于第 159–171 行。
    - `_test_case_to_dict(test_case)`：完成该模块中的具体处理。`test_case`（评测用例）。Convert test case to dictionary.  Args:     test_case: TestCase to convert  Returns:     Dictionary representation位于第 173–223 行。
    - `filter_by_metadata()`：过滤。仅依赖实例或类自身状态，不额外接收调用方参数。Filter test cases by metadata fields.  Args:     **kwargs: Metadata fields to match  Returns:     New EvaluationDataset 位于第 225–244 行。
    - `__len__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。Get number of test cases.位于第 246–248 行。
    - `__repr__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。String representation.位于第 250–254 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/evaluation/evaluators.py`

- **目录**：`src/vanna/core/evaluation`。**文件名**：`evaluators.py`。**语言/类型**：Python。**行数**：377。**字节**：12574。
- **主要功能简介**：评测框架，含轨迹、输出、LLM-as-Judge、效率四类评估器。
- **模块文档字符串**：Built-in evaluators for common evaluation tasks.

This module provides ready-to-use evaluators for:
- Trajectory evaluation (tools called, order, efficiency)
- Output evaluation (content matching, quality)
- LLM-as-judge evaluation (custom criteria)
- Efficiency evaluation (time, tokens, cost)
- **结构摘要**：类 4 个，模块函数 0 个，方法 13 个。
  - **类 `TrajectoryEvaluator`**（核心类型或辅助类，基类：Evaluator，行 18–90）：Evaluate the path the agent took (tools called, order, etc).  Checks if the agent called the expected tools and didn't call unexpected ones. Useful for verifying agent reasoning and planning.
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 26–27 行。
    - `async evaluate(test_case, agent_result)`：完成该模块中的具体处理。`test_case`（评测用例）、`agent_result`（一次 Agent 运行的采集结果）。Evaluate tool call trajectory.位于第 29–90 行。
  - **类 `OutputEvaluator`**（核心类型或辅助类，基类：Evaluator，行 93–168）：Evaluate the final output quality.  Checks if the output contains expected content and doesn't contain forbidden content. Case-insensitive substring matching.
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 101–102 行。
    - `async evaluate(test_case, agent_result)`：完成该模块中的具体处理。`test_case`（评测用例）、`agent_result`（一次 Agent 运行的采集结果）。Evaluate output content.位于第 104–168 行。
  - **类 `LLMAsJudgeEvaluator`**（核心类型或辅助类，基类：Evaluator，行 171–289）：Use an LLM to judge agent performance based on custom criteria.  This evaluator uses a separate LLM to assess the quality of the agent's output based on natural language criteria.
    - `__init__(judge_llm, criteria)`：提供对象协议或生命周期约定。`judge_llm`（该函数的业务参数，详见源码类型标注）、`criteria`（该函数的业务参数，详见源码类型标注）。Initialize LLM-as-judge evaluator.  Args:     judge_llm: The LLM service to use for judging     criteria: Natural langua位于第 178–186 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 189–190 行。
    - `async evaluate(test_case, agent_result)`：完成该模块中的具体处理。`test_case`（评测用例）、`agent_result`（一次 Agent 运行的采集结果）。Evaluate using LLM as judge.位于第 192–263 行。
    - `_parse_score(judgment)`：解析输入。`judgment`（该函数的业务参数，详见源码类型标注）。Parse score from judge response.位于第 265–274 行。
    - `_parse_passed(judgment)`：解析输入。`judgment`（该函数的业务参数，详见源码类型标注）。Parse pass/fail from judge response.位于第 276–282 行。
    - `_parse_reasoning(judgment)`：解析输入。`judgment`（该函数的业务参数，详见源码类型标注）。Parse reasoning from judge response.位于第 284–289 行。
  - **类 `EfficiencyEvaluator`**（核心类型或辅助类，基类：Evaluator，行 292–376）：Evaluate resource usage (time, tokens, cost).  Checks if the agent completed within acceptable resource limits.
    - `__init__(max_execution_time_ms, max_tokens, max_cost_usd)`：提供对象协议或生命周期约定。`max_execution_time_ms`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）、`max_cost_usd`（该函数的业务参数，详见源码类型标注）。Initialize efficiency evaluator.  Args:     max_execution_time_ms: Maximum allowed execution time in milliseconds     ma位于第 298–313 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 316–317 行。
    - `async evaluate(test_case, agent_result)`：完成该模块中的具体处理。`test_case`（评测用例）、`agent_result`（一次 Agent 运行的采集结果）。Evaluate resource efficiency.位于第 319–376 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/evaluation/report.py`

- **目录**：`src/vanna/core/evaluation`。**文件名**：`report.py`。**语言/类型**：Python。**行数**：290。**字节**：10626。
- **主要功能简介**：评测框架，含轨迹、输出、LLM-as-Judge、效率四类评估器。
- **模块文档字符串**：Evaluation reporting with HTML, CSV, and console output.

This module provides classes for generating evaluation reports,
including comparison reports for evaluating multiple agent variants.
- **结构摘要**：类 2 个，模块函数 0 个，方法 11 个。
  - **类 `EvaluationReport`**（核心类型或辅助类，基类：无，行 17–85）：Report for a single agent's evaluation results.  Attributes:     agent_name: Name of the agent evaluated     results: List of results for each test case     evaluators: List of evaluators used     metadata: Additional me
    - `pass_rate()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Calculate overall pass rate (0.0 to 1.0).位于第 34–39 行。
    - `average_score()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Calculate average score across all test cases.位于第 41–45 行。
    - `average_time()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Calculate average execution time in milliseconds.位于第 47–51 行。
    - `total_tokens()`：转换为目标结构。仅依赖实例或类自身状态，不额外接收调用方参数。Calculate total tokens used across all test cases.位于第 53–55 行。
    - `get_failures()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get all failed test cases.位于第 57–59 行。
    - `print_summary()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Print summary to console.位于第 61–85 行。
  - **类 `ComparisonReport`**（核心类型或辅助类，基类：无，行 89–289）：Report comparing multiple agent variants.  This is the primary report type for LLM comparison use cases.  Attributes:     variants: List of agent variants compared     reports: Dict mapping variant name to EvaluationRepo
    - `print_summary()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Print comparison summary to console.位于第 106–130 行。
    - `get_best_variant(metric)`：读取并返回。`metric`（该函数的业务参数，详见源码类型标注）。Get the best performing variant by metric.  Args:     metric: Metric to optimize ('score', 'speed', 'pass_rate')  Return位于第 132–148 行。
    - `save_csv(path)`：持久化保存。`path`（该函数的业务参数，详见源码类型标注）。Save detailed CSV for further analysis.  Each row represents one test case × one variant combination.位于第 150–192 行。
    - `save_html(path)`：持久化保存。`path`（该函数的业务参数，详见源码类型标注）。Save interactive HTML comparison report.  Generates a rich HTML report with: - Summary statistics - Charts comparing var位于第 194–204 行。
    - `_generate_html()`：生成。仅依赖实例或类自身状态，不额外接收调用方参数。Generate HTML content for report.位于第 206–289 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/evaluation/runner.py`

- **目录**：`src/vanna/core/evaluation`。**文件名**：`runner.py`。**语言/类型**：Python。**行数**：314。**字节**：10108。
- **主要功能简介**：评测框架，含轨迹、输出、LLM-as-Judge、效率四类评估器。
- **模块文档字符串**：Evaluation runner with parallel execution support.

This module provides the EvaluationRunner class that executes test cases
against agents with configurable parallelism for efficient evaluation,
especially when comparing multiple LLMs or model versions.
- **结构摘要**：类 1 个，模块函数 0 个，方法 8 个。
  - **类 `EvaluationRunner`**（抽象或具体服务/工具实现，基类：无，行 29–313）：Run evaluations with parallel execution support.  The primary use case is comparing multiple agent variants (e.g., different LLMs) on the same set of test cases. The runner executes test cases in parallel with configurab
    - `__init__(evaluators, max_concurrency, observability_provider)`：提供对象协议或生命周期约定。`evaluators`（该函数的业务参数，详见源码类型标注）、`max_concurrency`（该函数的业务参数，详见源码类型标注）、`observability_provider`（该函数的业务参数，详见源码类型标注）。Initialize the evaluation runner.  Args:     evaluators: List of evaluators to apply to each test case     max_concurren位于第 47–63 行。
    - `async run_evaluation(agent, test_cases)`：运行。`agent`（Agent 实例）、`test_cases`（该函数的业务参数，详见源码类型标注）。Run evaluation on a single agent.  Args:     agent: The agent to evaluate     test_cases: List of test cases to run  Ret位于第 65–87 行。
    - `async compare_agents(agent_variants, test_cases)`：完成该模块中的具体处理。`agent_variants`（该函数的业务参数，详见源码类型标注）、`test_cases`（该函数的业务参数，详见源码类型标注）。Compare multiple agent variants on same test cases.  This is the PRIMARY use case for LLM comparison. Runs all variants 位于第 89–133 行。
    - `async compare_agents_streaming(agent_variants, test_cases)`：完成该模块中的具体处理。`agent_variants`（该函数的业务参数，详见源码类型标注）、`test_cases`（该函数的业务参数，详见源码类型标注）。Stream comparison results as they complete.  Useful for long-running evaluations where you want to see progress updates 位于第 135–173 行。
    - `async _run_agent_variant(variant, test_cases)`：运行。`variant`（该函数的业务参数，详见源码类型标注）、`test_cases`（该函数的业务参数，详见源码类型标注）。Run a single agent variant on all test cases.  Args:     variant: The agent variant to evaluate     test_cases: Test cas位于第 175–212 行。
    - `async _run_test_cases_parallel(agent, test_cases)`：运行。`agent`（Agent 实例）、`test_cases`（该函数的业务参数，详见源码类型标注）。Run test cases in parallel with concurrency limit.  Args:     agent: The agent to run test cases on     test_cases: Test位于第 214–232 行。
    - `async _run_single_test_case(agent, test_case)`：运行。`agent`（Agent 实例）、`test_case`（评测用例）。Run a single test case with semaphore to limit concurrency.  Args:     agent: The agent to execute     test_case: The te位于第 234–265 行。
    - `async _execute_agent(agent, test_case)`：执行业务逻辑。`agent`（Agent 实例）、`test_case`（评测用例）。Execute agent and capture full trajectory.  Args:     agent: The agent to execute     test_case: The test case to run  R位于第 267–313 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/filter/__init__.py`

- **目录**：`src/vanna/core/filter`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：11。**字节**：259。
- **主要功能简介**：会话过滤器，在送入 LLM 前裁剪或脱敏历史消息。
- **模块文档字符串**：Conversation filtering system for managing conversation history.

This module provides interfaces for filtering and transforming conversation
history before it's sent to the LLM.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/filter/base.py`

- **目录**：`src/vanna/core/filter`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：68。**字节**：2035。
- **主要功能简介**：会话过滤器，在送入 LLM 前裁剪或脱敏历史消息。
- **模块文档字符串**：Base conversation filter interface.

Conversation filters allow you to transform conversation history before
it's sent to the LLM for processing.
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `ConversationFilter`**（抽象或具体服务/工具实现，基类：ABC，行 15–67）：Filter for transforming conversation history.  Subclass this to create custom filters that can: - Remove sensitive information - Summarize long conversations - Manage context window limits - Deduplicate similar messages 
    - `async filter_messages(messages)`：过滤。`messages`（该函数的业务参数，详见源码类型标注）。Filter and transform conversation messages.  Args:     messages: List of conversation messages  Returns:     Filtered/tr位于第 54–67 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/lifecycle/__init__.py`

- **目录**：`src/vanna/core/lifecycle`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：11。**字节**：233。
- **主要功能简介**：生命周期钩子，可在消息前后、工具前后插入配额、过滤、日志。
- **模块文档字符串**：Lifecycle hook system for agent execution.

This module provides hooks for intercepting and modifying agent behavior
at various points in the execution lifecycle.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/lifecycle/base.py`

- **目录**：`src/vanna/core/lifecycle`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：84。**字节**：2292。
- **主要功能简介**：生命周期钩子，可在消息前后、工具前后插入配额、过滤、日志。
- **模块文档字符串**：Base lifecycle hook interface.

Lifecycle hooks allow you to intercept and customize agent behavior
at key points in the execution flow.
- **结构摘要**：类 1 个，模块函数 0 个，方法 4 个。
  - **类 `LifecycleHook`**（抽象或具体服务/工具实现，基类：ABC，行 17–83）：Hook into agent execution lifecycle.  Subclass this to create custom hooks that can: - Modify messages before processing - Add logging or telemetry - Enforce quotas or rate limits - Transform tool results - Add custom va
    - `async before_message(user, message)`：完成该模块中的具体处理。`user`（当前用户对象，携带 id 与 group_memberships）、`message`（用户自然语言输入）。Called before processing a user message.  Args:     user: User sending the message     message: Original message content位于第 39–52 行。
    - `async after_message(result)`：完成该模块中的具体处理。`result`（该函数的业务参数，详见源码类型标注）。Called after message has been fully processed.  Args:     result: Final result from message processing位于第 54–60 行。
    - `async before_tool(tool, context)`：完成该模块中的具体处理。`tool`（工具实例）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Called before tool execution.  Args:     tool: Tool about to be executed     context: Tool execution context  Raises:   位于第 62–72 行。
    - `async after_tool(result)`：完成该模块中的具体处理。`result`（该函数的业务参数，详见源码类型标注）。Called after tool execution.  Args:     result: Result from tool execution  Returns:     Modified ToolResult, or None to位于第 74–83 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/llm/__init__.py`

- **目录**：`src/vanna/core/llm`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：17。**字节**：324。
- **主要功能简介**：大模型通信的请求/响应/流式分片模型与服务接口。
- **模块文档字符串**：LLM domain.

This module provides the core abstractions for LLM services in the Vanna Agents framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/llm/base.py`

- **目录**：`src/vanna/core/llm`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：41。**字节**：1067。
- **主要功能简介**：大模型通信的请求/响应/流式分片模型与服务接口。
- **模块文档字符串**：LLM domain interface.

This module contains the abstract base class for LLM services.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `LlmService`**（抽象或具体服务/工具实现，基类：ABC，行 13–40）：Service for LLM communication.
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Send a request to the LLM.位于第 17–19 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Stream a request to the LLM.  Args:     request: The LLM request to stream  Yields:     LlmStreamChunk instances as they位于第 22–35 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Validate tool schemas and return any errors.位于第 38–40 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/llm/models.py`

- **目录**：`src/vanna/core/llm`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：62。**字节**：1962。
- **主要功能简介**：大模型通信的请求/响应/流式分片模型与服务接口。
- **模块文档字符串**：LLM domain models.

This module contains data models for LLM communication.
- **结构摘要**：类 4 个，模块函数 0 个，方法 1 个。
  - **类 `LlmMessage`**（Pydantic 数据模型，基类：BaseModel，行 15–21）：Message format for LLM communication.
  - **类 `LlmRequest`**（Pydantic 数据模型，基类：BaseModel，行 24–38）：Request to LLM service.
  - **类 `LlmResponse`**（Pydantic 数据模型，基类：BaseModel，行 41–52）：Response from LLM.
    - `is_tool_call()`：判断是否满足条件。仅依赖实例或类自身状态，不额外接收调用方参数。Check if this response contains tool calls.位于第 50–52 行。
  - **类 `LlmStreamChunk`**（Pydantic 数据模型，基类：BaseModel，行 55–61）：Streaming chunk from LLM.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/middleware/__init__.py`

- **目录**：`src/vanna/core/middleware`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：11。**字节**：233。
- **主要功能简介**：LLM 中间件，可缓存、改写提示词、记录成本。
- **模块文档字符串**：Middleware system for LLM request/response interception.

This module provides middleware interfaces for intercepting and transforming
LLM requests and responses.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/middleware/base.py`

- **目录**：`src/vanna/core/middleware`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：70。**字节**：1985。
- **主要功能简介**：LLM 中间件，可缓存、改写提示词、记录成本。
- **模块文档字符串**：Base LLM middleware interface.

Middleware allows you to intercept and transform LLM requests and responses
for caching, monitoring, content filtering, and more.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `LlmMiddleware`**（抽象或具体服务/工具实现，基类：ABC，行 15–69）：Middleware for intercepting LLM requests and responses.  Subclass this to create custom middleware that can: - Cache LLM responses - Log requests/responses - Filter or modify content - Track costs and usage - Implement f
    - `async before_llm_request(request)`：完成该模块中的具体处理。`request`（下游请求对象）。Called before sending request to LLM.  Args:     request: The LLM request about to be sent  Returns:     Modified reques位于第 46–55 行。
    - `async after_llm_response(request, response)`：完成该模块中的具体处理。`request`（下游请求对象）、`response`（下游响应对象）。Called after receiving response from LLM.  Args:     request: The original request     response: The LLM response  Retur位于第 57–69 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/observability/__init__.py`

- **目录**：`src/vanna/core/observability`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：12。**字节**：284。
- **主要功能简介**：Span 与 Metric 抽象，用于追踪一次 Agent 调用的耗时与错误。
- **模块文档字符串**：Observability system for telemetry and monitoring.

This module provides interfaces for collecting metrics, traces, and
monitoring agent behavior.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/observability/base.py`

- **目录**：`src/vanna/core/observability`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：89。**字节**：2557。
- **主要功能简介**：Span 与 Metric 抽象，用于追踪一次 Agent 调用的耗时与错误。
- **模块文档字符串**：Base observability provider interface.

Observability providers allow you to collect telemetry data about
agent execution for monitoring and debugging.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `ObservabilityProvider`**（抽象或具体服务/工具实现，基类：ABC，行 14–88）：Provider for collecting telemetry and observability data.  Subclass this to create custom observability integrations that can: - Emit metrics to monitoring systems - Create distributed traces - Log performance data - Tra
    - `async record_metric(name, value, unit, tags)`：完成该模块中的具体处理。`name`（该函数的业务参数，详见源码类型标注）、`value`（该函数的业务参数，详见源码类型标注）、`unit`（该函数的业务参数，详见源码类型标注）、`tags`（指标标签）。Record a metric measurement.  Args:     name: Metric name (e.g., "agent.request.duration")     value: Metric value     u位于第 48–63 行。
    - `async create_span(name, attributes)`：创建并初始化。`name`（该函数的业务参数，详见源码类型标注）、`attributes`（该函数的业务参数，详见源码类型标注）。Create a new span for tracing.  Args:     name: Span name/operation     attributes: Initial span attributes  Returns:   位于第 65–80 行。
    - `async end_span(span)`：完成该模块中的具体处理。`span`（可观测性 Span）。End a span and record it.  Args:     span: The span to end位于第 82–88 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/observability/models.py`

- **目录**：`src/vanna/core/observability`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：48。**字节**：1638。
- **主要功能简介**：Span 与 Metric 抽象，用于追踪一次 Agent 调用的耗时与错误。
- **模块文档字符串**：Observability models for spans and metrics.
- **结构摘要**：类 2 个，模块函数 0 个，方法 3 个。
  - **类 `Span`**（Pydantic 数据模型，基类：BaseModel，行 12–37）：Represents a unit of work for distributed tracing.
    - `end()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Mark span as ended.位于第 24–27 行。
    - `duration_ms()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Get span duration in milliseconds.位于第 29–33 行。
    - `set_attribute(key, value)`：设置并更新。`key`（该函数的业务参数，详见源码类型标注）、`value`（该函数的业务参数，详见源码类型标注）。Set a span attribute.位于第 35–37 行。
  - **类 `Metric`**（Pydantic 数据模型，基类：BaseModel，行 40–47）：Represents a metric measurement.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/recovery/__init__.py`

- **目录**：`src/vanna/core/recovery`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：12。**字节**：335。
- **主要功能简介**：错误恢复策略，决定重试、放弃或改写请求。
- **模块文档字符串**：Error recovery system for handling failures gracefully.

This module provides interfaces for custom error handling, retry logic,
and fallback strategies.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/recovery/base.py`

- **目录**：`src/vanna/core/recovery`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：85。**字节**：2736。
- **主要功能简介**：错误恢复策略，决定重试、放弃或改写请求。
- **模块文档字符串**：Base error recovery strategy interface.

Recovery strategies allow you to customize how the agent handles errors
during tool execution and LLM communication.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `ErrorRecoveryStrategy`**（抽象或具体服务/工具实现，基类：ABC，行 18–84）：Strategy for handling errors and implementing retry logic.  Subclass this to create custom error recovery strategies that can: - Retry failed operations with backoff - Fallback to alternative approaches - Log errors to e
    - `async handle_tool_error(error, context, attempt)`：处理请求或事件。`error`（异常对象）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`attempt`（该函数的业务参数，详见源码类型标注）。Handle errors during tool execution.  Args:     error: The exception that occurred     context: Tool execution context  位于第 50–66 行。
    - `async handle_llm_error(error, request, attempt)`：处理请求或事件。`error`（异常对象）、`request`（下游请求对象）、`attempt`（该函数的业务参数，详见源码类型标注）。Handle errors during LLM communication.  Args:     error: The exception that occurred     request: The LLM request that 位于第 68–84 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/recovery/models.py`

- **目录**：`src/vanna/core/recovery`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：33。**字节**：811。
- **主要功能简介**：错误恢复策略，决定重试、放弃或改写请求。
- **模块文档字符串**：Recovery action models for error handling.
- **结构摘要**：类 2 个，模块函数 0 个，方法 0 个。
  - **类 `RecoveryActionType`**（核心类型或辅助类，基类：str, Enum，行 11–17）：Types of recovery actions.
  - **类 `RecoveryAction`**（Pydantic 数据模型，基类：BaseModel，行 20–32）：Action to take when recovering from an error.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/registry.py`

- **目录**：`src/vanna/core`。**文件名**：`registry.py`。**语言/类型**：Python。**行数**：279。**字节**：9469。
- **主要功能简介**：工具注册中心，负责权限校验、参数校验、参数变换、审计与执行。
- **阅读提示**：工具权限、校验、transform_args、审计与 execute 的中枢。
- **模块文档字符串**：Tool registry for the Vanna Agents framework.

This module provides the ToolRegistry class for managing and executing tools.
- **结构摘要**：类 2 个，模块函数 0 个，方法 14 个。
  - **类 `_LocalToolWrapper`**（抽象或具体服务/工具实现，基类：Tool[T]，行 20–43）：Wrapper for tools with configurable access groups.
    - `__init__(wrapped_tool, access_groups)`：提供对象协议或生命周期约定。`wrapped_tool`（该函数的业务参数，详见源码类型标注）、`access_groups`（允许访问的用户组）。位于第 23–25 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 28–29 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 32–33 行。
    - `access_groups()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 36–37 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 39–40 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 42–43 行。
  - **类 `ToolRegistry`**（核心类型或辅助类，基类：无，行 46–278）：Registry for managing tools.
    - `__init__(audit_logger, audit_config)`：提供对象协议或生命周期约定。`audit_logger`（该函数的业务参数，详见源码类型标注）、`audit_config`（该函数的业务参数，详见源码类型标注）。位于第 49–61 行。
    - `register_local_tool(tool, access_groups)`：注册到容器。`tool`（工具实例）、`access_groups`（允许访问的用户组）。Register a local tool with optional access group restrictions.  Args:     tool: The tool to register     access_groups: 位于第 63–80 行。
    - `async get_tool(name)`：读取并返回。`name`（该函数的业务参数，详见源码类型标注）。Get a tool by name.位于第 82–84 行。
    - `async list_tools()`：枚举并列出。仅依赖实例或类自身状态，不额外接收调用方参数。List all registered tool names.位于第 86–88 行。
    - `async get_schemas(user)`：读取并返回。`user`（当前用户对象，携带 id 与 group_memberships）。Get schemas for all tools accessible to user.位于第 90–96 行。
    - `async _validate_tool_permissions(tool, user)`：校验合法性。`tool`（工具实例）、`user`（当前用户对象，携带 id 与 group_memberships）。Validate if user has access to tool based on group membership.  Checks for intersection between user's group memberships位于第 98–111 行。
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Transform and validate tool arguments based on user context.  This method allows per-user transformation of tool argumen位于第 113–142 行。
    - `async execute(tool_call, context)`：执行业务逻辑。`tool_call`（LLM 发出的工具调用）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute a tool call with validation.位于第 144–278 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/rich_component.py`

- **目录**：`src/vanna/core`。**文件名**：`rich_component.py`。**语言/类型**：Python。**行数**：157。**字节**：4868。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Base classes for rich UI components.

This module provides the base RichComponent class and supporting enums
for the component system.
- **结构摘要**：类 3 个，模块函数 0 个，方法 4 个。
  - **类 `ComponentType`**（UI 组件模型，基类：str, Enum，行 19–60）：Types of rich UI components.
  - **类 `ComponentLifecycle`**（UI 组件模型，基类：str, Enum，行 63–69）：Component lifecycle operations.
  - **类 `RichComponent`**（Pydantic 数据模型，基类：BaseModel，行 72–156）：Base class for all rich UI components.
    - `update()`：更新。仅依赖实例或类自身状态，不额外接收调用方参数。Create an updated copy of this component.位于第 84–90 行。
    - `hide()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Create a hidden copy of this component.位于第 92–94 行。
    - `show()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Create a visible copy of this component.位于第 96–98 行。
    - `serialize_for_frontend()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Normalize component payload for the frontend renderer.  The frontend expects component-specific fields to live under the位于第 100–156 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/simple_component.py`

- **目录**：`src/vanna/core`。**文件名**：`simple_component.py`。**语言/类型**：Python。**行数**：28。**字节**：820。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Base classes for simple UI components.
- **结构摘要**：类 2 个，模块函数 0 个，方法 1 个。
  - **类 `SimpleComponentType`**（UI 组件模型，基类：str, Enum，行 8–11）：职责见方法列表。
  - **类 `SimpleComponent`**（Pydantic 数据模型，基类：BaseModel，行 14–27）：A simple UI component with basic attributes.
    - `serialize_for_frontend()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Serialize simple component for API consumption.位于第 25–27 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/storage/__init__.py`

- **目录**：`src/vanna/core/storage`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：15。**字节**：278。
- **主要功能简介**：会话与消息持久化抽象。
- **模块文档字符串**：Storage domain.

This module provides the core abstractions for conversation storage in the Vanna Agents framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/storage/base.py`

- **目录**：`src/vanna/core/storage`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：47。**字节**：1269。
- **主要功能简介**：会话与消息持久化抽象。
- **模块文档字符串**：Storage domain interface.

This module contains the abstract base class for conversation storage.
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `ConversationStore`**（抽象或具体服务/工具实现，基类：ABC，行 14–46）：Abstract base class for conversation storage.
    - `async create_conversation(conversation_id, user, initial_message)`：创建并初始化。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）、`initial_message`（该函数的业务参数，详见源码类型标注）。Create a new conversation with the specified ID.位于第 18–22 行。
    - `async get_conversation(conversation_id, user)`：读取并返回。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）。Get conversation by ID, scoped to user.位于第 25–29 行。
    - `async update_conversation(conversation)`：更新。`conversation`（会话聚合根）。Update conversation with new messages.位于第 32–34 行。
    - `async delete_conversation(conversation_id, user)`：删除并清理。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）。Delete conversation.位于第 37–39 行。
    - `async list_conversations(user, limit, offset)`：枚举并列出。`user`（当前用户对象，携带 id 与 group_memberships）、`limit`（返回条数上限）、`offset`（该函数的业务参数，详见源码类型标注）。List conversations for user.位于第 42–46 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/storage/models.py`

- **目录**：`src/vanna/core/storage`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：47。**字节**：1556。
- **主要功能简介**：会话与消息持久化抽象。
- **模块文档字符串**：Storage domain models.

This module contains data models for conversation storage.
- **结构摘要**：类 2 个，模块函数 0 个，方法 1 个。
  - **类 `Message`**（Pydantic 数据模型，基类：BaseModel，行 16–26）：Single message in a conversation.
  - **类 `Conversation`**（Pydantic 数据模型，基类：BaseModel，行 29–46）：Conversation containing multiple messages.
    - `add_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。Add a message to the conversation.位于第 43–46 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/system_prompt/__init__.py`

- **目录**：`src/vanna/core/system_prompt`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：14。**字节**：296。
- **主要功能简介**：系统提示词构建器。
- **模块文档字符串**：System prompt domain.

This module provides the core abstractions for building system prompts in the Vanna Agents framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/system_prompt/base.py`

- **目录**：`src/vanna/core/system_prompt`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：37。**字节**：986。
- **主要功能简介**：系统提示词构建器。
- **模块文档字符串**：System prompt builder interface.

This module contains the abstract base class for system prompt builders.
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `SystemPromptBuilder`**（抽象或具体服务/工具实现，基类：ABC，行 15–36）：Abstract base class for system prompt builders.  Subclasses should implement the build_system_prompt method to generate system prompts based on user context and available tools.
    - `async build_system_prompt(user, tools)`：组装并构建。`user`（当前用户对象，携带 id 与 group_memberships）、`tools`（该函数的业务参数，详见源码类型标注）。Build a system prompt based on user context and available tools.  Args:     user: The user making the request     tools:位于第 23–36 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/system_prompt/default.py`

- **目录**：`src/vanna/core/system_prompt`。**文件名**：`default.py`。**语言/类型**：Python。**行数**：158。**字节**：6926。
- **主要功能简介**：系统提示词构建器。
- **模块文档字符串**：Default system prompt builder implementation with memory workflow support.

This module provides a default implementation of the SystemPromptBuilder interface
that automatically includes memory workflow instructions when memory tools are available.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `DefaultSystemPromptBuilder`**（核心类型或辅助类，基类：SystemPromptBuilder，行 18–157）：Default system prompt builder with automatic memory workflow integration.  Dynamically generates system prompts that include memory workflow instructions when memory tools (search_saved_correct_tool_uses and save_questio
    - `__init__(base_prompt)`：提供对象协议或生命周期约定。`base_prompt`（该函数的业务参数，详见源码类型标注）。Initialize with an optional base prompt.  Args:     base_prompt: Optional base system prompt. If not provided, uses a de位于第 26–32 行。
    - `async build_system_prompt(user, tools)`：组装并构建。`user`（当前用户对象，携带 id 与 group_memberships）、`tools`（该函数的业务参数，详见源码类型标注）。Build a system prompt with memory workflow instructions.  Args:     user: The user making the request     tools: List of位于第 34–157 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/tool/__init__.py`

- **目录**：`src/vanna/core/tool`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：19。**字节**：342。
- **主要功能简介**：工具领域模型与抽象基类，定义 LLM 可调用的工具契约。
- **模块文档字符串**：Tool domain.

This module provides the core abstractions for tools in the Vanna Agents framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/tool/base.py`

- **目录**：`src/vanna/core/tool`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：71。**字节**：1871。
- **主要功能简介**：工具领域模型与抽象基类，定义 LLM 可调用的工具契约。
- **模块文档字符串**：Tool domain interface.

This module contains the abstract base class for tools.
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `Tool`**（抽象或具体服务/工具实现，基类：ABC, Generic[T]，行 16–70）：Abstract base class for tools.
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Unique name for this tool.位于第 21–23 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Description of what this tool does.位于第 27–29 行。
    - `access_groups()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Groups permitted to access this tool.位于第 32–34 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Return the Pydantic model for arguments.位于第 37–39 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Execute the tool with validated arguments.  Args:     context: Execution context containing user, conversation_id, and r位于第 42–52 行。
    - `get_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Generate tool schema for LLM.位于第 54–70 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/tool/models.py`

- **目录**：`src/vanna/core/tool`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：85。**字节**：2790。
- **主要功能简介**：工具领域模型与抽象基类，定义 LLM 可调用的工具契约。
- **模块文档字符串**：Tool domain models.

This module contains data models for tool execution.
- **结构摘要**：类 6 个，模块函数 0 个，方法 0 个。
  - **类 `ToolCall`**（Pydantic 数据模型，基类：BaseModel，行 20–25）：Represents a tool call from the LLM.
  - **类 `ToolContext`**（Pydantic 数据模型，基类：BaseModel，行 28–44）：Context passed to all tool executions.
  - **类 `ToolResult`**（Pydantic 数据模型，基类：BaseModel，行 47–61）：Result from tool execution.  Changes: - `result_for_llm`: string that will be sent back to the LLM. - `ui_component`: optional UI payload for rendering in clients.
  - **类 `ToolSchema`**（Pydantic 数据模型，基类：BaseModel，行 64–72）：Schema describing a tool for LLM consumption.
  - **类 `ToolRejection`**（Pydantic 数据模型，基类：BaseModel，行 75–84）：Indicates tool execution should be rejected with a message.  Used by transform_args to reject tool execution when arguments cannot be appropriately transformed for the user's context.
  - **类 `Config`**（配置模型，基类：无，行 43–44）：职责见方法列表。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/user/__init__.py`

- **目录**：`src/vanna/core/user`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：18。**字节**：339。
- **主要功能简介**：用户身份、分组与请求上下文，是行级安全和 UI 特性开关的根基。
- **模块文档字符串**：User domain.

This module provides the core abstractions for user management in the Vanna Agents framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/user/base.py`

- **目录**：`src/vanna/core/user`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：30。**字节**：753。
- **主要功能简介**：用户身份、分组与请求上下文，是行级安全和 UI 特性开关的根基。
- **模块文档字符串**：User domain interface.

This module contains the abstract base class for user services.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `UserService`**（抽象或具体服务/工具实现，基类：ABC，行 13–29）：Service for user management and authentication.
    - `async get_user(user_id)`：读取并返回。`user_id`（该函数的业务参数，详见源码类型标注）。Get user by ID.位于第 17–19 行。
    - `async authenticate(credentials)`：完成该模块中的具体处理。`credentials`（该函数的业务参数，详见源码类型标注）。Authenticate user and return User object if successful.位于第 22–24 行。
    - `async has_permission(user, permission)`：完成该模块中的具体处理。`user`（当前用户对象，携带 id 与 group_memberships）、`permission`（该函数的业务参数，详见源码类型标注）。Check if user has specific permission.位于第 27–29 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/user/models.py`

- **目录**：`src/vanna/core/user`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：26。**字节**：742。
- **主要功能简介**：用户身份、分组与请求上下文，是行级安全和 UI 特性开关的根基。
- **模块文档字符串**：User domain models.

This module contains data models for user management.
- **结构摘要**：类 1 个，模块函数 0 个，方法 0 个。
  - **类 `User`**（Pydantic 数据模型，基类：BaseModel，行 12–25）：User model for authentication and scoping.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/user/request_context.py`

- **目录**：`src/vanna/core/user`。**文件名**：`request_context.py`。**语言/类型**：Python。**行数**：71。**字节**：2156。
- **主要功能简介**：用户身份、分组与请求上下文，是行级安全和 UI 特性开关的根基。
- **模块文档字符串**：Request context for user resolution.

This module provides the RequestContext model for passing web request
information to UserResolver implementations.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `RequestContext`**（Pydantic 数据模型，基类：BaseModel，行 13–70）：Context from a web request for user resolution.  This structured object replaces raw dictionaries for passing request data to UserResolver implementations, making it easier to access cookies, headers, and other request m
    - `get_cookie(name, default)`：读取并返回。`name`（该函数的业务参数，详见源码类型标注）、`default`（该函数的业务参数，详见源码类型标注）。Get cookie value by name.  Args:     name: Cookie name     default: Default value if cookie not found  Returns:     Cook位于第 43–53 行。
    - `get_header(name, default)`：读取并返回。`name`（该函数的业务参数，详见源码类型标注）、`default`（该函数的业务参数，详见源码类型标注）。Get header value by name (case-insensitive).  Args:     name: Header name     default: Default value if header not found位于第 55–70 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/user/resolver.py`

- **目录**：`src/vanna/core/user`。**文件名**：`resolver.py`。**语言/类型**：Python。**行数**：43。**字节**：1293。
- **主要功能简介**：用户身份、分组与请求上下文，是行级安全和 UI 特性开关的根基。
- **模块文档字符串**：User resolver interface for web request authentication.

This module provides the abstract base class for resolving web requests
to authenticated User objects.
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `UserResolver`**（抽象或具体服务/工具实现，基类：ABC，行 14–42）：Resolves web requests to authenticated users.  Implementations of this interface handle the specifics of extracting user identity from request context (cookies, headers, tokens, etc.) and creating authenticated User obje
    - `async resolve_user(request_context)`：解析并还原。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）。Resolve user from request context.  Args:     request_context: Structured request context with cookies, headers, etc.  R位于第 30–42 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/validation.py`

- **目录**：`src/vanna/core`。**文件名**：`validation.py`。**语言/类型**：Python。**行数**：165。**字节**：5484。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
- **模块文档字符串**：Development utilities for validating Pydantic models.

This module provides utilities that can be used during development
and testing to catch forward reference issues early.
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `validate_pydantic_models_in_package`**：校验合法性。`package_name`（该函数的业务参数，详见源码类型标注）。Validate all Pydantic models in a package for completeness.  This function can be used in tests or development scripts to catch forward reference issues before 行 14–110。
  - **函数 `check_models_health`**：检查。无显式位置参数。Quick health check for all core Pydantic models.  Returns:     True if all models are healthy, False otherwise行 113–142。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/workflow/__init__.py`

- **目录**：`src/vanna/core/workflow`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：13。**字节**：475。
- **主要功能简介**：工作流处理器，可在进入 LLM 循环前短路处理 /help、/status 等命令。
- **模块文档字符串**：Workflow handler system for deterministic workflow execution.

This module provides the WorkflowHandler interface for intercepting user messages
and executing deterministic workflows before they reach the LLM. This is useful
for command handling, pattern-based routing, and state-based workflows.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/workflow/base.py`

- **目录**：`src/vanna/core/workflow`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：255。**字节**：10059。
- **主要功能简介**：工作流处理器，可在进入 LLM 循环前短路处理 /help、/status 等命令。
- **模块文档字符串**：Base workflow handler interface.

Workflow triggers allow you to execute deterministic workflows in response to
user messages before they are sent to the LLM. This is useful for:
- Command handling (e.g., /help, /reset)
- Pattern-based routing (e.g., report generation)
- State-based workflows (e.g., onboarding flows)
- Quota enforcement with custom responses
- **结构摘要**：类 2 个，模块函数 0 个，方法 2 个。
  - **类 `WorkflowResult`**（核心类型或辅助类，基类：无，行 32–71）：Result from a workflow handler attempt.  When a workflow handles a message, it can optionally return UI components to stream to the user and/or mutate the conversation state.  Attributes:     should_skip_llm: If True, th
  - **类 `WorkflowHandler`**（抽象或具体服务/工具实现，基类：ABC，行 74–254）：Base class for handling deterministic workflows before LLM processing.  Implement this interface to intercept user messages and execute deterministic workflows instead of sending to the LLM. This is the first extensibili
    - `async try_handle(agent, user, conversation, message)`：完成该模块中的具体处理。`agent`（Agent 实例）、`user`（当前用户对象，携带 id 与 group_memberships）、`conversation`（会话聚合根）、`message`（用户自然语言输入）。Attempt to handle a workflow for the given message.  This method is called for every user message before it reaches the 位于第 134–195 行。
    - `async get_starter_ui(agent, user, conversation)`：读取并返回。`agent`（Agent 实例）、`user`（当前用户对象，携带 id 与 group_memberships）、`conversation`（会话聚合根）。Provide UI components when a conversation starts.  Override this method to show starter buttons, welcome messages, or qu位于第 197–254 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/core/workflow/default.py`

- **目录**：`src/vanna/core/workflow`。**文件名**：`default.py`。**语言/类型**：Python。**行数**：790。**字节**：31946。
- **主要功能简介**：工作流处理器，可在进入 LLM 循环前短路处理 /help、/status 等命令。
- **模块文档字符串**：Default workflow handler implementation with setup health checking.

This module provides a default implementation of the WorkflowHandler interface
that provides a smart starter UI based on available tools and setup status.
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `DefaultWorkflowHandler`**（核心类型或辅助类，基类：WorkflowHandler，行 32–789）：Default workflow handler that provides setup health checking and starter UI.  This handler provides a starter UI that: - Checks if run_sql tool is available (critical) - Checks if memory tools are available (warning if m
    - `__init__(welcome_message)`：提供对象协议或生命周期约定。`welcome_message`（该函数的业务参数，详见源码类型标注）。Initialize with optional custom welcome message.  Args:     welcome_message: Optional custom welcome message. If not pro位于第 42–49 行。
    - `async try_handle(agent, user, conversation, message)`：完成该模块中的具体处理。`agent`（Agent 实例）、`user`（当前用户对象，携带 id 与 group_memberships）、`conversation`（会话聚合根）、`message`（用户自然语言输入）。Handle basic commands, but mostly passes through to LLM.位于第 51–162 行。
    - `async get_starter_ui(agent, user, conversation)`：读取并返回。`agent`（Agent 实例）、`user`（当前用户对象，携带 id 与 group_memberships）、`conversation`（会话聚合根）。Generate starter UI based on available tools and setup status.位于第 164–192 行。
    - `_generate_starter_card(analysis, is_admin)`：生成。`analysis`（该函数的业务参数，详见源码类型标注）、`is_admin`（该函数的业务参数，详见源码类型标注）。Generate a single concise starter card based on role and setup status.位于第 194–204 行。
    - `_generate_admin_starter_card(analysis)`：生成。`analysis`（该函数的业务参数，详见源码类型标注）。Generate admin starter card with setup info and memory management.位于第 206–263 行。
    - `_generate_user_starter_card(analysis)`：生成。`analysis`（该函数的业务参数，详见源码类型标注）。Generate simple user starter view using RichTextComponent.位于第 265–283 行。
    - `_analyze_setup(tool_names)`：完成该模块中的具体处理。`tool_names`（该函数的业务参数，详见源码类型标注）。Analyze the current tool setup and return status.位于第 285–330 行。
    - `_generate_setup_status_cards(analysis)`：生成。`analysis`（该函数的业务参数，详见源码类型标注）。Generate status cards showing setup health (used by /status command).位于第 332–397 行。
    - `_generate_setup_guidance(analysis)`：生成。`analysis`（该函数的业务参数，详见源码类型标注）。Generate setup guidance based on what's missing (used by /status command).位于第 399–453 行。
    - `async _generate_status_check(agent, user)`：生成。`agent`（Agent 实例）、`user`（当前用户对象，携带 id 与 group_memberships）。Generate a detailed status check response.位于第 455–510 行。
    - `async _get_recent_memories(agent, user, conversation)`：读取并返回。`agent`（Agent 实例）、`user`（当前用户对象，携带 id 与 group_memberships）、`conversation`（会话聚合根）。Get and display recent memories from agent memory.位于第 512–681 行。
    - `async _delete_memory(agent, user, conversation, memory_id)`：删除并清理。`agent`（Agent 实例）、`user`（当前用户对象，携带 id 与 group_memberships）、`conversation`（会话聚合根）、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 683–789 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/__init__.py`

- **目录**：`src/vanna/examples`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：53。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Examples for using the Vanna Agents framework.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/__main__.py`

- **目录**：`src/vanna/examples`。**文件名**：`__main__.py`。**语言/类型**：Python。**行数**：45。**字节**：1396。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Interactive example runner for Vanna Agents.
- **结构摘要**：类 0 个，模块函数 1 个，方法 0 个。
  - **函数 `main`**：完成该模块中的具体处理。无显式位置参数。Run an example interactively.行 9–40。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/anthropic_quickstart.py`

- **目录**：`src/vanna/examples`。**文件名**：`anthropic_quickstart.py`。**语言/类型**：Python。**行数**：81。**字节**：2314。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Anthropic example using AnthropicLlmService.

Loads environment from .env (via python-dotenv), uses model 'claude-sonnet-4-20250514'
by default, and sends a simple message through a Agent.

Run:
  PYTHONPATH=. python vanna/examples/anthropic_quickstart.py
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `ensure_env`**：确保前置条件。无显式位置参数。行 17–32。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。行 35–76。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/artifact_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`artifact_example.py`。**语言/类型**：Python。**行数**：294。**字节**：12210。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example demonstrating the artifact system in Vanna Agents.

This script shows how agents can create interactive artifacts that can be
rendered externally by developers listening for the 'artifact-opened' event.
- **结构摘要**：类 1 个，模块函数 2 个，方法 5 个。
  - **类 `ArtifactDemoAgent`**（核心类型或辅助类，基类：Agent，行 18–218）：Demo agent that creates various types of artifacts.
    - `__init__(llm_service)`：提供对象协议或生命周期约定。`llm_service`（该函数的业务参数，详见源码类型标注）。位于第 21–32 行。
    - `async send_message(user, message)`：发送到下游。`user`（当前用户对象，携带 id 与 group_memberships）、`message`（用户自然语言输入）。Handle user messages and create appropriate artifacts.位于第 34–61 行。
    - `async create_html_artifact()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。Create a simple HTML artifact.位于第 63–88 行。
    - `async create_d3_visualization()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。Create a D3.js visualization artifact.位于第 90–161 行。
    - `async create_dashboard_artifact()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。Create a dashboard-style artifact.位于第 163–218 行。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent for REPL and server usage.  Returns:     Configured ArtifactDemoAgent instance行 221–227。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Main demo function.行 230–289。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/claude_sqlite_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`claude_sqlite_example.py`。**语言/类型**：Python。**行数**：237。**字节**：7951。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Claude example using the SQL query tool with the Chinook database.

This example demonstrates using the RunSqlTool with SqliteRunner and Claude's AI
to intelligently query and analyze the Chinook database, with automatic visualization support.

Requirements:
- ANTHROPIC_API_KEY environment variable or .env file
- anthropic package: pip install -e .[anthropic]
- plotly package: pip install -e .[vis
- **结构摘要**：类 0 个，模块函数 3 个，方法 0 个。
  - **函数 `ensure_env`**：确保前置条件。无显式位置参数。行 26–41。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。行 44–163。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent with Claude and SQLite query tool.  This function is called by the vanna server framework.  Returns:     Configured Agent with Claude LLM an行 166–232。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/coding_agent_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`coding_agent_example.py`。**语言/类型**：Python。**行数**：301。**字节**：11015。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example coding agent using the vanna-agents framework.

This example demonstrates building an agent that can edit code files,
following the concepts from the "How to Build an Agent" article.
The agent includes tools for file operations and uses an LLM service
that can understand and modify code.

Usage:
  PYTHONPATH=. python vanna/examples/coding_agent_example.py
- **结构摘要**：类 1 个，模块函数 3 个，方法 4 个。
  - **类 `CodingLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 35–196）：LLM service that simulates a coding assistant.  This demonstrates the minimal implementation needed for an agent as described in the article - just needs to understand tool calls and respond appropriately.
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Handle non-streaming requests.位于第 44–47 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Handle streaming requests.位于第 49–66 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Validate tools - no errors for this simple implementation.位于第 68–70 行。
    - `_build_response(request)`：组装并构建。`request`（下游请求对象）。Build a response based on the conversation context.位于第 72–196 行。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a coding agent with file operation tools.  This follows the pattern from the article - minimal code to create a powerful code-editing agent. Uses depende行 199–229。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Demonstrate the coding agent in action.  As the article mentions: "300 lines of code and three tools and now you're able to talk to an alien intelligence that e行 232–285。
  - **函数 `_extract_filename`**：完成该模块中的具体处理。`message`（用户自然语言输入）。Extract a likely filename token from a user message.行 288–296。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/custom_system_prompt_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`custom_system_prompt_example.py`。**语言/类型**：Python。**行数**：175。**字节**：5541。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example demonstrating custom system prompt builder with dependency injection.

This example shows how to create a custom SystemPromptBuilder that dynamically
generates system prompts based on user context and available tools.

Usage:
  python -m vanna.examples.custom_system_prompt_example
- **结构摘要**：类 2 个，模块函数 1 个，方法 3 个。
  - **类 `CustomSystemPromptBuilder`**（核心类型或辅助类，基类：SystemPromptBuilder，行 17–58）：Custom system prompt builder that personalizes prompts based on user.
    - `async build_system_prompt(user, tools)`：组装并构建。`user`（当前用户对象，携带 id 与 group_memberships）、`tools`（该函数的业务参数，详见源码类型标注）。Build a personalized system prompt.  Args:     user: The user making the request     tools: List of tools available to t位于第 20–58 行。
  - **类 `SQLAssistantSystemPromptBuilder`**（核心类型或辅助类，基类：SystemPromptBuilder，行 61–104）：System prompt builder specifically for SQL database assistants.
    - `__init__(database_name)`：提供对象协议或生命周期约定。`database_name`（该函数的业务参数，详见源码类型标注）。Initialize with database context.  Args:     database_name: Name of the database being queried位于第 64–70 行。
    - `async build_system_prompt(user, tools)`：组装并构建。`user`（当前用户对象，携带 id 与 group_memberships）、`tools`（该函数的业务参数，详见源码类型标注）。Build a SQL-focused system prompt.  Args:     user: The user making the request     tools: List of tools available to th位于第 72–104 行。
  - **函数 `async demo`**：完成该模块中的具体处理。无显式位置参数。Demonstrate custom system prompt builders.行 107–168。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/default_workflow_handler_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`default_workflow_handler_example.py`。**语言/类型**：Python。**行数**：209。**字节**：7238。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example demonstrating the DefaultWorkflowHandler with setup health checking.

This example shows how the DefaultWorkflowHandler provides intelligent starter UI
that adapts based on available tools and helps users understand their setup status.

Run:
  PYTHONPATH=. python vanna/examples/default_workflow_handler_example.py
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `async demonstrate_setup_scenarios`**：完成该模块中的具体处理。无显式位置参数。Demonstrate different setup scenarios with DefaultWorkflowHandler.行 26–199。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the DefaultWorkflowHandler demonstration.行 202–204。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/email_auth_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`email_auth_example.py`。**语言/类型**：Python。**行数**：341。**字节**：11527。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Email authentication example for the Vanna Agents framework.

This example demonstrates how to create an agent with email-based authentication
where users are prompted for their email address in chat and the system creates
a user profile based on that email.

## What This Example Shows

1. **UserService Implementation**: A demo `DemoEmailUserService` that:
   - Stores users in memory
   - Authenti
- **结构摘要**：类 3 个，模块函数 5 个，方法 10 个。
  - **类 `DemoEmailUserService`**（抽象或具体服务/工具实现，基类：UserService，行 72–117）：Demo user service that authenticates users by email.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。Initialize with in-memory user store.位于第 75–78 行。
    - `async get_user(user_id)`：读取并返回。`user_id`（该函数的业务参数，详见源码类型标注）。Get user by ID.位于第 80–82 行。
    - `async authenticate(credentials)`：完成该模块中的具体处理。`credentials`（该函数的业务参数，详见源码类型标注）。Authenticate user by email.位于第 84–109 行。
    - `async has_permission(user, permission)`：完成该模块中的具体处理。`user`（当前用户对象，携带 id 与 group_memberships）、`permission`（该函数的业务参数，详见源码类型标注）。Check if user has permission.位于第 111–113 行。
    - `_is_valid_email(email)`：判断是否满足条件。`email`（该函数的业务参数，详见源码类型标注）。Simple email validation.位于第 115–117 行。
  - **类 `AuthArgs`**（Pydantic 数据模型，基类：BaseModel，行 121–124）：Arguments for authentication.
  - **类 `AuthTool`**（抽象或具体服务/工具实现，基类：Tool[AuthArgs]，行 127–191）：Tool to authenticate users by email.
    - `__init__(user_service)`：提供对象协议或生命周期约定。`user_service`（该函数的业务参数，详见源码类型标注）。位于第 130–131 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 134–135 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 138–139 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 141–142 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Execute authentication.位于第 144–191 行。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent for REPL and server usage.  Returns:     Configured Agent instance with email authentication行 194–200。
  - **函数 `create_auth_agent`**：创建并初始化。无显式位置参数。Create agent with email authentication.行 203–240。
  - **函数 `async demo_auth_flow`**：完成该模块中的具体处理。无显式位置参数。Demonstrate the authentication flow with simple output.行 243–325。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the authentication example.行 328–330。
  - **函数 `run_interactive`**：运行。无显式位置参数。Entry point for interactive usage.行 333–336。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/evaluation_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`evaluation_example.py`。**语言/类型**：Python。**行数**：270。**字节**：8521。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Evaluation System Example

This example demonstrates how to use the evaluation framework to test
and compare agents. Shows:
- Creating test cases programmatically
- Running evaluations with multiple evaluators
- Comparing agent variants (e.g., different LLMs)
- Generating reports

Usage:
    PYTHONPATH=. python vanna/examples/evaluation_example.py
- **结构摘要**：类 0 个，模块函数 6 个，方法 0 个。
  - **函数 `create_sample_dataset`**：创建并初始化。无显式位置参数。Create a sample dataset for demonstration.行 30–75。
  - **函数 `create_test_agent`**：创建并初始化。`name`（该函数的业务参数，详见源码类型标注）、`response_content`（该函数的业务参数，详见源码类型标注）。Create a test agent with mock LLM.行 78–84。
  - **函数 `async demo_single_agent_evaluation`**：完成该模块中的具体处理。无显式位置参数。Demonstrate evaluating a single agent.行 87–124。
  - **函数 `async demo_agent_comparison`**：完成该模块中的具体处理。无显式位置参数。Demonstrate comparing multiple agent variants.行 127–193。
  - **函数 `async demo_dataset_operations`**：完成该模块中的具体处理。无显式位置参数。Demonstrate dataset creation and manipulation.行 196–238。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run all evaluation demos.行 241–265。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/extensibility_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`extensibility_example.py`。**语言/类型**：Python。**行数**：263。**字节**：9315。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Comprehensive example demonstrating all extensibility interfaces.

This example shows how to use:
- LlmMiddleware for caching
- ErrorRecoveryStrategy for retry logic
- ToolContextEnricher for adding user preferences
- ConversationFilter for context window management
- ObservabilityProvider for monitoring
- **结构摘要**：类 6 个，模块函数 1 个，方法 20 个。
  - **类 `CachingMiddleware`**（可插拔扩展点实现，基类：LlmMiddleware，行 37–67）：Cache LLM responses to reduce costs and latency.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 40–43 行。
    - `_compute_cache_key(request)`：计算。`request`（下游请求对象）。Create cache key from request.位于第 45–48 行。
    - `async before_llm_request(request)`：完成该模块中的具体处理。`request`（下游请求对象）。Check cache before sending request.位于第 50–56 行。
    - `async after_llm_response(request, response)`：完成该模块中的具体处理。`request`（下游请求对象）、`response`（下游响应对象）。Cache the response.位于第 58–67 行。
  - **类 `ExponentialBackoffStrategy`**（核心类型或辅助类，基类：ErrorRecoveryStrategy，行 71–117）：Retry failed operations with exponential backoff.
    - `__init__(max_retries)`：提供对象协议或生命周期约定。`max_retries`（该函数的业务参数，详见源码类型标注）。位于第 74–75 行。
    - `async handle_tool_error(error, context, attempt)`：处理请求或事件。`error`（异常对象）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`attempt`（该函数的业务参数，详见源码类型标注）。Retry tool errors with exponential backoff.位于第 77–96 行。
    - `async handle_llm_error(error, request, attempt)`：处理请求或事件。`error`（异常对象）、`request`（下游请求对象）、`attempt`（该函数的业务参数，详见源码类型标注）。Retry LLM errors with backoff.位于第 98–117 行。
  - **类 `UserPreferencesEnricher`**（抽象或具体服务/工具实现，基类：ToolContextEnricher，行 121–140）：Enrich context with user preferences.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 124–132 行。
    - `async enrich_context(context)`：富集上下文。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Add user preferences to context.位于第 134–140 行。
  - **类 `ContextWindowFilter`**（可插拔扩展点实现，基类：ConversationFilter，行 144–164）：Limit conversation to fit within context window.
    - `__init__(max_messages)`：提供对象协议或生命周期约定。`max_messages`（该函数的业务参数，详见源码类型标注）。位于第 147–148 行。
    - `async filter_messages(messages)`：过滤。`messages`（该函数的业务参数，详见源码类型标注）。Keep only recent messages within limit.位于第 150–164 行。
  - **类 `LoggingObservabilityProvider`**（核心类型或辅助类，基类：ObservabilityProvider，行 168–201）：Log metrics and spans for monitoring.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 171–173 行。
    - `async record_metric(name, value, unit, tags)`：完成该模块中的具体处理。`name`（该函数的业务参数，详见源码类型标注）、`value`（该函数的业务参数，详见源码类型标注）、`unit`（该函数的业务参数，详见源码类型标注）、`tags`（指标标签）。Record and log a metric.位于第 175–186 行。
    - `async create_span(name, attributes)`：创建并初始化。`name`（该函数的业务参数，详见源码类型标注）、`attributes`（该函数的业务参数，详见源码类型标注）。Create a span for tracing.位于第 188–194 行。
    - `async end_span(span)`：完成该模块中的具体处理。`span`（可观测性 Span）。End and record a span.位于第 196–201 行。
  - **类 `MockStore`**（核心类型或辅助类，基类：无，行 218–238）：职责见方法列表。
    - `async get_conversation(cid, uid)`：读取并返回。`cid`（该函数的业务参数，详见源码类型标注）、`uid`（该函数的业务参数，详见源码类型标注）。位于第 219–220 行。
    - `async create_conversation(cid, uid, title)`：创建并初始化。`cid`（该函数的业务参数，详见源码类型标注）、`uid`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。位于第 222–227 行。
    - `async update_conversation(conv)`：更新。`conv`（该函数的业务参数，详见源码类型标注）。位于第 229–230 行。
    - `async delete_conversation(cid, uid)`：删除并清理。`cid`（该函数的业务参数，详见源码类型标注）、`uid`（该函数的业务参数，详见源码类型标注）。位于第 232–233 行。
    - `async list_conversations(uid, limit, offset)`：枚举并列出。`uid`（该函数的业务参数，详见源码类型标注）、`limit`（返回条数上限）、`offset`（该函数的业务参数，详见源码类型标注）。位于第 235–238 行。
  - **函数 `async run_example`**：运行。无显式位置参数。Example showing all extensibility interfaces working together.行 204–258。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/minimal_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`minimal_example.py`。**语言/类型**：Python。**行数**：68。**字节**：1939。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Minimal Claude + SQLite example ready for FastAPI.
- **结构摘要**：类 0 个，模块函数 1 个，方法 0 个。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。行 30–67。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/mock_auth_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`mock_auth_example.py`。**语言/类型**：Python。**行数**：228。**字节**：7543。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Mock authentication example to verify user resolution is working.

This example demonstrates the new UserResolver architecture where:
1. UserResolver is a required parameter of Agent
2. Agent.send_message() accepts RequestContext (not User directly)
3. The Agent resolves the user internally using the UserResolver

The agent uses an LLM middleware to inject user info into the response,
so we can ve
- **结构摘要**：类 1 个，模块函数 4 个，方法 1 个。
  - **类 `UserEchoMiddleware`**（可插拔扩展点实现，基类：LlmMiddleware，行 28–45）：Middleware that injects user email into LLM responses.
    - `async after_llm_response(request, response)`：完成该模块中的具体处理。`request`（下游请求对象）、`response`（下游响应对象）。Inject user email into response.位于第 31–45 行。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent for server usage.  Returns:     Configured Agent instance with cookie-based authentication行 48–78。
  - **函数 `async demo_authentication`**：完成该模块中的具体处理。无显式位置参数。Demonstrate authentication with different request contexts.行 81–212。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the authentication example.行 215–217。
  - **函数 `run_interactive`**：运行。无显式位置参数。Entry point for interactive usage.行 220–223。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/mock_custom_tool.py`

- **目录**：`src/vanna/examples`。**文件名**：`mock_custom_tool.py`。**语言/类型**：Python。**行数**：312。**字节**：10598。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Mock example showing how to create and use custom tools.

This example demonstrates creating a simple calculator tool
and registering it with an agent that uses a mock LLM service.
It now includes a `MockCalculatorLlmService` that automatically
invokes the calculator tool with random numbers before echoing
back the computed answer.

Usage:
  Template: Copy this file and modify for your custom tool
- **结构摘要**：类 3 个，模块函数 3 个，方法 10 个。
  - **类 `CalculatorArgs`**（Pydantic 数据模型，基类：BaseModel，行 52–59）：Arguments for the calculator tool.
  - **类 `CalculatorTool`**（抽象或具体服务/工具实现，基类：Tool[CalculatorArgs]，行 62–155）：A simple calculator tool.
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 66–67 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 70–71 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 73–74 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Execute the calculator operation.位于第 76–155 行。
  - **类 `MockCalculatorLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 158–234）：LLM service that exercises the calculator tool before echoing the result.
    - `__init__(seed)`：提供对象协议或生命周期约定。`seed`（该函数的业务参数，详见源码类型标注）。位于第 161–162 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Handle non-streaming calculator interactions.位于第 164–167 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Provide streaming compatibility by yielding a single chunk.位于第 169–183 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Mock validation - no errors.位于第 185–187 行。
    - `_build_response(request)`：组装并构建。`request`（下游请求对象）。Create a response that either calls the tool or echoes its result.位于第 189–217 行。
    - `_random_operands()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Generate operation and operands suited for the calculator tool.位于第 219–234 行。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent with custom calculator tool.  Returns:     Configured Agent with calculator tool and mock calculator LLM行 237–256。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the mock custom tool example.行 259–301。
  - **函数 `run_interactive`**：运行。无显式位置参数。Entry point for interactive usage.行 304–307。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/mock_quickstart.py`

- **目录**：`src/vanna/examples`。**文件名**：`mock_quickstart.py`。**语言/类型**：Python。**行数**：80。**字节**：2004。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Mock quickstart example for the Vanna Agents framework.

This example shows how to create a basic agent with a mock LLM service
and have a simple conversation.

Usage:
  Template: Copy this file and modify for your needs
  Interactive: python -m vanna.examples.mock_quickstart
  REPL: from vanna.examples.mock_quickstart import create_demo_agent
  Server: python -m vanna.servers --example mock_quick
- **结构摘要**：类 0 个，模块函数 3 个，方法 0 个。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent for REPL and server usage.  Returns:     Configured Agent instance行 25–41。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the mock quickstart example.行 44–69。
  - **函数 `run_interactive`**：运行。无显式位置参数。Entry point for interactive usage.行 72–75。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/mock_quota_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`mock_quota_example.py`。**语言/类型**：Python。**行数**：146。**字节**：4689。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Mock quota-based agent example using Mock LLM service.

This example demonstrates how to create a custom agent runner that
enforces user-based message quotas. It shows:
- Custom agent runner subclass
- Quota management and enforcement
- Error handling for quota exceeded cases
- Multiple users with different quotas

Run:
  PYTHONPATH=. python vanna/examples/mock_quota_example.py
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `async demonstrate_quota_system`**：完成该模块中的具体处理。无显式位置参数。Demonstrate the quota-based agent system.行 28–136。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the mock quota example.行 139–141。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/mock_rich_components_demo.py`

- **目录**：`src/vanna/examples`。**文件名**：`mock_rich_components_demo.py`。**语言/类型**：Python。**行数**：397。**字节**：12673。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Mock rich components demonstration example.

This example shows how to create an agent that emits rich, stateful components
including cards, task lists, and tool execution displays using a mock LLM service.

Usage:
  PYTHONPATH=. python vanna/examples/mock_rich_components_demo.py
- **结构摘要**：类 1 个，模块函数 3 个，方法 1 个。
  - **类 `RichComponentsAgent`**（UI 组件模型，基类：Agent，行 35–311）：Agent that demonstrates rich component capabilities.
    - `async send_message(user, message)`：发送到下游。`user`（当前用户对象，携带 id 与 group_memberships）、`message`（用户自然语言输入）。Send message and yield UiComponent(rich_component=rich) components.位于第 38–311 行。
  - **函数 `create_rich_demo_agent`**：创建并初始化。无显式位置参数。Create a primitive components demo agent.  Returns:     Configured RichComponentsAgent instance行 318–332。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the primitive components demo.行 335–386。
  - **函数 `run_interactive`**：运行。无显式位置参数。Entry point for interactive usage.行 389–392。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/mock_sqlite_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`mock_sqlite_example.py`。**语言/类型**：Python。**行数**：224。**字节**：7360。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Mock example showing how to use the SQL query tool with the Chinook database.

This example demonstrates using the RunSqlTool with SqliteRunner and a mock LLM service
that automatically executes sample SQL queries against the Chinook database.

Usage:
  Template: Copy this file and modify for your custom database
  Interactive: python -m vanna.examples.mock_sqlite_example
  REPL: from vanna.exampl
- **结构摘要**：类 1 个，模块函数 3 个，方法 5 个。
  - **类 `MockSqliteLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 52–123）：LLM service that exercises the SQLite query tool with sample queries.
    - `__init__(seed)`：提供对象协议或生命周期约定。`seed`（该函数的业务参数，详见源码类型标注）。位于第 55–66 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Handle non-streaming SQLite interactions.位于第 68–71 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Provide streaming compatibility by yielding a single chunk.位于第 73–87 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Mock validation - no errors.位于第 89–91 行。
    - `_build_response(request)`：组装并构建。`request`（下游请求对象）。Create a response that either calls the tool or explains its result.位于第 93–123 行。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent with SQLite query tool.  Returns:     Configured Agent with SQLite tool and mock LLM行 126–157。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the mock SQLite example.行 160–212。
  - **函数 `run_interactive`**：运行。无显式位置参数。Entry point for interactive usage.行 215–219。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/openai_quickstart.py`

- **目录**：`src/vanna/examples`。**文件名**：`openai_quickstart.py`。**语言/类型**：Python。**行数**：84。**字节**：2493。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：OpenAI example using OpenAILlmService.

Loads environment from .env (via python-dotenv), uses model 'gpt-5' by default,
and sends a simple message through a Agent.

Run:
  PYTHONPATH=. python vanna/examples/openai_quickstart.py
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `ensure_env`**：确保前置条件。无显式位置参数。行 17–32。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。行 35–79。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/primitive_components_demo.py`

- **目录**：`src/vanna/examples`。**文件名**：`primitive_components_demo.py`。**语言/类型**：Python。**行数**：306。**字节**：9742。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Demonstration of the new primitive component system.

This example shows how tools compose UI from primitive, domain-agnostic
components like StatusCardComponent, ProgressDisplayComponent, etc.

Usage:
  PYTHONPATH=. python vanna/examples/primitive_components_demo.py
- **结构摘要**：类 1 个，模块函数 3 个，方法 1 个。
  - **类 `PrimitiveComponentsAgent`**（UI 组件模型，基类：Agent，行 34–217）：Agent that demonstrates the new primitive component system.
    - `async send_message(user, message)`：发送到下游。`user`（当前用户对象，携带 id 与 group_memberships）、`message`（用户自然语言输入）。Send message and demonstrate primitive component composition.位于第 37–217 行。
  - **函数 `create_primitive_demo_agent`**：创建并初始化。无显式位置参数。Create a primitive components demo agent.  Returns:     Configured PrimitiveComponentsAgent instance行 220–234。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Run the primitive components demo.行 237–295。
  - **函数 `run_interactive`**：运行。无显式位置参数。Entry point for interactive usage.行 298–301。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/quota_lifecycle_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`quota_lifecycle_example.py`。**语言/类型**：Python。**行数**：140。**字节**：4470。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example demonstrating lifecycle hooks for user quota management.

This example shows how to use lifecycle hooks to add custom functionality
like quota management without creating custom agent runner subclasses.
- **结构摘要**：类 3 个，模块函数 1 个，方法 9 个。
  - **类 `QuotaExceededError`**（异常类型，基类：AgentError，行 13–16）：Raised when a user exceeds their message quota.
  - **类 `QuotaCheckHook`**（可插拔扩展点实现，基类：LifecycleHook，行 19–72）：Lifecycle hook that enforces user-based message quotas.
    - `__init__(default_quota)`：提供对象协议或生命周期约定。`default_quota`（该函数的业务参数，详见源码类型标注）。Initialize quota hook.  Args:     default_quota: Default quota per user if not specifically set位于第 22–30 行。
    - `set_user_quota(user_id, quota)`：设置并更新。`user_id`（该函数的业务参数，详见源码类型标注）、`quota`（该函数的业务参数，详见源码类型标注）。Set a specific quota for a user.位于第 32–34 行。
    - `get_user_quota(user_id)`：读取并返回。`user_id`（该函数的业务参数，详见源码类型标注）。Get the quota for a user.位于第 36–38 行。
    - `get_user_usage(user_id)`：读取并返回。`user_id`（该函数的业务参数，详见源码类型标注）。Get current usage count for a user.位于第 40–42 行。
    - `get_user_remaining(user_id)`：读取并返回。`user_id`（该函数的业务参数，详见源码类型标注）。Get remaining messages for a user.位于第 44–46 行。
    - `reset_user_usage(user_id)`：完成该模块中的具体处理。`user_id`（该函数的业务参数，详见源码类型标注）。Reset usage count for a user.位于第 48–50 行。
    - `async before_message(user, message)`：完成该模块中的具体处理。`user`（当前用户对象，携带 id 与 group_memberships）、`message`（用户自然语言输入）。Check quota before processing message.  Raises:     QuotaExceededError: If user has exceeded their quota位于第 52–72 行。
  - **类 `LoggingHook`**（可插拔扩展点实现，基类：LifecycleHook，行 75–85）：Example logging hook for demonstration.
    - `async before_message(user, message)`：完成该模块中的具体处理。`user`（当前用户对象，携带 id 与 group_memberships）、`message`（用户自然语言输入）。Log incoming messages.位于第 78–81 行。
    - `async after_message(result)`：完成该模块中的具体处理。`result`（该函数的业务参数，详见源码类型标注）。Log message completion.位于第 83–85 行。
  - **函数 `async run_example`**：运行。无显式位置参数。Example showing how to use lifecycle hooks with Agent.  Instead of creating a custom subclass, we compose the behavior using lifecycle hooks.行 88–133。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/examples/visualization_example.py`

- **目录**：`src/vanna/examples`。**文件名**：`visualization_example.py`。**语言/类型**：Python。**行数**：252。**字节**：8387。
- **主要功能简介**：可运行示例，覆盖认证、配额、评测、可视化、自定义工具。
- **模块文档字符串**：Example demonstrating SQL query execution with automatic visualization.

This example shows the integration of RunSqlTool and VisualizeDataTool,
demonstrating how SQL results are saved to CSV files and can be visualized
using the visualization tool with dependency injection.

Usage:
  PYTHONPATH=. python vanna/examples/visualization_example.py
- **结构摘要**：类 1 个，模块函数 2 个，方法 5 个。
  - **类 `VisualizationDemoLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 40–135）：Mock LLM that demonstrates SQL query and visualization workflow.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 43–45 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Handle non-streaming requests.位于第 47–50 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Handle streaming requests.位于第 52–66 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Validate tools - no errors.位于第 68–70 行。
    - `_build_response(request)`：组装并构建。`request`（下游请求对象）。Build response based on conversation state.位于第 72–135 行。
  - **函数 `create_demo_agent`**：创建并初始化。无显式位置参数。Create a demo agent with SQL and visualization tools.  This function is called by the vanna server framework.  Returns:     Configured Agent with SQL and visual行 138–185。
  - **函数 `async main`**：完成该模块中的具体处理。无显式位置参数。Demonstrate SQL query execution with automatic visualization.行 188–247。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/__init__.py`

- **目录**：`src/vanna/integrations`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：18。**字节**：383。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Integrations module.

This package contains concrete implementations of core abstractions and capabilities.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/anthropic/__init__.py`

- **目录**：`src/vanna/integrations/anthropic`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：10。**字节**：164。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Anthropic integration.

This module provides Anthropic LLM service implementation.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/anthropic/llm.py`

- **目录**：`src/vanna/integrations/anthropic`。**文件名**：`llm.py`。**语言/类型**：Python。**行数**：271。**字节**：10189。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Anthropic LLM service implementation.

Implements the LlmService interface using Anthropic's Messages API
(anthropic>=0.8.0). Supports non-streaming and streaming text output.
Tool-calls (tool_use blocks) are surfaced at the end of a stream or after a
non-streaming call as ToolCall entries.
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `AnthropicLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 27–270）：Anthropic Messages-backed LLM service.  Args:     model: Anthropic model name (e.g., "claude-sonnet-4-5", "claude-opus-4").         Defaults to "claude-sonnet-4-5". Can also be set via ANTHROPIC_MODEL env var.     api_ke
    - `__init__(model, api_key, base_url)`：提供对象协议或生命周期约定。`model`（模型名称）、`api_key`（供应商密钥）、`base_url`（该函数的业务参数，详见源码类型标注）。位于第 38–63 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Send a non-streaming request to Anthropic and return the response.位于第 65–90 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Stream a request to Anthropic.  Yields text chunks as they arrive. Emits tool-calls at the end by inspecting the final m位于第 92–121 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Basic validation of tool schemas for Anthropic.位于第 123–129 行。
    - `_build_payload(request)`：组装并构建。`request`（下游请求对象）。位于第 132–224 行。
    - `_parse_message_content(msg)`：解析输入。`msg`（该函数的业务参数，详见源码类型标注）。位于第 226–270 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/azureopenai/__init__.py`

- **目录**：`src/vanna/integrations/azureopenai`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：10。**字节**：175。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Azure OpenAI integration.

This module provides Azure OpenAI LLM service implementations.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/azureopenai/llm.py`

- **目录**：`src/vanna/integrations/azureopenai`。**文件名**：`llm.py`。**语言/类型**：Python。**行数**：330。**字节**：12428。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Azure OpenAI LLM service implementation.

Provides an `LlmService` backed by Azure OpenAI Chat Completions (openai>=1.0.0)
with support for streaming, deployment-scoped models, and Azure-specific
authentication flows.
- **结构摘要**：类 1 个，模块函数 1 个，方法 6 个。
  - **类 `AzureOpenAILlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 44–329）：Azure OpenAI Chat Completions-backed LLM service.  Wraps `openai.AzureOpenAI` so Vanna can talk to deployment-scoped models and either API key or Microsoft Entra ID authentication.  Args:     model: Deployment name in Az
    - `__init__(model, api_key, azure_endpoint, api_version, azure_ad_token_provider)`：提供对象协议或生命周期约定。`model`（模型名称）、`api_key`（供应商密钥）、`azure_endpoint`（该函数的业务参数，详见源码类型标注）、`api_version`（该函数的业务参数，详见源码类型标注）、`azure_ad_token_provider`（该函数的业务参数，详见源码类型标注）。位于第 62–118 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Send a non-streaming request to Azure OpenAI and return the response.位于第 120–150 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Stream a request to Azure OpenAI.  Emits `LlmStreamChunk` for textual deltas as they arrive. Tool-calls are accumulated 位于第 152–231 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Validate tool schemas. Returns a list of error messages.位于第 233–240 行。
    - `_build_payload(request)`：组装并构建。`request`（下游请求对象）。Build the API payload from LlmRequest.位于第 243–303 行。
    - `_extract_tool_calls_from_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。Extract tool calls from OpenAI message object.位于第 305–329 行。
  - **函数 `_is_reasoning_model`**：判断是否满足条件。`model`（模型名称）。Return True when the deployment targets a reasoning-only model.行 38–41。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/azuresearch/__init__.py`

- **目录**：`src/vanna/integrations/azuresearch`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：146。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Azure AI Search integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/azuresearch/agent_memory.py`

- **目录**：`src/vanna/integrations/azuresearch`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：414。**字节**：13823。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Azure AI Search implementation of AgentMemory.

This implementation uses Azure Cognitive Search for vector storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 14 个。
  - **类 `AzureAISearchAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 40–413）：Azure AI Search-based implementation of AgentMemory.
    - `__init__(endpoint, api_key, index_name, dimension)`：提供对象协议或生命周期约定。`endpoint`（该函数的业务参数，详见源码类型标注）、`api_key`（供应商密钥）、`index_name`（该函数的业务参数，详见源码类型标注）、`dimension`（该函数的业务参数，详见源码类型标注）。位于第 43–63 行。
    - `_get_index_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create index client.位于第 65–72 行。
    - `_get_search_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create search client.位于第 74–83 行。
    - `_ensure_index_exists()`：确保前置条件。仅依赖实例或类自身状态，不额外接收调用方参数。Create index if it doesn't exist.位于第 85–131 行。
    - `_create_embedding(text)`：创建并初始化。`text`（该函数的业务参数，详见源码类型标注）。Create a simple embedding from text (placeholder).位于第 133–138 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 140–171 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 173–225 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 227–259 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 261–273 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 275–296 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 298–334 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 336–365 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 367–379 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 381–413 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/bigquery/__init__.py`

- **目录**：`src/vanna/integrations/bigquery`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：108。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：BigQuery integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/bigquery/sql_runner.py`

- **目录**：`src/vanna/integrations/bigquery`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：82。**字节**：2691。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：BigQuery implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `BigQueryRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–81）：BigQuery implementation of the SqlRunner interface.
    - `__init__(project_id, cred_file_path)`：提供对象协议或生命周期约定。`project_id`（该函数的业务参数，详见源码类型标注）、`cred_file_path`（该函数的业务参数，详见源码类型标注）。Initialize with BigQuery connection parameters.  Args:     project_id: Google Cloud Project ID     cred_file_path: Path 位于第 13–36 行。
    - `_get_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create BigQuery client.位于第 38–60 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against BigQuery database and return results as DataFrame.  Args:     args: SQL query arguments     co位于第 62–81 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/chromadb/__init__.py`

- **目录**：`src/vanna/integrations/chromadb`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：105。**字节**：3642。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：ChromaDB integration for Vanna Agents.
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `get_device`**：读取并返回。无显式位置参数。Detect the best available device for embeddings.  This function checks for GPU availability and returns the appropriate device string for use with embedding mod行 8–46。
  - **函数 `create_sentence_transformer_embedding_function`**：创建并初始化。`model_name`（该函数的业务参数，详见源码类型标注）、`device`（该函数的业务参数，详见源码类型标注）。Create a SentenceTransformer embedding function with automatic device detection.  This convenience function creates a ChromaDB-compatible SentenceTransformer em行 49–97。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/chromadb/agent_memory.py`

- **目录**：`src/vanna/integrations/chromadb`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：489。**字节**：18347。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Local vector database implementation of AgentMemory.

This implementation uses ChromaDB for local vector storage of tool usage patterns.
- **结构摘要**：类 2 个，模块函数 0 个，方法 14 个。
  - **类 `ChromaAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 42–488）：ChromaDB-based implementation of AgentMemory.  This implementation uses ChromaDB's PersistentClient to store agent memories on disk, ensuring they persist across application restarts.  Key Features: - Persistent storage:
    - `__init__(persist_directory, collection_name, embedding_function)`：提供对象协议或生命周期约定。`persist_directory`（该函数的业务参数，详见源码类型标注）、`collection_name`（该函数的业务参数，详见源码类型标注）、`embedding_function`（该函数的业务参数，详见源码类型标注）。位于第 102–118 行。
    - `_get_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create ChromaDB client.位于第 120–127 行。
    - `_get_embedding_function()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create the embedding function.  If no embedding function was provided during initialization, uses ChromaDB's defa位于第 129–139 行。
    - `_get_collection()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create ChromaDB collection.位于第 141–158 行。
    - `_create_memory_id()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。Create a unique ID for a memory.位于第 160–164 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 166–199 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 201–265 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories. Returns most recent memories first.位于第 267–313 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID. Returns True if deleted, False if not found.位于第 315–331 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 333–354 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 356–403 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 405–433 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 435–450 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 452–488 行。
  - **类 `NotFoundError`**（异常类型，基类：Exception，行 23–26）：Fallback NotFoundError for older ChromaDB versions.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/clickhouse/__init__.py`

- **目录**：`src/vanna/integrations/clickhouse`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：114。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：ClickHouse integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/clickhouse/sql_runner.py`

- **目录**：`src/vanna/integrations/clickhouse`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：83。**字节**：2350。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：ClickHouse implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `ClickHouseRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–82）：ClickHouse implementation of the SqlRunner interface.
    - `__init__(host, database, user, password, port)`：提供对象协议或生命周期约定。`host`（该函数的业务参数，详见源码类型标注）、`database`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）。Initialize with ClickHouse connection parameters.  Args:     host: Database host address     database: Database name    位于第 13–47 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against ClickHouse database and return results as DataFrame.  Args:     args: SQL query arguments     位于第 49–82 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/duckdb/__init__.py`

- **目录**：`src/vanna/integrations/duckdb`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：102。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：DuckDB integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/duckdb/sql_runner.py`

- **目录**：`src/vanna/integrations/duckdb`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：66。**字节**：2088。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：DuckDB implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `DuckDBRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–65）：DuckDB implementation of the SqlRunner interface.
    - `__init__(database_path, init_sql)`：提供对象协议或生命周期约定。`database_path`（该函数的业务参数，详见源码类型标注）、`init_sql`（该函数的业务参数，详见源码类型标注）。Initialize with DuckDB connection parameters.  Args:     database_path: Path to the DuckDB database file.               位于第 13–37 行。
    - `_get_connection()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create DuckDB connection.位于第 39–45 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against DuckDB database and return results as DataFrame.  Args:     args: SQL query arguments     cont位于第 47–65 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/faiss/__init__.py`

- **目录**：`src/vanna/integrations/faiss`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：120。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：FAISS integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/faiss/agent_memory.py`

- **目录**：`src/vanna/integrations/faiss`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：436。**字节**：14370。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：FAISS vector database implementation of AgentMemory.

This implementation uses FAISS for local vector storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 13 个。
  - **类 `FAISSAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 34–435）：FAISS-based implementation of AgentMemory.
    - `__init__(index_path, persist_path, dimension, metric)`：提供对象协议或生命周期约定。`index_path`（该函数的业务参数，详见源码类型标注）、`persist_path`（该函数的业务参数，详见源码类型标注）、`dimension`（该函数的业务参数，详见源码类型标注）、`metric`（该函数的业务参数，详见源码类型标注）。位于第 37–56 行。
    - `_load_index()`：加载。仅依赖实例或类自身状态，不额外接收调用方参数。Load or create FAISS index.位于第 58–75 行。
    - `_save_index()`：持久化保存。仅依赖实例或类自身状态，不额外接收调用方参数。Save FAISS index to disk.位于第 77–84 行。
    - `_create_embedding(text)`：创建并初始化。`text`（该函数的业务参数，详见源码类型标注）。Create a simple embedding from text (placeholder).位于第 86–102 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 104–137 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 139–205 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 207–240 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 242–260 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 262–286 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 288–344 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 346–375 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 377–395 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 397–435 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/google/__init__.py`

- **目录**：`src/vanna/integrations/google`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：10。**字节**：159。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Google AI integrations.

This module provides Google AI service implementations.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/google/gemini.py`

- **目录**：`src/vanna/integrations/google`。**文件名**：`gemini.py`。**语言/类型**：Python。**行数**：371。**字节**：13091。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Google Gemini LLM service implementation.

Implements the LlmService interface using Google's Gen AI SDK
(google-genai). Supports non-streaming and streaming text output,
as well as function calling (tool use).
- **结构摘要**：类 1 个，模块函数 0 个，方法 8 个。
  - **类 `GeminiLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 27–370）：Google Gemini-backed LLM service.  Args:     model: Gemini model name (e.g., "gemini-2.5-pro", "gemini-2.5-flash").         Defaults to "gemini-2.5-pro". Can also be set via GEMINI_MODEL env var.     api_key: API key; fa
    - `__init__(model, api_key, temperature)`：提供对象协议或生命周期约定。`model`（模型名称）、`api_key`（供应商密钥）、`temperature`（该函数的业务参数，详见源码类型标注）。位于第 39–74 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Send a non-streaming request to Gemini and return the response.位于第 76–123 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Stream a request to Gemini.  Yields text chunks as they arrive. Emits tool calls at the end.位于第 125–173 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Basic validation of tool schemas for Gemini.位于第 175–183 行。
    - `_build_payload(request)`：组装并构建。`request`（下游请求对象）。Build the payload for Gemini API.  Returns:     Tuple of (contents, config)位于第 186–290 行。
    - `_parse_response(response)`：解析输入。`response`（下游响应对象）。Parse a Gemini response into text and tool calls.位于第 292–326 行。
    - `_parse_response_chunk(chunk)`：解析输入。`chunk`（该函数的业务参数，详见源码类型标注）。Parse a streaming chunk (same logic as _parse_response).位于第 328–330 行。
    - `_clean_schema_for_gemini(schema)`：完成该模块中的具体处理。`schema`（该函数的业务参数，详见源码类型标注）。Clean JSON Schema to only include fields supported by Gemini.  Gemini only supports a subset of OpenAPI schema. This rem位于第 332–370 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/hive/__init__.py`

- **目录**：`src/vanna/integrations/hive`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：96。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Hive integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/hive/sql_runner.py`

- **目录**：`src/vanna/integrations/hive`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：88。**字节**：2584。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Hive implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `HiveRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–87）：Hive implementation of the SqlRunner interface.
    - `__init__(host, database, user, password, port, auth)`：提供对象协议或生命周期约定。`host`（该函数的业务参数，详见源码类型标注）、`database`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）、`auth`（该函数的业务参数，详见源码类型标注）。Initialize with Hive connection parameters.  Args:     host: The host of the Hive database     database: The name of the位于第 13–49 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against Hive database and return results as DataFrame.  Args:     args: SQL query arguments     contex位于第 51–87 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/local/__init__.py`

- **目录**：`src/vanna/integrations/local`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：18。**字节**：408。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Local integration.

This module provides built-in local implementations.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/local/agent_memory/__init__.py`

- **目录**：`src/vanna/integrations/local/agent_memory`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：115。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Local agent memory implementations.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/local/agent_memory/in_memory.py`

- **目录**：`src/vanna/integrations/local/agent_memory`。**文件名**：`in_memory.py`。**语言/类型**：Python。**行数**：286。**字节**：10061。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Demo in-memory implementation of AgentMemory.

This implementation provides a zero-dependency, minimal storage solution that
keeps all memories in RAM. It uses simple similarity algorithms (Jaccard and
difflib) instead of vector embeddings. Perfect for demos and testing.
- **结构摘要**：类 1 个，模块函数 0 个，方法 14 个。
  - **类 `DemoAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 28–285）：Minimal, dependency-free in-memory storage for demos and testing. - O(n) search over an in-memory list - Simple similarity: max(Jaccard(token sets), difflib ratio) - Optional FIFO eviction via max_items - Async-safe with
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。Initialize the in-memory storage.  Args:     max_items: Maximum number of memories to keep. Oldest memories are         位于第 37–48 行。
    - `_now_iso()`：完成该模块中的具体处理。无显式位置参数。Get current timestamp in ISO format.位于第 51–53 行。
    - `_normalize(text)`：完成该模块中的具体处理。`text`（该函数的业务参数，详见源码类型标注）。Normalize text by lowercasing and collapsing whitespace.位于第 56–58 行。
    - `_tokenize(text)`：转换为目标结构。`text`（该函数的业务参数，详见源码类型标注）。Simple tokenizer that splits on whitespace.位于第 61–63 行。
    - `_similarity(a, b)`：完成该模块中的具体处理。`a`（该函数的业务参数，详见源码类型标注）、`b`（该函数的业务参数，详见源码类型标注）。Calculate similarity between two strings using multiple methods.  Returns the maximum of Jaccard similarity and difflib 位于第 66–87 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern for future reference.位于第 89–113 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Store a text memory in RAM.位于第 115–125 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns based on a question.位于第 127–164 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search free-form text memories using the demo similarity metric.位于第 166–197 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories. Returns most recent memories first.位于第 199–205 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Return recently added text memories.位于第 207–212 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a stored text memory by ID.位于第 214–221 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID. Returns True if deleted, False if not found.位于第 223–230 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories. Returns number of memories deleted.位于第 232–285 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/local/audit.py`

- **目录**：`src/vanna/integrations/local`。**文件名**：`audit.py`。**语言/类型**：Python。**行数**：60。**字节**：1838。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Local audit logger implementation using Python logging.

This module provides a simple audit logger that writes events using
the standard Python logging module, useful for development and testing.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `LoggingAuditLogger`**（核心类型或辅助类，基类：AuditLogger，行 17–59）：Audit logger that writes events to Python logger as structured JSON.  This implementation uses logger.info() to emit audit events as JSON, making them easy to parse and route to log aggregation systems.  Example:     aud
    - `__init__(log_level)`：提供对象协议或生命周期约定。`log_level`（该函数的业务参数，详见源码类型标注）。Initialize the logging audit logger.  Args:     log_level: Log level to use for audit events (default: INFO)位于第 31–37 行。
    - `async log_event(event)`：记录审计或日志。`event`（该函数的业务参数，详见源码类型标注）。Log an audit event as structured JSON.  Args:     event: The audit event to log位于第 39–59 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/local/file_system.py`

- **目录**：`src/vanna/integrations/local`。**文件名**：`file_system.py`。**语言/类型**：Python。**行数**：243。**字节**：8476。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Local file system implementation.

This module provides a local file system implementation with per-user isolation.
- **结构摘要**：类 1 个，模块函数 0 个，方法 10 个。
  - **类 `LocalFileSystem`**（核心类型或辅助类，基类：FileSystem，行 18–242）：Local file system implementation with per-user isolation.
    - `__init__(working_directory)`：提供对象协议或生命周期约定。`working_directory`（该函数的业务参数，详见源码类型标注）。Initialize with a working directory.  Args:     working_directory: Base directory where user-specific folders will be cr位于第 21–27 行。
    - `_get_user_directory(context)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Get the user-specific directory by hashing the user ID.  Args:     context: Tool context containing user information  Re位于第 29–45 行。
    - `_resolve_path(path, context)`：解析并还原。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Resolve a path relative to the user's directory.  Args:     path: Path relative to user directory     context: Tool cont位于第 47–68 行。
    - `async list_files(directory, context)`：枚举并列出。`directory`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。List files in a directory within the user's isolated space.位于第 70–85 行。
    - `async read_file(filename, context)`：完成该模块中的具体处理。`filename`（文件名）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Read the contents of a file within the user's isolated space.位于第 87–97 行。
    - `async write_file(filename, content, context, overwrite)`：完成该模块中的具体处理。`filename`（文件名）、`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`overwrite`（该函数的业务参数，详见源码类型标注）。Write content to a file within the user's isolated space.位于第 99–113 行。
    - `async exists(path, context)`：完成该模块中的具体处理。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a file or directory exists within the user's isolated space.位于第 115–121 行。
    - `async is_directory(path, context)`：判断是否满足条件。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a path is a directory within the user's isolated space.位于第 123–129 行。
    - `async search_files(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for files within the user's isolated space.位于第 131–205 行。
    - `async run_bash(command, context)`：运行。`command`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute a bash command within the user's isolated space.位于第 207–242 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/local/file_system_conversation_store.py`

- **目录**：`src/vanna/integrations/local`。**文件名**：`file_system_conversation_store.py`。**语言/类型**：Python。**行数**：256。**字节**：9133。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：File system conversation store implementation.

This module provides a file-based implementation of the ConversationStore
interface that persists conversations to disk as a directory structure.
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `FileSystemConversationStore`**（核心类型或辅助类，基类：ConversationStore，行 19–255）：File system-based conversation store.  Stores conversations as directories with individual message files: conversations/{conversation_id}/     metadata.json - conversation metadata (id, user info, timestamps)     message
    - `__init__(base_dir)`：提供对象协议或生命周期约定。`base_dir`（该函数的业务参数，详见源码类型标注）。Initialize the file system conversation store.  Args:     base_dir: Base directory for storing conversations位于第 29–36 行。
    - `_get_conversation_dir(conversation_id)`：读取并返回。`conversation_id`（会话标识，空则新建）。Get the directory path for a conversation.位于第 38–40 行。
    - `_get_metadata_path(conversation_id)`：读取并返回。`conversation_id`（会话标识，空则新建）。Get the metadata file path for a conversation.位于第 42–44 行。
    - `_get_messages_dir(conversation_id)`：读取并返回。`conversation_id`（会话标识，空则新建）。Get the messages directory for a conversation.位于第 46–48 行。
    - `_save_metadata(conversation)`：持久化保存。`conversation`（会话聚合根）。Save conversation metadata to disk.位于第 50–64 行。
    - `_load_messages(conversation_id)`：加载。`conversation_id`（会话标识，空则新建）。Load all messages for a conversation.位于第 66–87 行。
    - `_append_message(conversation_id, message, index)`：完成该模块中的具体处理。`conversation_id`（会话标识，空则新建）、`message`（用户自然语言输入）、`index`（该函数的业务参数，详见源码类型标注）。Append a message to the conversation.位于第 89–102 行。
    - `async create_conversation(conversation_id, user, initial_message)`：创建并初始化。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）、`initial_message`（该函数的业务参数，详见源码类型标注）。Create a new conversation with the specified ID.位于第 104–120 行。
    - `async get_conversation(conversation_id, user)`：读取并返回。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）。Get conversation by ID, scoped to user.位于第 122–155 行。
    - `async update_conversation(conversation)`：更新。`conversation`（会话聚合根）。Update conversation with new messages.位于第 157–173 行。
    - `async delete_conversation(conversation_id, user)`：删除并清理。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）。Delete conversation.位于第 175–206 行。
    - `async list_conversations(user, limit, offset)`：枚举并列出。`user`（当前用户对象，携带 id 与 group_memberships）、`limit`（返回条数上限）、`offset`（该函数的业务参数，详见源码类型标注）。List conversations for user.位于第 208–255 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/local/storage.py`

- **目录**：`src/vanna/integrations/local`。**文件名**：`storage.py`。**语言/类型**：Python。**行数**：63。**字节**：2272。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：In-memory conversation store implementation.

This module provides a simple in-memory implementation of the ConversationStore
interface, useful for testing and development.
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `MemoryConversationStore`**（核心类型或辅助类，基类：ConversationStore，行 14–62）：In-memory conversation store.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 17–18 行。
    - `async create_conversation(conversation_id, user, initial_message)`：创建并初始化。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）、`initial_message`（该函数的业务参数，详见源码类型标注）。Create a new conversation with the specified ID.位于第 20–30 行。
    - `async get_conversation(conversation_id, user)`：读取并返回。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）。Get conversation by ID, scoped to user.位于第 32–39 行。
    - `async update_conversation(conversation)`：更新。`conversation`（会话聚合根）。Update conversation with new messages.位于第 41–43 行。
    - `async delete_conversation(conversation_id, user)`：删除并清理。`conversation_id`（会话标识，空则新建）、`user`（当前用户对象，携带 id 与 group_memberships）。Delete conversation.位于第 45–51 行。
    - `async list_conversations(user, limit, offset)`：枚举并列出。`user`（当前用户对象，携带 id 与 group_memberships）、`limit`（返回条数上限）、`offset`（该函数的业务参数，详见源码类型标注）。List conversations for user.位于第 53–62 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/marqo/__init__.py`

- **目录**：`src/vanna/integrations/marqo`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：120。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Marqo integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/marqo/agent_memory.py`

- **目录**：`src/vanna/integrations/marqo`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：355。**字节**：11276。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Marqo vector database implementation of AgentMemory.

This implementation uses Marqo for vector storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 11 个。
  - **类 `MarqoAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 31–354）：Marqo-based implementation of AgentMemory.
    - `__init__(url, index_name, api_key)`：提供对象协议或生命周期约定。`url`（该函数的业务参数，详见源码类型标注）、`index_name`（该函数的业务参数，详见源码类型标注）、`api_key`（供应商密钥）。位于第 34–49 行。
    - `_get_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create Marqo client.位于第 51–62 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 64–95 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 97–147 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 149–182 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 184–196 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 198–220 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 222–260 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 262–290 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 292–304 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 306–354 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/milvus/__init__.py`

- **目录**：`src/vanna/integrations/milvus`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：123。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Milvus integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/milvus/agent_memory.py`

- **目录**：`src/vanna/integrations/milvus`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：459。**字节**：14975。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Milvus vector database implementation of AgentMemory.

This implementation uses Milvus for distributed vector storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `MilvusAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 38–458）：Milvus-based implementation of AgentMemory.
    - `__init__(collection_name, host, port, alias, dimension)`：提供对象协议或生命周期约定。`collection_name`（该函数的业务参数，详见源码类型标注）、`host`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）、`alias`（该函数的业务参数，详见源码类型标注）、`dimension`（该函数的业务参数，详见源码类型标注）。位于第 41–60 行。
    - `_get_collection()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create Milvus collection.位于第 62–118 行。
    - `_create_embedding(text)`：创建并初始化。`text`（该函数的业务参数，详见源码类型标注）。Create a simple embedding from text (placeholder).位于第 120–125 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 127–159 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 161–232 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 234–282 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 284–297 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 299–325 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 327–380 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 382–415 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 417–430 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 432–458 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/mock/__init__.py`

- **目录**：`src/vanna/integrations/mock`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：10。**字节**：145。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Mock integration.

This module provides mock implementations for testing.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/mock/llm.py`

- **目录**：`src/vanna/integrations/mock`。**文件名**：`llm.py`。**语言/类型**：Python。**行数**：66。**字节**：2215。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Mock LLM service implementation for testing.

This module provides a simple mock implementation of the LlmService interface,
useful for testing and development without requiring actual LLM API calls.
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `MockLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 15–65）：Mock LLM service that returns predefined responses.
    - `__init__(response_content)`：提供对象协议或生命周期约定。`response_content`（该函数的业务参数，详见源码类型标注）。位于第 18–20 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Send a request to the mock LLM.位于第 22–34 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Stream a request to the mock LLM.位于第 36–52 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Validate tool schemas and return any errors.位于第 54–57 行。
    - `set_response(content)`：设置并更新。`content`（文本内容）。Set the response content for testing.位于第 59–61 行。
    - `reset_call_count()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Reset the call counter.位于第 63–65 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/mssql/__init__.py`

- **目录**：`src/vanna/integrations/mssql`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：114。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Microsoft SQL Server integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/mssql/sql_runner.py`

- **目录**：`src/vanna/integrations/mssql`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：67。**字节**：2142。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Microsoft SQL Server implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `MSSQLRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–66）：Microsoft SQL Server implementation of the SqlRunner interface.
    - `__init__(odbc_conn_str)`：提供对象协议或生命周期约定。`odbc_conn_str`（该函数的业务参数，详见源码类型标注）。Initialize with MSSQL connection parameters.  Args:     odbc_conn_str: The ODBC connection string for SQL Server     **k位于第 13–48 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against MSSQL database and return results as DataFrame.  Args:     args: SQL query arguments     conte位于第 50–66 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/mysql/__init__.py`

- **目录**：`src/vanna/integrations/mysql`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：99。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：MySQL integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/mysql/sql_runner.py`

- **目录**：`src/vanna/integrations/mysql`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：93。**字节**：2534。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：MySQL implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `MySQLRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–92）：MySQL implementation of the SqlRunner interface.
    - `__init__(host, database, user, password, port)`：提供对象协议或生命周期约定。`host`（该函数的业务参数，详见源码类型标注）、`database`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）。Initialize with MySQL connection parameters.  Args:     host: Database host address     database: Database name     user位于第 13–46 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against MySQL database and return results as DataFrame.  Args:     args: SQL query arguments     conte位于第 48–92 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/ollama/__init__.py`

- **目录**：`src/vanna/integrations/ollama`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：112。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Ollama integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/ollama/llm.py`

- **目录**：`src/vanna/integrations/ollama`。**文件名**：`llm.py`。**语言/类型**：Python。**行数**：253。**字节**：8668。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Ollama LLM service implementation.

This module provides an implementation of the LlmService interface backed by
Ollama's local LLM API. It supports non-streaming responses and streaming
of text content. Tool calling support depends on the Ollama model being used.
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `OllamaLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 24–252）：Ollama-backed LLM service for local model inference.  Args:     model: Ollama model name (e.g., "gpt-oss:20b").     host: Ollama server URL; defaults to "http://localhost:11434" or env `OLLAMA_HOST`.     timeout: Request
    - `__init__(model, host, timeout, num_ctx, temperature)`：提供对象协议或生命周期约定。`model`（模型名称）、`host`（该函数的业务参数，详见源码类型标注）、`timeout`（该函数的业务参数，详见源码类型标注）、`num_ctx`（该函数的业务参数，详见源码类型标注）、`temperature`（该函数的业务参数，详见源码类型标注）。位于第 36–63 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Send a non-streaming request to Ollama and return the response.位于第 65–96 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Stream a request to Ollama.  Emits `LlmStreamChunk` for textual deltas as they arrive. Tool calls are accumulated and em位于第 98–142 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Validate tool schemas. Returns a list of error messages.位于第 144–153 行。
    - `_build_payload(request)`：组装并构建。`request`（下游请求对象）。Build the Ollama chat payload from LlmRequest.位于第 156–214 行。
    - `_extract_tool_calls_from_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。Extract tool calls from Ollama message.位于第 216–252 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/openai/__init__.py`

- **目录**：`src/vanna/integrations/openai`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：11。**字节**：225。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：OpenAI integration.

This module provides OpenAI LLM service implementations.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/openai/llm.py`

- **目录**：`src/vanna/integrations/openai`。**文件名**：`llm.py`。**语言/类型**：Python。**行数**：268。**字节**：10127。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：OpenAI LLM service implementation.

This module provides an implementation of the LlmService interface backed by
OpenAI's Chat Completions API (openai>=1.0.0). It supports non-streaming
responses and best-effort streaming of text content. Tool/function calling is
passed through when tools are provided, but full tool-call conversation
round-tripping may require adding assistant tool-call messages t
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `OpenAILlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 27–267）：OpenAI Chat Completions-backed LLM service.  Args:     model: OpenAI model name (e.g., "gpt-5").     api_key: API key; falls back to env `OPENAI_API_KEY`.     organization: Optional org; env `OPENAI_ORG` if unset.     ba
    - `__init__(model, api_key, organization, base_url)`：提供对象协议或生命周期约定。`model`（模型名称）、`api_key`（供应商密钥）、`organization`（该函数的业务参数，详见源码类型标注）、`base_url`（该函数的业务参数，详见源码类型标注）。位于第 38–66 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Send a non-streaming request to OpenAI and return the response.位于第 68–98 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Stream a request to OpenAI.  Emits `LlmStreamChunk` for textual deltas as they arrive. Tool-calls are accumulated and em位于第 100–178 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Validate tool schemas. Returns a list of error messages.位于第 180–187 行。
    - `_build_payload(request)`：组装并构建。`request`（下游请求对象）。位于第 190–242 行。
    - `_extract_tool_calls_from_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 244–267 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/openai/responses.py`

- **目录**：`src/vanna/integrations/openai`。**文件名**：`responses.py`。**语言/类型**：Python。**行数**：164。**字节**：6609。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **结构摘要**：类 1 个，模块函数 0 个，方法 8 个。
  - **类 `OpenAIResponsesService`**（抽象或具体服务/工具实现，基类：LlmService，行 14–163）：职责见方法列表。
    - `__init__(api_key, model)`：提供对象协议或生命周期约定。`api_key`（供应商密钥）、`model`（模型名称）。位于第 15–27 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。位于第 29–40 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。位于第 42–58 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。位于第 60–61 行。
    - `_payload(request)`：完成该模块中的具体处理。`request`（下游请求对象）。位于第 65–74 行。
    - `_debug_print(label, obj)`：完成该模块中的具体处理。`label`（该函数的业务参数，详见源码类型标注）、`obj`（该函数的业务参数，详见源码类型标注）。位于第 76–84 行。
    - `_extract(resp)`：完成该模块中的具体处理。`resp`（该函数的业务参数，详见源码类型标注）。位于第 86–123 行。
    - `_serialize_tool(tool)`：完成该模块中的具体处理。`tool`（工具实例）。Convert a tool schema into the dict format expected by OpenAI Responses.位于第 125–163 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/opensearch/__init__.py`

- **目录**：`src/vanna/integrations/opensearch`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：135。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：OpenSearch integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/opensearch/agent_memory.py`

- **目录**：`src/vanna/integrations/opensearch`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：412。**字节**：13654。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：OpenSearch vector database implementation of AgentMemory.

This implementation uses OpenSearch for distributed search and storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `OpenSearchAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 31–411）：OpenSearch-based implementation of AgentMemory.
    - `__init__(index_name, hosts, http_auth, use_ssl, verify_certs, dimension)`：提供对象协议或生命周期约定。`index_name`（该函数的业务参数，详见源码类型标注）、`hosts`（该函数的业务参数，详见源码类型标注）、`http_auth`（该函数的业务参数，详见源码类型标注）、`use_ssl`（该函数的业务参数，详见源码类型标注）、`verify_certs`（该函数的业务参数，详见源码类型标注）、`dimension`（该函数的业务参数，详见源码类型标注）。位于第 34–55 行。
    - `_get_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create OpenSearch client.位于第 57–97 行。
    - `_create_embedding(text)`：创建并初始化。`text`（该函数的业务参数，详见源码类型标注）。Create a simple embedding from text (placeholder).位于第 99–104 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 106–139 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 141–203 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 205–240 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 242–254 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 256–280 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 282–333 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 335–366 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 368–380 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 382–411 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/oracle/__init__.py`

- **目录**：`src/vanna/integrations/oracle`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：102。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Oracle integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/oracle/sql_runner.py`

- **目录**：`src/vanna/integrations/oracle`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：76。**字节**：2272。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Oracle implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `OracleRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–75）：Oracle implementation of the SqlRunner interface.
    - `__init__(user, password, dsn)`：提供对象协议或生命周期约定。`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`dsn`（该函数的业务参数，详见源码类型标注）。Initialize with Oracle connection parameters.  Args:     user: Oracle database user name     password: Oracle database u位于第 13–34 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against Oracle database and return results as DataFrame.  Args:     args: SQL query arguments     cont位于第 36–75 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/pinecone/__init__.py`

- **目录**：`src/vanna/integrations/pinecone`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：129。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Pinecone integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/pinecone/agent_memory.py`

- **目录**：`src/vanna/integrations/pinecone`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：330。**字节**：10759。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Pinecone vector database implementation of AgentMemory.

This implementation uses Pinecone for cloud-based vector storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 13 个。
  - **类 `PineconeAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 31–329）：Pinecone-based implementation of AgentMemory.
    - `__init__(api_key, index_name, environment, dimension, metric)`：提供对象协议或生命周期约定。`api_key`（供应商密钥）、`index_name`（该函数的业务参数，详见源码类型标注）、`environment`（该函数的业务参数，详见源码类型标注）、`dimension`（该函数的业务参数，详见源码类型标注）、`metric`（该函数的业务参数，详见源码类型标注）。位于第 34–54 行。
    - `_get_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create Pinecone client.位于第 56–60 行。
    - `_get_index()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create Pinecone index.位于第 62–77 行。
    - `_create_embedding(text)`：创建并初始化。`text`（该函数的业务参数，详见源码类型标注）。Create a simple embedding from text (placeholder - should use actual embedding model).位于第 79–85 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 87–117 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 119–172 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 174–190 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 192–204 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 206–226 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 228–270 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 272–285 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 287–299 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 301–329 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/plotly/__init__.py`

- **目录**：`src/vanna/integrations/plotly`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：134。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Plotly integration for chart generation.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/plotly/chart_generator.py`

- **目录**：`src/vanna/integrations/plotly`。**文件名**：`chart_generator.py`。**语言/类型**：Python。**行数**：314。**字节**：11289。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Plotly-based chart generator with automatic chart type selection.
- **结构摘要**：类 1 个，模块函数 0 个，方法 10 个。
  - **类 `PlotlyChartGenerator`**（核心类型或辅助类，基类：无，行 11–313）：Generate Plotly charts using heuristics based on DataFrame characteristics.
    - `generate_chart(df, title)`：生成。`df`（pandas.DataFrame）、`title`（该函数的业务参数，详见源码类型标注）。Generate a Plotly chart based on DataFrame shape and types.  Heuristics: - 4+ columns: table - 1 numeric column: histogr位于第 26–103 行。
    - `_apply_standard_layout(fig)`：完成该模块中的具体处理。`fig`（该函数的业务参数，详见源码类型标注）。Apply consistent Vanna brand styling to all charts.  Uses Vanna brand colors from the landing page for a cohesive look. 位于第 105–124 行。
    - `_create_histogram(df, column, title)`：创建并初始化。`df`（pandas.DataFrame）、`column`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。Create a histogram for a single numeric column.位于第 126–136 行。
    - `_create_bar_chart(df, x_col, y_col, title)`：创建并初始化。`df`（pandas.DataFrame）、`x_col`（该函数的业务参数，详见源码类型标注）、`y_col`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。Create a bar chart for categorical vs numeric data.位于第 138–153 行。
    - `_create_scatter_plot(df, x_col, y_col, title)`：创建并初始化。`df`（pandas.DataFrame）、`x_col`（该函数的业务参数，详见源码类型标注）、`y_col`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。Create a scatter plot for two numeric columns.位于第 155–168 行。
    - `_create_correlation_heatmap(df, columns, title)`：创建并初始化。`df`（pandas.DataFrame）、`columns`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。Create a correlation heatmap for multiple numeric columns.位于第 170–195 行。
    - `_create_time_series_chart(df, time_col, value_cols, title)`：创建并初始化。`df`（pandas.DataFrame）、`time_col`（该函数的业务参数，详见源码类型标注）、`value_cols`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。Create a time series line chart.位于第 197–222 行。
    - `_create_grouped_bar_chart(df, categorical_cols, title)`：创建并初始化。`df`（pandas.DataFrame）、`categorical_cols`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。Create a grouped bar chart for multiple categorical columns.位于第 224–255 行。
    - `_create_generic_chart(df, col1, col2, title)`：创建并初始化。`df`（pandas.DataFrame）、`col1`（该函数的业务参数，详见源码类型标注）、`col2`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）。Create a generic chart for any two columns.位于第 257–276 行。
    - `_create_table(df, title)`：创建并初始化。`df`（pandas.DataFrame）、`title`（该函数的业务参数，详见源码类型标注）。Create a Plotly table for DataFrames with 4 or more columns.位于第 278–313 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/postgres/__init__.py`

- **目录**：`src/vanna/integrations/postgres`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：10。**字节**：158。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：PostgreSQL integration.

This module provides PostgreSQL runner implementation.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/postgres/sql_runner.py`

- **目录**：`src/vanna/integrations/postgres`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：113。**字节**：3926。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：PostgreSQL implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `PostgresRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–112）：PostgreSQL implementation of the SqlRunner interface.
    - `__init__(connection_string, host, port, database, user, password)`：提供对象协议或生命周期约定。`connection_string`（该函数的业务参数，详见源码类型标注）、`host`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）、`database`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）。Initialize with PostgreSQL connection parameters.  You can either provide a connection_string OR individual parameters (位于第 13–63 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against PostgreSQL database and return results as DataFrame.  Args:     args: SQL query arguments     位于第 65–112 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/premium/agent_memory/__init__.py`

- **目录**：`src/vanna/integrations/premium/agent_memory`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：121。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Cloud-based agent memory implementations.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/premium/agent_memory/premium.py`

- **目录**：`src/vanna/integrations/premium/agent_memory`。**文件名**：`premium.py`。**语言/类型**：Python。**行数**：187。**字节**：5840。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Cloud-based implementation of AgentMemory.

This implementation uses Vanna's premium cloud service for storing and searching
tool usage patterns with advanced similarity search and analytics.
- **结构摘要**：类 1 个，模块函数 0 个，方法 11 个。
  - **类 `CloudAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 23–186）：Cloud-based implementation of AgentMemory.
    - `__init__(api_base_url, api_key, organization_id)`：提供对象协议或生命周期约定。`api_base_url`（该函数的业务参数，详见源码类型标注）、`api_key`（供应商密钥）、`organization_id`（该函数的业务参数，详见源码类型标注）。位于第 26–35 行。
    - `_get_headers()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get request headers with authentication.位于第 37–44 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern to premium cloud storage.位于第 46–71 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns in premium cloud storage.位于第 73–108 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories from premium cloud storage.位于第 110–128 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID from premium cloud storage.位于第 130–140 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Cloud implementation does not yet support text memories.位于第 142–144 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Cloud implementation does not yet support text memories.位于第 146–155 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Cloud implementation does not yet support text memories.位于第 157–161 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Cloud implementation does not yet support text memories.位于第 163–165 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories from premium cloud storage.位于第 167–186 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/presto/__init__.py`

- **目录**：`src/vanna/integrations/presto`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：102。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Presto integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/presto/sql_runner.py`

- **目录**：`src/vanna/integrations/presto`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：108。**字节**：3556。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Presto implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `PrestoRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–107）：Presto implementation of the SqlRunner interface.
    - `__init__(host, catalog, schema, user, password, port, combined_pem_path, protocol, requests_kwargs)`：提供对象协议或生命周期约定。`host`（该函数的业务参数，详见源码类型标注）、`catalog`（该函数的业务参数，详见源码类型标注）、`schema`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）、`combined_pem_path`（该函数的业务参数，详见源码类型标注）、`protocol`（该函数的业务参数，详见源码类型标注）、`requests_kwargs`（该函数的业务参数，详见源码类型标注）。Initialize with Presto connection parameters.  Args:     host: The host address of the Presto database     catalog: The 位于第 13–62 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against Presto database and return results as DataFrame.  Args:     args: SQL query arguments     cont位于第 64–107 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/qdrant/__init__.py`

- **目录**：`src/vanna/integrations/qdrant`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：123。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Qdrant integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/qdrant/agent_memory.py`

- **目录**：`src/vanna/integrations/qdrant`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：467。**字节**：15752。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Qdrant vector database implementation of AgentMemory.

This implementation uses Qdrant for vector storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `QdrantAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 39–466）：Qdrant-based implementation of AgentMemory.
    - `__init__(collection_name, url, path, api_key, dimension)`：提供对象协议或生命周期约定。`collection_name`（该函数的业务参数，详见源码类型标注）、`url`（该函数的业务参数，详见源码类型标注）、`path`（该函数的业务参数，详见源码类型标注）、`api_key`（供应商密钥）、`dimension`（该函数的业务参数，详见源码类型标注）。位于第 42–61 行。
    - `_get_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create Qdrant client.位于第 63–80 行。
    - `_create_embedding(text)`：创建并初始化。`text`（该函数的业务参数，详见源码类型标注）。Create a simple embedding from text (placeholder).位于第 82–87 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 89–120 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 122–192 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 194–238 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID. Returns True if deleted, False if not found.位于第 240–265 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 267–289 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 291–349 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 351–393 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 395–420 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 422–466 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/snowflake/__init__.py`

- **目录**：`src/vanna/integrations/snowflake`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：6。**字节**：111。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Snowflake integration for Vanna.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/snowflake/sql_runner.py`

- **目录**：`src/vanna/integrations/snowflake`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：148。**字节**：5303。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Snowflake implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `SnowflakeRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 11–147）：Snowflake implementation of the SqlRunner interface.
    - `__init__(account, username, password, database, role, warehouse, private_key_path, private_key_passphrase, private_key_content)`：提供对象协议或生命周期约定。`account`（该函数的业务参数，详见源码类型标注）、`username`（该函数的业务参数，详见源码类型标注）、`password`（该函数的业务参数，详见源码类型标注）、`database`（该函数的业务参数，详见源码类型标注）、`role`（该函数的业务参数，详见源码类型标注）、`warehouse`（该函数的业务参数，详见源码类型标注）、`private_key_path`（该函数的业务参数，详见源码类型标注）、`private_key_passphrase`（该函数的业务参数，详见源码类型标注）、`private_key_content`（该函数的业务参数，详见源码类型标注）。Initialize with Snowflake connection parameters.  Args:     account: Snowflake account identifier     username: Database位于第 14–75 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against Snowflake database and return results as DataFrame.  Args:     args: SQL query arguments     c位于第 77–147 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/sqlite/__init__.py`

- **目录**：`src/vanna/integrations/sqlite`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：10。**字节**：146。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：SQLite integration.

This module provides SQLite runner implementation.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/sqlite/sql_runner.py`

- **目录**：`src/vanna/integrations/sqlite`。**文件名**：`sql_runner.py`。**语言/类型**：Python。**行数**：66。**字节**：2118。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：SQLite implementation of SqlRunner interface.
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `SqliteRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 10–65）：SQLite implementation of the SqlRunner interface.
    - `__init__(database_path)`：提供对象协议或生命周期约定。`database_path`（该函数的业务参数，详见源码类型标注）。Initialize with a SQLite database path.  Args:     database_path: Path to the SQLite database file位于第 13–19 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query against SQLite database and return results as DataFrame.  Args:     args: SQL query arguments     cont位于第 21–65 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/weaviate/__init__.py`

- **目录**：`src/vanna/integrations/weaviate`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：129。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Weaviate integration for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/integrations/weaviate/agent_memory.py`

- **目录**：`src/vanna/integrations/weaviate`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：429。**字节**：15027。
- **主要功能简介**：第三方集成：LLM、数据库、向量库、本地存储、Plotly。
- **模块文档字符串**：Weaviate vector database implementation of AgentMemory.

This implementation uses Weaviate for semantic search and storage of tool usage patterns.
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `WeaviateAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 36–428）：Weaviate-based implementation of AgentMemory.
    - `__init__(collection_name, url, api_key, dimension)`：提供对象协议或生命周期约定。`collection_name`（该函数的业务参数，详见源码类型标注）、`url`（该函数的业务参数，详见源码类型标注）、`api_key`（供应商密钥）、`dimension`（该函数的业务参数，详见源码类型标注）。位于第 39–56 行。
    - `_get_client()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Get or create Weaviate client.位于第 58–86 行。
    - `_create_embedding(text)`：创建并初始化。`text`（该函数的业务参数，详见源码类型标注）。Create a simple embedding from text (placeholder).位于第 88–93 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern.位于第 95–127 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns.位于第 129–189 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.位于第 191–232 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID.位于第 234–247 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save a text memory.位于第 249–275 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar text memories.位于第 277–326 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added text memories.位于第 328–369 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID.位于第 371–384 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.位于第 386–428 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/ZhipuAI/ZhipuAI_Chat.py`

- **目录**：`src/vanna/legacy/ZhipuAI`。**文件名**：`ZhipuAI_Chat.py`。**语言/类型**：Python。**行数**：234。**字节**：8725。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 15 个。
  - **类 `ZhipuAI_Chat`**（核心类型或辅助类，基类：VannaBase，行 10–233）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 11–19 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 23–24 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 27–28 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 31–32 行。
    - `str_to_approx_token_count(string)`：完成该模块中的具体处理。`string`（该函数的业务参数，详见源码类型标注）。位于第 35–36 行。
    - `add_ddl_to_prompt(initial_prompt, ddl_list, max_tokens)`：完成该模块中的具体处理。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`ddl_list`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）。位于第 39–53 行。
    - `add_documentation_to_prompt(initial_prompt, documentation_List, max_tokens)`：完成该模块中的具体处理。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`documentation_List`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）。位于第 56–70 行。
    - `add_sql_to_prompt(initial_prompt, sql_List, max_tokens)`：完成该模块中的具体处理。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`sql_List`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）。位于第 73–87 行。
    - `get_sql_prompt(question, question_sql_list, ddl_list, doc_list)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）、`question_sql_list`（该函数的业务参数，详见源码类型标注）、`ddl_list`（该函数的业务参数，详见源码类型标注）、`doc_list`（该函数的业务参数，详见源码类型标注）。位于第 89–119 行。
    - `get_followup_questions_prompt(question, df, question_sql_list, ddl_list, doc_list)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）、`df`（pandas.DataFrame）、`question_sql_list`（该函数的业务参数，详见源码类型标注）、`ddl_list`（该函数的业务参数，详见源码类型标注）、`doc_list`（该函数的业务参数，详见源码类型标注）。位于第 121–151 行。
    - `generate_question(sql)`：生成。`sql`（待执行的 SQL 文本）。位于第 153–164 行。
    - `_extract_python_code(markdown_string)`：完成该模块中的具体处理。`markdown_string`（该函数的业务参数，详见源码类型标注）。位于第 166–182 行。
    - `_sanitize_plotly_code(raw_plotly_code)`：完成该模块中的具体处理。`raw_plotly_code`（该函数的业务参数，详见源码类型标注）。位于第 184–188 行。
    - `generate_plotly_code(question, sql, df_metadata)`：生成。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`df_metadata`（该函数的业务参数，详见源码类型标注）。位于第 190–212 行。
    - `submit_prompt(prompt, max_tokens, temperature, top_p, stop)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）、`temperature`（该函数的业务参数，详见源码类型标注）、`top_p`（该函数的业务参数，详见源码类型标注）、`stop`（该函数的业务参数，详见源码类型标注）。位于第 214–233 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/ZhipuAI/ZhipuAI_embeddings.py`

- **目录**：`src/vanna/legacy/ZhipuAI`。**文件名**：`ZhipuAI_embeddings.py`。**语言/类型**：Python。**行数**：80。**字节**：2790。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 2 个，模块函数 0 个，方法 4 个。
  - **类 `ZhipuAI_Embeddings`**（核心类型或辅助类，基类：VannaBase，行 7–28）：[future functionality] This function is used to generate embeddings from ZhipuAI.  Args:     VannaBase (_type_): _description_
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 15–20 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 22–28 行。
  - **类 `ZhipuAIEmbeddingFunction`**（核心类型或辅助类，基类：EmbeddingFunction[Documents]，行 31–79）：A embeddingFunction that uses ZhipuAI to generate embeddings which can use in chromadb. usage: class MyVanna(ChromaDB_VectorStore, ZhipuAI_Chat):     def __init__(self, config=None):         ChromaDB_VectorStore.__init__
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 48–58 行。
    - `__call__(input)`：提供对象协议或生命周期约定。`input`（该函数的业务参数，详见源码类型标注）。位于第 60–79 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/ZhipuAI/__init__.py`

- **目录**：`src/vanna/legacy/ZhipuAI`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：3。**字节**：116。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/__init__.py`

- **目录**：`src/vanna/legacy`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：404。**字节**：9256。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 2 个，模块函数 38 个，方法 6 个。
  - **类 `TrainingPlanItem`**（核心类型或辅助类，基类：无，行 173–189）：职责见方法列表。
    - `__str__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 179–185 行。
  - **类 `TrainingPlan`**（核心类型或辅助类，基类：无，行 192–250）：A class representing a training plan. You can see what's in it, and remove items from it that you don't want trained.  **Example:** ```python plan = vn.get_training_plan()  plan.get_summary() ```
    - `__init__(plan)`：提供对象协议或生命周期约定。`plan`（该函数的业务参数，详见源码类型标注）。位于第 207–208 行。
    - `__str__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 210–211 行。
    - `__repr__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 213–214 行。
    - `get_summary()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。**Example:** ```python plan = vn.get_training_plan()  plan.get_summary() ```  Get a summary of the training plan.  Retur位于第 216–231 行。
    - `remove_item(item)`：移除。`item`（该函数的业务参数，详见源码类型标注）。**Example:** ```python plan = vn.get_training_plan()  plan.remove_item("Train on SQL: What is the average salary of empl位于第 233–250 行。
  - **函数 `error_deprecation`**：完成该模块中的具体处理。无显式位置参数。行 47–57。
  - **函数 `__unauthenticated_rpc_call`**：提供对象协议或生命周期约定。`method`（该函数的业务参数，详见源码类型标注）、`params`（该函数的业务参数，详见源码类型标注）。行 60–69。
  - **函数 `__dataclass_to_dict`**：提供对象协议或生命周期约定。`obj`（该函数的业务参数，详见源码类型标注）。行 72–73。
  - **函数 `get_api_key`**：读取并返回。`email`（该函数的业务参数，详见源码类型标注）、`otp_code`（该函数的业务参数，详见源码类型标注）。**Example:** ```python vn.get_api_key(email="my-email@example.com") ```  Login to the Vanna.AI API.  Args:     email (str): The email address to login with.    行 76–131。
  - **函数 `set_api_key`**：设置并更新。`key`（该函数的业务参数，详见源码类型标注）。行 134–135。
  - **函数 `get_models`**：读取并返回。无显式位置参数。行 138–139。
  - **函数 `create_model`**：创建并初始化。`model`（模型名称）、`db_type`（该函数的业务参数，详见源码类型标注）。行 142–143。
  - **函数 `add_user_to_model`**：完成该模块中的具体处理。`model`（模型名称）、`email`（该函数的业务参数，详见源码类型标注）、`is_admin`（该函数的业务参数，详见源码类型标注）。行 146–147。
  - **函数 `update_model_visibility`**：更新。`public`（该函数的业务参数，详见源码类型标注）。行 150–151。
  - **函数 `set_model`**：设置并更新。`model`（模型名称）。行 154–155。
  - **函数 `add_sql`**：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`tag`（该函数的业务参数，详见源码类型标注）。行 158–161。
  - **函数 `add_ddl`**：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。行 164–165。
  - **函数 `add_documentation`**：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。行 168–169。
  - **函数 `get_training_plan_postgres`**：读取并返回。`filter_databases`（该函数的业务参数，详见源码类型标注）、`filter_schemas`（该函数的业务参数，详见源码类型标注）、`include_information_schema`（该函数的业务参数，详见源码类型标注）、`use_historical_queries`（该函数的业务参数，详见源码类型标注）。行 253–259。
  - **函数 `get_training_plan_generic`**：读取并返回。`df`（pandas.DataFrame）。行 262–263。
  - **函数 `get_training_plan_experimental`**：读取并返回。`filter_databases`（该函数的业务参数，详见源码类型标注）、`filter_schemas`（该函数的业务参数，详见源码类型标注）、`include_information_schema`（该函数的业务参数，详见源码类型标注）、`use_historical_queries`（该函数的业务参数，详见源码类型标注）。行 266–272。
  - **函数 `train`**：写入训练或记忆样本。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`ddl`（该函数的业务参数，详见源码类型标注）、`documentation`（该函数的业务参数，详见源码类型标注）、`json_file`（该函数的业务参数，详见源码类型标注）、`sql_file`（该函数的业务参数，详见源码类型标注）、`plan`（该函数的业务参数，详见源码类型标注）。行 275–284。
  - **函数 `flag_sql_for_review`**：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`error_msg`（该函数的业务参数，详见源码类型标注）。行 287–290。
  - **函数 `remove_sql`**：移除。`question`（该函数的业务参数，详见源码类型标注）。行 293–294。
  - **函数 `remove_training_data`**：移除。`id`（该函数的业务参数，详见源码类型标注）。行 297–298。
  - **函数 `generate_sql`**：生成。`question`（该函数的业务参数，详见源码类型标注）。行 301–302。
  - **函数 `get_related_training_data`**：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。行 305–306。
  - **函数 `generate_meta`**：生成。`question`（该函数的业务参数，详见源码类型标注）。行 309–310。
  - **函数 `generate_followup_questions`**：生成。`question`（该函数的业务参数，详见源码类型标注）、`df`（pandas.DataFrame）。行 313–314。
  - **函数 `generate_questions`**：生成。无显式位置参数。行 317–318。
  - **函数 `ask`**：向模型提问。`question`（该函数的业务参数，详见源码类型标注）、`print_results`（该函数的业务参数，详见源码类型标注）、`auto_train`（该函数的业务参数，详见源码类型标注）、`generate_followups`（该函数的业务参数，详见源码类型标注）。行 321–335。
  - **函数 `generate_plotly_code`**：生成。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`df`（pandas.DataFrame）、`chart_instructions`（该函数的业务参数，详见源码类型标注）。行 338–344。
  - **函数 `get_plotly_figure`**：读取并返回。`plotly_code`（该函数的业务参数，详见源码类型标注）、`df`（pandas.DataFrame）、`dark_mode`（该函数的业务参数，详见源码类型标注）。行 347–350。
  - **函数 `get_results`**：读取并返回。`cs`（该函数的业务参数，详见源码类型标注）、`default_database`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。行 353–354。
  - **函数 `generate_explanation`**：生成。`sql`（待执行的 SQL 文本）。行 357–358。
  - **函数 `generate_question`**：生成。`sql`（待执行的 SQL 文本）。行 361–362。
  - **函数 `get_all_questions`**：读取并返回。无显式位置参数。行 365–366。
  - **函数 `get_training_data`**：读取并返回。无显式位置参数。行 369–370。
  - **函数 `connect_to_sqlite`**：建立连接。`url`（该函数的业务参数，详见源码类型标注）。行 373–374。
  - **函数 `connect_to_snowflake`**：建立连接。`account`（该函数的业务参数，详见源码类型标注）、`username`（该函数的业务参数，详见源码类型标注）、`password`（该函数的业务参数，详见源码类型标注）、`database`（该函数的业务参数，详见源码类型标注）、`schema`（该函数的业务参数，详见源码类型标注）、`role`（该函数的业务参数，详见源码类型标注）。行 377–385。
  - **函数 `connect_to_postgres`**：建立连接。`host`（该函数的业务参数，详见源码类型标注）、`dbname`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）。行 388–395。
  - **函数 `connect_to_bigquery`**：建立连接。`cred_file_path`（该函数的业务参数，详见源码类型标注）、`project_id`（该函数的业务参数，详见源码类型标注）。行 398–399。
  - **函数 `connect_to_duckdb`**：建立连接。`url`（该函数的业务参数，详见源码类型标注）、`init_sql`（该函数的业务参数，详见源码类型标注）。行 402–403。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/adapter.py`

- **目录**：`src/vanna/legacy`。**文件名**：`adapter.py`。**语言/类型**：Python。**行数**：464。**字节**：17024。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **模块文档字符串**：Legacy VannaBase adapter for the Vanna Agents framework.

This module provides a LegacyVannaAdapter that bridges legacy VannaBase objects
with the new ToolRegistry system by auto-registering legacy methods as tools
with appropriate group-based access control.
- **结构摘要**：类 2 个，模块函数 0 个，方法 13 个。
  - **类 `LegacySqlRunner`**（抽象或具体服务/工具实现，基类：SqlRunner，行 32–63）：SqlRunner implementation that wraps a legacy VannaBase instance.  This class bridges the new SqlRunner interface with legacy VannaBase run_sql methods, allowing legacy database connections to work with the new tool-based
    - `__init__(vn)`：提供对象协议或生命周期约定。`vn`（该函数的业务参数，详见源码类型标注）。Initialize with a legacy VannaBase instance.  Args:     vn: The legacy VannaBase instance with an initialized run_sql me位于第 40–46 行。
    - `async run_sql(args, context)`：运行。`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute SQL query using the legacy VannaBase run_sql method.  Args:     args: SQL query arguments containing the SQL str位于第 48–63 行。
  - **类 `LegacyVannaAdapter`**（抽象或具体服务/工具实现，基类：ToolRegistry, AgentMemory，行 66–463）：Adapter that wraps a legacy VannaBase object and exposes its methods as tools.  This adapter automatically registers specific VannaBase methods as tools in the registry with configurable group-based access control. This 
    - `__init__(vn, audit_logger, audit_config)`：提供对象协议或生命周期约定。`vn`（该函数的业务参数，详见源码类型标注）、`audit_logger`（该函数的业务参数，详见源码类型标注）、`audit_config`（该函数的业务参数，详见源码类型标注）。Initialize the adapter with a legacy VannaBase instance.  Args:     vanna: The legacy VannaBase instance to wrap     aud位于第 97–114 行。
    - `_register_tools()`：注册到容器。仅依赖实例或类自身状态，不额外接收调用方参数。Register legacy VannaBase methods as tools with appropriate permissions.  Registers the following tools: - RunSqlTool: W位于第 116–138 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。Save a tool usage pattern by storing it as a question-sql pair.  Args:     question: The user question     tool_name: Na位于第 142–167 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for similar tool usage patterns using legacy question-sql lookup.  Args:     question: The question to search for位于第 169–219 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Save text memory using legacy add_documentation method.  Args:     content: The documentation content to save     contex位于第 221–239 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search text memories using legacy get_related_documentation method.  Args:     query: The query to search for     contex位于第 241–298 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Get recently added memories.  Note: Legacy VannaBase does not provide a direct way to get recent memories, so we retriev位于第 300–333 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。Fetch recently stored text memories.  Note: Legacy VannaBase does not provide a direct way to get recent text memories, 位于第 335–376 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a memory by its ID using legacy remove_training_data method.  Args:     context: Tool execution context     memor位于第 378–390 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。Delete a text memory by its ID using legacy remove_training_data method.  Args:     context: Tool execution context     位于第 392–404 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。Clear stored memories.  Note: Legacy VannaBase does not provide a direct clear method, so this operation is not supporte位于第 406–425 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/advanced/__init__.py`

- **目录**：`src/vanna/legacy/advanced`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：29。**字节**：672。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `VannaAdvanced`**（抽象或具体服务/工具实现，基类：ABC，行 4–28）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 5–6 行。
    - `get_function(question, additional_data)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）、`additional_data`（该函数的业务参数，详见源码类型标注）。位于第 9–10 行。
    - `create_function(question, sql, plotly_code)`：创建并初始化。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`plotly_code`（该函数的业务参数，详见源码类型标注）。位于第 13–16 行。
    - `update_function(old_function_name, updated_function)`：更新。`old_function_name`（该函数的业务参数，详见源码类型标注）、`updated_function`（该函数的业务参数，详见源码类型标注）。位于第 19–20 行。
    - `delete_function(function_name)`：删除并清理。`function_name`（该函数的业务参数，详见源码类型标注）。位于第 23–24 行。
    - `get_all_functions()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 27–28 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/anthropic/__init__.py`

- **目录**：`src/vanna/legacy/anthropic`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：43。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/anthropic/anthropic_chat.py`

- **目录**：`src/vanna/legacy/anthropic`。**文件名**：`anthropic_chat.py`。**语言/类型**：Python。**行数**：81。**字节**：2661。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `Anthropic_Chat`**（核心类型或辅助类，基类：VannaBase，行 8–80）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 9–31 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 33–34 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 36–37 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 39–40 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 42–80 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/azuresearch/__init__.py`

- **目录**：`src/vanna/legacy/azuresearch`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：58。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/azuresearch/azuresearch_vector.py`

- **目录**：`src/vanna/legacy/azuresearch`。**文件名**：`azuresearch_vector.py`。**语言/类型**：Python。**行数**：275。**字节**：10128。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 13 个。
  - **类 `AzureAISearch_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 28–274）：AzureAISearch_VectorStore is a class that provides a vector store for Azure AI Search.  Args:     config (dict): Configuration dictionary. Defaults to {}. You must provide an API key in the config.         - azure_search
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 48–91 行。
    - `_create_index()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 93–141 行。
    - `_get_indexes()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 143–144 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 146–155 行。
    - `add_documentation(doc)`：完成该模块中的具体处理。`doc`（该函数的业务参数，详见源码类型标注）。位于第 157–166 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 168–180 行。
    - `get_related_ddl(text)`：读取并返回。`text`（该函数的业务参数，详见源码类型标注）。位于第 182–198 行。
    - `get_related_documentation(text)`：读取并返回。`text`（该函数的业务参数，详见源码类型标注）。位于第 200–218 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 220–238 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 240–262 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 264–266 行。
    - `remove_index()`：移除。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 268–269 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 271–274 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/base/__init__.py`

- **目录**：`src/vanna/legacy/base`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：28。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/base/base.py`

- **目录**：`src/vanna/legacy/base`。**文件名**：`base.py`。**语言/类型**：Python。**行数**：2126。**字节**：74806。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **模块文档字符串**：# Nomenclature

| Prefix | Definition | Examples |
| --- | --- | --- |
| `vn.get_` | Fetch some data | [`vn.get_related_ddl(...)`][vanna.base.base.VannaBase.get_related_ddl] |
| `vn.add_` | Adds something to the retrieval layer | [`vn.add_question_sql(...)`][vanna.base.base.VannaBase.add_question_sql] <br> [`vn.add_ddl(...)`][vanna.base.base.VannaBase.add_ddl] |
| `vn.generate_` | Generates someth
- **结构摘要**：类 1 个，模块函数 0 个，方法 53 个。
  - **类 `VannaBase`**（抽象或具体服务/工具实现，基类：ABC，行 72–2125）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 73–82 行。
    - `log(message, title)`：记录审计或日志。`message`（用户自然语言输入）、`title`（该函数的业务参数，详见源码类型标注）。位于第 84–85 行。
    - `_response_language()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 87–91 行。
    - `generate_sql(question, allow_llm_to_see_data)`：生成。`question`（该函数的业务参数，详见源码类型标注）、`allow_llm_to_see_data`（该函数的业务参数，详见源码类型标注）。Example: ```python vn.generate_sql("What are the top 10 customers by sales?") ```  Uses the LLM to generate a SQL query 位于第 93–168 行。
    - `extract_sql(llm_response)`：完成该模块中的具体处理。`llm_response`（该函数的业务参数，详见源码类型标注）。        Example:         ```python         vn.extract_sql("Here's the SQL query in a code block: ```sql SELECT * FROM cu位于第 170–236 行。
    - `is_sql_valid(sql)`：判断是否满足条件。`sql`（待执行的 SQL 文本）。Example: ```python vn.is_sql_valid("SELECT * FROM customers") ``` Checks if the SQL query is valid. This is usually used位于第 238–260 行。
    - `should_generate_chart(df)`：完成该模块中的具体处理。`df`（pandas.DataFrame）。Example: ```python vn.should_generate_chart(df) ```  Checks if a chart should be generated for the given DataFrame. By d位于第 262–282 行。
    - `generate_rewritten_question(last_question, new_question)`：生成。`last_question`（该函数的业务参数，详见源码类型标注）、`new_question`（该函数的业务参数，详见源码类型标注）。**Example:** ```python rewritten_question = vn.generate_rewritten_question("Who are the top 5 customers by sales?", "Sho位于第 284–318 行。
    - `generate_followup_questions(question, sql, df, n_questions)`：生成。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`df`（pandas.DataFrame）、`n_questions`（该函数的业务参数，详见源码类型标注）。**Example:** ```python vn.generate_followup_questions("What are the top 10 customers by sales?", sql, df) ```  Generate 位于第 320–354 行。
    - `generate_questions()`：生成。仅依赖实例或类自身状态，不额外接收调用方参数。**Example:** ```python vn.generate_questions() ```  Generate a list of questions that you can ask Vanna.AI.位于第 356–367 行。
    - `generate_summary(question, df)`：生成。`question`（该函数的业务参数，详见源码类型标注）、`df`（pandas.DataFrame）。**Example:** ```python vn.generate_summary("What are the top 10 customers by sales?", df) ```  Generate a summary of the位于第 369–398 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 402–403 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。This method is used to get similar questions and their corresponding SQL statements.  Args:     question (str): The ques位于第 407–417 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。This method is used to get related DDL statements to a question.  Args:     question (str): The question to get related 位于第 420–430 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。This method is used to get related documentation to a question.  Args:     question (str): The question to get related d位于第 433–443 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。This method is used to add a question and its corresponding SQL query to the training data.  Args:     question (str): T位于第 446–457 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。This method is used to add a DDL statement to the training data.  Args:     ddl (str): The DDL statement to add.  Return位于第 460–470 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。This method is used to add documentation to the training data.  Args:     documentation (str): The documentation to add.位于第 473–483 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。Example: ```python vn.get_training_data() ```  This method is used to get all the training data from the retrieval layer位于第 486–498 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。Example: ```python vn.remove_training_data(id="123-ddl") ```  This method is used to remove training data from the retri位于第 501–516 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 521–522 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 525–526 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 529–530 行。
    - `str_to_approx_token_count(string)`：完成该模块中的具体处理。`string`（该函数的业务参数，详见源码类型标注）。位于第 532–533 行。
    - `add_ddl_to_prompt(initial_prompt, ddl_list, max_tokens)`：完成该模块中的具体处理。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`ddl_list`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）。位于第 535–549 行。
    - `add_documentation_to_prompt(initial_prompt, documentation_list, max_tokens)`：完成该模块中的具体处理。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`documentation_list`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）。位于第 551–568 行。
    - `add_sql_to_prompt(initial_prompt, sql_list, max_tokens)`：完成该模块中的具体处理。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`sql_list`（该函数的业务参数，详见源码类型标注）、`max_tokens`（该函数的业务参数，详见源码类型标注）。位于第 570–584 行。
    - `get_sql_prompt(initial_prompt, question, question_sql_list, ddl_list, doc_list)`：读取并返回。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`question`（该函数的业务参数，详见源码类型标注）、`question_sql_list`（该函数的业务参数，详见源码类型标注）、`ddl_list`（该函数的业务参数，详见源码类型标注）、`doc_list`（该函数的业务参数，详见源码类型标注）。Example: ```python vn.get_sql_prompt(     question="What are the top 10 customers by sales?",     question_sql_list=[{"q位于第 586–658 行。
    - `get_followup_questions_prompt(question, question_sql_list, ddl_list, doc_list)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）、`question_sql_list`（该函数的业务参数，详见源码类型标注）、`ddl_list`（该函数的业务参数，详见源码类型标注）、`doc_list`（该函数的业务参数，详见源码类型标注）。位于第 660–689 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。Example: ```python vn.submit_prompt(     [         vn.system_message("The user will give you SQL and you will try to gue位于第 692–712 行。
    - `generate_question(sql)`：生成。`sql`（待执行的 SQL 文本）。位于第 714–725 行。
    - `_extract_python_code(markdown_string)`：完成该模块中的具体处理。`markdown_string`（该函数的业务参数，详见源码类型标注）。位于第 727–746 行。
    - `_sanitize_plotly_code(raw_plotly_code)`：完成该模块中的具体处理。`raw_plotly_code`（该函数的业务参数，详见源码类型标注）。位于第 748–752 行。
    - `generate_plotly_code(question, sql, df_metadata)`：生成。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`df_metadata`（该函数的业务参数，详见源码类型标注）。位于第 754–776 行。
    - `connect_to_snowflake(account, username, password, database, role, warehouse)`：建立连接。`account`（该函数的业务参数，详见源码类型标注）、`username`（该函数的业务参数，详见源码类型标注）、`password`（该函数的业务参数，详见源码类型标注）、`database`（该函数的业务参数，详见源码类型标注）、`role`（该函数的业务参数，详见源码类型标注）、`warehouse`（该函数的业务参数，详见源码类型标注）。位于第 780–860 行。
    - `connect_to_sqlite(url, check_same_thread)`：建立连接。`url`（该函数的业务参数，详见源码类型标注）、`check_same_thread`（该函数的业务参数，详见源码类型标注）。Connect to a SQLite database. This is just a helper function to set [`vn.run_sql`][vanna.base.base.VannaBase.run_sql]  A位于第 862–894 行。
    - `connect_to_postgres(host, dbname, user, password, port)`：建立连接。`host`（该函数的业务参数，详见源码类型标注）、`dbname`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）。Connect to postgres using the psycopg2 connector. This is just a helper function to set [`vn.run_sql`][vanna.base.base.V位于第 896–1024 行。
    - `connect_to_mysql(host, dbname, user, password, port)`：建立连接。`host`（该函数的业务参数，详见源码类型标注）、`dbname`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）。位于第 1026–1111 行。
    - `connect_to_clickhouse(host, dbname, user, password, port)`：建立连接。`host`（该函数的业务参数，详见源码类型标注）、`dbname`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）。位于第 1113–1189 行。
    - `connect_to_oracle(user, password, dsn)`：建立连接。`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`dsn`（该函数的业务参数，详见源码类型标注）。Connect to an Oracle db using oracledb package. This is just a helper function to set [`vn.run_sql`][vanna.base.base.Van位于第 1191–1273 行。
    - `connect_to_bigquery(cred_file_path, project_id)`：建立连接。`cred_file_path`（该函数的业务参数，详见源码类型标注）、`project_id`（该函数的业务参数，详见源码类型标注）。Connect to gcs using the bigquery connector. This is just a helper function to set [`vn.run_sql`][vanna.base.base.VannaB位于第 1275–1356 行。
    - `connect_to_duckdb(url, init_sql)`：建立连接。`url`（该函数的业务参数，详见源码类型标注）、`init_sql`（该函数的业务参数，详见源码类型标注）。Connect to a DuckDB database. This is just a helper function to set [`vn.run_sql`][vanna.base.base.VannaBase.run_sql]  A位于第 1358–1405 行。
    - `connect_to_mssql(odbc_conn_str)`：建立连接。`odbc_conn_str`（该函数的业务参数，详见源码类型标注）。Connect to a Microsoft SQL Server database. This is just a helper function to set [`vn.run_sql`][vanna.base.base.VannaBa位于第 1407–1453 行。
    - `connect_to_presto(host, catalog, schema, user, password, port, combined_pem_path, protocol, requests_kwargs)`：建立连接。`host`（该函数的业务参数，详见源码类型标注）、`catalog`（该函数的业务参数，详见源码类型标注）、`schema`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）、`combined_pem_path`（该函数的业务参数，详见源码类型标注）、`protocol`（该函数的业务参数，详见源码类型标注）、`requests_kwargs`（该函数的业务参数，详见源码类型标注）。Connect to a Presto database using the specified parameters.  Args:     host (str): The host address of the Presto datab位于第 1455–1572 行。
    - `connect_to_hive(host, dbname, user, password, port, auth)`：建立连接。`host`（该函数的业务参数，详见源码类型标注）、`dbname`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）、`password`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）、`auth`（该函数的业务参数，详见源码类型标注）。Connect to a Hive database. This is just a helper function to set [`vn.run_sql`][vanna.base.base.VannaBase.run_sql] Conn位于第 1574–1671 行。
    - `run_sql(sql)`：运行。`sql`（待执行的 SQL 文本）。Example: ```python vn.run_sql("SELECT * FROM my_table") ```  Run a SQL query on the connected database.  Args:     sql (位于第 1673–1690 行。
    - `ask(question, print_results, auto_train, visualize, allow_llm_to_see_data)`：向模型提问。`question`（该函数的业务参数，详见源码类型标注）、`print_results`（该函数的业务参数，详见源码类型标注）、`auto_train`（该函数的业务参数，详见源码类型标注）、`visualize`（该函数的业务参数，详见源码类型标注）、`allow_llm_to_see_data`（该函数的业务参数，详见源码类型标注）。**Example:** ```python vn.ask("What are the top 10 customers by sales?") ```  Ask Vanna.AI a question and get the SQL qu位于第 1692–1805 行。
    - `train(question, sql, ddl, documentation, plan)`：写入训练或记忆样本。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`ddl`（该函数的业务参数，详见源码类型标注）、`documentation`（该函数的业务参数，详见源码类型标注）、`plan`（该函数的业务参数，详见源码类型标注）。**Example:** ```python vn.train() ```  Train Vanna.AI on a question and its corresponding SQL query. If you call it with位于第 1807–1860 行。
    - `_get_databases()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 1862–1875 行。
    - `_get_information_schema_tables(database)`：读取并返回。`database`（该函数的业务参数，详见源码类型标注）。位于第 1877–1880 行。
    - `get_training_plan_generic(df)`：读取并返回。`df`（pandas.DataFrame）。This method is used to generate a training plan from an information schema dataframe.  Basically what it does is breaks 位于第 1882–1940 行。
    - `get_training_plan_snowflake(filter_databases, filter_schemas, include_information_schema, use_historical_queries)`：读取并返回。`filter_databases`（该函数的业务参数，详见源码类型标注）、`filter_schemas`（该函数的业务参数，详见源码类型标注）、`include_information_schema`（该函数的业务参数，详见源码类型标注）、`use_historical_queries`（该函数的业务参数，详见源码类型标注）。位于第 1942–2070 行。
    - `get_plotly_figure(plotly_code, df, dark_mode)`：读取并返回。`plotly_code`（该函数的业务参数，详见源码类型标注）、`df`（pandas.DataFrame）、`dark_mode`（该函数的业务参数，详见源码类型标注）。**Example:** ```python fig = vn.get_plotly_figure(     plotly_code="fig = px.bar(df, x='name', y='salary')",     df=df )位于第 2072–2125 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/bedrock/__init__.py`

- **目录**：`src/vanna/legacy/bedrock`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：47。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/bedrock/bedrock_converse.py`

- **目录**：`src/vanna/legacy/bedrock`。**文件名**：`bedrock_converse.py`。**语言/类型**：Python。**行数**：87。**字节**：2842。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `Bedrock_Converse`**（核心类型或辅助类，基类：VannaBase，行 10–86）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 11–39 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 41–42 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 44–45 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 47–48 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 50–86 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/chromadb/__init__.py`

- **目录**：`src/vanna/legacy/chromadb`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：50。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/chromadb/chromadb_vector.py`

- **目录**：`src/vanna/legacy/chromadb`。**文件名**：`chromadb_vector.py`。**语言/类型**：Python。**行数**：260。**字节**：8814。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `ChromaDB_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 15–259）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 16–59 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 61–65 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 67–82 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 84–91 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 93–100 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 102–165 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 167–178 行。
    - `remove_collection(collection_name)`：移除。`collection_name`（该函数的业务参数，详见源码类型标注）。This function can reset the collection to empty state.  Args:     collection_name (str): sql or ddl or documentation  Re位于第 180–209 行。
    - `_extract_documents(query_results)`：完成该模块中的具体处理。`query_results`（该函数的业务参数，详见源码类型标注）。Static method to extract the documents from the results of a query.  Args:     query_results (pd.DataFrame): The datafra位于第 212–235 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 237–243 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 245–251 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 253–259 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/cohere/__init__.py`

- **目录**：`src/vanna/legacy/cohere`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：3。**字节**：86。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/cohere/cohere_chat.py`

- **目录**：`src/vanna/legacy/cohere`。**文件名**：`cohere_chat.py`。**语言/类型**：Python。**行数**：100。**字节**：3533。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `Cohere_Chat`**（核心类型或辅助类，基类：VannaBase，行 8–99）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 9–43 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 45–49 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 51–52 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 54–55 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 57–99 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/cohere/cohere_embeddings.py`

- **目录**：`src/vanna/legacy/cohere`。**文件名**：`cohere_embeddings.py`。**语言/类型**：Python。**行数**：78。**字节**：2608。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `Cohere_Embeddings`**（核心类型或辅助类，基类：VannaBase，行 8–77）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 9–39 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 41–77 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/deepseek/__init__.py`

- **目录**：`src/vanna/legacy/deepseek`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：40。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/deepseek/deepseek_chat.py`

- **目录**：`src/vanna/legacy/deepseek`。**文件名**：`deepseek_chat.py`。**语言/类型**：Python。**行数**：60。**字节**：1841。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `DeepSeekChat`**（核心类型或辅助类，基类：VannaBase，行 18–59）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 19–33 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 35–36 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 38–39 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 41–42 行。
    - `generate_sql(question)`：生成。`question`（该函数的业务参数，详见源码类型标注）。位于第 44–51 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 53–59 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/exceptions/__init__.py`

- **目录**：`src/vanna/legacy/exceptions`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：47。**字节**：685。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 8 个，模块函数 0 个，方法 0 个。
  - **类 `ImproperlyConfigured`**（配置模型，基类：Exception，行 1–4）：Raise for incorrect configuration.
  - **类 `DependencyError`**（异常类型，基类：Exception，行 7–10）：Raise for missing dependencies.
  - **类 `ConnectionError`**（异常类型，基类：Exception，行 13–16）：Raise for connection
  - **类 `OTPCodeError`**（异常类型，基类：Exception，行 19–22）：Raise for invalid otp or not able to send it
  - **类 `SQLRemoveError`**（异常类型，基类：Exception，行 25–28）：Raise when not able to remove SQL
  - **类 `ExecutionError`**（异常类型，基类：Exception，行 31–34）：Raise when not able to execute Code
  - **类 `ValidationError`**（异常类型，基类：Exception，行 37–40）：Raise for validations
  - **类 `APIError`**（异常类型，基类：Exception，行 43–46）：Raise for API errors
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/faiss/__init__.py`

- **目录**：`src/vanna/legacy/faiss`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：25。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/faiss/faiss.py`

- **目录**：`src/vanna/legacy/faiss`。**文件名**：`faiss.py`。**语言/类型**：Python。**行数**：231。**字节**：8938。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 17 个。
  - **类 `FAISS`**（核心类型或辅助类，基类：VannaBase，行 14–230）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 15–76 行。
    - `_load_or_create_index(filename)`：加载。`filename`（文件名）。位于第 78–82 行。
    - `_load_or_create_metadata(filename)`：加载。`filename`（文件名）。位于第 84–89 行。
    - `_save_index(index, filename)`：持久化保存。`index`（该函数的业务参数，详见源码类型标注）、`filename`（文件名）。位于第 91–94 行。
    - `_save_metadata(metadata, filename)`：持久化保存。`metadata`（扩展元数据字典）、`filename`（文件名）。位于第 96–100 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 102–107 行。
    - `_add_to_index(index, metadata_list, text, extra_metadata)`：完成该模块中的具体处理。`index`（该函数的业务参数，详见源码类型标注）、`metadata_list`（该函数的业务参数，详见源码类型标注）、`text`（该函数的业务参数，详见源码类型标注）、`extra_metadata`（该函数的业务参数，详见源码类型标注）。位于第 109–114 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 116–125 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 127–133 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 135–144 行。
    - `_get_similar(index, metadata_list, text, n_results)`：读取并返回。`index`（该函数的业务参数，详见源码类型标注）、`metadata_list`（该函数的业务参数，详见源码类型标注）、`text`（该函数的业务参数，详见源码类型标注）、`n_results`（该函数的业务参数，详见源码类型标注）。位于第 146–151 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 153–156 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 158–164 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 166–175 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 177–187 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 189–213 行。
    - `remove_collection(collection_name)`：移除。`collection_name`（该函数的业务参数，详见源码类型标注）。位于第 215–230 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/flask/__init__.py`

- **目录**：`src/vanna/legacy/flask`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：1344。**字节**：44664。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 4 个，模块函数 0 个，方法 16 个。
  - **类 `Cache`**（抽象或具体服务/工具实现，基类：ABC，行 21–59）：Define the interface for a cache that can be used to store data in a Flask app.
    - `generate_id()`：生成。仅依赖实例或类自身状态，不额外接收调用方参数。Generate a unique ID for the cache.位于第 27–31 行。
    - `get(id, field)`：读取并返回。`id`（该函数的业务参数，详见源码类型标注）、`field`（该函数的业务参数，详见源码类型标注）。Get a value from the cache.位于第 34–38 行。
    - `get_all(field_list)`：读取并返回。`field_list`（该函数的业务参数，详见源码类型标注）。Get all values from the cache.位于第 41–45 行。
    - `set(id, field, value)`：设置并更新。`id`（该函数的业务参数，详见源码类型标注）、`field`（该函数的业务参数，详见源码类型标注）、`value`（该函数的业务参数，详见源码类型标注）。Set a value in the cache.位于第 48–52 行。
    - `delete(id)`：删除并清理。`id`（该函数的业务参数，详见源码类型标注）。Delete a value from the cache.位于第 55–59 行。
  - **类 `MemoryCache`**（核心类型或辅助类，基类：Cache，行 62–92）：职责见方法列表。
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 63–64 行。
    - `generate_id()`：生成。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 66–67 行。
    - `set(id, field, value)`：设置并更新。`id`（该函数的业务参数，详见源码类型标注）、`field`（该函数的业务参数，详见源码类型标注）、`value`（该函数的业务参数，详见源码类型标注）。位于第 69–73 行。
    - `get(id, field)`：读取并返回。`id`（该函数的业务参数，详见源码类型标注）、`field`（该函数的业务参数，详见源码类型标注）。位于第 75–82 行。
    - `get_all(field_list)`：读取并返回。`field_list`（该函数的业务参数，详见源码类型标注）。位于第 84–88 行。
    - `delete(id)`：删除并清理。`id`（该函数的业务参数，详见源码类型标注）。位于第 90–92 行。
  - **类 `VannaFlaskAPI`**（核心类型或辅助类，基类：无，行 95–1205）：职责见方法列表。
    - `requires_cache(required_fields, optional_fields)`：完成该模块中的具体处理。`required_fields`（该函数的业务参数，详见源码类型标注）、`optional_fields`（该函数的业务参数，详见源码类型标注）。位于第 98–128 行。
    - `requires_auth(f)`：完成该模块中的具体处理。`f`（该函数的业务参数，详见源码类型标注）。位于第 130–143 行。
    - `__init__(vn, cache, auth, debug, allow_llm_to_see_data, chart)`：提供对象协议或生命周期约定。`vn`（该函数的业务参数，详见源码类型标注）、`cache`（该函数的业务参数，详见源码类型标注）、`auth`（该函数的业务参数，详见源码类型标注）、`debug`（该函数的业务参数，详见源码类型标注）、`allow_llm_to_see_data`（该函数的业务参数，详见源码类型标注）、`chart`（该函数的业务参数，详见源码类型标注）。Expose a Flask API that can be used to interact with a Vanna instance.  Args:     vn: The Vanna instance to interact wit位于第 145–1174 行。
    - `run()`：运行。仅依赖实例或类自身状态，不额外接收调用方参数。Run the Flask app.  Args:     *args: Arguments to pass to Flask's run method.     **kwargs: Keyword arguments to pass to位于第 1176–1205 行。
  - **类 `VannaFlaskApp`**（核心类型或辅助类，基类：VannaFlaskAPI，行 1208–1343）：职责见方法列表。
    - `__init__(vn, cache, auth, debug, allow_llm_to_see_data, logo, title, subtitle, show_training_data, suggested_questions, sql, table, csv_download, chart, redraw_chart, auto_fix_sql, ask_results_correct, followup_questions, summarization, function_generation, index_html_path, assets_folder)`：提供对象协议或生命周期约定。`vn`（该函数的业务参数，详见源码类型标注）、`cache`（该函数的业务参数，详见源码类型标注）、`auth`（该函数的业务参数，详见源码类型标注）、`debug`（该函数的业务参数，详见源码类型标注）、`allow_llm_to_see_data`（该函数的业务参数，详见源码类型标注）、`logo`（该函数的业务参数，详见源码类型标注）、`title`（该函数的业务参数，详见源码类型标注）、`subtitle`（该函数的业务参数，详见源码类型标注）、`show_training_data`（该函数的业务参数，详见源码类型标注）、`suggested_questions`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`table`（该函数的业务参数，详见源码类型标注）、`csv_download`（该函数的业务参数，详见源码类型标注）、`chart`（该函数的业务参数，详见源码类型标注）、`redraw_chart`（该函数的业务参数，详见源码类型标注）、`auto_fix_sql`（该函数的业务参数，详见源码类型标注）、`ask_results_correct`（该函数的业务参数，详见源码类型标注）、`followup_questions`（该函数的业务参数，详见源码类型标注）、`summarization`（该函数的业务参数，详见源码类型标注）、`function_generation`（该函数的业务参数，详见源码类型标注）、`index_html_path`（该函数的业务参数，详见源码类型标注）、`assets_folder`（该函数的业务参数，详见源码类型标注）。Expose a Flask app that can be used to interact with a Vanna instance.  Args:     vn: The Vanna instance to interact wit位于第 1209–1343 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/flask/assets.py`

- **目录**：`src/vanna/legacy/flask`。**文件名**：`assets.py`。**语言/类型**：Python。**行数**：59。**字节**：453463。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/flask/auth.py`

- **目录**：`src/vanna/legacy/flask`。**文件名**：`auth.py`。**语言/类型**：Python。**行数**：57。**字节**：1247。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 2 个，模块函数 0 个，方法 14 个。
  - **类 `AuthInterface`**（抽象或具体服务/工具实现，基类：ABC，行 6–33）：职责见方法列表。
    - `get_user(flask_request)`：读取并返回。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 8–9 行。
    - `is_logged_in(user)`：判断是否满足条件。`user`（当前用户对象，携带 id 与 group_memberships）。位于第 12–13 行。
    - `override_config_for_user(user, config)`：完成该模块中的具体处理。`user`（当前用户对象，携带 id 与 group_memberships）、`config`（配置对象）。位于第 16–17 行。
    - `login_form()`：记录审计或日志。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 20–21 行。
    - `login_handler(flask_request)`：记录审计或日志。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 24–25 行。
    - `callback_handler(flask_request)`：完成该模块中的具体处理。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 28–29 行。
    - `logout_handler(flask_request)`：记录审计或日志。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 32–33 行。
  - **类 `NoAuth`**（核心类型或辅助类，基类：AuthInterface，行 36–56）：职责见方法列表。
    - `get_user(flask_request)`：读取并返回。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 37–38 行。
    - `is_logged_in(user)`：判断是否满足条件。`user`（当前用户对象，携带 id 与 group_memberships）。位于第 40–41 行。
    - `override_config_for_user(user, config)`：完成该模块中的具体处理。`user`（当前用户对象，携带 id 与 group_memberships）、`config`（配置对象）。位于第 43–44 行。
    - `login_form()`：记录审计或日志。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 46–47 行。
    - `login_handler(flask_request)`：记录审计或日志。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 49–50 行。
    - `callback_handler(flask_request)`：完成该模块中的具体处理。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 52–53 行。
    - `logout_handler(flask_request)`：记录审计或日志。`flask_request`（该函数的业务参数，详见源码类型标注）。位于第 55–56 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/google/__init__.py`

- **目录**：`src/vanna/legacy/google`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：3。**字节**：92。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/google/bigquery_vector.py`

- **目录**：`src/vanna/legacy/google`。**文件名**：`bigquery_vector.py`。**语言/类型**：Python。**行数**：283。**字节**：9784。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 15 个。
  - **类 `BigQuery_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 13–282）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 14–83 行。
    - `store_training_data(training_data_type, question, content, embedding)`：完成该模块中的具体处理。`training_data_type`（该函数的业务参数，详见源码类型标注）、`question`（该函数的业务参数，详见源码类型标注）、`content`（文本内容）、`embedding`（该函数的业务参数，详见源码类型标注）。位于第 103–127 行。
    - `fetch_similar_training_data(training_data_type, question, n_results)`：完成该模块中的具体处理。`training_data_type`（该函数的业务参数，详见源码类型标注）、`question`（该函数的业务参数，详见源码类型标注）、`n_results`（该函数的业务参数，详见源码类型标注）。位于第 129–155 行。
    - `get_embeddings(data, task)`：读取并返回。`data`（该函数的业务参数，详见源码类型标注）、`task`（该函数的业务参数，详见源码类型标注）。位于第 157–177 行。
    - `generate_question_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 179–185 行。
    - `generate_storage_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 187–204 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 206–207 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 209–217 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 219–225 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 227–235 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 237–247 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 249–254 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 256–264 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 266–271 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 273–282 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/google/gemini_chat.py`

- **目录**：`src/vanna/legacy/google`。**文件名**：`gemini_chat.py`。**语言/类型**：Python。**行数**：79。**字节**：2751。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `GoogleGeminiChat`**（核心类型或辅助类，基类：VannaBase，行 6–78）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 7–60 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 62–63 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 65–66 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 68–69 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 71–78 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/hf/__init__.py`

- **目录**：`src/vanna/legacy/hf`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：19。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/hf/hf.py`

- **目录**：`src/vanna/legacy/hf`。**文件名**：`hf.py`。**语言/类型**：Python。**行数**：81。**字节**：3019。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 7 个。
  - **类 `Hf`**（核心类型或辅助类，基类：VannaBase，行 7–80）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 8–19 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 21–22 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 24–25 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 27–28 行。
    - `extract_sql_query(text)`：完成该模块中的具体处理。`text`（该函数的业务参数，详见源码类型标注）。Extracts the first SQL statement after the word 'select', ignoring case, matches until the first semicolon, three backti位于第 30–50 行。
    - `generate_sql(question)`：生成。`question`（该函数的业务参数，详见源码类型标注）。位于第 52–61 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 63–80 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/local.py`

- **目录**：`src/vanna/legacy`。**文件名**：`local.py`。**语言/类型**：Python。**行数**：9。**字节**：313。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 1 个。
  - **类 `LocalContext_OpenAI`**（核心类型或辅助类，基类：ChromaDB_VectorStore, OpenAI_Chat，行 5–8）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 6–8 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/marqo/__init__.py`

- **目录**：`src/vanna/legacy/marqo`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：37。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/marqo/marqo.py`

- **目录**：`src/vanna/legacy/marqo`。**文件名**：`marqo.py`。**语言/类型**：Python。**行数**：169。**字节**：5242。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 11 个。
  - **类 `Marqo_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 9–168）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 10–31 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 33–35 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 37–50 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 52–63 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 65–76 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 78–113 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 115–126 行。
    - `_extract_documents(data)`：完成该模块中的具体处理。`data`（该函数的业务参数，详见源码类型标注）。位于第 130–153 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 155–158 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 160–163 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 165–168 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/milvus/__init__.py`

- **目录**：`src/vanna/legacy/milvus`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：46。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/milvus/milvus_vector.py`

- **目录**：`src/vanna/legacy/milvus`。**文件名**：`milvus_vector.py`。**语言/类型**：Python。**行数**：329。**字节**：11642。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 14 个。
  - **类 `Milvus_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 24–328）：Vectorstore implementation using Milvus - https://milvus.io/docs/quickstart.md  Args:     - config (dict, optional): Dictionary of `Milvus_VectorStore config` options. Defaults to `None`.         - milvus_client: A `pymi
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 37–53 行。
    - `_create_collections()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 55–58 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 60–61 行。
    - `_create_sql_collection(name)`：创建并初始化。`name`（该函数的业务参数，详见源码类型标注）。位于第 63–99 行。
    - `_create_ddl_collection(name)`：创建并初始化。`name`（该函数的业务参数，详见源码类型标注）。位于第 101–134 行。
    - `_create_doc_collection(name)`：创建并初始化。`name`（该函数的业务参数，详见源码类型标注）。位于第 136–169 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 171–180 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 182–191 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 193–202 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 204–249 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 251–273 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 275–294 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 296–315 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 317–328 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/mistral/__init__.py`

- **目录**：`src/vanna/legacy/mistral`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：29。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/mistral/mistral.py`

- **目录**：`src/vanna/legacy/mistral`。**文件名**：`mistral.py`。**语言/类型**：Python。**行数**：52。**字节**：1494。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `Mistral`**（核心类型或辅助类，基类：VannaBase，行 9–51）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 10–25 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 27–28 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 30–31 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 33–34 行。
    - `generate_sql(question)`：生成。`question`（该函数的业务参数，详见源码类型标注）。位于第 36–43 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 45–51 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/mock/__init__.py`

- **目录**：`src/vanna/legacy/mock`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：4。**字节**：97。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/mock/embedding.py`

- **目录**：`src/vanna/legacy/mock`。**文件名**：`embedding.py`。**语言/类型**：Python。**行数**：12。**字节**：250。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `MockEmbedding`**（核心类型或辅助类，基类：VannaBase，行 6–11）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 7–8 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 10–11 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/mock/llm.py`

- **目录**：`src/vanna/legacy/mock`。**文件名**：`llm.py`。**语言/类型**：Python。**行数**：19。**字节**：517。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `MockLLM`**（核心类型或辅助类，基类：VannaBase，行 4–18）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 5–6 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 8–9 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 11–12 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 14–15 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 17–18 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/mock/vectordb.py`

- **目录**：`src/vanna/legacy/mock`。**文件名**：`vectordb.py`。**语言/类型**：Python。**行数**：68。**字节**：3223。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 10 个。
  - **类 `MockVectorDB`**（核心类型或辅助类，基类：VannaBase，行 6–67）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 7–8 行。
    - `_get_id(value)`：读取并返回。`value`（该函数的业务参数，详见源码类型标注）。位于第 10–12 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 14–15 行。
    - `add_documentation(doc)`：完成该模块中的具体处理。`doc`（该函数的业务参数，详见源码类型标注）。位于第 17–18 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 20–21 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 23–24 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 26–27 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 29–30 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 32–64 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 66–67 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/ollama/__init__.py`

- **目录**：`src/vanna/legacy/ollama`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：27。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/ollama/ollama.py`

- **目录**：`src/vanna/legacy/ollama`。**文件名**：`ollama.py`。**语言/类型**：Python。**行数**：111。**字节**：4122。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 7 个。
  - **类 `Ollama`**（核心类型或辅助类，基类：VannaBase，行 10–110）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 11–37 行。
    - `__pull_model_if_ne(ollama_client, model)`：提供对象协议或生命周期约定。`ollama_client`（该函数的业务参数，详见源码类型标注）、`model`（模型名称）。位于第 40–46 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 48–49 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 51–52 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 54–55 行。
    - `extract_sql(llm_response)`：完成该模块中的具体处理。`llm_response`（该函数的业务参数，详见源码类型标注）。Extracts the first SQL statement after the word 'select', ignoring case, matches until the first semicolon, three backti位于第 57–90 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 92–110 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/openai/__init__.py`

- **目录**：`src/vanna/legacy/openai`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：3。**字节**：86。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/openai/openai_chat.py`

- **目录**：`src/vanna/legacy/openai`。**文件名**：`openai_chat.py`。**语言/类型**：Python。**行数**：125。**字节**：4367。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `OpenAI_Chat`**（核心类型或辅助类，基类：VannaBase，行 8–124）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 9–42 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 44–45 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 47–48 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 50–51 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 53–124 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/openai/openai_embeddings.py`

- **目录**：`src/vanna/legacy/openai`。**文件名**：`openai_embeddings.py`。**语言/类型**：Python。**行数**：47。**字节**：1260。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `OpenAI_Embeddings`**（核心类型或辅助类，基类：VannaBase，行 6–46）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 7–32 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 34–46 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/opensearch/__init__.py`

- **目录**：`src/vanna/legacy/opensearch`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：3。**字节**：126。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/opensearch/opensearch_vector.py`

- **目录**：`src/vanna/legacy/opensearch`。**文件名**：`opensearch_vector.py`。**语言/类型**：Python。**行数**：370。**字节**：13465。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 12 个。
  - **类 `OpenSearch_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 11–359）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 12–216 行。
    - `create_index()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 218–225 行。
    - `create_index_if_not_exists(index_name, index_settings)`：创建并初始化。`index_name`（该函数的业务参数，详见源码类型标注）、`index_settings`（该函数的业务参数，详见源码类型标注）。位于第 227–238 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 240–247 行。
    - `add_documentation(doc)`：完成该模块中的具体处理。`doc`（该函数的业务参数，详见源码类型标注）。位于第 249–256 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 258–265 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 267–272 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 274–278 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 280–289 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 291–338 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 340–355 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 357–359 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/opensearch/opensearch_vector_semantic.py`

- **目录**：`src/vanna/legacy/opensearch`。**文件名**：`opensearch_vector_semantic.py`。**语言/类型**：Python。**行数**：201。**字节**：7516。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 10 个。
  - **类 `OpenSearch_Semantic_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 10–200）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 11–75 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 77–80 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 82–85 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 87–98 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 100–104 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 106–110 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 112–116 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 118–180 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 182–197 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 199–200 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/oracle/__init__.py`

- **目录**：`src/vanna/legacy/oracle`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：46。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/oracle/oracle_vector.py`

- **目录**：`src/vanna/legacy/oracle`。**文件名**：`oracle_vector.py`。**语言/类型**：Python。**行数**：585。**字节**：17533。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 18 个。
  - **类 `Oracle_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 14–584）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 15–41 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 43–47 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 49–95 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 97–139 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 141–183 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 185–297 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 299–314 行。
    - `update_training_data(id, train_type, question)`：更新。`id`（该函数的业务参数，详见源码类型标注）、`train_type`（该函数的业务参数，详见源码类型标注）、`question`（该函数的业务参数，详见源码类型标注）。位于第 316–366 行。
    - `_extract_documents(query_results)`：完成该模块中的具体处理。`query_results`（该函数的业务参数，详见源码类型标注）。Static method to extract the documents from the results of a query.  Args:     query_results (pd.DataFrame): The datafra位于第 369–387 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 389–406 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 408–428 行。
    - `search_tables_metadata(engine, catalog, schema, table_name, ddl, size)`：检索相似项。`engine`（该函数的业务参数，详见源码类型标注）、`catalog`（该函数的业务参数，详见源码类型标注）、`schema`（该函数的业务参数，详见源码类型标注）、`table_name`（该函数的业务参数，详见源码类型标注）、`ddl`（该函数的业务参数，详见源码类型标注）、`size`（该函数的业务参数，详见源码类型标注）。位于第 430–440 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 442–463 行。
    - `create_tables_if_not_exists()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 465–493 行。
    - `create_collections_if_not_exists(name, cmetadata)`：创建并初始化。`name`（该函数的业务参数，详见源码类型标注）、`cmetadata`（该函数的业务参数，详见源码类型标注）。Get or create a collection. Returns [Collection, bool] where the bool is True if the collection was created.位于第 495–529 行。
    - `get_collection(name)`：读取并返回。`name`（该函数的业务参数，详见源码类型标注）。位于第 531–532 行。
    - `get_by_name(name)`：读取并返回。`name`（该函数的业务参数，详见源码类型标注）。位于第 534–554 行。
    - `delete_collection(name)`：删除并清理。`name`（该函数的业务参数，详见源码类型标注）。位于第 556–584 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/pgvector/__init__.py`

- **目录**：`src/vanna/legacy/pgvector`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：37。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/pgvector/pgvector.py`

- **目录**：`src/vanna/legacy/pgvector`。**文件名**：`pgvector.py`。**语言/类型**：Python。**行数**：283。**字节**：10398。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 13 个。
  - **类 `PG_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 16–282）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 17–52 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 54–70 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 72–79 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 81–88 行。
    - `get_collection(collection_name)`：读取并返回。`collection_name`（该函数的业务参数，详见源码类型标注）。位于第 90–99 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 101–105 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 107–111 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 113–117 行。
    - `train(question, sql, ddl, documentation, plan, createdat)`：写入训练或记忆样本。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`ddl`（该函数的业务参数，详见源码类型标注）、`documentation`（该函数的业务参数，详见源码类型标注）、`plan`（该函数的业务参数，详见源码类型标注）、`createdat`（该函数的业务参数，详见源码类型标注）。位于第 119–153 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 155–208 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 210–236 行。
    - `remove_collection(collection_name)`：移除。`collection_name`（该函数的业务参数，详见源码类型标注）。位于第 238–279 行。
    - `generate_embedding()`：生成。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 281–282 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/pinecone/__init__.py`

- **目录**：`src/vanna/legacy/pinecone`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：4。**字节**：90。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/pinecone/pinecone_vector.py`

- **目录**：`src/vanna/legacy/pinecone`。**文件名**：`pinecone_vector.py`。**语言/类型**：Python。**行数**：276。**字节**：11445。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 14 个。
  - **类 `PineconeDB_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 12–275）：Vectorstore using PineconeDB  Args:     config (dict): Configuration dictionary. Defaults to {}. You must provide either a Pinecone Client or an API key in the config.         - client (Pinecone, optional): Pinecone clie
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 36–80 行。
    - `_set_index_host(host)`：设置并更新。`host`（该函数的业务参数，详见源码类型标注）。位于第 82–83 行。
    - `_setup_index()`：设置并更新。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 85–107 行。
    - `_get_indexes()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 109–110 行。
    - `_check_if_embedding_exists(id, namespace)`：检查。`id`（该函数的业务参数，详见源码类型标注）、`namespace`（该函数的业务参数，详见源码类型标注）。位于第 112–116 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 118–127 行。
    - `add_documentation(doc)`：完成该模块中的具体处理。`doc`（该函数的业务参数，详见源码类型标注）。位于第 129–143 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 145–169 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 171–179 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 181–193 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 195–213 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 215–257 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 259–270 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 272–275 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qdrant/__init__.py`

- **目录**：`src/vanna/legacy/qdrant`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：4。**字节**：73。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qdrant/qdrant.py`

- **目录**：`src/vanna/legacy/qdrant`。**文件名**：`qdrant.py`。**语言/类型**：Python。**行数**：330。**字节**：12483。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 16 个。
  - **类 `Qdrant_VectorStore`**（核心类型或辅助类，基类：VannaBase，行 13–329）：Vectorstore implementation using Qdrant - https://qdrant.tech/  Args:     - config (dict, optional): Dictionary of `Qdrant_VectorStore config` options. Defaults to `{}`.         - client: A `qdrant_client.QdrantClient` i
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 41–83 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 85–103 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 105–119 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 121–137 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 139–200 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 202–208 行。
    - `remove_collection(collection_name)`：移除。`collection_name`（该函数的业务参数，详见源码类型标注）。This function can reset the collection to empty state.  Args:     collection_name (str): sql or ddl or documentation  Re位于第 210–225 行。
    - `embeddings_dimension()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 228–229 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 231–239 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 241–249 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 251–259 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 261–267 行。
    - `_get_all_points(collection_name)`：读取并返回。`collection_name`（该函数的业务参数，详见源码类型标注）。位于第 269–289 行。
    - `_setup_collections()`：设置并更新。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 291–319 行。
    - `_format_point_id(id, collection_name)`：格式化。`id`（该函数的业务参数，详见源码类型标注）、`collection_name`（该函数的业务参数，详见源码类型标注）。位于第 321–322 行。
    - `_parse_point_id(id)`：解析输入。`id`（该函数的业务参数，详见源码类型标注）。位于第 324–329 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qianfan/Qianfan_Chat.py`

- **目录**：`src/vanna/legacy/qianfan`。**文件名**：`Qianfan_Chat.py`。**语言/类型**：Python。**行数**：171。**字节**：6099。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 6 个。
  - **类 `Qianfan_Chat`**（核心类型或辅助类，基类：VannaBase，行 6–170）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 7–34 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 36–37 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 39–40 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 42–43 行。
    - `get_sql_prompt(initial_prompt, question, question_sql_list, ddl_list, doc_list)`：读取并返回。`initial_prompt`（该函数的业务参数，详见源码类型标注）、`question`（该函数的业务参数，详见源码类型标注）、`question_sql_list`（该函数的业务参数，详见源码类型标注）、`ddl_list`（该函数的业务参数，详见源码类型标注）、`doc_list`（该函数的业务参数，详见源码类型标注）。Example: ```python vn.get_sql_prompt(     question="What are the top 10 customers by sales?",     question_sql_list=[{"q位于第 45–119 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 121–170 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qianfan/Qianfan_embeddings.py`

- **目录**：`src/vanna/legacy/qianfan`。**文件名**：`Qianfan_embeddings.py`。**语言/类型**：Python。**行数**：37。**字节**：1078。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `Qianfan_Embeddings`**（核心类型或辅助类，基类：VannaBase，行 6–36）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 7–22 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 24–36 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qianfan/__init__.py`

- **目录**：`src/vanna/legacy/qianfan`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：3。**字节**：90。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qianwen/QianwenAI_chat.py`

- **目录**：`src/vanna/legacy/qianwen`。**文件名**：`QianwenAI_chat.py`。**语言/类型**：Python。**行数**：133。**字节**：4673。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `QianWenAI_Chat`**（核心类型或辅助类，基类：VannaBase，行 8–132）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 9–50 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 52–53 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 55–56 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 58–59 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 61–132 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qianwen/QianwenAI_embeddings.py`

- **目录**：`src/vanna/legacy/qianwen`。**文件名**：`QianwenAI_embeddings.py`。**语言/类型**：Python。**行数**：47。**字节**：1253。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 2 个。
  - **类 `QianWenAI_Embeddings`**（核心类型或辅助类，基类：VannaBase，行 6–46）：职责见方法列表。
    - `__init__(client, config)`：提供对象协议或生命周期约定。`client`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 7–32 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 34–46 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/qianwen/__init__.py`

- **目录**：`src/vanna/legacy/qianwen`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：3。**字节**：98。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/remote.py`

- **目录**：`src/vanna/legacy`。**文件名**：`remote.py`。**语言/类型**：Python。**行数**：80。**字节**：1948。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `VannaDefault`**（核心类型或辅助类，基类：VannaDB_VectorStore，行 40–79）：职责见方法列表。
    - `__init__(model, api_key, config)`：提供对象协议或生命周期约定。`model`（模型名称）、`api_key`（供应商密钥）、`config`（配置对象）。位于第 41–54 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 56–57 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 59–60 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 62–63 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 65–79 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/types/__init__.py`

- **目录**：`src/vanna/legacy/types`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：293。**字节**：4957。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 34 个，模块函数 0 个，方法 6 个。
  - **类 `Status`**（核心类型或辅助类，基类：无，行 8–10）：职责见方法列表。
  - **类 `StatusWithId`**（核心类型或辅助类，基类：无，行 14–17）：职责见方法列表。
  - **类 `QuestionList`**（核心类型或辅助类，基类：无，行 21–22）：职责见方法列表。
  - **类 `FullQuestionDocument`**（核心类型或辅助类，基类：无，行 26–31）：职责见方法列表。
  - **类 `QuestionSQLPair`**（核心类型或辅助类，基类：无，行 35–38）：职责见方法列表。
  - **类 `Organization`**（核心类型或辅助类，基类：无，行 42–45）：职责见方法列表。
  - **类 `OrganizationList`**（核心类型或辅助类，基类：无，行 49–50）：职责见方法列表。
  - **类 `QuestionStringList`**（核心类型或辅助类，基类：无，行 54–55）：职责见方法列表。
  - **类 `Visibility`**（核心类型或辅助类，基类：无，行 59–60）：职责见方法列表。
  - **类 `UserEmail`**（核心类型或辅助类，基类：无，行 64–65）：职责见方法列表。
  - **类 `NewOrganization`**（核心类型或辅助类，基类：无，行 69–71）：职责见方法列表。
  - **类 `NewOrganizationMember`**（核心类型或辅助类，基类：无，行 75–78）：职责见方法列表。
  - **类 `UserOTP`**（核心类型或辅助类，基类：无，行 82–84）：职责见方法列表。
  - **类 `ApiKey`**（核心类型或辅助类，基类：无，行 88–89）：职责见方法列表。
  - **类 `QuestionId`**（核心类型或辅助类，基类：无，行 93–94）：职责见方法列表。
  - **类 `Question`**（核心类型或辅助类，基类：无，行 98–99）：职责见方法列表。
  - **类 `QuestionCategory`**（核心类型或辅助类，基类：无，行 103–114）：职责见方法列表。
  - **类 `AccuracyStats`**（核心类型或辅助类，基类：无，行 118–120）：职责见方法列表。
  - **类 `Followup`**（核心类型或辅助类，基类：无，行 124–125）：职责见方法列表。
  - **类 `QuestionEmbedding`**（核心类型或辅助类，基类：无，行 129–131）：职责见方法列表。
  - **类 `Connection`**（核心类型或辅助类，基类：无，行 135–137）：职责见方法列表。
  - **类 `SQLAnswer`**（核心类型或辅助类，基类：无，行 141–145）：职责见方法列表。
  - **类 `Explanation`**（核心类型或辅助类，基类：无，行 149–150）：职责见方法列表。
  - **类 `DataResult`**（核心类型或辅助类，基类：无，行 154–159）：职责见方法列表。
  - **类 `PlotlyResult`**（核心类型或辅助类，基类：无，行 163–164）：职责见方法列表。
  - **类 `WarehouseDefinition`**（核心类型或辅助类，基类：无，行 168–170）：职责见方法列表。
  - **类 `TableDefinition`**（核心类型或辅助类，基类：无，行 174–178）：职责见方法列表。
  - **类 `ColumnDefinition`**（核心类型或辅助类，基类：无，行 182–188）：职责见方法列表。
  - **类 `Diagram`**（核心类型或辅助类，基类：无，行 192–194）：职责见方法列表。
  - **类 `StringData`**（核心类型或辅助类，基类：无，行 198–199）：职责见方法列表。
  - **类 `DataFrameJSON`**（核心类型或辅助类，基类：无，行 203–204）：职责见方法列表。
  - **类 `TrainingData`**（核心类型或辅助类，基类：无，行 208–211）：职责见方法列表。
  - **类 `TrainingPlanItem`**（核心类型或辅助类，基类：无，行 215–231）：职责见方法列表。
    - `__str__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 221–227 行。
  - **类 `TrainingPlan`**（核心类型或辅助类，基类：无，行 234–292）：A class representing a training plan. You can see what's in it, and remove items from it that you don't want trained.  **Example:** ```python plan = vn.get_training_plan()  plan.get_summary() ```
    - `__init__(plan)`：提供对象协议或生命周期约定。`plan`（该函数的业务参数，详见源码类型标注）。位于第 249–250 行。
    - `__str__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 252–253 行。
    - `__repr__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 255–256 行。
    - `get_summary()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。**Example:** ```python plan = vn.get_training_plan()  plan.get_summary() ```  Get a summary of the training plan.  Retur位于第 258–273 行。
    - `remove_item(item)`：移除。`item`（该函数的业务参数，详见源码类型标注）。**Example:** ```python plan = vn.get_training_plan()  plan.remove_item("Train on SQL: What is the average salary of empl位于第 275–292 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/utils.py`

- **目录**：`src/vanna/legacy`。**文件名**：`utils.py`。**语言/类型**：Python。**行数**：73。**字节**：2202。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 0 个，模块函数 3 个，方法 0 个。
  - **函数 `validate_config_path`**：校验合法性。`path`（该函数的业务参数，详见源码类型标注）。行 10–20。
  - **函数 `sanitize_model_name`**：完成该模块中的具体处理。`model_name`（该函数的业务参数，详见源码类型标注）。行 23–48。
  - **函数 `deterministic_uuid`**：完成该模块中的具体处理。`content`（文本内容）。Creates deterministic UUID on hash value of string or byte content.  Args:     content: String or byte representation of data.  Returns:     UUID of the content行 51–72。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/vannadb/__init__.py`

- **目录**：`src/vanna/legacy/vannadb`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：48。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/vannadb/vannadb_vector.py`

- **目录**：`src/vanna/legacy/vannadb`。**文件名**：`vannadb_vector.py`。**语言/类型**：Python。**行数**：469。**字节**：15047。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 20 个。
  - **类 `VannaDB_VectorStore`**（核心类型或辅助类，基类：VannaBase, VannaAdvanced，行 24–468）：职责见方法列表。
    - `__init__(vanna_model, vanna_api_key, config)`：提供对象协议或生命周期约定。`vanna_model`（该函数的业务参数，详见源码类型标注）、`vanna_api_key`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。位于第 25–42 行。
    - `_rpc_call(method, params)`：完成该模块中的具体处理。`method`（该函数的业务参数，详见源码类型标注）、`params`（该函数的业务参数，详见源码类型标注）。位于第 44–64 行。
    - `_dataclass_to_dict(obj)`：完成该模块中的具体处理。`obj`（该函数的业务参数，详见源码类型标注）。位于第 66–67 行。
    - `get_all_functions()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 69–106 行。
    - `get_function(question, additional_data)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）、`additional_data`（该函数的业务参数，详见源码类型标注）。位于第 108–158 行。
    - `create_function(question, sql, plotly_code)`：创建并初始化。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）、`plotly_code`（该函数的业务参数，详见源码类型标注）。位于第 160–200 行。
    - `update_function(old_function_name, updated_function)`：更新。`old_function_name`（该函数的业务参数，详见源码类型标注）、`updated_function`（该函数的业务参数，详见源码类型标注）。Update an existing SQL function based on the provided parameters.  Args:     old_function_name (str): The current name o位于第 202–282 行。
    - `delete_function(function_name)`：删除并清理。`function_name`（该函数的业务参数，详见源码类型标注）。位于第 284–307 行。
    - `create_model(model)`：创建并初始化。`model`（模型名称）。**Example:** ```python success = vn.create_model("my_model") ``` Create a new model.  Args:     model (str): The name of位于第 309–333 行。
    - `get_models()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。**Example:** ```python models = vn.get_models() ```  List the models that belong to the user.  Returns:     List[str]: A位于第 335–354 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 356–358 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 360–375 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 377–387 行。
    - `add_documentation(documentation)`：完成该模块中的具体处理。`documentation`（该函数的业务参数，详见源码类型标注）。位于第 389–399 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 401–414 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 416–429 行。
    - `get_related_training_data_cached(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 431–444 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 446–452 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 454–460 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 462–468 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/vllm/__init__.py`

- **目录**：`src/vanna/legacy/vllm`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：23。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/vllm/vllm.py`

- **目录**：`src/vanna/legacy/vllm`。**文件名**：`vllm.py`。**语言/类型**：Python。**行数**：98。**字节**：3083。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 7 个。
  - **类 `Vllm`**（核心类型或辅助类，基类：VannaBase，行 8–97）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 9–29 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 31–32 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 34–35 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 37–38 行。
    - `extract_sql_query(text)`：完成该模块中的具体处理。`text`（该函数的业务参数，详见源码类型标注）。Extracts the first SQL statement after the word 'select', ignoring case, matches until the first semicolon, three backti位于第 40–60 行。
    - `generate_sql(question)`：生成。`question`（该函数的业务参数，详见源码类型标注）。位于第 62–71 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 73–97 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/weaviate/__init__.py`

- **目录**：`src/vanna/legacy/weaviate`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：46。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/weaviate/weaviate_vector.py`

- **目录**：`src/vanna/legacy/weaviate`。**文件名**：`weaviate_vector.py`。**语言/类型**：Python。**行数**：195。**字节**：7861。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 14 个。
  - **类 `WeaviateDatabase`**（核心类型或辅助类，基类：VannaBase，行 8–194）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。Initialize the VannaEnhanced class with the provided configuration.  :param config: Dictionary containing configuration 位于第 9–47 行。
    - `_create_collections_if_not_exist()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 49–73 行。
    - `_initialize_weaviate_client()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 75–89 行。
    - `generate_embedding(data)`：生成。`data`（该函数的业务参数，详见源码类型标注）。位于第 91–94 行。
    - `_insert_data(cluster_key, data_object, vector)`：完成该模块中的具体处理。`cluster_key`（该函数的业务参数，详见源码类型标注）、`data_object`（该函数的业务参数，详见源码类型标注）、`vector`（该函数的业务参数，详见源码类型标注）。位于第 96–102 行。
    - `add_ddl(ddl)`：完成该模块中的具体处理。`ddl`（该函数的业务参数，详见源码类型标注）。位于第 104–109 行。
    - `add_documentation(doc)`：完成该模块中的具体处理。`doc`（该函数的业务参数，详见源码类型标注）。位于第 111–116 行。
    - `add_question_sql(question, sql)`：完成该模块中的具体处理。`question`（该函数的业务参数，详见源码类型标注）、`sql`（待执行的 SQL 文本）。位于第 118–126 行。
    - `_query_collection(cluster_key, vector_input, return_properties)`：完成该模块中的具体处理。`cluster_key`（该函数的业务参数，详见源码类型标注）、`vector_input`（该函数的业务参数，详见源码类型标注）、`return_properties`（该函数的业务参数，详见源码类型标注）。位于第 128–142 行。
    - `get_related_ddl(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 144–147 行。
    - `get_related_documentation(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 149–152 行。
    - `get_similar_question_sql(question)`：读取并返回。`question`（该函数的业务参数，详见源码类型标注）。位于第 154–162 行。
    - `get_training_data()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 164–173 行。
    - `remove_training_data(id)`：移除。`id`（该函数的业务参数，详见源码类型标注）。位于第 175–194 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/xinference/__init__.py`

- **目录**：`src/vanna/legacy/xinference`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：2。**字节**：35。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/legacy/xinference/xinference.py`

- **目录**：`src/vanna/legacy/xinference`。**文件名**：`xinference.py`。**语言/类型**：Python。**行数**：54。**字节**：1880。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：0.x 兼容实现。仅修缺陷，不扩展。迁移请用 LegacyVannaAdapter 与 integrations。
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `Xinference`**（核心类型或辅助类，基类：VannaBase，行 9–53）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 10–18 行。
    - `system_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 20–21 行。
    - `user_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 23–24 行。
    - `assistant_message(message)`：完成该模块中的具体处理。`message`（用户自然语言输入）。位于第 26–27 行。
    - `submit_prompt(prompt)`：完成该模块中的具体处理。`prompt`（该函数的业务参数，详见源码类型标注）。位于第 29–53 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/py.typed`

- **目录**：`src/vanna`。**文件名**：`py.typed`。**语言/类型**：.typed。**行数**：0。**字节**：0。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/__init__.py`

- **目录**：`src/vanna/servers`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：17。**字节**：412。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Server implementations for the Vanna Agents framework.

This module provides Flask and FastAPI server factories for serving
Vanna agents over HTTP with SSE, WebSocket, and polling endpoints.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/__main__.py`

- **目录**：`src/vanna/servers`。**文件名**：`__main__.py`。**语言/类型**：Python。**行数**：9。**字节**：130。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Entry point for running Vanna Agents servers.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/base/__init__.py`

- **目录**：`src/vanna/servers/base`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：19。**字节**：407。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Base server components for the Vanna Agents framework.

This module provides framework-agnostic components for handling chat
requests and responses.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/base/chat_handler.py`

- **目录**：`src/vanna/servers/base`。**文件名**：`chat_handler.py`。**语言/类型**：Python。**行数**：66。**字节**：1777。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Framework-agnostic chat handling logic.
- **结构摘要**：类 1 个，模块函数 0 个，方法 4 个。
  - **类 `ChatHandler`**（核心类型或辅助类，基类：无，行 12–65）：Core chat handling logic - framework agnostic.
    - `__init__(agent)`：提供对象协议或生命周期约定。`agent`（Agent 实例）。Initialize chat handler.  Args:     agent: The agent to handle chat requests位于第 15–24 行。
    - `async handle_stream(request)`：处理请求或事件。`request`（下游请求对象）。Stream chat responses.  Args:     request: Chat request  Yields:     Chat stream chunks位于第 26–46 行。
    - `async handle_poll(request)`：处理请求或事件。`request`（下游请求对象）。Handle polling-based chat.  Args:     request: Chat request  Returns:     Complete chat response位于第 48–61 行。
    - `_generate_conversation_id()`：生成。仅依赖实例或类自身状态，不额外接收调用方参数。Generate new conversation ID.位于第 63–65 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/base/models.py`

- **目录**：`src/vanna/servers/base`。**文件名**：`models.py`。**语言/类型**：Python。**行数**：112。**字节**：3767。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Request and response models for server endpoints.
- **结构摘要**：类 3 个，模块函数 0 个，方法 3 个。
  - **类 `ChatRequest`**（Pydantic 数据模型，基类：BaseModel，行 16–30）：Request model for chat endpoints.
  - **类 `ChatStreamChunk`**（Pydantic 数据模型，基类：BaseModel，行 33–89）：Single chunk in a streaming chat response.
    - `from_component(component, conversation_id, request_id)`：从外部结构转换而来。`component`（该函数的业务参数，详见源码类型标注）、`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）。Create chunk from UI component or rich component.位于第 47–76 行。
    - `from_component_update(update, conversation_id, request_id)`：从外部结构转换而来。`update`（该函数的业务参数，详见源码类型标注）、`conversation_id`（会话标识，空则新建）、`request_id`（该函数的业务参数，详见源码类型标注）。Create chunk from component update.位于第 79–89 行。
  - **类 `ChatResponse`**（Pydantic 数据模型，基类：BaseModel，行 92–111）：Complete chat response for polling endpoints.
    - `from_chunks(chunks)`：从外部结构转换而来。`chunks`（该函数的业务参数，详见源码类型标注）。Create response from chunks.位于第 101–111 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/base/rich_chat_handler.py`

- **目录**：`src/vanna/servers/base`。**文件名**：`rich_chat_handler.py`。**语言/类型**：Python。**行数**：142。**字节**：5491。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/base/templates.py`

- **目录**：`src/vanna/servers/base`。**文件名**：`templates.py`。**语言/类型**：Python。**行数**：332。**字节**：14418。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：HTML templates for Vanna Agents servers.
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `get_vanna_component_script`**：读取并返回。`dev_mode`（该函数的业务参数，详见源码类型标注）、`static_path`（该函数的业务参数，详见源码类型标注）、`cdn_url`（该函数的业务参数，详见源码类型标注）。Get the script tag for loading Vanna web components.  Args:     dev_mode: If True, load from local static files     static_path: Path to static assets in dev mo行 8–28。
  - **函数 `get_index_html`**：读取并返回。`dev_mode`（该函数的业务参数，详见源码类型标注）、`static_path`（该函数的业务参数，详见源码类型标注）、`cdn_url`（该函数的业务参数，详见源码类型标注）、`api_base_url`（该函数的业务参数，详见源码类型标注）。Generate index HTML with configurable component loading.  Args:     dev_mode: If True, load components from local static files     static_path: Path to static a行 31–327。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/cli/__init__.py`

- **目录**：`src/vanna/servers/cli`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：130。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：CLI components for Vanna Agents servers.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/cli/server_runner.py`

- **目录**：`src/vanna/servers/cli`。**文件名**：`server_runner.py`。**语言/类型**：Python。**行数**：205。**字节**：7010。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：CLI for running Vanna Agents servers with example agents.
- **结构摘要**：类 1 个，模块函数 1 个，方法 2 个。
  - **类 `ExampleAgentLoader`**（核心类型或辅助类，基类：无，行 14–71）：Loads example agents for the CLI.
    - `list_available_examples()`：枚举并列出。无显式位置参数。Return available examples with descriptions.位于第 18–31 行。
    - `load_example_agent(example_name)`：加载。`example_name`（该函数的业务参数，详见源码类型标注）。Load an example agent by name.  Args:     example_name: Name of the example to load  Returns:     Configured agent insta位于第 34–71 行。
  - **函数 `main`**：完成该模块中的具体处理。`framework`（该函数的业务参数，详见源码类型标注）、`port`（该函数的业务参数，详见源码类型标注）、`host`（该函数的业务参数，详见源码类型标注）、`example`（该函数的业务参数，详见源码类型标注）、`list_examples`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）、`debug`（该函数的业务参数，详见源码类型标注）、`dev`（该函数的业务参数，详见源码类型标注）、`static_folder`（该函数的业务参数，详见源码类型标注）、`cdn_url`（该函数的业务参数，详见源码类型标注）。Run Vanna Agents server with optional example agent.行 104–200。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/fastapi/__init__.py`

- **目录**：`src/vanna/servers/fastapi`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：127。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：FastAPI server implementation for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/fastapi/app.py`

- **目录**：`src/vanna/servers/fastapi`。**文件名**：`app.py`。**语言/类型**：Python。**行数**：164。**字节**：5518。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：FastAPI server factory for Vanna Agents.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `VannaFastAPIServer`**（核心类型或辅助类，基类：无，行 16–163）：FastAPI server factory for Vanna Agents.
    - `__init__(agent, config)`：提供对象协议或生命周期约定。`agent`（Agent 实例）、`config`（配置对象）。Initialize FastAPI server.  Args:     agent: The agent to serve (must have user_resolver configured)     config: Optiona位于第 19–28 行。
    - `create_app()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。Create configured FastAPI app.  Returns:     Configured FastAPI application位于第 30–80 行。
    - `run()`：运行。仅依赖实例或类自身状态，不额外接收调用方参数。Run the FastAPI server.  This method automatically detects if running in an async environment (Jupyter, Colab, IPython, 位于第 82–163 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/fastapi/routes.py`

- **目录**：`src/vanna/servers/fastapi`。**文件名**：`routes.py`。**语言/类型**：Python。**行数**：184。**字节**：6907。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：FastAPI route implementations for Vanna Agents.
- **结构摘要**：类 0 个，模块函数 1 个，方法 0 个。
  - **函数 `register_chat_routes`**：注册到容器。`app`（该函数的业务参数，详见源码类型标注）、`chat_handler`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。Register chat routes on FastAPI app.  Args:     app: FastAPI application     chat_handler: Chat handler instance     config: Server configuration行 17–183。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/flask/__init__.py`

- **目录**：`src/vanna/servers/flask`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：8。**字节**：121。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Flask server implementation for Vanna Agents.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/flask/app.py`

- **目录**：`src/vanna/servers/flask`。**文件名**：`app.py`。**语言/类型**：Python。**行数**：133。**字节**：4118。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Flask server factory for Vanna Agents.
- **结构摘要**：类 1 个，模块函数 0 个，方法 3 个。
  - **类 `VannaFlaskServer`**（核心类型或辅助类，基类：无，行 16–132）：Flask server factory for Vanna Agents.
    - `__init__(agent, config)`：提供对象协议或生命周期约定。`agent`（Agent 实例）、`config`（配置对象）。Initialize Flask server.  Args:     agent: The agent to serve (must have user_resolver configured)     config: Optional 位于第 19–28 行。
    - `create_app()`：创建并初始化。仅依赖实例或类自身状态，不额外接收调用方参数。Create configured Flask app.  Returns:     Configured Flask application位于第 30–58 行。
    - `run()`：运行。仅依赖实例或类自身状态，不额外接收调用方参数。Run the Flask server.  This method automatically detects if running in an async environment (Jupyter, Colab, IPython, et位于第 60–132 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/servers/flask/routes.py`

- **目录**：`src/vanna/servers/flask`。**文件名**：`routes.py`。**语言/类型**：Python。**行数**：138。**字节**：4763。
- **主要功能简介**：FastAPI/Flask/CLI 服务端，把 Agent 暴露为 SSE 聊天接口。
- **模块文档字符串**：Flask route implementations for Vanna Agents.
- **结构摘要**：类 0 个，模块函数 1 个，方法 0 个。
  - **函数 `register_chat_routes`**：注册到容器。`app`（该函数的业务参数，详见源码类型标注）、`chat_handler`（该函数的业务参数，详见源码类型标注）、`config`（配置对象）。Register chat routes on Flask app.  Args:     app: Flask application     chat_handler: Chat handler instance     config: Server configuration行 17–137。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/tools/__init__.py`

- **目录**：`src/vanna/tools`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：42。**字节**：865。
- **主要功能简介**：内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。
- **模块文档字符串**：Built-in tool implementations.
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/tools/agent_memory.py`

- **目录**：`src/vanna/tools`。**文件名**：`agent_memory.py`。**语言/类型**：Python。**行数**：323。**字节**：12243。
- **主要功能简介**：内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。
- **模块文档字符串**：Agent memory tools.

This module provides agent memory operations through an abstract AgentMemory interface,
allowing for different implementations (local vector DB, remote cloud service, etc.).
The tools access AgentMemory via ToolContext, which is populated by the Agent.
- **结构摘要**：类 6 个，模块函数 0 个，方法 12 个。
  - **类 `SaveQuestionToolArgsParams`**（Pydantic 数据模型，基类：BaseModel，行 25–34）：Parameters for saving question-tool-argument combinations.
  - **类 `SearchSavedCorrectToolUsesParams`**（Pydantic 数据模型，基类：BaseModel，行 37–51）：Parameters for searching saved tool usage patterns.
  - **类 `SaveTextMemoryParams`**（Pydantic 数据模型，基类：BaseModel，行 54–57）：Parameters for saving free-form text memories.
  - **类 `SaveQuestionToolArgsTool`**（抽象或具体服务/工具实现，基类：Tool[SaveQuestionToolArgsParams]，行 60–117）：Tool for saving successful question-tool-argument combinations.
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 64–65 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 68–71 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 73–74 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Save the tool usage pattern to agent memory.位于第 76–117 行。
  - **类 `SearchSavedCorrectToolUsesTool`**（抽象或具体服务/工具实现，基类：Tool[SearchSavedCorrectToolUsesParams]，行 120–266）：Tool for searching saved tool usage patterns.
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 124–125 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 128–129 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 131–132 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Search for similar tool usage patterns.位于第 134–266 行。
  - **类 `SaveTextMemoryTool`**（抽象或具体服务/工具实现，基类：Tool[SaveTextMemoryParams]，行 269–322）：Tool for saving free-form text memories.
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 273–274 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 277–278 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 280–281 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Save a text memory to agent memory.位于第 283–322 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/tools/file_system.py`

- **目录**：`src/vanna/tools`。**文件名**：`file_system.py`。**语言/类型**：Python。**行数**：880。**字节**：29840。
- **主要功能简介**：内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。
- **模块文档字符串**：File system tools with dependency injection support.

This module provides file system operations through an abstract FileSystem interface,
allowing for different implementations (local, remote, sandboxed, etc.).
The tools accept a FileSystem instance via dependency injection.
- **结构摘要**：类 15 个，模块函数 2 个，方法 44 个。
  - **类 `FileSearchMatch`**（核心类型或辅助类，基类：无，行 33–37）：Represents a single search result within a file system.
  - **类 `CommandResult`**（核心类型或辅助类，基类：无，行 41–46）：Represents the result of executing a shell command.
  - **类 `FileSystem`**（抽象或具体服务/工具实现，基类：ABC，行 69–120）：Abstract base class for file system operations.
    - `async list_files(directory, context)`：枚举并列出。`directory`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。List files in a directory.位于第 73–75 行。
    - `async read_file(filename, context)`：完成该模块中的具体处理。`filename`（文件名）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Read the contents of a file.位于第 78–80 行。
    - `async write_file(filename, content, context, overwrite)`：完成该模块中的具体处理。`filename`（文件名）、`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`overwrite`（该函数的业务参数，详见源码类型标注）。Write content to a file.位于第 83–87 行。
    - `async exists(path, context)`：完成该模块中的具体处理。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a file or directory exists.位于第 90–92 行。
    - `async is_directory(path, context)`：判断是否满足条件。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a path is a directory.位于第 95–97 行。
    - `async search_files(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for files matching a query within the accessible namespace.位于第 100–109 行。
    - `async run_bash(command, context)`：运行。`command`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute a bash command within the accessible namespace.位于第 112–120 行。
  - **类 `LocalFileSystem`**（核心类型或辅助类，基类：FileSystem，行 123–336）：Local file system implementation with per-user isolation.
    - `__init__(working_directory)`：提供对象协议或生命周期约定。`working_directory`（该函数的业务参数，详见源码类型标注）。Initialize with a working directory.  Args:     working_directory: Base directory where user-specific folders will be cr位于第 126–132 行。
    - `_get_user_directory(context)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Get the user-specific directory by hashing the user ID.  Args:     context: Tool context containing user information  Re位于第 134–150 行。
    - `_resolve_path(path, context)`：解析并还原。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Resolve a path relative to the user's directory.  Args:     path: Path relative to user directory     context: Tool cont位于第 152–173 行。
    - `async list_files(directory, context)`：枚举并列出。`directory`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。List files in a directory within the user's isolated space.位于第 175–190 行。
    - `async read_file(filename, context)`：完成该模块中的具体处理。`filename`（文件名）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Read the contents of a file within the user's isolated space.位于第 192–202 行。
    - `async write_file(filename, content, context, overwrite)`：完成该模块中的具体处理。`filename`（文件名）、`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`overwrite`（该函数的业务参数，详见源码类型标注）。Write content to a file within the user's isolated space.位于第 204–218 行。
    - `async exists(path, context)`：完成该模块中的具体处理。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a file or directory exists within the user's isolated space.位于第 220–226 行。
    - `async is_directory(path, context)`：判断是否满足条件。`path`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Check if a path is a directory within the user's isolated space.位于第 228–234 行。
    - `async search_files(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Search for files within the user's isolated space.位于第 236–299 行。
    - `async run_bash(command, context)`：运行。`command`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Execute a bash command within the user's isolated space.位于第 301–336 行。
  - **类 `SearchFilesArgs`**（Pydantic 数据模型，基类：BaseModel，行 339–352）：Arguments for searching files.
  - **类 `SearchFilesTool`**（抽象或具体服务/工具实现，基类：Tool[SearchFilesArgs]，行 355–443）：Tool to search for files using the injected file system implementation.
    - `__init__(file_system)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）。位于第 358–359 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 362–363 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 366–367 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 369–370 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 372–443 行。
  - **类 `ListFilesArgs`**（Pydantic 数据模型，基类：BaseModel，行 446–451）：Arguments for listing files.
  - **类 `ListFilesTool`**（抽象或具体服务/工具实现，基类：Tool[ListFilesArgs]，行 454–509）：Tool to list files in a directory using dependency injection for file system access.
    - `__init__(file_system)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）。Initialize with optional file system dependency.位于第 457–459 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 462–463 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 466–467 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 469–470 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 472–509 行。
  - **类 `ReadFileArgs`**（Pydantic 数据模型，基类：BaseModel，行 512–515）：Arguments for reading a file.
  - **类 `ReadFileTool`**（抽象或具体服务/工具实现，基类：Tool[ReadFileArgs]，行 518–569）：Tool to read file contents using dependency injection for file system access.
    - `__init__(file_system)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）。Initialize with optional file system dependency.位于第 521–523 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 526–527 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 530–531 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 533–534 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 536–569 行。
  - **类 `WriteFileArgs`**（Pydantic 数据模型，基类：BaseModel，行 572–579）：Arguments for writing a file.
  - **类 `WriteFileTool`**（抽象或具体服务/工具实现，基类：Tool[WriteFileArgs]，行 582–636）：Tool to write content to a file using dependency injection for file system access.
    - `__init__(file_system)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）。Initialize with optional file system dependency.位于第 585–587 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 590–591 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 594–595 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 597–598 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 600–636 行。
  - **类 `LineEdit`**（Pydantic 数据模型，基类：BaseModel，行 639–663）：Definition of a single line-based edit operation.
    - `validate_line_range()`：校验合法性。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 657–663 行。
  - **类 `EditFileArgs`**（Pydantic 数据模型，基类：BaseModel，行 666–673）：Arguments for editing one or more sections within a file.
  - **类 `EditFileTool`**（抽象或具体服务/工具实现，基类：Tool[EditFileArgs]，行 676–864）：Tool to apply line-based edits to an existing file.
    - `__init__(file_system)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）。位于第 679–680 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 683–684 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 687–688 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 690–691 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 693–846 行。
    - `_range_error(filename, start_line, end_line, message)`：完成该模块中的具体处理。`filename`（文件名）、`start_line`（该函数的业务参数，详见源码类型标注）、`end_line`（该函数的业务参数，详见源码类型标注）、`message`（用户自然语言输入）。位于第 848–864 行。
  - **函数 `_make_snippet`**：完成该模块中的具体处理。`text`（该函数的业务参数，详见源码类型标注）、`query`（检索查询文本）、`context_window`（该函数的业务参数，详见源码类型标注）。Return a short snippet around the first occurrence of query in text.行 49–66。
  - **函数 `create_file_system_tools`**：创建并初始化。`file_system`（该函数的业务参数，详见源码类型标注）。Create a set of file system tools with optional dependency injection.行 868–879。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/tools/python.py`

- **目录**：`src/vanna/tools`。**文件名**：`python.py`。**语言/类型**：Python。**行数**：223。**字节**：6582。
- **主要功能简介**：内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。
- **模块文档字符串**：Python-specific tooling built on top of the file system service.
- **结构摘要**：类 4 个，模块函数 5 个，方法 10 个。
  - **类 `RunPythonFileArgs`**（Pydantic 数据模型，基类：BaseModel，行 25–39）：Arguments required to execute a Python file.
  - **类 `RunPythonFileTool`**（抽象或具体服务/工具实现，基类：Tool[RunPythonFileArgs]，行 42–83）：Execute a Python file using the provided file system service.
    - `__init__(file_system)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）。位于第 45–46 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 49–50 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 53–54 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 56–57 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 59–83 行。
  - **类 `PipInstallArgs`**（Pydantic 数据模型，基类：BaseModel，行 86–104）：Arguments required to run pip install.
  - **类 `PipInstallTool`**（抽象或具体服务/工具实现，基类：Tool[PipInstallArgs]，行 107–148）：Install Python packages using pip inside the workspace environment.
    - `__init__(file_system)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）。位于第 110–111 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 114–115 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 118–119 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 121–122 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 124–148 行。
  - **函数 `create_python_tools`**：创建并初始化。`file_system`（该函数的业务参数，详见源码类型标注）。Create Python-specific tools backed by a shared file system service.行 151–158。
  - **函数 `_quote_command`**：完成该模块中的具体处理。`parts`（该函数的业务参数，详见源码类型标注）。行 161–162。
  - **函数 `_truncate`**：完成该模块中的具体处理。`text`（该函数的业务参数，详见源码类型标注）、`limit`（返回条数上限）。行 165–168。
  - **函数 `_result_from_command`**：完成该模块中的具体处理。`summary`（该函数的业务参数，详见源码类型标注）、`command`（该函数的业务参数，详见源码类型标注）、`result`（该函数的业务参数，详见源码类型标注）。行 171–206。
  - **函数 `_error_result`**：完成该模块中的具体处理。`message`（用户自然语言输入）。行 209–222。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/tools/run_sql.py`

- **目录**：`src/vanna/tools`。**文件名**：`run_sql.py`。**语言/类型**：Python。**行数**：166。**字节**：6951。
- **主要功能简介**：内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。
- **模块文档字符串**：Generic SQL query execution tool with dependency injection.
- **结构摘要**：类 1 个，模块函数 0 个，方法 5 个。
  - **类 `RunSqlTool`**（抽象或具体服务/工具实现，基类：Tool[RunSqlToolArgs]，行 18–165）：Tool that executes SQL queries using an injected SqlRunner implementation.
    - `__init__(sql_runner, file_system, custom_tool_name, custom_tool_description)`：提供对象协议或生命周期约定。`sql_runner`（该函数的业务参数，详见源码类型标注）、`file_system`（该函数的业务参数，详见源码类型标注）、`custom_tool_name`（该函数的业务参数，详见源码类型标注）、`custom_tool_description`（该函数的业务参数，详见源码类型标注）。Initialize the tool with a SqlRunner implementation.  Args:     sql_runner: SqlRunner implementation that handles actual位于第 21–39 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 42–43 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 46–51 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 53–54 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Execute a SQL query using the injected SqlRunner.位于第 56–165 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/tools/visualize_data.py`

- **目录**：`src/vanna/tools`。**文件名**：`visualize_data.py`。**语言/类型**：Python。**行数**：196。**字节**：7551。
- **主要功能简介**：内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。
- **模块文档字符串**：Tool for visualizing DataFrame data from CSV files.
- **结构摘要**：类 2 个，模块函数 0 个，方法 5 个。
  - **类 `VisualizeDataArgs`**（Pydantic 数据模型，基类：BaseModel，行 23–29）：Arguments for visualize_data tool.
  - **类 `VisualizeDataTool`**（抽象或具体服务/工具实现，基类：Tool[VisualizeDataArgs]，行 32–195）：Tool that reads CSV files and generates visualizations using dependency injection.
    - `__init__(file_system, plotly_generator)`：提供对象协议或生命周期约定。`file_system`（该函数的业务参数，详见源码类型标注）、`plotly_generator`（该函数的业务参数，详见源码类型标注）。Initialize the tool with FileSystem and PlotlyChartGenerator.  Args:     file_system: FileSystem implementation for read位于第 35–47 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 50–51 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 54–55 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 57–58 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。Read CSV file and generate visualization.位于第 60–195 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/utils/__init__.py`

- **目录**：`src/vanna/utils`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：0。**字节**：0。
- **主要功能简介**：Vanna 2.0 框架源码模块，为自然语言到 SQL 的企业级 Agent 提供可插拔能力。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `src/vanna/web_components/__init__.py`

- **目录**：`src/vanna/web_components`。**文件名**：`__init__.py`。**语言/类型**：Python。**行数**：45。**字节**：1040。
- **主要功能简介**：简单组件与富组件的服务端数据模型，对应前端 Web Component。
- **模块文档字符串**：Web components for Vanna Agents.

This module provides web components built with Lit that can be embedded
in web applications to provide rich UI for Vanna agent interactions.
- **结构摘要**：类 0 个，模块函数 2 个，方法 0 个。
  - **函数 `get_component_files`**：读取并返回。无显式位置参数。Get paths to all web component files.行 13–19。
  - **函数 `get_component_html`**：读取并返回。无显式位置参数。Get HTML template for including components.行 22–41。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/conftest.py`

- **目录**：`tests`。**文件名**：`conftest.py`。**语言/类型**：Python。**行数**：89。**字节**：3036。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Pytest configuration and shared fixtures for Vanna v2 test suite.
- **结构摘要**：类 0 个，模块函数 3 个，方法 0 个。
  - **函数 `pytest_configure`**：完成该模块中的具体处理。`config`（配置对象）。Configure pytest with custom markers.行 14–26。
  - **函数 `pytest_collection_modifyitems`**：完成该模块中的具体处理。`config`（配置对象）、`items`（该函数的业务参数，详见源码类型标注）。Automatically skip tests if required API keys are missing.行 29–66。
  - **函数 `chinook_db`**：完成该模块中的具体处理。`tmp_path_factory`（该函数的业务参数，详见源码类型标注）。Downloads the Chinook SQLite database and returns a SqliteRunner.  Uses session scope so the database is only downloaded once per test session.行 70–88。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_agent_memory.py`

- **目录**：`tests`。**文件名**：`test_agent_memory.py`。**语言/类型**：Python。**行数**：447。**字节**：15252。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Integration tests for AgentMemory implementations.

These tests verify actual functionality against running services.
Tests are marked by implementation and skip if dependencies are unavailable.
- **结构摘要**：类 1 个，模块函数 5 个，方法 11 个。
  - **类 `TestLocalAgentMemory`**（pytest 测试类，基类：无，行 94–446）：Tests for local AgentMemory implementations (ChromaDB, Qdrant, FAISS).
    - `async test_save_and_search(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test saving and searching tool usage patterns.位于第 98–123 行。
    - `async test_multiple_memories(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test storing and searching multiple memories.位于第 126–159 行。
    - `async test_tool_filter(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test filtering by tool name.位于第 162–191 行。
    - `async test_clear_memories(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test clearing memories.位于第 194–207 行。
    - `async test_get_recent_memories(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test getting recent memories.位于第 210–237 行。
    - `async test_delete_by_id(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test deleting memories by ID.位于第 240–273 行。
    - `async test_save_and_search_text_memories(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test saving and searching text memories.位于第 276–309 行。
    - `async test_multiple_text_memories(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test storing and searching multiple text memories.位于第 312–337 行。
    - `async test_get_recent_text_memories(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test getting recent text memories.位于第 340–360 行。
    - `async test_delete_text_memory(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test deleting text memories by ID.位于第 363–390 行。
    - `async test_mixed_tool_and_text_memories(memory_fixture, test_user, request)`：完成该模块中的具体处理。`memory_fixture`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`request`（下游请求对象）。Test that tool memories and text memories can coexist without errors.位于第 393–446 行。
  - **函数 `test_user`**：完成该模块中的具体处理。无显式位置参数。Test user for context.行 19–26。
  - **函数 `create_test_context`**：创建并初始化。`test_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Helper to create test context with specific agent memory.行 29–37。
  - **函数 `chromadb_memory`**：完成该模块中的具体处理。无显式位置参数。Create ChromaDB memory instance.行 41–55。
  - **函数 `qdrant_memory`**：完成该模块中的具体处理。无显式位置参数。Create Qdrant memory instance.行 59–71。
  - **函数 `faiss_memory`**：完成该模块中的具体处理。无显式位置参数。Create FAISS memory instance.行 75–87。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_agent_memory_sanity.py`

- **目录**：`tests`。**文件名**：`test_agent_memory_sanity.py`。**语言/类型**：Python。**行数**：708。**字节**：25180。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Sanity tests for AgentMemory implementations.

These tests verify that:
1. Each AgentMemory implementation correctly implements the AgentMemory interface
2. Imports are working correctly for all vector store modules
3. Basic class instantiation works (without requiring actual service connections)

Note: These tests do NOT execute actual vector operations against services.
They are lightweight sani
- **结构摘要**：类 15 个，模块函数 0 个，方法 50 个。
  - **类 `TestAgentMemoryInterface`**（pytest 测试类，基类：无，行 18–74）：Test that the AgentMemory interface is properly defined.
    - `test_agent_memory_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that AgentMemory can be imported.位于第 21–25 行。
    - `test_agent_memory_is_abstract()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that AgentMemory is an abstract base class.位于第 27–31 行。
    - `test_agent_memory_has_required_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that AgentMemory defines all required abstract methods.位于第 33–54 行。
    - `test_all_methods_are_async()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that all AgentMemory methods are async.位于第 56–74 行。
  - **类 `TestToolMemoryModel`**（pytest 测试类，基类：无，行 77–123）：Test the ToolMemory model.
    - `test_tool_memory_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ToolMemory can be imported.位于第 80–84 行。
    - `test_tool_memory_is_pydantic_model()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ToolMemory is a Pydantic model.位于第 86–91 行。
    - `test_tool_memory_has_required_fields()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ToolMemory has all required fields.位于第 93–106 行。
    - `test_tool_memory_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ToolMemory can be instantiated.位于第 108–123 行。
  - **类 `TestToolMemorySearchResultModel`**（pytest 测试类，基类：无，行 126–145）：Test the ToolMemorySearchResult model.
    - `test_memory_search_result_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ToolMemorySearchResult can be imported.位于第 129–133 行。
    - `test_memory_search_result_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ToolMemorySearchResult can be instantiated.位于第 135–145 行。
  - **类 `TestTextMemoryModel`**（pytest 测试类，基类：无，行 148–188）：Test the TextMemory model.
    - `test_text_memory_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that TextMemory can be imported.位于第 151–155 行。
    - `test_text_memory_is_pydantic_model()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that TextMemory is a Pydantic model.位于第 157–162 行。
    - `test_text_memory_has_required_fields()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that TextMemory has all required fields.位于第 164–177 行。
    - `test_text_memory_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that TextMemory can be instantiated.位于第 179–188 行。
  - **类 `TestTextMemorySearchResultModel`**（pytest 测试类，基类：无，行 191–210）：Test the TextMemorySearchResult model.
    - `test_text_memory_search_result_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that TextMemorySearchResult can be imported.位于第 194–198 行。
    - `test_text_memory_search_result_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that TextMemorySearchResult can be instantiated.位于第 200–210 行。
  - **类 `TestChromaDBAgentMemory`**（pytest 测试类，基类：无，行 213–276）：Sanity tests for ChromaDB AgentMemory implementation.
    - `test_chromadb_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ChromaAgentMemory can be imported.位于第 216–223 行。
    - `test_chromadb_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ChromaAgentMemory implements AgentMemory.位于第 225–233 行。
    - `test_chromadb_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ChromaAgentMemory implements all required methods.位于第 235–259 行。
    - `test_chromadb_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ChromaAgentMemory can be instantiated.位于第 261–276 行。
  - **类 `TestQdrantAgentMemory`**（pytest 测试类，基类：无，行 279–333）：Sanity tests for Qdrant AgentMemory implementation.
    - `test_qdrant_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that QdrantAgentMemory can be imported.位于第 282–289 行。
    - `test_qdrant_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that QdrantAgentMemory implements AgentMemory.位于第 291–299 行。
    - `test_qdrant_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that QdrantAgentMemory implements all required methods.位于第 301–321 行。
    - `test_qdrant_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that QdrantAgentMemory can be instantiated.位于第 323–333 行。
  - **类 `TestPineconeAgentMemory`**（pytest 测试类，基类：无，行 336–378）：Sanity tests for Pinecone AgentMemory implementation.
    - `test_pinecone_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PineconeAgentMemory can be imported.位于第 339–346 行。
    - `test_pinecone_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PineconeAgentMemory implements AgentMemory.位于第 348–356 行。
    - `test_pinecone_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PineconeAgentMemory implements all required methods.位于第 358–378 行。
  - **类 `TestMilvusAgentMemory`**（pytest 测试类，基类：无，行 381–423）：Sanity tests for Milvus AgentMemory implementation.
    - `test_milvus_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MilvusAgentMemory can be imported.位于第 384–391 行。
    - `test_milvus_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MilvusAgentMemory implements AgentMemory.位于第 393–401 行。
    - `test_milvus_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MilvusAgentMemory implements all required methods.位于第 403–423 行。
  - **类 `TestWeaviateAgentMemory`**（pytest 测试类，基类：无，行 426–468）：Sanity tests for Weaviate AgentMemory implementation.
    - `test_weaviate_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that WeaviateAgentMemory can be imported.位于第 429–436 行。
    - `test_weaviate_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that WeaviateAgentMemory implements AgentMemory.位于第 438–446 行。
    - `test_weaviate_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that WeaviateAgentMemory implements all required methods.位于第 448–468 行。
  - **类 `TestFAISSAgentMemory`**（pytest 测试类，基类：无，行 471–527）：Sanity tests for FAISS AgentMemory implementation.
    - `test_faiss_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that FAISSAgentMemory can be imported.位于第 474–481 行。
    - `test_faiss_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that FAISSAgentMemory implements AgentMemory.位于第 483–491 行。
    - `test_faiss_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that FAISSAgentMemory implements all required methods.位于第 493–513 行。
    - `test_faiss_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that FAISSAgentMemory can be instantiated.位于第 515–527 行。
  - **类 `TestOpenSearchAgentMemory`**（pytest 测试类，基类：无，行 530–572）：Sanity tests for OpenSearch AgentMemory implementation.
    - `test_opensearch_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that OpenSearchAgentMemory can be imported.位于第 533–540 行。
    - `test_opensearch_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that OpenSearchAgentMemory implements AgentMemory.位于第 542–550 行。
    - `test_opensearch_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that OpenSearchAgentMemory implements all required methods.位于第 552–572 行。
  - **类 `TestAzureAISearchAgentMemory`**（pytest 测试类，基类：无，行 575–617）：Sanity tests for Azure AI Search AgentMemory implementation.
    - `test_azuresearch_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that AzureAISearchAgentMemory can be imported.位于第 578–585 行。
    - `test_azuresearch_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that AzureAISearchAgentMemory implements AgentMemory.位于第 587–595 行。
    - `test_azuresearch_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that AzureAISearchAgentMemory implements all required methods.位于第 597–617 行。
  - **类 `TestMarqoAgentMemory`**（pytest 测试类，基类：无，行 620–662）：Sanity tests for Marqo AgentMemory implementation.
    - `test_marqo_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MarqoAgentMemory can be imported.位于第 623–630 行。
    - `test_marqo_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MarqoAgentMemory implements AgentMemory.位于第 632–640 行。
    - `test_marqo_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MarqoAgentMemory implements all required methods.位于第 642–662 行。
  - **类 `TestDemoAgentMemory`**（pytest 测试类，基类：无，行 665–707）：Sanity tests for DemoAgentMemory (in-memory) implementation.
    - `test_demo_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DemoAgentMemory can be imported.位于第 668–672 行。
    - `test_demo_implements_agent_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DemoAgentMemory implements AgentMemory.位于第 674–679 行。
    - `test_demo_has_all_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DemoAgentMemory implements all required methods.位于第 681–698 行。
    - `test_demo_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DemoAgentMemory can be instantiated.位于第 700–707 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_agents.py`

- **目录**：`tests`。**文件名**：`test_agents.py`。**语言/类型**：Python。**行数**：196。**字节**：6667。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Simple end-to-end tests for Vanna agents.

Tests use agent.send_message and validate the response components.
- **结构摘要**：类 1 个，模块函数 7 个，方法 1 个。
  - **类 `SimpleUserResolver`**（核心类型或辅助类，基类：UserResolver，行 14–20）：Simple user resolver for tests - always returns the same test user.
    - `async resolve_user(request_context)`：解析并还原。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）。位于第 17–20 行。
  - **函数 `create_agent`**：创建并初始化。`llm_service`（该函数的业务参数，详见源码类型标注）、`sql_runner`（该函数的业务参数，详见源码类型标注）。Helper to create a configured agent.行 23–52。
  - **函数 `async test_agent_top_artist`**：完成该模块中的具体处理。`agent`（Agent 实例）、`expected_artist`（该函数的业务参数，详见源码类型标注）。Common test logic for testing agent responses about top artist by sales.行 55–105。
  - **函数 `async test_anthropic_top_artist`**：完成该模块中的具体处理。`chinook_db`（该函数的业务参数，详见源码类型标注）。Test Anthropic agent finding the top artist by sales.行 110–118。
  - **函数 `async test_openai_top_artist`**：完成该模块中的具体处理。`chinook_db`（该函数的业务参数，详见源码类型标注）。Test OpenAI agent finding the top artist by sales.行 123–131。
  - **函数 `async test_azure_openai_top_artist`**：完成该模块中的具体处理。`chinook_db`（该函数的业务参数，详见源码类型标注）。Test Azure OpenAI agent finding the top artist by sales.行 136–154。
  - **函数 `async test_ollama_top_artist`**：完成该模块中的具体处理。`chinook_db`（该函数的业务参数，详见源码类型标注）。Test Ollama agent finding the top artist by sales.行 172–182。
  - **函数 `async test_gemini_top_artist`**：完成该模块中的具体处理。`chinook_db`（该函数的业务参数，详见源码类型标注）。Test Gemini agent finding the top artist by sales.行 187–195。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_azureopenai_llm.py`

- **目录**：`tests`。**文件名**：`test_azureopenai_llm.py`。**语言/类型**：Python。**行数**：298。**字节**：11388。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Unit tests for Azure OpenAI LLM service integration.

These tests validate the Azure OpenAI integration without making actual API calls.
- **结构摘要**：类 4 个，模块函数 0 个，方法 18 个。
  - **类 `TestReasoningModelDetection`**（pytest 测试类，基类：无，行 13–45）：Test reasoning model detection logic.
    - `test_is_reasoning_model_o1()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that o1 models are detected as reasoning models.位于第 16–20 行。
    - `test_is_reasoning_model_o3()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that o3 models are detected as reasoning models.位于第 22–24 行。
    - `test_is_reasoning_model_gpt5()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that GPT-5 series models are detected as reasoning models.位于第 26–32 行。
    - `test_is_not_reasoning_model()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that standard models are not detected as reasoning models.位于第 34–39 行。
    - `test_case_insensitive_detection()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that model detection is case insensitive.位于第 41–45 行。
  - **类 `TestAzureOpenAILlmServiceInitialization`**（pytest 测试类，基类：无，行 48–158）：Test Azure OpenAI service initialization.
    - `test_init_with_all_params(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test initialization with all parameters provided.位于第 52–69 行。
    - `test_init_with_reasoning_model(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test initialization with a reasoning model.位于第 72–81 行。
    - `test_init_from_environment(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test initialization from environment variables.位于第 93–103 行。
    - `test_init_missing_model_raises(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that missing model parameter raises ValueError.位于第 106–112 行。
    - `test_init_missing_endpoint_raises(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that missing azure_endpoint raises ValueError.位于第 115–121 行。
    - `test_init_missing_auth_raises(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that missing authentication raises ValueError.位于第 124–130 行。
    - `test_init_with_azure_ad_token_provider(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test initialization with Azure AD token provider.位于第 133–145 行。
    - `test_init_default_api_version(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that default API version is used when not specified.位于第 148–158 行。
  - **类 `TestAzureOpenAILlmServicePayloadBuilding`**（pytest 测试类，基类：无，行 161–273）：Test payload building for API requests.
    - `test_build_payload_includes_temperature_for_standard_model(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that temperature is included for standard models.位于第 165–188 行。
    - `test_build_payload_excludes_temperature_for_reasoning_model(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that temperature is excluded for reasoning models.位于第 191–213 行。
    - `test_build_payload_with_system_prompt(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that system prompt is added to messages.位于第 216–237 行。
    - `test_build_payload_with_tools(mock_azure_openai)`：完成该模块中的具体处理。`mock_azure_openai`（该函数的业务参数，详见源码类型标注）。Test that tools are properly formatted in payload.位于第 240–273 行。
  - **类 `TestImportError`**（pytest 测试类，基类：无，行 276–297）：Test import error handling.
    - `test_import_error_message()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that helpful error message is shown when openai is not installed.位于第 279–297 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_chromadb_persistence_fix.py`

- **目录**：`tests`。**文件名**：`test_chromadb_persistence_fix.py`。**语言/类型**：Python。**行数**：187。**字节**：6187。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Test for ChromaDB persistence fix.

This test verifies that ChromaDB collections can be retrieved without triggering
unnecessary embedding function initialization/model downloads.
- **结构摘要**：类 0 个，模块函数 4 个，方法 0 个。
  - **函数 `test_user`**：完成该模块中的具体处理。无显式位置参数。Test user for context.行 19–26。
  - **函数 `create_test_context`**：创建并初始化。`test_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Helper to create test context.行 29–37。
  - **函数 `async test_chromadb_collection_retrieval_without_embedding_function`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test that existing ChromaDB collections can be retrieved without initializing the embedding function (avoiding model downloads).  This test simulates the real-w行 41–133。
  - **函数 `async test_chromadb_collection_creation_with_embedding_function`**：完成该模块中的具体处理。无显式位置参数。Test that NEW ChromaDB collections are created WITH the embedding function.行 137–180。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_database_sanity.py`

- **目录**：`tests`。**文件名**：`test_database_sanity.py`。**语言/类型**：Python。**行数**：739。**字节**：28169。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Sanity tests for database implementations.

These tests verify that:
1. Each database implementation correctly implements the SqlRunner interface
2. Imports are working correctly for all database modules
3. Basic class instantiation works (without requiring actual database connections)

Note: These tests do NOT execute actual queries against databases.
They are lightweight sanity checks for the im
- **结构摘要**：类 17 个，模块函数 0 个，方法 76 个。
  - **类 `TestSqlRunnerInterface`**（pytest 测试类，基类：无，行 19–61）：Test that the SqlRunner interface is properly defined.
    - `test_sql_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqlRunner can be imported.位于第 22–26 行。
    - `test_sql_runner_is_abstract()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqlRunner is an abstract base class.位于第 28–33 行。
    - `test_sql_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqlRunner defines the run_sql abstract method.位于第 35–40 行。
    - `test_run_sql_method_signature()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that run_sql has the correct method signature.位于第 42–53 行。
    - `test_run_sql_is_async()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that run_sql is defined as an async method.位于第 55–61 行。
  - **类 `TestRunSqlToolArgsModel`**（pytest 测试类，基类：无，行 64–87）：Test the RunSqlToolArgs model.
    - `test_run_sql_tool_args_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that RunSqlToolArgs can be imported.位于第 67–71 行。
    - `test_run_sql_tool_args_has_sql_field()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that RunSqlToolArgs has a 'sql' field.位于第 73–80 行。
    - `test_run_sql_tool_args_is_pydantic_model()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that RunSqlToolArgs is a Pydantic model.位于第 82–87 行。
  - **类 `TestPostgresRunner`**（抽象或具体服务/工具实现，基类：无，行 90–158）：Sanity tests for PostgresRunner implementation.
    - `test_postgres_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PostgresRunner can be imported.位于第 93–97 行。
    - `test_postgres_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PostgresRunner implements SqlRunner interface.位于第 99–104 行。
    - `test_postgres_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PostgresRunner implements run_sql method.位于第 106–112 行。
    - `test_postgres_runner_instantiation_with_connection_string()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PostgresRunner can be instantiated with connection string.位于第 114–122 行。
    - `test_postgres_runner_instantiation_with_params()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PostgresRunner can be instantiated with individual parameters.位于第 124–139 行。
    - `test_postgres_runner_requires_valid_params()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PostgresRunner raises error with invalid parameters.位于第 141–146 行。
    - `test_postgres_runner_checks_psycopg2_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PostgresRunner checks for psycopg2 package.位于第 148–158 行。
  - **类 `TestSqliteRunner`**（抽象或具体服务/工具实现，基类：无，行 161–199）：Sanity tests for SqliteRunner implementation.
    - `test_sqlite_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqliteRunner can be imported.位于第 164–168 行。
    - `test_sqlite_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqliteRunner implements SqlRunner interface.位于第 170–175 行。
    - `test_sqlite_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqliteRunner implements run_sql method.位于第 177–183 行。
    - `test_sqlite_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqliteRunner can be instantiated with a database path.位于第 185–191 行。
    - `test_sqlite_uses_builtin_sqlite3()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SqliteRunner uses Python's built-in sqlite3 module.位于第 193–199 行。
  - **类 `TestLegacySqlRunner`**（抽象或具体服务/工具实现，基类：无，行 202–236）：Sanity tests for LegacySqlRunner adapter.
    - `test_legacy_sql_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that LegacySqlRunner can be imported.位于第 205–209 行。
    - `test_legacy_sql_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that LegacySqlRunner implements SqlRunner interface.位于第 211–216 行。
    - `test_legacy_sql_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that LegacySqlRunner implements run_sql method.位于第 218–224 行。
    - `test_legacy_sql_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that LegacySqlRunner can be instantiated with a VannaBase instance.位于第 226–236 行。
  - **类 `TestDatabaseIntegrationModules`**（pytest 测试类，基类：无，行 239–270）：Test that database integration modules can be imported.
    - `test_postgres_module_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that the postgres integration module can be imported.位于第 242–249 行。
    - `test_sqlite_module_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that the sqlite integration module can be imported.位于第 251–258 行。
    - `test_postgres_module_exports_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that postgres module exports PostgresRunner.位于第 260–264 行。
    - `test_sqlite_module_exports_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that sqlite module exports SqliteRunner.位于第 266–270 行。
  - **类 `TestLegacyVannaBaseConnections`**（pytest 测试类，基类：无，行 273–307）：Test that legacy VannaBase connection methods exist.
    - `test_vanna_base_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that VannaBase can be imported.位于第 276–280 行。
    - `test_vanna_base_has_connection_methods()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that VannaBase has various database connection methods.位于第 282–301 行。
    - `test_vanna_base_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that VannaBase has a run_sql method.位于第 303–307 行。
  - **类 `TestLegacyVannaAdapter`**（pytest 测试类，基类：无，行 310–324）：Test the LegacyVannaAdapter.
    - `test_legacy_vanna_adapter_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that LegacyVannaAdapter can be imported.位于第 313–317 行。
    - `test_legacy_vanna_adapter_is_tool_registry()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that LegacyVannaAdapter extends ToolRegistry.位于第 319–324 行。
  - **类 `TestSnowflakeRunner`**（抽象或具体服务/工具实现，基类：无，行 327–467）：Sanity tests for SnowflakeRunner implementation.
    - `test_snowflake_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner can be imported.位于第 330–334 行。
    - `test_snowflake_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner implements SqlRunner interface.位于第 336–341 行。
    - `test_snowflake_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner implements run_sql method.位于第 343–348 行。
    - `test_snowflake_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner can be instantiated with required parameters.位于第 350–361 行。
    - `test_snowflake_runner_key_pair_auth_with_path(tmp_path)`：完成该模块中的具体处理。`tmp_path`（该函数的业务参数，详见源码类型标注）。Test that SnowflakeRunner can be instantiated with RSA key-pair authentication using path.位于第 363–385 行。
    - `test_snowflake_runner_key_pair_auth_with_content()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner can be instantiated with RSA key-pair authentication using content.位于第 387–405 行。
    - `test_snowflake_runner_key_pair_auth_without_passphrase(tmp_path)`：完成该模块中的具体处理。`tmp_path`（该函数的业务参数，详见源码类型标注）。Test that SnowflakeRunner works with unencrypted private key (no passphrase).位于第 407–424 行。
    - `test_snowflake_runner_missing_auth_raises_error()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner raises error when no authentication method is provided.位于第 426–437 行。
    - `test_snowflake_runner_invalid_key_path_raises_error()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner raises error when private key file doesn't exist.位于第 439–450 行。
    - `test_snowflake_runner_password_auth_backwards_compatible()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that SnowflakeRunner maintains backward compatibility with password auth.位于第 452–467 行。
  - **类 `TestMySQLRunner`**（抽象或具体服务/工具实现，基类：无，行 470–501）：Sanity tests for MySQLRunner implementation.
    - `test_mysql_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MySQLRunner can be imported.位于第 473–477 行。
    - `test_mysql_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MySQLRunner implements SqlRunner interface.位于第 479–484 行。
    - `test_mysql_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MySQLRunner implements run_sql method.位于第 486–491 行。
    - `test_mysql_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MySQLRunner can be instantiated with required parameters.位于第 493–501 行。
  - **类 `TestClickHouseRunner`**（抽象或具体服务/工具实现，基类：无，行 504–535）：Sanity tests for ClickHouseRunner implementation.
    - `test_clickhouse_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ClickHouseRunner can be imported.位于第 507–511 行。
    - `test_clickhouse_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ClickHouseRunner implements SqlRunner interface.位于第 513–518 行。
    - `test_clickhouse_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ClickHouseRunner implements run_sql method.位于第 520–525 行。
    - `test_clickhouse_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that ClickHouseRunner can be instantiated with required parameters.位于第 527–535 行。
  - **类 `TestOracleRunner`**（抽象或具体服务/工具实现，基类：无，行 538–569）：Sanity tests for OracleRunner implementation.
    - `test_oracle_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that OracleRunner can be imported.位于第 541–545 行。
    - `test_oracle_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that OracleRunner implements SqlRunner interface.位于第 547–552 行。
    - `test_oracle_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that OracleRunner implements run_sql method.位于第 554–559 行。
    - `test_oracle_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that OracleRunner can be instantiated with required parameters.位于第 561–569 行。
  - **类 `TestBigQueryRunner`**（抽象或具体服务/工具实现，基类：无，行 572–601）：Sanity tests for BigQueryRunner implementation.
    - `test_bigquery_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that BigQueryRunner can be imported.位于第 575–579 行。
    - `test_bigquery_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that BigQueryRunner implements SqlRunner interface.位于第 581–586 行。
    - `test_bigquery_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that BigQueryRunner implements run_sql method.位于第 588–593 行。
    - `test_bigquery_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that BigQueryRunner can be instantiated with required parameters.位于第 595–601 行。
  - **类 `TestDuckDBRunner`**（抽象或具体服务/工具实现，基类：无，行 604–641）：Sanity tests for DuckDBRunner implementation.
    - `test_duckdb_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DuckDBRunner can be imported.位于第 607–611 行。
    - `test_duckdb_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DuckDBRunner implements SqlRunner interface.位于第 613–618 行。
    - `test_duckdb_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DuckDBRunner implements run_sql method.位于第 620–625 行。
    - `test_duckdb_runner_instantiation_memory()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DuckDBRunner can be instantiated for in-memory database.位于第 627–633 行。
    - `test_duckdb_runner_instantiation_file()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that DuckDBRunner can be instantiated with file path.位于第 635–641 行。
  - **类 `TestMSSQLRunner`**（抽象或具体服务/工具实现，基类：无，行 644–674）：Sanity tests for MSSQLRunner implementation.
    - `test_mssql_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MSSQLRunner can be imported.位于第 647–651 行。
    - `test_mssql_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MSSQLRunner implements SqlRunner interface.位于第 653–658 行。
    - `test_mssql_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MSSQLRunner implements run_sql method.位于第 660–665 行。
    - `test_mssql_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that MSSQLRunner can be instantiated with ODBC connection string.位于第 667–674 行。
  - **类 `TestPrestoRunner`**（抽象或具体服务/工具实现，基类：无，行 677–706）：Sanity tests for PrestoRunner implementation.
    - `test_presto_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PrestoRunner can be imported.位于第 680–684 行。
    - `test_presto_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PrestoRunner implements SqlRunner interface.位于第 686–691 行。
    - `test_presto_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PrestoRunner implements run_sql method.位于第 693–698 行。
    - `test_presto_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that PrestoRunner can be instantiated with required parameters.位于第 700–706 行。
  - **类 `TestHiveRunner`**（抽象或具体服务/工具实现，基类：无，行 709–738）：Sanity tests for HiveRunner implementation.
    - `test_hive_runner_import()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that HiveRunner can be imported.位于第 712–716 行。
    - `test_hive_runner_implements_sql_runner()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that HiveRunner implements SqlRunner interface.位于第 718–723 行。
    - `test_hive_runner_has_run_sql_method()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that HiveRunner implements run_sql method.位于第 725–730 行。
    - `test_hive_runner_instantiation()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。Test that HiveRunner can be instantiated with required parameters.位于第 732–738 行。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_gemini_integration.py`

- **目录**：`tests`。**文件名**：`test_gemini_integration.py`。**语言/类型**：Python。**行数**：189。**字节**：5603。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Google Gemini integration tests.

Basic unit tests for the Gemini LLM service integration.
End-to-end tests are in test_agents.py.

Note: Tests requiring API calls need GOOGLE_API_KEY environment variable.
- **结构摘要**：类 0 个，模块函数 7 个，方法 0 个。
  - **函数 `test_user`**：完成该模块中的具体处理。无显式位置参数。Test user for LLM requests.行 18–25。
  - **函数 `async test_gemini_import`**：完成该模块中的具体处理。无显式位置参数。Test that Gemini integration can be imported.行 30–35。
  - **函数 `async test_gemini_initialization_without_key`**：完成该模块中的具体处理。无显式位置参数。Test that Gemini service raises error without API key.行 40–56。
  - **函数 `async test_gemini_initialization`**：完成该模块中的具体处理。无显式位置参数。Test that Gemini service can be initialized with API key.行 61–76。
  - **函数 `async test_gemini_basic_request`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test a basic request without tools.行 81–110。
  - **函数 `async test_gemini_streaming_request`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test streaming request.行 115–144。
  - **函数 `async test_gemini_validate_tools`**：完成该模块中的具体处理。无显式位置参数。Test tool validation (does not require API key for actual calls).行 149–188。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_legacy_adapter.py`

- **目录**：`tests`。**文件名**：`test_legacy_adapter.py`。**语言/类型**：Python。**行数**：164。**字节**：6227。
- **主要功能简介**：Vanna 0.x 兼容层与旧向量/聊天实现。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Test for LegacyVannaAdapter retrofit functionality.

This test validates that a legacy VannaBase instance can be wrapped
with LegacyVannaAdapter and used with the new Agents framework.
- **结构摘要**：类 3 个，模块函数 2 个，方法 3 个。
  - **类 `SimpleUserResolver`**（核心类型或辅助类，基类：UserResolver，行 15–24）：Simple user resolver for tests - returns test user or admin.
    - `async resolve_user(request_context)`：解析并还原。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）。位于第 18–24 行。
  - **类 `MyVanna`**（核心类型或辅助类，基类：ChromaDB_VectorStore, MockLLM，行 38–41）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 39–41 行。
  - **类 `MyVanna`**（核心类型或辅助类，基类：ChromaDB_VectorStore, MockLLM，行 109–112）：职责见方法列表。
    - `__init__(config)`：提供对象协议或生命周期约定。`config`（配置对象）。位于第 110–112 行。
  - **函数 `async test_legacy_adapter_with_anthropic`**：完成该模块中的具体处理。无显式位置参数。Test LegacyVannaAdapter wrapping a legacy VannaBase instance with Anthropic LLM.行 29–95。
  - **函数 `async test_legacy_adapter_memory_operations`**：完成该模块中的具体处理。无显式位置参数。Test that LegacyVannaAdapter properly implements AgentMemory interface.行 100–163。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_llm_context_enhancer.py`

- **目录**：`tests`。**文件名**：`test_llm_context_enhancer.py`。**语言/类型**：Python。**行数**：397。**字节**：13546。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Unit tests for LlmContextEnhancer functionality.

These tests validate that the Agent properly calls the LlmContextEnhancer methods
to enhance system prompts and user messages.
- **结构摘要**：类 4 个，模块函数 5 个，方法 18 个。
  - **类 `MockAgentMemory`**（核心类型或辅助类，基类：AgentMemory，行 25–80）：Mock AgentMemory for testing.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 28–29 行。
    - `async save_tool_usage(question, tool_name, args, context, success, metadata)`：持久化保存。`question`（该函数的业务参数，详见源码类型标注）、`tool_name`（该函数的业务参数，详见源码类型标注）、`args`（已通过 Pydantic 校验的工具参数）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`success`（该函数的业务参数，详见源码类型标注）、`metadata`（扩展元数据字典）。位于第 31–34 行。
    - `async save_text_memory(content, context)`：持久化保存。`content`（文本内容）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。位于第 36–41 行。
    - `async search_similar_usage(question, context)`：检索相似项。`question`（该函数的业务参数，详见源码类型标注）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。位于第 43–52 行。
    - `async search_text_memories(query, context)`：检索相似项。`query`（检索查询文本）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Return mock search results based on stored memories.位于第 54–65 行。
    - `async get_recent_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。位于第 67–68 行。
    - `async get_recent_text_memories(context, limit)`：读取并返回。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`limit`（返回条数上限）。位于第 70–71 行。
    - `async delete_by_id(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。位于第 73–74 行。
    - `async delete_text_memory(context, memory_id)`：删除并清理。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`memory_id`（该函数的业务参数，详见源码类型标注）。位于第 76–77 行。
    - `async clear_memories(context, tool_name, before_date)`：清空。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`tool_name`（该函数的业务参数，详见源码类型标注）、`before_date`（该函数的业务参数，详见源码类型标注）。位于第 79–80 行。
  - **类 `MockLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 83–114）：Mock LLM service that records calls.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 86–88 行。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。Record the call and return a mock response.位于第 90–101 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。Mock streaming - just yield a single chunk.位于第 103–110 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。Mock validation - no errors.位于第 112–114 行。
  - **类 `SimpleUserResolver`**（核心类型或辅助类，基类：UserResolver，行 117–123）：Simple user resolver for tests.
    - `async resolve_user(request_context)`：解析并还原。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）。位于第 120–123 行。
  - **类 `TrackingEnhancer`**（可插拔扩展点实现，基类：LlmContextEnhancer，行 126–162）：Custom enhancer that tracks when methods are called.
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 129–131 行。
    - `async enhance_system_prompt(system_prompt, user_message, user)`：增强提示或消息。`system_prompt`（系统提示词）、`user_message`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）。Track call and add a marker to the system prompt.位于第 133–144 行。
    - `async enhance_user_messages(messages, user)`：增强提示或消息。`messages`（该函数的业务参数，详见源码类型标注）、`user`（当前用户对象，携带 id 与 group_memberships）。Track call and add a marker to messages.位于第 146–162 行。
  - **函数 `async test_custom_enhancer_system_prompt_is_called`**：完成该模块中的具体处理。无显式位置参数。Test that a custom LlmContextEnhancer.enhance_system_prompt is called by the Agent.行 166–210。
  - **函数 `async test_custom_enhancer_user_messages_is_called`**：完成该模块中的具体处理。无显式位置参数。Test that a custom LlmContextEnhancer.enhance_user_messages is called by the Agent.行 214–254。
  - **函数 `async test_default_enhancer_with_agent_memory`**：完成该模块中的具体处理。无显式位置参数。Test that DefaultLlmContextEnhancer properly enhances system prompt with memories.行 258–318。
  - **函数 `async test_default_enhancer_without_agent_memory`**：完成该模块中的具体处理。无显式位置参数。Test that DefaultLlmContextEnhancer works without agent memory (no enhancement).行 322–359。
  - **函数 `async test_no_enhancer_means_no_enhancement`**：完成该模块中的具体处理。无显式位置参数。Test that when no enhancer is provided, no enhancement occurs.行 363–396。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_memory_tools.py`

- **目录**：`tests`。**文件名**：`test_memory_tools.py`。**语言/类型**：Python。**行数**：296。**字节**：10359。
- **主要功能简介**：内置工具：run_sql、visualize_data、记忆工具、Python 执行、文件系统。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Tests for agent memory tools, including UI feature access control.
- **结构摘要**：类 1 个，模块函数 4 个，方法 5 个。
  - **类 `TestMemoryToolDetailedResults`**（pytest 测试类，基类：无，行 42–291）：Test memory tool detailed results feature.
    - `async test_admin_sees_detailed_results(search_tool, demo_agent_memory, admin_user)`：完成该模块中的具体处理。`search_tool`（该函数的业务参数，详见源码类型标注）、`demo_agent_memory`（该函数的业务参数，详见源码类型标注）、`admin_user`（该函数的业务参数，详见源码类型标注）。Test that admin users see detailed memory results in a collapsible card.位于第 46–97 行。
    - `async test_non_admin_sees_simple_status(search_tool, demo_agent_memory, regular_user)`：完成该模块中的具体处理。`search_tool`（该函数的业务参数，详见源码类型标注）、`demo_agent_memory`（该函数的业务参数，详见源码类型标注）、`regular_user`（该函数的业务参数，详见源码类型标注）。Test that non-admin users see simple status message.位于第 100–142 行。
    - `async test_detailed_results_include_all_memory_fields(search_tool, demo_agent_memory, admin_user)`：完成该模块中的具体处理。`search_tool`（该函数的业务参数，详见源码类型标注）、`demo_agent_memory`（该函数的业务参数，详见源码类型标注）、`admin_user`（该函数的业务参数，详见源码类型标注）。Test that detailed results include all relevant memory fields.位于第 145–187 行。
    - `async test_no_results_works_for_both_admin_and_user(search_tool, demo_agent_memory, admin_user, regular_user)`：完成该模块中的具体处理。`search_tool`（该函数的业务参数，详见源码类型标注）、`demo_agent_memory`（该函数的业务参数，详见源码类型标注）、`admin_user`（该函数的业务参数，详见源码类型标注）、`regular_user`（该函数的业务参数，详见源码类型标注）。Test that admin sees card with 0 results while regular user sees status bar.位于第 193–242 行。
    - `async test_llm_result_same_for_admin_and_user(search_tool, demo_agent_memory, admin_user, regular_user)`：完成该模块中的具体处理。`search_tool`（该函数的业务参数，详见源码类型标注）、`demo_agent_memory`（该函数的业务参数，详见源码类型标注）、`admin_user`（该函数的业务参数，详见源码类型标注）、`regular_user`（该函数的业务参数，详见源码类型标注）。Test that the LLM receives the same information regardless of UI feature access.位于第 245–291 行。
  - **函数 `demo_agent_memory`**：完成该模块中的具体处理。无显式位置参数。Create a demo agent memory instance.行 19–21。
  - **函数 `admin_user`**：完成该模块中的具体处理。无显式位置参数。Create an admin user.行 25–27。
  - **函数 `regular_user`**：完成该模块中的具体处理。无显式位置参数。Create a regular user.行 31–33。
  - **函数 `search_tool`**：检索相似项。无显式位置参数。Create a search tool instance.行 37–39。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_ollama_direct.py`

- **目录**：`tests`。**文件名**：`test_ollama_direct.py`。**语言/类型**：Python。**行数**：314。**字节**：9552。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Direct Ollama integration tests to diagnose and verify the integration.

These tests check each aspect of the Ollama integration separately.
- **结构摘要**：类 0 个，模块函数 8 个，方法 0 个。
  - **函数 `test_user`**：完成该模块中的具体处理。无显式位置参数。Test user for LLM requests.行 14–21。
  - **函数 `async test_ollama_import`**：完成该模块中的具体处理。无显式位置参数。Test that Ollama integration can be imported.行 26–34。
  - **函数 `async test_ollama_initialization`**：完成该模块中的具体处理。无显式位置参数。Test that Ollama service can be initialized.行 39–58。
  - **函数 `async test_ollama_basic_request`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test a basic request without tools.行 63–99。
  - **函数 `async test_ollama_pydantic_response`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test that the response is a valid Pydantic model.行 104–142。
  - **函数 `async test_ollama_streaming`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test streaming responses.行 147–189。
  - **函数 `async test_ollama_tool_calling_attempt`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test tool calling with Ollama (may not work with all models).行 194–257。
  - **函数 `async test_ollama_payload_building`**：完成该模块中的具体处理。`test_user`（该函数的业务参数，详见源码类型标注）。Test that the payload is built correctly.行 262–313。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_tool_permissions.py`

- **目录**：`tests`。**文件名**：`test_tool_permissions.py`。**语言/类型**：Python。**行数**：916。**字节**：29205。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Tests for tool access control and permissions.
- **结构摘要**：类 10 个，模块函数 23 个，方法 18 个。
  - **类 `SimpleToolArgs`**（Pydantic 数据模型，基类：BaseModel，行 25–28）：Simple args for testing.
  - **类 `MockTool`**（抽象或具体服务/工具实现，基类：Tool[SimpleToolArgs]，行 31–51）：Mock tool for testing.
    - `__init__(tool_name)`：提供对象协议或生命周期约定。`tool_name`（该函数的业务参数，详见源码类型标注）。位于第 34–35 行。
    - `name()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 38–39 行。
    - `description()`：完成该模块中的具体处理。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 42–43 行。
    - `get_args_schema()`：读取并返回。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 45–46 行。
    - `async execute(context, args)`：执行业务逻辑。`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））、`args`（已通过 Pydantic 校验的工具参数）。位于第 48–51 行。
  - **类 `CustomTransformRegistry`**（抽象或具体服务/工具实现，基类：ToolRegistry，行 458–475）：Custom registry that modifies arguments.
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Custom transform that appends user info to message.位于第 461–475 行。
  - **类 `RejectionRegistry`**（抽象或具体服务/工具实现，基类：ToolRegistry，行 511–527）：Custom registry that rejects certain arguments.
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Reject args containing 'forbidden' keyword.位于第 514–527 行。
  - **类 `RowLevelSecurityRegistry`**（抽象或具体服务/工具实现，基类：ToolRegistry，行 597–623）：Custom registry that applies row-level security transformations.
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。Apply RLS by modifying SQL queries based on user groups.位于第 600–623 行。
  - **类 `InstrumentedRegistry`**（抽象或具体服务/工具实现，基类：ToolRegistry，行 690–699）：职责见方法列表。
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 691–694 行。
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。位于第 696–699 行。
  - **类 `ParameterCheckRegistry`**（抽象或具体服务/工具实现，基类：ToolRegistry，行 734–747）：职责见方法列表。
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 735–740 行。
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。位于第 742–747 行。
  - **类 `InstrumentedAgentRegistry`**（抽象或具体服务/工具实现，基类：ToolRegistry，行 798–817）：职责见方法列表。
    - `__init__()`：提供对象协议或生命周期约定。仅依赖实例或类自身状态，不额外接收调用方参数。位于第 799–801 行。
    - `async transform_args(tool, args, user, context)`：按上下文转换。`tool`（工具实例）、`args`（已通过 Pydantic 校验的工具参数）、`user`（当前用户对象，携带 id 与 group_memberships）、`context`（工具执行上下文（含 User、conversation_id、request_id、AgentMemory））。位于第 803–817 行。
  - **类 `TestUserResolver`**（pytest 测试类，基类：UserResolver，行 820–827）：职责见方法列表。
    - `async resolve_user(request_context)`：解析并还原。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）。位于第 821–827 行。
  - **类 `MockLlmService`**（抽象或具体服务/工具实现，基类：LlmService，行 830–863）：职责见方法列表。
    - `async send_request(request)`：发送到下游。`request`（下游请求对象）。位于第 831–843 行。
    - `async stream_request(request)`：以流式方式产出。`request`（下游请求对象）。位于第 845–859 行。
    - `async validate_tools(tools)`：校验合法性。`tools`（该函数的业务参数，详见源码类型标注）。位于第 861–863 行。
  - **函数 `agent_memory`**：完成该模块中的具体处理。无显式位置参数。Agent memory for testing.行 55–57。
  - **函数 `admin_user`**：完成该模块中的具体处理。无显式位置参数。Admin user with admin group.行 61–68。
  - **函数 `regular_user`**：完成该模块中的具体处理。无显式位置参数。Regular user with user group.行 72–79。
  - **函数 `analyst_user`**：完成该模块中的具体处理。无显式位置参数。Analyst user with analyst group.行 83–90。
  - **函数 `guest_user`**：完成该模块中的具体处理。无显式位置参数。Guest user with no groups.行 94–98。
  - **函数 `async test_tool_access_empty_groups_allows_all`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that empty access_groups allows all users.行 102–131。
  - **函数 `async test_tool_access_granted_matching_group`**：完成该模块中的具体处理。`admin_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that user with matching group can access tool.行 135–163。
  - **函数 `async test_tool_access_denied_no_matching_group`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that user without matching group cannot access tool.行 167–198。
  - **函数 `async test_tool_access_multiple_allowed_groups`**：完成该模块中的具体处理。`analyst_user`（该函数的业务参数，详见源码类型标注）、`admin_user`（该函数的业务参数，详见源码类型标注）、`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test tool with multiple allowed groups.行 202–252。
  - **函数 `async test_tool_access_guest_user_denied`**：完成该模块中的具体处理。`guest_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that guest user with no groups cannot access restricted tools.行 256–282。
  - **函数 `async test_get_schemas_filters_by_user`**：完成该模块中的具体处理。`admin_user`（该函数的业务参数，详见源码类型标注）、`regular_user`（该函数的业务参数，详见源码类型标注）。Test that get_schemas only returns tools accessible to user.行 286–315。
  - **函数 `async test_tool_not_found`**：完成该模块中的具体处理。`agent_memory`（该函数的业务参数，详见源码类型标注）。Test execution of non-existent tool.行 319–343。
  - **函数 `async test_duplicate_tool_registration`**：完成该模块中的具体处理。无显式位置参数。Test that registering the same tool twice raises error.行 347–362。
  - **函数 `async test_tool_access_group_intersection`**：完成该模块中的具体处理。`admin_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that access is granted on ANY matching group (not all groups).行 366–394。
  - **函数 `async test_list_tools`**：完成该模块中的具体处理。无显式位置参数。Test listing all registered tools.行 398–416。
  - **函数 `async test_transform_args_default_no_transformation`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that default transform_args implementation returns args unchanged.行 425–455。
  - **函数 `async test_transform_args_custom_modification`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test custom transform_args that modifies arguments.行 479–508。
  - **函数 `async test_transform_args_rejection`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test transform_args returning ToolRejection.行 531–561。
  - **函数 `async test_transform_args_allows_approved_content`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that transform_args allows approved content.行 565–594。
  - **函数 `async test_transform_args_row_level_security`**：完成该模块中的具体处理。`admin_user`（该函数的业务参数，详见源码类型标注）、`analyst_user`（该函数的业务参数，详见源码类型标注）、`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test transform_args implementing row-level security.行 627–682。
  - **函数 `async test_transform_args_called_during_execution`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that transform_args is called during tool execution flow.行 686–726。
  - **函数 `async test_transform_args_receives_correct_parameters`**：完成该模块中的具体处理。`regular_user`（该函数的业务参数，详见源码类型标注）、`agent_memory`（该函数的业务参数，详见源码类型标注）。Test that transform_args receives correct parameters.行 730–782。
  - **函数 `async test_transform_args_called_during_agent_send_message`**：完成该模块中的具体处理。无显式位置参数。Test that transform_args is called during Agent.send_message workflow.行 786–915。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tests/test_workflow.py`

- **目录**：`tests`。**文件名**：`test_workflow.py`。**语言/类型**：Python。**行数**：875。**字节**：30729。
- **主要功能简介**：pytest 测试，覆盖权限、工作流、记忆、数据库健康检查与 LLM 集成。
- **阅读提示**：自动化规格。新增功能必须在此增加对等用例。
- **模块文档字符串**：Tests for the default workflow handler, including memory display and deletion.
- **结构摘要**：类 14 个，模块函数 6 个，方法 36 个。
  - **类 `SimpleUserResolver`**（核心类型或辅助类，基类：UserResolver，行 19–25）：Simple user resolver for tests.
    - `async resolve_user(request_context)`：解析并还原。`request_context`（来自 HTTP 的 RequestContext，含 cookies/headers/metadata）。位于第 22–25 行。
  - **类 `MockAgent`**（核心类型或辅助类，基类：无，行 28–33）：Mock agent for testing workflow handlers.
    - `__init__(agent_memory, tool_registry)`：提供对象协议或生命周期约定。`agent_memory`（该函数的业务参数，详见源码类型标注）、`tool_registry`（该函数的业务参数，详见源码类型标注）。位于第 31–33 行。
  - **类 `MockToolRegistry`**（核心类型或辅助类，基类：无，行 36–41）：Mock tool registry for testing.
    - `async get_schemas(user)`：读取并返回。`user`（当前用户对象，携带 id 与 group_memberships）。Return mock tool schemas.位于第 39–41 行。
  - **类 `MockConversation`**（核心类型或辅助类，基类：无，行 44–48）：Mock conversation for testing.
    - `__init__(conversation_id)`：提供对象协议或生命周期约定。`conversation_id`（会话标识，空则新建）。位于第 47–48 行。
  - **类 `TestWorkflowCommands`**（pytest 测试类，基类：无，行 88–154）：Test basic workflow command handling.
    - `async test_help_command(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that /help command returns help message (non-admin view).位于第 92–111 行。
    - `async test_status_command(workflow_handler, agent_with_memory, admin_test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`admin_test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that /status command returns status information (admin only).位于第 114–127 行。
    - `async test_memories_command(workflow_handler, agent_with_memory, admin_test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`admin_test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that /memories command returns memory list (admin only).位于第 130–143 行。
    - `async test_unknown_command(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that unknown commands are passed to LLM.位于第 146–154 行。
  - **类 `TestMemoriesView`**（pytest 测试类，基类：无，行 157–373）：Test the memories view functionality.
    - `async test_memories_no_agent_memory(workflow_handler, agent_without_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_without_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test memories view when agent has no memory capability.位于第 161–173 行。
    - `async test_memories_empty(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test memories view when no memories exist.位于第 176–188 行。
    - `async test_memories_with_tool_memories(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test memories view displays tool memories correctly.位于第 191–241 行。
    - `async test_memories_with_text_memories(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test memories view displays text memories correctly.位于第 244–283 行。
    - `async test_memories_with_both_types(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test memories view displays both tool and text memories.位于第 286–333 行。
    - `async test_memories_have_delete_buttons(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that memory cards include delete buttons.位于第 336–373 行。
  - **类 `TestMemoryDeletion`**（pytest 测试类，基类：无，行 376–540）：Test memory deletion functionality.
    - `async test_delete_no_agent_memory(workflow_handler, agent_without_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_without_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test delete command when agent has no memory capability.位于第 380–390 行。
    - `async test_delete_no_id_provided(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test delete command without memory ID.位于第 393–404 行。
    - `async test_delete_nonexistent_memory(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test deleting a memory that doesn't exist.位于第 407–417 行。
    - `async test_delete_tool_memory_success(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test successfully deleting a tool memory.位于第 420–461 行。
    - `async test_delete_text_memory_success(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test successfully deleting a text memory.位于第 464–498 行。
    - `async test_delete_command_parsing(workflow_handler, agent_with_memory, admin_test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`admin_test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that /delete command is properly parsed (admin only).位于第 501–540 行。
  - **类 `TestWorkflowComponentStructure`**（pytest 测试类，基类：无，行 543–599）：Test the structure of components returned by workflow.
    - `async test_help_has_rich_component(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that help command returns properly structured components.位于第 547–558 行。
    - `async test_memories_cards_have_proper_structure(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that memory cards have proper structure.位于第 561–599 行。
  - **类 `TestStarterUI`**（pytest 测试类，基类：无，行 602–752）：Test the starter UI functionality.
    - `async test_starter_ui_single_component(workflow_handler, agent_with_memory, test_user, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_user`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that starter UI returns a single component.位于第 606–628 行。
    - `async test_starter_ui_user_view(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that non-admin users get a simple welcome message via RichTextComponent.位于第 631–661 行。
    - `async test_starter_ui_admin_view(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that admin users get setup status and memory management info.位于第 664–712 行。
    - `async test_starter_ui_admin_without_memory(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that admin users see setup status even without memory tools.位于第 715–752 行。
  - **类 `TestAdminOnlyCommands`**（pytest 测试类，基类：无，行 755–870）：Test that admin-only commands are properly restricted.
    - `async test_non_admin_cannot_access_status(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that non-admin users cannot access /status command.位于第 759–774 行。
    - `async test_non_admin_cannot_access_memories(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that non-admin users cannot access /memories command.位于第 777–792 行。
    - `async test_non_admin_cannot_delete_memories(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that non-admin users cannot use /delete command.位于第 795–810 行。
    - `async test_admin_can_access_memories(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that admin users can access /memories command.位于第 813–829 行。
    - `async test_help_shows_admin_commands_for_admin(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that /help shows admin commands for admin users.位于第 832–850 行。
    - `async test_help_hides_admin_commands_for_non_admin(workflow_handler, agent_with_memory, test_conversation)`：完成该模块中的具体处理。`workflow_handler`（该函数的业务参数，详见源码类型标注）、`agent_with_memory`（该函数的业务参数，详见源码类型标注）、`test_conversation`（该函数的业务参数，详见源码类型标注）。Test that /help hides admin commands for non-admin users.位于第 853–870 行。
  - **类 `MockToolRegistryWithSQL`**（核心类型或辅助类，基类：无，行 612–618）：职责见方法列表。
    - `async get_schemas(user)`：读取并返回。`user`（当前用户对象，携带 id 与 group_memberships）。位于第 613–618 行。
  - **类 `MockToolRegistryWithSQL`**（核心类型或辅助类，基类：无，行 641–647）：职责见方法列表。
    - `async get_schemas(user)`：读取并返回。`user`（当前用户对象，携带 id 与 group_memberships）。位于第 642–647 行。
  - **类 `MockToolRegistryComplete`**（核心类型或辅助类，基类：无，行 674–690）：职责见方法列表。
    - `async get_schemas(user)`：读取并返回。`user`（当前用户对象，携带 id 与 group_memberships）。位于第 675–690 行。
  - **类 `MockToolRegistrySQL`**（核心类型或辅助类，基类：无，行 725–731）：职责见方法列表。
    - `async get_schemas(user)`：读取并返回。`user`（当前用户对象，携带 id 与 group_memberships）。位于第 726–731 行。
  - **函数 `test_user`**：完成该模块中的具体处理。无显式位置参数。Create a test user.行 52–54。
  - **函数 `admin_test_user`**：完成该模块中的具体处理。无显式位置参数。Create an admin test user for tests that require admin access.行 58–60。
  - **函数 `test_conversation`**：完成该模块中的具体处理。无显式位置参数。Create a test conversation.行 64–66。
  - **函数 `workflow_handler`**：完成该模块中的具体处理。无显式位置参数。Create a workflow handler instance.行 70–72。
  - **函数 `agent_with_memory`**：完成该模块中的具体处理。无显式位置参数。Create a mock agent with memory.行 76–79。
  - **函数 `agent_without_memory`**：完成该模块中的具体处理。无显式位置参数。Create a mock agent without memory.行 83–85。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

### `tox.ini`

- **目录**：`.`。**文件名**：`tox.ini`。**语言/类型**：.ini。**行数**：243。**字节**：7537。
- **主要功能简介**：项目支撑文件，用于构建、文档、示例或配置。
- **阅读提示**：定义 unit、各 LLM、各数据库 sanity 与质量检查环境。
  维护建议：修改本文件时保持公开符号稳定；若必须改名，应在 `__init__.py` 保留别名并在迁移指南记录。涉及用户数据或 SQL 的改动必须补充权限与失败路径测试。不要在此文件引入新的必选重依赖。

## 3. 关键调用链与阅读顺序

推荐阅读顺序：`src/vanna/__init__.py` 看导出 → `core/agent/agent.py` 看循环 → `core/registry.py` 看权限 →`core/user/models.py` 看 User → `tools/run_sql.py` 看问数 → `servers/base/chat_handler.py` 看 SSE →`frontends/webcomponent/src/components/vanna-chat.ts` 看 UI → `tests/test_tool_permissions.py` 看规格。然后再按需进入 integrations 与 legacy。不要从 legacy/base.py 开始，以免被 0.x 心智模型带偏。

## 4. 测试与前端源码补充说明

tests/conftest.py 提供跨文件夹具。test_agents.py 是真实模型黄金路径。test_database_sanity.py 与 test_agent_memory_sanity.py 保证可选集成至少可构造。前端 stories 文件不是生产运行时，但是组件视觉契约的一部分，改样式应同时改 stories。

## 5. 统计复核

扫描文件 371，总行 87211，Python 301/43417，类 384，函数 219，方法 1293。若你本地统计略有出入，通常是因为未跟踪文件、生成物或不同换行符。以 pyproject 版本发布的 sdist 会排除 frontends/tests/notebooks/.github（见 tool.flit.sdist.exclude），因此「安装到 site-packages 的源码」比「本仓库源码」更小。二次开发应以仓库为准。
