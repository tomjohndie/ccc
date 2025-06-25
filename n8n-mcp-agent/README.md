# n8n MCP 代理演示

此仓库包含一个 n8n 工作流，演示如何构建一个使用 Model Context Protocol（MCP）来访问并执行外部工具的 AI 代理。  
本示例使用了 n8n 社区版 MCP 节点（https://github.com/nerding-io/n8n-nodes-mcp）。请先下载安装该节点，详情请参考项目说明。

## 概览

n8n MCP 代理演示工作流展示了：

1. 如何在 n8n 中创建一个可动态发现并调用工具的 AI 代理  
2. 与多个 MCP 客户端（Brave Search 与 Convex）集成  
3. 使用 PostgreSQL 实现持久化聊天记忆  

## MCP 工具集成

工作流包含两组 MCP 客户端工具：

1. Brave Search 工具  
   - 列出可用 Brave Search 工具  
   - 使用指定参数执行某个 Brave Search 工具  

2. Convex 工具  
   - 列出可用 Convex 工具  
   - 使用指定参数执行某个 Convex 工具  

## 工作原理

1. 通过聊天触发器接收用户消息  
2. AI 代理使用 OpenAI 模型处理该消息  
3. 代理可以：  
   - 列出 Brave Search 或 Convex 的可用工具  
   - 以 JSON 参数格式调用具体工具执行操作  
   - 通过 PostgreSQL 保存并维护对话上下文  

工作流的 System Message（系统消息）会指导 AI 遵循以下流程：  
- 在调用工具前，先列出相关能力的可用工具  
- 解析工具名称、描述、参数模式等信息  
- 按照指定格式封装参数并执行工具  
- 对于无参数工具，传入空对象即可  
- 对于文件系统，仅能访问 `/files` 目录  

### 系统消息示例

```
You are a helpful assistant who has access to a bunch of tools to assist with user queries. Before you try to execute any tool, you need to call the tool to list available tools for the capability you want to leverage.

When you list tools available, you'll get a list back of items that look like:

name:[tool_name]
description:[tool description to tell you when and how to use the tool]
schema
0:[param 1]
1:[param 2]
...
n-1:[param n]

Then when you call a tool, you need to give the tool name exactly as given to you, and the tool parameters need to be a json object like:

{
  "param 1": "param 1 value",
  ...
  "param n": "param n value"
}

If there are no parameters for the tool, just pass in an empty object.

For the file system, you have access to the /files directory and that is it.
```

## 环境准备

- 已部署的 n8n 实例  
- n8n MCP 节点  
- PostgreSQL 数据库（用于持久化聊天记忆）  
- API 凭据：  
  - OpenAI API Key  
  - Brave Search API 凭据  
  - Convex API 凭据  

## 安装与配置

1. 在 n8n 中导入 `MCP_Agent_Demo.json` 工作流  
2. 安装 [n8n 社区版 MCP 节点](https://github.com/nerding-io/n8n-nodes-mcp)  
3. 在 n8n 设置中配置以下凭据：  
   - OpenAI API  
   - PostgreSQL 数据库连接  
   - Brave Search MCP 客户端  
   - Convex MCP 客户端  

## 使用方法

1. 激活工作流  
2. 与代理开始对话  
3. 提出需要调用 Brave Search 或 Convex 工具的问题  
4. 代理会自动发现并调用合适工具，给出回答  

## 扩展工作流

你可以通过以下方式扩展此工作流：  
- 添加更多 MCP 客户端，为代理提供更多工具能力  
- 自定义系统消息，调整代理行为  
- 在调用前或调用后添加处理节点，丰富代理功能  

## 相关链接

- Model Context Protocol (MCP)：https://github.com/modelcontextprotocol/mcp  
- n8n 官方文档：https://docs.n8n.io/  
- n8n MCP 节点：https://github.com/nerding-io/n8n-nodes-mcp
