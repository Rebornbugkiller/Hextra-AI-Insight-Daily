# Chark的AI资讯日报

一个基于 Cloudflare Workers 和智谱 GLM-4-plus 大模型构建的 AI 资讯聚合与智能分析平台。通过多数据源聚合、AI 智能摘要、内容自动生成等功能，为用户提供每日精选的 AI 领域资讯。

## 项目概述

本项目是一个 Serverless 架构的 AI 应用后端系统，主要解决信息过载问题。系统自动从多个数据源（Folo、GitHub Trending、HuggingFace Papers、Reddit、Twitter 等）抓取 AI 相关资讯，利用大语言模型进行智能摘要和深度分析，最终生成结构化的日报内容。

## 技术架构

### 核心技术栈

- **运行环境**: Cloudflare Workers (Serverless)
- **存储**: Cloudflare KV (分布式键值存储)
- **AI 模型**: 智谱 GLM-4-plus (OpenAI 兼容 API)
- **数据源**: Folo API、GitHub API、Twitter API、Reddit API 等
- **部署工具**: Wrangler CLI
- **开发语言**: JavaScript (ES6+)

### 系统架构

```
数据源层 → 数据抓取层 → 数据清洗层 → AI 处理层 → 内容生成层 → 存储层
   ↓           ↓            ↓            ↓            ↓           ↓
Folo API   数据抓取     内容清洗    GLM-4-plus   日报生成    Cloudflare KV
GitHub     并发处理     格式统一    智能摘要      播客脚本    GitHub API
Twitter    错误重试     去重过滤    深度分析      RSS Feed
```

## 核心功能

### 1. 多数据源聚合

- 支持 Folo、GitHub Trending、HuggingFace Papers、Reddit、Twitter 等多个数据源
- 每个数据源采用模块化设计，可独立配置和扩展
- 支持并发抓取，提升数据获取效率
- 实现统一的数据转换接口，将异构数据源标准化

### 2. 大模型 API 集成

- 集成智谱 GLM-4-plus 模型，支持 OpenAI 兼容 API 调用
- 实现流式响应处理，优化用户体验
- 支持自定义 Prompt 工程，针对不同场景优化提示词
- 实现 Token 优化和成本控制机制
- 通过 KV 缓存降低 API 调用频率，节省成本

### 3. 智能内容处理

- **内容摘要**: 使用大模型自动生成高质量摘要，提取关键信息
- **深度分析**: 对热点话题进行多角度分析和观点提炼
- **播客脚本生成**: 将文本内容转换为口语化播客脚本
- **多语言支持**: 支持中英文内容处理和翻译

### 4. 数据持久化与缓存

- 使用 Cloudflare KV 存储抓取的数据源内容
- 支持历史数据查询和回溯
- 实现智能缓存策略，减少重复 API 调用
- 通过 TTL 机制保证数据一致性

### 5. 自动化工作流

- 支持定时触发数据抓取和内容生成
- 自动推送到 GitHub 仓库
- 提供 RSS Feed 订阅功能
- 支持手动触发和批量处理

## 项目亮点

### 🤖 大模型 API 集成与优化

深度集成智谱 GLM-4-plus 模型，实现完整的 OpenAI 兼容 API 调用体系。通过流式响应处理、错误重试机制和 Token 优化，确保 API 调用的稳定性和成本控制。采用 KV 缓存策略，有效降低 API 调用频率，在保证内容质量的同时显著节省成本。

### ⚡ 高性能边缘计算架构

基于 Cloudflare Workers 构建 Serverless 后端，实现全球边缘部署和零冷启动。采用异步并发处理机制，支持多数据源并行抓取，大幅提升数据处理效率。通过 KV 持久化存储优化数据访问性能，确保系统在高并发场景下的稳定性。

### 🔄 智能内容处理流水线

设计多阶段 AI 处理流程，从数据抓取、内容清洗到智能摘要和深度分析，实现端到端的自动化内容生成。支持自定义 Prompt 工程和模型参数调优，可根据不同场景灵活调整处理策略，实现从原始数据到结构化内容的智能转换。

### 📊 可扩展的数据源架构

采用模块化设计，每个数据源独立封装为可插拔组件，通过统一的数据转换层实现异构数据源的标准化处理。支持 Folo、GitHub、Twitter 等多源数据聚合，易于扩展新的数据源，满足不同场景的个性化需求。

## 快速开始

### 环境要求

- Node.js 18+ 
- npm 或 yarn
- Cloudflare 账号

### 安装步骤

1. **克隆项目**
   ```bash
   git clone https://github.com/Rebornbugkiller/CloudFlare-AI-Insight-Daily.git
   cd CloudFlare-AI-Insight-Daily
   ```

2. **安装依赖**
   ```bash
   npm install
   ```

3. **安装 Wrangler CLI**
   ```bash
   npm install -g wrangler
   ```

4. **配置环境变量**

   编辑 `wrangler.toml` 文件，配置以下关键参数：
   - `OPENAI_API_KEY`: 智谱 API Key
   - `OPENAI_API_URL`: 智谱 API 地址
   - `DEFAULT_OPEN_MODEL`: 模型名称（如 `glm-4-plus`）
   - `GITHUB_TOKEN`: GitHub Personal Access Token
   - `GITHUB_REPO_OWNER`: GitHub 仓库所有者
   - `GITHUB_REPO_NAME`: GitHub 仓库名称
   - `FOLO_COOKIE_KV_KEY`: Folo Cookie 存储键名
   - 其他数据源配置...

5. **创建 Cloudflare KV 命名空间**

   在 Cloudflare 控制台创建 KV 命名空间，并将 ID 填入 `wrangler.toml` 的 `kv_namespaces` 配置中。

6. **登录 Cloudflare**
   ```bash
   wrangler login
   ```

7. **部署到 Cloudflare**
   ```bash
   wrangler deploy
   ```

### 本地开发

```bash
wrangler dev
```

访问 `http://localhost:8787` 进行本地调试。

## 使用说明

### 访问后端管理界面

部署成功后，访问您的 Worker 地址：
```
https://your-worker.workers.dev/getContentHtml
```

默认登录信息：
- 用户名: `root`
- 密码: `toor`

### 生成日报流程

1. 访问管理界面，查看今天抓取的数据源内容
2. 选择要包含在日报中的新闻条目
3. 点击"生成 AI 日报"，系统会调用 GLM-4-plus 生成摘要
4. 生成完成后，可以保存到 GitHub 或查看预览

### RSS 订阅

生成 RSS 数据后，可通过以下地址订阅：
```
https://your-worker.workers.dev/rss
```

## 项目结构

```
CloudFlare-AI-Insight-Daily/
├── src/
│   ├── index.js                 # Worker 入口文件
│   ├── handlers/                # 请求处理器
│   │   ├── genAIContent.js     # AI 内容生成
│   │   ├── commitToGitHub.js   # GitHub 提交
│   │   ├── getContent.js       # 内容获取
│   │   └── ...
│   ├── dataSources/            # 数据源模块
│   │   ├── aibase.js
│   │   ├── github-trending.js
│   │   ├── twitter.js
│   │   └── ...
│   ├── prompt/                 # Prompt 模板
│   ├── chatapi.js              # 大模型 API 调用
│   ├── kv.js                   # KV 存储操作
│   └── helpers.js              # 工具函数
├── wrangler.toml               # Cloudflare 配置
└── README.md
```

## 配置说明

主要配置项在 `wrangler.toml` 文件中：

- **AI 模型配置**: `USE_MODEL_PLATFORM`、`OPENAI_API_KEY`、`OPENAI_API_URL`、`DEFAULT_OPEN_MODEL`
- **GitHub 配置**: `GITHUB_TOKEN`、`GITHUB_REPO_OWNER`、`GITHUB_REPO_NAME`、`GITHUB_BRANCH`
- **数据源配置**: `FOLO_COOKIE_KV_KEY`、`FOLO_DATA_API`、各数据源的 `FEED_ID` 和 `FETCH_PAGES`
- **播客配置**: `PODCAST_TITLE`、`PODCAST_BEGIN`、`PODCAST_END`

详细配置说明请参考 `wrangler.toml` 文件中的注释。

## 在线演示

- **后端管理界面**: https://ai-daily.lol2690412438.workers.dev/
- **RSS 订阅**: https://ai-daily.lol2690412438.workers.dev/rss

## 技术难点与解决方案

### 1. 大模型 API 调用优化

**问题**: API 调用成本高，响应时间长

**解决方案**:
- 实现 KV 缓存机制，避免重复调用
- 采用流式响应，提升用户体验
- 优化 Prompt 设计，减少 Token 消耗
- 实现错误重试和降级策略

### 2. 多数据源并发处理

**问题**: 多个数据源串行抓取效率低

**解决方案**:
- 使用 Promise.allSettled 实现并发抓取
- 为每个数据源设置超时和重试机制
- 统一数据格式，简化后续处理

### 3. 数据一致性问题

**问题**: KV 存储与数据库可能存在不一致

**解决方案**:
- 采用"先更新数据库，后删除缓存"策略
- 使用 TTL 机制作为兜底方案
- 通过消息队列实现重试机制

## 未来规划

- [ ] 支持更多大模型（OpenAI、Claude、DeepSeek 等）
- [ ] 实现更智能的内容推荐算法
- [ ] 添加用户个性化订阅功能
- [ ] 优化 Prompt 工程，提升生成质量
- [ ] 支持更多数据源接入
- [ ] 实现内容质量评估机制

## 贡献指南

欢迎提交 Issue 和 Pull Request！

## 开源协议

本项目基于 MIT 协议开源。

## 联系方式

- GitHub: [@Rebornbugkiller](https://github.com/Rebornbugkiller)
- 项目地址: https://github.com/Rebornbugkiller/CloudFlare-AI-Insight-Daily

---

如果这个项目对您有帮助，欢迎给个 ⭐ Star 支持一下！
