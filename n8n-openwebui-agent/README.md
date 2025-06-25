# n8n 与 OpenWebUI Agent 集成

本仓库包含一个 n8n 工作流模板，能够将 n8n Agent 与 [Open WebUI](https://openwebui.com) 集成，使你可以通过 Open WebUI 界面直接与 n8n 驱动的 AI Agent 进行对话。

如果你自托管了 n8n 并通过 Ollama 等方式运行本地 LLM，这个方案可以让你完全离线运行由 n8n 驱动的 Agent！

> 本模板需配合 [Open WebUI 函数——n8n_pipe](https://openwebui.com/f/coleam/n8n_pipe) 一同使用，以完成与 n8n 的对接。

## 快速开始

以下步骤假设你已安装并运行好 n8n。

1. 安装 Open WebUI——具体请参见 [Open WebUI GitHub 仓库](https://github.com/open-webui/open-webui) 中的说明。  
2. 打开 [n8n_pipe 函数页面](https://openwebui.com/f/coleam/n8n_pipe)，点击“Get”，输入你本地的 Open WebUI 地址（例如 http://localhost:3000）。  
3. 在你的 n8n 实例中导入 `Open_WebUI_Agent_Template.json`，并配置：  
   - Webhook 端点与安全校验（Header 验证）  
   - OpenAI 或其他 LLM 提供商的 API 凭据  
   - PostgreSQL 数据库连接（用于会话记忆）  
   - 根据需求自定义 Agent 流程与工具  
4. 在 Open WebUI 界面左下角「Profile」→「Admin Panel」→「Functions」中，找到 n8n_pipe 函数，点击设置图标，填写：  
   - n8n Webhook URL  
   - Bearer Token  
   - 输入字段 Key  
   - 输出字段 Key  

   以上参数就是函数与特定 n8n Agent 链接的“阀门”。

## 概览

`Open_WebUI_Agent_Template.json` 工作流提供了一个即用型模板，使你能将 n8n Agent 通过 Open WebUI 的函数调用系统暴露为接口服务。此集成具备以下特点：

- 通过 Webhook 暴露 n8n AI Agent 服务  
- 使用 PostgreSQL 存储会话记忆，保持上下文连续  
- 调用 OpenAI（或其他模型）生成回复  
- 集成网页搜索等工具能力  
- 与 Open WebUI 聊天界面无缝对接  

## 工作原理

1. Webhook 端点：`invoke-n8n-agent` 接收来自 Open WebUI 的请求。  
2. 身份验证：通过 Header 验证确保仅授权请求可被处理。  
3. 会话记忆：基于 Open WebUI 提供的 session ID，利用 PostgreSQL Chat Memory 保存聊天上下文。  
4. AI 处理：调用 OpenAI 等语言模型处置用户输入并生成回复。  
5. 工具集成：模板内置网页搜索等工具，Agent 可视需调用检索最新信息。  
6. 响应返回：将 Agent 生成的回复格式化后，发送回 Open WebUI 界面展示给用户。

## 安装指南

### 前提条件

- n8n 实例（自托管或云端）  
- OpenAI API 凭据  
- PostgreSQL 数据库（用于会话记忆）  
- 已安装并运行的 Open WebUI

### 安装步骤

1. 在 n8n 中导入模板：  
   - 进入「Workflows」  
   - 点击「Import from File」，选择下载好的 `Open_WebUI_Agent_Template.json`  
2. 配置凭据：  
   - 添加 OpenAI 或其他 LLM 的 API 凭据  
   - 配置 PostgreSQL 数据库连接  
   - 设置 Webhook 的 Header 身份验证  
3. 激活工作流：  
   - 启用该工作流，使 Webhook 端点生效  
4. 连接到 Open WebUI：  
   - 打开 [n8n_pipe 函数页面](https://openwebui.com/f/coleam/n8n_pipe)  
   - 点击“Get”，输入 Open WebUI 实例地址  
   - 编辑“阀门”设置，将 n8n Webhook URL、Bearer Token、输入/输出字段 Key 填入

## 自定义

你可以根据需要对该模板进行任意自定义，将其打造成功能丰富的 AI Agent。可以替换或添加不同的 LLM、使用其他数据库存储会话历史、集成更多工具，乃至构建完全个性化的 Agent 流程！
