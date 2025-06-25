# NBA 预测机器人

作者：[Elo Mukoro](https://nbaagent-production.up.railway.app/)

一个由 AI 驱动的 NBA 比赛预测与投注洞察机器人，提供比赛预测、投注赔率和赛前分析。旨在帮助用户做出数据驱动的决策，评估即将到来的对阵、可能的结果和相关统计数据，为体育博彩爱好者提供有价值的洞察。

## 功能
- 实时 NBA 比赛预测  
- 投注赔率与分析  
- 针对具体球队的深度洞察  
- 历史比赛数据分析  
- 交互式网页界面  

## 技术栈
- 后端：FastAPI、Python  
- 前端：HTML、CSS、JavaScript  
- 数据库：Supabase  
- 接口：BallDontLie API、OpenAI API  

## 快速启动
1. 克隆代码仓库  
2. 安装依赖：  
   ```bash
   pip install -r requirements.txt
   ```  
3. 在项目根目录创建并编辑 `.env` 文件，添加以下环境变量：  
   - BALLDONTLIE_API_KEY  
   - OPENAI_API_KEY  
   - API_BEARER_TOKEN  
   - SUPABASE_URL  
   - SUPABASE_SERVICE_KEY  

4. 启动应用：  
   ```bash
   uvicorn nba_agent:app --reload
   ```

## 使用
在浏览器中访问 `http://localhost:8001`，即可与机器人交互。

## API 端点
- POST `/api/nba_agent`：主要的预测接口

## 贡献

此机器人属于 oTTomator Agents 集合。如有贡献或问题，请参阅主仓库指南。
