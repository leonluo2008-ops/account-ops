# account-ops

**AI 工程师独立创作者账号运营 skill** — 把日常工作（GitHub 项目调研 + 实测 + 整合到工作流）沉淀成可复用的内容资产。

## 3 层架构

```
┌─────────────────────────────────────────────────────┐
│  Layer 1 · Skill（公共知识，推远端）                   │
│  • 工作流 SOP、模板、决策框架                          │
│  • 推到 GitHub：leonluo2008-ops/account-ops         │
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

**核心安全约束**：
- ❌ 永不把 `.env.local` 推到远端（**已在 .gitignore 强制排除**）
- ❌ 永不把 API key 写进 SKILL.md / README.md
- ✅ skill 引用 ENV 用 `os.environ.get("NOTION_API_KEY")` 间接读

## 快速开始（clone → 跑）

```bash
# 1. clone skill
git clone git@github.com:leonluo2008-ops/account-ops.git
cd account-ops

# 2. 复制 ENV 模板
cp .env.local.example .env.local
chmod 600 .env.local

# 3. 编辑 .env.local，填入真实值
#   - NOTION_API_KEY（从 https://www.notion.so/my-integrations 拿）
#   - 9 个 page/database ID（从你的 Notion workspace 拿）

# 4. 验证 ENV 加载成功
set -a && source .env.local && set +a
echo "API key prefix: ${NOTION_API_KEY:0:8}"
echo "调研池 DB: $NOTION_DATABASE_RESEARCH_POOL_ID"
```

## 触发词

**主触发词**：Agent工程师账号

**其他触发词**：
- 账号运营 / 创作者账号 / AI 内容创作者
- 内容矩阵 / 选题 / 实测视频
- 视频脚本 / 置顶评论 / 封面提示词
- 变现节奏 / 账号定位 / 人设
- Notion 账号工作流 / 调研池 / 选题库
- B 站视频 / YouTube 同步 / 抖音剪精华

## 定位

- 一句话：**每周挖 1-2 个 GitHub 上的 AI 神器，测它到底有没有用**
- 主阵地：B 站 + YouTube
- 流量入口：抖音（剪精华）
- 暂缓：视频号（3 个月后再考虑）

## 4 大核心工作流

1. **选题决策**（调研池 → 选题库）
2. **脚本撰写**（选题 → 配音稿）
3. **多平台发布**（剪辑完 → 4 平台上线）
4. **月度/季度复盘**

## 4 类内容（A/B/C/D）

- **A 单项目实测**（每周 1-2 条）
- **B 横向对比**（每月 1-2 条）
- **C 整合到工作流**（每月 1-2 条）
- **D 行业观察**（每月 1 条）

## 目录结构

```
account-ops/
├── SKILL.md                              # 入口（AI 加载）
├── README.md                             # 本文件
├── .env.local.example                    # ENV 模板（推远端）
├── .gitignore                            # 排除 .env.local 等敏感文件
├── references/                           # 详细文档
│   ├── account-positioning.md            # 账号定位
│   ├── workflow-sop.md                   # 4 大核心工作流 SOP
│   ├── content-matrix.md                 # A/B/C/D 4 类内容
│   ├── platform-strategy.md              # 平台分发策略
│   ├── monetization-rhythm.md            # 变现节奏
│   └── notion-schema.md                  # Notion 完整 schema + API 范式
├── templates/                            # 9 份模板
│   ├── video-script-template.md
│   ├── video-outline-template.md
│   ├── bilibili-intro-template.md
│   ├── youtube-intro-template.md
│   ├── thumbnail-prompt-template.md
│   ├── pinned-comment-template.md
│   ├── title-formula-library.md
│   ├── research-pool-entry-template.md
│   └── notion-row-template.md            # 3 database 添加行模板
└── assets/                               # 占位（图标/封面）
```

## 关联资源

- **Notion 工作系统**：「AI 工程师独立创作者」父页面（含 5 子页面 + 3 数据库）
- **GitHub 仓**：本 skill 同步维护
- **本地 ENV**：`.env.local`（git 不可见）

## 版本

**v1.0.0**（2026-06-09）：首版，覆盖 4 核心工作流 + 9 模板 + 3 层架构（Skill / Notion / 本地 ENV）

## 安全须知（fork 前必读）

如果你 fork 这个 skill：

1. **必须**保留 `.gitignore` 里的 `.env.local` 排除规则
2. **不要**在 commit 里出现任何 `ntn_xxx` / `secret_xxx` 形式的字符串
3. **不要**在 issue / discussion 里贴你的 Notion page ID（虽然不算绝密，但**结构性信息暴露 = 知道你的工作区结构**）
4. 任何想"分享我的 Notion 链接"的想法，**改成分享 .env.local.example**

## License

MIT
