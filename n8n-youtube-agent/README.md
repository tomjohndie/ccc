# n8n YouTube 助手

作者：[Dominik Fretz](https://www.linkedin.com/in/dominikfretz/)

**平台：** n8n（你可以将 `.json` 文件导入到你自己的 n8n 实例中以查看工作流）

## Agent 功能

此 Agent 可根据 YouTube 视频链接获取视频的字幕，并将字幕内容添加到向量数据库中，生成摘要、关键要点、引用等信息，并将其存储到 Supabase 数据库中。随后，你可以就这些视频进行对话：请求视频摘要、对特定视频进行后续提问，或搜索已添加的视频——例如，想起某个视频中提到了一本书、某个工具或一个人名时，只需向 Agent 提问即可。

### 功能亮点

- 将 YouTube 视频字幕添加到 Supabase 向量存储  
- 获取视频摘要  
- 对视频内容进行后续提问  
- 搜索已添加的视频  

### 依赖项

- Claude Haiku（Anthropic）  
- OpenAI Embedding 模型  
- Supabase  
- Supadata  

### 配置要求

- YouTube Data API 凭据（用于 “Fetch video details” 节点）  
- Supadata.ai 凭据（用于获取视频字幕）  
- OpenAI API 凭据（用于生成 Embeddings）  
- Anthropic Claude API 凭据  
- 在 Supabase 中创建 `videos` 表（参见 `supabase.table.sql`）

## 贡献

此 Agent 属于 oTTomator Agents 集合。如有贡献建议或问题，请参阅主仓库的指南。
