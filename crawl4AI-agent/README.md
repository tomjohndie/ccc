# Pydantic AI：文档爬取器与 RAG 代理

一个基于 Pydantic AI 和 Supabase 构建的智能文档爬取与 RAG（检索增强生成）代理。该代理能够爬取文档网站，将内容存储到向量数据库，并通过检索与分析相关的文档片段，为用户问题提供智能答案。

## 功能

- 文档网站爬取与分块  
- 使用 Supabase 存储向量数据库  
- 基于 OpenAI 嵌入的语义搜索  
- RAG 驱动的问答  
- 保留代码块格式  
- Streamlit 界面进行交互式查询  
- 提供 API 端点和 Web 界面两种使用方式  

## 前置条件

- Python 3.11 及以上  
- Supabase 账号及数据库  
- OpenAI API Key  
- （可选）Streamlit 用于 Web 界面  

## 安装

1. 克隆仓库：  
   ```bash
   git clone https://github.com/coleam00/ottomator-agents.git
   cd ottomator-agents/crawl4AI-agent
   ```

2. 安装依赖（建议使用虚拟环境）：  
   ```bash
   python -m venv venv
   source venv/bin/activate    # Windows 上：venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. 配置环境变量：  
   - 将 `.env.example` 重命名为 `.env`  
   - 编辑 `.env`，填写你的 API Key 和参数：  
     ```env
     OPENAI_API_KEY=your_openai_api_key
     SUPABASE_URL=your_supabase_url
     SUPABASE_SERVICE_KEY=your_supabase_service_key
     LLM_MODEL=gpt-4o-mini  # 或你偏好的 OpenAI 模型
     ```

## 使用方法

### 数据库初始化

在 Supabase 的 SQL 编辑器中，执行 `site_pages.sql` 中的命令来：  
1. 创建所需数据表  
2. 启用向量相似度搜索  
3. 设置行级安全（RLS）策略  

### 爬取文档

运行以下命令，将文档爬取并存入向量数据库：  
```bash
python crawl_pydantic_ai_docs.py
```
该脚本会：  
1. 从站点地图中获取 URL  
2. 爬取每个页面并分块  
3. 生成嵌入向量并存入 Supabase  

### Streamlit Web 界面

启动交互式 Web 界面：  
```bash
streamlit run streamlit_ui.py
```
访问地址： http://localhost:8501

## 配置

### 数据库模式

Supabase 中的表结构如下：  
```sql
CREATE TABLE site_pages (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    url TEXT,
    chunk_number INTEGER,
    title TEXT,
    summary TEXT,
    content TEXT,
    metadata JSONB,
    embedding VECTOR(1536)
);
```

### 分块配置

可在 `crawl_pydantic_ai_docs.py` 中调整分块参数：  
```python
chunk_size = 5000  # 每块字符数上限
```
分块器会智能保留：  
- 代码块  
- 段落边界  
- 句子边界  

## 项目结构

- `crawl_pydantic_ai_docs.py`：文档爬取与处理脚本  
- `pydantic_ai_expert.py`：RAG 代理实现  
- `streamlit_ui.py`：Web 界面  
- `site_pages.sql`：数据库初始化 SQL  
- `requirements.txt`：依赖清单  

## Live Agent Studio 版本

如需查看该代理在 Live Agent Studio 中的实现，可参考 `studio-integration-api` 目录，其中包含生产环境 API 端点示例。

## 错误处理

系统已内置健壮的错误处理机制，覆盖：  
- 爬取过程中的网络故障  
- API 调用限流  
- 数据库连接异常  
- 嵌入生成失败  
- 无效 URL 或内容格式问题
