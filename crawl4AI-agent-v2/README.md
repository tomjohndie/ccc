# Pydantic AI 文档爬取器 & RAG 代理

一个由 Crawl4AI 和 Pydantic AI 驱动的智能文档爬取与检索增强生成（RAG）系统。该项目让你能够从任意网站、`.txt`/Markdown 页面（如 `llms.txt`）或站点地图中爬取、分块并向量化文档，并通过 Streamlit 界面对知识库进行交互。

---

## 功能

- **灵活的文档爬取**：支持普通网站、`.txt`/Markdown 页面（如 `llms.txt`）以及站点地图。
- **并行与递归爬取**：采用内存自适应批处理，高效抓取大型文档站点。
- **智能分块**：按 Markdown 标题层级分块，确保分块结果最适合向量检索。
- **向量数据库集成**：使用 ChromaDB 存储分块及其元数据，实现快速语义检索。
- **Streamlit RAG 界面**：通过 LLM 驱动的语义搜索查询文档。
- **可扩展示例**：一系列模块化脚本，展示不同的爬取和 RAG 工作流。

---

## 前置要求

- Python 3.11 及以上
- OpenAI API Key（用于生成嵌入和 LLM 驱动的搜索）
- 安装 `requirements.txt` 中的 Crawl4AI/Playwright 等依赖
- （可选）Streamlit，用于 Web 界面

---

## 安装

1. 克隆仓库：
   ```
   git clone https://github.com/coleam00/ottomator-agents.git
   cd ottomator-agents/crawl4AI-agent-v2
   ```

2. 安装依赖：
   ```
   python -m venv venv
   source venv/bin/activate    # Windows 上使用：venv\Scripts\activate
   pip install -r requirements.txt
   playwright install
   ```

3. 配置环境变量：
   - 复制 `.env.example` 为 `.env`
   - 编辑 `.env`，填写你的 API Key 和偏好设置：
     ```env
     OPENAI_API_KEY=your_openai_api_key
     MODEL_CHOICE=gpt-4.1-mini  # 或你首选的 OpenAI 模型
     ```

---

## 用法

### 1. 爬取并插入文档

主脚本 [`insert_docs.py`](insert_docs.py) 用于爬取并将文档向量化后存入数据库。

#### 支持的 URL 类型

- **普通文档网站**：递归爬取所有内部链接，并按 URL（忽略锚点）去重。
- **Markdown 或 .txt 页面（如 llms.txt）**：抓取并分块 Markdown 内容。
- **站点地图（`sitemap.xml`）**：批量爬取地图中列出的所有 URL。

#### 示例

```bash
python insert_docs.py <URL> [--collection mydocs] [--db-dir ./chroma_db] \
                     [--embedding-model all-MiniLM-L6-v2] [--chunk-size 1000] \
                     [--max-depth 3] [--max-concurrent 10] [--batch-size 100]
```

参数说明：  
- `URL`：要爬取的根地址、`.txt` 文件或站点地图。  
- `--collection`：ChromaDB 集合名称（默认：`docs`）。  
- `--db-dir`：ChromaDB 数据存放目录（默认：`./chroma_db`）。  
- `--embedding-model`：嵌入模型（默认：`all-MiniLM-L6-v2`）。  
- `--chunk-size`：每个分块的最大字符数（默认：1000）。  
- `--max-depth`：爬取深度（默认：3）。  
- `--max-concurrent`：并发浏览器会话数（默认：10）。  
- `--batch-size`：向 ChromaDB 批量插入时的批量大小（默认：100）。

三种类型示例：  
```bash
python insert_docs.py https://ai.pydantic.dev/
python insert_docs.py https://ai.pydantic.dev/llms-full.txt
python insert_docs.py https://ai.pydantic.dev/sitemap.xml
```

#### 分块策略

- 按 `#`、`##`、`###` 依次分层分块。
- 若仍超出限制，则按字符数强制拆分。
- 最终所有分块均小于 `--chunk-size` 指定的字符数。

#### 元数据

每个分块会存储以下元数据：  
- 来源 URL  
- 分块索引  
- 提取的标题层级  
- 字符数和单词数统计  

---

### 2. 示例脚本

`crawl4AI-examples/` 目录下包含演示不同爬取和分块策略的模块化脚本：

- `3-crawl_sitemap_in_parallel.py`：并行批量爬取站点地图中的链接，并跟踪内存使用。  
- `4-crawl_llms_txt.py`：爬取 Markdown 或 `.txt` 文件，按标题分块并打印结果。  
- `5-crawl_site_recursively.py`：递归爬取站点内部链接，并按 URL 去重。

这些脚本可直接用于实验或作为自定义爬虫的模板。

---

### 3. 运行 Streamlit RAG 界面

完成文档爬取与插入后，启动 Streamlit 应用以进行语义搜索和问答：

```bash
streamlit run streamlit_app.py
```

- 应用地址： http://localhost:8501  
- 在界面中输入自然语言查询，你将获得基于上下文的丰富答案。

---

## 项目结构

```
crawl4AI-agent-v2/
├── crawl4AI-examples/
│   ├── 3-crawl_docs_FAST.py
│   ├── 4-crawl_and_chunk_markdown.py
│   └── 5-crawl_recursive_internal_links.py
├── insert_docs.py
├── rag_agent.py
├── streamlit_app.py
├── utils.py
├── requirements.txt
├── .env.example
└── README.md
```

---

## 高级用法与定制

- **分块大小**：调整 `--chunk-size` 以满足检索需求。  
- **嵌入模型**：使用 `--embedding-model` 切换不同模型。  
- **爬取配置**：修改 `--max-depth` 与 `--max-concurrent` 应对大规模站点。  
- **向量库**：指定不同的 ChromaDB 目录或集合，实现多项目管理。

---

## 常见问题排查

- 确保依赖安装完整，环境变量已正确配置。  
- 对于大型站点，可增加机器内存或减少 `--max-concurrent`。  
- 若遇到爬取问题，可先运行示例脚本进行分步调试。
