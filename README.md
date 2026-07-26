[PUBLIC-READY]

# BongBong & Avatar 双机器人陪伴系统 / BongBong & Avatar Dual-Bot Companion System

面向家庭长者陪伴场景的双 Telegram 机器人：一个负责日常聊天与知识服务，一个负责娱乐与内容生成，共享记忆并支持多模型交叉验证。
Two Telegram bots for family/eldercare companionship — one for daily chat and knowledge services, one for entertainment and content generation, sharing a memory layer and a multi-model cross-verification feature.

## 技术栈 / Stack

- Node.js（ESM）+ `node-telegram-bot-api`
- AI：`@google/generative-ai`（Gemini）、`openai`（兼容接口调用 Grok / OpenAI 兼容模型）
- 数据：`mongodb`（含向量检索）、`@supabase/supabase-js`
- 集成：`@notionhq/client`（Notion 同步）
- 部署：Docker + docker-compose，`/health` 健康检查端点

## 功能 / Features

- **双机器人分工**：主聊天 Bot（日常对话/笔记/养生/新闻/创作）+ Avatar/Admin Bot（签证查询、脑力游戏、AI 图片/视频生成）——见 `docs/DUAL_BOT_ARCHITECTURE.md` 与 `src/services/dualBotService.js`
- **真實之眼 / Eye of Truth**：多模型交叉验证（`src/services/eyeOfTruthService.js`）
- **智能模型路由**：按字数阈值和关键词在默认/复杂/娱乐等多档模型间切换（`modelRouter.js`、`smartRouter.js`）
- **记忆系统**：跨对话上下文 + 群组共享记忆（`memoryService.js`、`smartMemoryService.js`、`groupMemoryService.js`）
- **语音/图像处理**：`voiceHandler.js` / `voiceHandlerV2.js`、`imageService.js`、`visionService.js`
- **Notion 同步**：`notionSyncService.js`
- Two Telegram bots split by role; Gemini+Grok cross-verification ("Eye of Truth"); complexity/keyword-based model routing; cross-conversation + shared group memory; voice/image handling; Notion sync.

## 本地运行 / Getting started

```bash
npm install
cp .env.example .env   # 填入真实值 / fill in real values
npm run dev             # nodemon 热重载 / hot reload
# 或 / or
npm start                # node src/index.js
# 或用 Docker / or via Docker
docker compose up -d
```

## 环境变量 / Env

按代码实际读取的完整清单（`.env.example` 目前缺 `TELEGRAM_BOT_TOKEN_AVATAR` 等几项，已按代码补全），值均为占位，真实值由 Doppler 注入。
Complete list based on actual `process.env` reads in the codebase (`.env.example` is missing a few of these, e.g. `TELEGRAM_BOT_TOKEN_AVATAR` — filled in below from real usage); all values are placeholders, real values injected via Doppler.

```
TELEGRAM_BOT_TOKEN=
TELEGRAM_BOT_TOKEN_AVATAR=
TELEGRAM_CHAT_ID=
GEMINI_API_KEY=
GROK_API_KEY=
OPENAI_API_KEY=
MONGODB_URI=
MONGODB_DB_NAME=
NOTION_API_KEY=
NOTION_TOKEN=
NOTION_TG_CHAT_DB_ID=
MODEL_DEFAULT=
MODEL_COMPLEX=
MODEL_ROAST=
MODEL_FAST=
MODEL_BEST=
MODEL_EMOTIONAL=
MODEL_MEDICAL=
MODEL_FORTUNE=
MODEL_EXPERIMENTAL=
MODEL_GEMINI_FALLBACK=
COMPLEXITY_WORD_THRESHOLD=
COMPLEXITY_KEYWORDS=
ROAST_KEYWORDS=
TRIGGER_KEYWORDS=
AUTO_REPLY_MODE=
VECTOR_DIMENSION=
VECTOR_SIMILARITY_THRESHOLD=
MAX_SEARCH_RESULTS=
```

生产部署（`docker-compose.yml`）通过额外别名（如 `MONGODB_VPC_URI`、`GEMINI_API_OECE_TECH_`、`GROK_ONE_`）注入同样的值，供 DO 内网连接使用。
The production deploy (`docker-compose.yml`) maps these to differently-named aliases (e.g. `MONGODB_VPC_URI`, `GEMINI_API_OECE_TECH_`, `GROK_ONE_`) for the DO VPC deployment.

## 完成度 / Status

能跑，功能完整（双机器人、记忆系统、多模型路由均已实现），无自动化测试。
Runnable and feature-complete (dual bots, memory system, multi-model routing all implemented); no automated tests yet.
