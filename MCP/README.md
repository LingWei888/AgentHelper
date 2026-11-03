# MCP (Model Context Protocol) Servers Repository

欢迎来到 MCP 专属开发文件夹！本目录专门用于存放、开发和管理所有基于 **Model Context Protocol** 的服务端程序（MCP Servers）以及相关的工具（Tools）、资源（Resources）和提示词（Prompts）。

## 1. MCP 的本质含义

**MCP (Model Context Protocol)** 是一个由 Anthropic 牵头开源的标准化协议。

* **本质属性**：它是一个开放的标准架构，类似于 AI 时代的“USB 接口”。它定义了 AI 大模型（Client）如何与外部系统（Server）进行安全、标准的双向通信。
* **核心作用**：让 AI 助手（如 Claude Desktop、Cursor 等）能够安全地访问您的本地文件、公司内部数据库、甚至调用外部 API，从而极大地扩展大模型的能力边界。
* **三大核心能力**：
  1. **Resources（资源）**：为 AI 提供上下文数据（如读取本地文件、数据库记录）。
  2. **Tools（工具）**：允许 AI 执行特定动作（如发送邮件、执行脚本、查询 API）。
  3. **Prompts（提示词）**：提供预定义的提示词模板，帮助用户更好地向 AI 提问。

## 2. 目录结构说明

为了保证 MCP 服务的模块化和易扩展性，本文件夹推荐采用以下目录结构（以 Node.js / Python 常见结构为例）：

```
📦 MCP-Workspace
 ┣ 📂 src/              # 核心业务逻辑和 Server 启动代码
 ┃ ┣ 📂 tools/          # 具体工具的实现逻辑 (例如: search_db, fetch_api)
 ┃ ┣ 📂 resources/      # 资源提供者的实现逻辑
 ┃ ┣ 📂 prompts/        # 提示词模板的定义
 ┃ ┗ 📜 index.js        # (或 main.py) MCP Server 的主入口文件
 ┣ 📂 config/           # 配置文件 (如数据库连接、API 密钥等，注意不要提交敏感信息)
 ┣ 📂 tests/            # 测试脚本，用于通过 MCP Inspector 或本地脚本测试 Tools
 ┣ 📂 docs/             # 存放开发文档、Schema 定义或接口说明
 ┣ 📜 package.json      # (Node.js) 项目依赖和运行脚本 / (Python 则为 pyproject.toml)
 ┣ 📜 .env.example      # 环境变量示例文件
 ┗ 📜 README.md         # 本说明文档
```

## 3. 基本用法 (Usage)

开发并运行您自己的 MCP Server 通常分为以下几步：

### 3.1 安装依赖并启动 (以 Node.js SDK 为例)

首先，进入您的项目目录并安装相关 SDK：

```bash
# 安装依赖
npm install @modelcontextprotocol/sdk

# 启动 Server (默认通过标准输入输出 stdio 通信)
node src/index.js
```

### 3.2 接入 AI 客户端 (例如 Claude Desktop)

要让 AI 能够使用您的 MCP Server，需要在客户端配置文件中进行挂载。
以 **Claude Desktop** 为例，修改配置如下（通常位于 `~/.claude/claude_desktop_config.json` 或 `%APPDATA%\Claude\claude_desktop_config.json`）：

```json
{
  "mcpServers": {
    "my-custom-mcp": {
      "command": "node",
      "args": [
        "/绝对路径/到您的/MCP-Workspace/src/index.js"
      ],
      "env": {
        "MY_API_KEY": "your_secret_key"
      }
    }
  }
}
```
配置完成后，重启 Claude Desktop，您就可以在聊天界面看到并使用您开发的工具了！

### 3.3 调试工具 (MCP Inspector)

在开发阶段，您可以使用官方提供的 MCP Inspector 工具进行独立调试，无需每次都重启 AI 客户端：

```bash
npx @modelcontextprotocol/inspector node src/index.js
```

## 4. 开发规范与注意事项

为了写出稳定、高效的 MCP Server，请遵循以下原则：

1. **输入验证**：AI 传入的参数可能存在幻觉。必须在 Tool 的执行逻辑中使用严格的 schema 校验（如 Zod 或 Pydantic）验证所有的输入参数。
2. **安全性 (Security)**：MCP Server 往往运行在您的本地计算机或内网服务器上。确保您的 Tool 不会执行高危的系统命令（例如防御命令注入），并在必要时实施权限控制（Human-in-the-loop）。
3. **错误处理**：向 AI 客户端返回清晰、带有明确解释的错误信息，这样大模型能够根据报错自行修正重试，而不是直接崩溃。
4. **无状态设计 (Stateless)**：尽量保证每一次 Tool 调用是独立的。如果必须要有状态，请使用数据库或本地文件系统进行持久化。

## 5. 参考资料

* [Model Context Protocol 官方文档](https://modelcontextprotocol.io/)
* [MCP 官方 GitHub 仓库](https://github.com/modelcontextprotocol)
* [MCP Inspector 调试工具指南](https://github.com/modelcontextprotocol/inspector)

*文档更新时间：2026年*