---
name: account-ops
description: "AI 工程师独立创作者账号运营：GitHub 项目实测 → 视频 → 多平台发布 → 数据复盘 的完整工作流。覆盖选题决策、脚本撰写、录剪 SOP、封面/简介/评论模板、月度复盘。**触发词：Agent工程师账号、账号运营、创作者账号、内容矩阵、选题、实测视频、视频脚本、置顶评论、变现节奏、Notion 账号工作流。**"
version: 1.0.0
author: leonluo2008
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [creator, account, content, bilibili, youtube, notion, sop]
    homepage: https://github.com/leonluo2008-ops/account-ops
prerequisites:
  env_vars: []
  commands: []
---

# account-ops

**AI 工程师独立创作者账号运营 skill** —— 把"日常工作（GitHub 项目调研 + 实测 + 整合到工作流）"沉淀成"可复用的内容资产"。

## 触发条件

**用户提到以下任一关键词** → 加载本 skill：

- **Agent工程师账号**（**主触发词**，必命中）
- 账号运营、创作者账号、AI 内容创作者
- 内容矩阵、选题、实测视频
- 视频脚本、置顶评论、封面提示词
- 变现节奏、账号定位、人设
- Notion 账号工作流、调研池、选题库
- B 站视频、YouTube 同步、抖音剪精华

## 核心定位（v1.0.0 锁定）

**一句话**：**AI 工程师独立创作者：每周挖 1-2 个 GitHub 上的 AI 神器，测它到底有没有用，整合进我的工作流。**

**人设标签**：
- AI 工程师 / Agent 操盘手
- 开源项目调研 × 实测 × 整合到工作流
- 目标用户：想学 AI 的创作者/工程师，愿意为方法论付费

**差异化锚点**：
- ❌ 不教"怎么按按钮"
- ✅ 讲"我怎么决策、怎么翻车、怎么复盘"
- ✅ 实测 + 整合到工作流（**不云测评**）

## 内容矩阵（A/B/C/D 四类）

| 类型 | 频率 | 典型形态 | 时长 |
|---|---|---|---|
| **A 单项目实测** | 每周 1-2 条 | 完整测一个项目 | 10-15 min |
| **B 横向对比** | 每月 1-2 条 | 5-10 个同类项目对比 | 15-20 min |
| **C 整合到工作流** | 每月 1-2 条 | 怎么把好东西塞进 pipeline | 10-15 min |
| **D 行业观察** | 每月 1 条 | GitHub Trending 周报 / 行业趋势 | 8-10 min |

## 4 大核心工作流（v1.0.0 范围）

| # | 工作流 | 详细 SOP |
|---|---|---|
| 1 | **选题决策**（调研池 → 选题库） | → `references/workflow-sop.md#1-选题决策` |
| 2 | **脚本撰写**（选题 → 配音稿） | → `references/workflow-sop.md#2-脚本撰写` |
| 3 | **多平台发布**（剪辑完 → 4 平台上线） | → `references/workflow-sop.md#3-多平台发布` |
| 4 | **月度/季度复盘** | → `references/workflow-sop.md#4-月度季度复盘` |

**4 个占位工作流**（v1.1+ 补）：录屏配音、剪辑字幕、互动回复、内容日历

## 平台分发策略

| 平台 | 角色 | 动作 |
|---|---|---|
| **B 站** | 主阵地 | 长视频（8-15 min）首发 |
| **YouTube** | 同步主阵地 | 同 B 站 + 英文字幕 |
| **抖音** | 流量入口 | 剪精华 1-3 min + 挂 B 站链接 |
| **视频号** | 暂缓 | 3 个月后开通（**不分散精力**） |

## 变现节奏（不急，沉淀优先）

| 阶段 | 时间 | 动作 |
|---|---|---|
| 沉淀期 | 0-6 月 | 30-50 条视频，纯发内容，不收钱 |
| 私域期 | 6-12 月 | 公众号 → 知识星球（199-299/年） |
| 叠加期 | 12+ 月 | 系统课（999-1999）+ 咨询 + 招聘引流 |

## 3 层架构（Skill / Notion / 本地 ENV）

```
┌─────────────────────────────────────────────────────┐
│  Layer 1 · Skill（公共知识，推远端）                   │
│  • 工作流 SOP、模板、决策框架                           │
│  • 推到 GitHub：leonluo2008-ops/account-ops          │
├─────────────────────────────────────────────────────┤
│  Layer 2 · Notion（数据 + 视图，云端）                │
│  • 调研池、选题库、脚本库的真实数据                     │
│  • 5 子页面 + 3 database                              │
├─────────────────────────────────────────────────────┤
│  Layer 3 · 本地 ENV（敏感信息，本地）                  │
│  • NOTION_API_KEY（真敏感）                          │
│  • page/database ID（结构性引用）                    │
│  • .env.local（git 绝看不到，chmod 600）             │
│  • .env.local.example（推远端的模板）                │
└─────────────────────────────────────────────────────┘
```

**关键约束**：
- ❌ 永不把 `.env.local` 推到远端
- ❌ 永不把 API key 写进 SKILL.md / README.md
- ✅ skill 引用 ENV 用 `os.environ.get("NOTION_API_KEY")` 间接读
- ✅ 加载方式：shell `set -a && source .env.local && set +a` / Python `load_dotenv(".env.local")`

---

## Notion 工作系统

**父页面**：AI 工程师独立创作者

| 子页面/数据库 | 用途 | 入口 |
|---|---|---|
| 🎯 战略层 | 定位/矩阵/变现节奏（说明文） | 父页面下 |
| 📅 执行层 | 节奏管理 | 父页面下 |
| 📥 项目调研池 | GitHub 项目候选 | 📅 执行层下 |
| 🎬 视频选题库 | 选题 + 状态追踪 | 📅 执行层下 |
| 🎬 制作层 | 录/剪/配/字幕 SOP | 父页面下 |
| 📝 视频脚本库 | 脚本 + 状态 | 🎬 制作层下 |
| 📊 复盘层 | 数据追踪 | 父页面下 |
| 💰 变现层 | 变现路径规划 | 父页面下 |

**完整 schema + ID + API 范式 + silent fail 防护** → 见 `references/notion-schema.md`

**状态流转**：

```
选题：💡待策划 → 📝写脚本中 → 🎙️待录制 → ✂️剪辑中 → 🚀已发布
调研：🔍待调研 → 📖读README中 → 🧪实测中 → ✅可整合 / ❌放弃
脚本：📝草稿 → ✅定稿待录 → 🎬已发布 / ⏸️搁置
```

## 工作流路由（用户需求进来 → 走哪条路）

```
用户需求
├── "我要开始做这个账号"              → 走 references/account-positioning.md
├── "我想选题/有个项目"               → 走 workflow #1（选题决策）
├── "我要写脚本/这期视频怎么写"         → 走 workflow #2（脚本撰写）
├── "我录完了/怎么发布/多平台"         → 走 workflow #3（多平台发布）
├── "我想复盘/看数据"                 → 走 workflow #4（月度/季度复盘）
├── "我想要封面/简介/评论模板"         → 走 templates/ 下对应模板
├── "我想看标题怎么写"                → 走 templates/title-formula-library.md
├── "我怎么用 Notion 改数据"           → 走 references/notion-schema.md + templates/notion-row-template.md
├── "Notion API 报错了"               → 走 references/notion-schema.md §5（端点 + color + language 白名单）
└── "我想加新 page/database"           → 走 references/notion-schema.md §5.2（silent fail 防护 SOP）
```

## 速查：模板库入口

| 模板 | 路径 |
|---|---|
| 视频脚本模板 | `templates/video-script-template.md` |
| B 站简介模板 | `templates/bilibili-intro-template.md` |
| YouTube 简介模板 | `templates/youtube-intro-template.md` |
| 封面提示词模板 | `templates/thumbnail-prompt-template.md` |
| 置顶评论模板 | `templates/pinned-comment-template.md` |
| 标题公式库 | `templates/title-formula-library.md` |
| 视频大纲模板 | `templates/video-outline-template.md` |
| 调研池条目模板 | `templates/research-pool-entry-template.md` |
| **Notion 3 database 添加行模板** | `templates/notion-row-template.md` |

## 速查：references 库入口

| 文档 | 路径 |
|---|---|
| 账号定位 | `references/account-positioning.md` |
| 4 大工作流 SOP 详细 | `references/workflow-sop.md` |
| 内容矩阵详解 | `references/content-matrix.md` |
| 平台分发策略 | `references/platform-strategy.md` |
| 变现节奏规划 | `references/monetization-rhythm.md` |
| **Notion 完整 schema + API 范式** | `references/notion-schema.md` |

## 速查：本地 ENV

| 字段 | 存放位置 |
|---|---|
| 真实值 | `.env.local`（本地，chmod 600，git 绝看不到） |
| 模板 | `.env.local.example`（推远端） |
| 字段说明 | `references/notion-schema.md §6` |

## v1.0.0 已知边界

- ✅ 4 核心工作流：选题/脚本/发布/复盘
- ⏸️ 4 占位工作流（v1.1+）：录屏配音/剪辑字幕/互动回复/内容日历
- ⏸️ SKO 自我进化能力（v2.0 路线图）
- ⏸️ Notion 自动同步脚本（v1.2 路线图）

## 关键原则（铁律）

1. **副业节奏，不急** —— 6 个月内不碰变现，先沉淀内容资产
2. **不要为了做账号而做账号** —— 实测 + 整合工作流是本业，**视频是顺便**
3. **B 站/YouTube 一次录完两边发** —— 不为单平台重做内容
4. **抖音是引流工具** —— 不是内容生产
5. **视频号 3 个月后再说** —— 别分散精力
6. **每条视频必须有"反常识"或"踩坑"** —— 才有差异化
7. **真人配音 + 屏幕录**为主（70%），**半出镜**为辅（25%），**全出镜**为仪式感（5%）
