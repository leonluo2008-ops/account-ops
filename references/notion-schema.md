# Notion 工作系统 Schema 完整文档

> **这个文件是 account-ops skill 跟 Notion 数据打交道的"地图"**——所有 page ID、database ID、property schema、API 范式、silent fail 防护都在这里。

## 1. 3 层架构（必读）

```
┌─────────────────────────────────────────────────────┐
│  Layer 1 · Skill（公共知识，推远端）                   │
│  ────────────────────────────────────────────────── │
│  • 工作流 SOP、模板、决策框架                          │
│  • references/ + templates/ + SKILL.md              │
│  • 推到 GitHub：leonluo2008-ops/account-ops         │
├─────────────────────────────────────────────────────┤
│  Layer 2 · Notion（数据 + 视图，云端）                │
│  ────────────────────────────────────────────────── │
│  • 调研池、选题库、脚本库的真实数据                     │
│  • 5 子页面：战略/执行/制作/复盘/变现                 │
│  • 不在 git 里，但 page ID 在 .env.local             │
├─────────────────────────────────────────────────────┤
│  Layer 3 · 本地 ENV（敏感信息，本地）                  │
│  ────────────────────────────────────────────────── │
│  • NOTION_API_KEY（真敏感）                          │
│  • page/database ID（结构性引用）                    │
│  • .env.local（git 绝看不到）                        │
│  • .env.local.example（推远端的模板）                │
└─────────────────────────────────────────────────────┘
```

**关键约束**：
- ❌ **永不**把 `.env.local` 推到远端
- ❌ **永不**把 API key 写进 SKILL.md / README.md
- ✅ skill 引用 ENV 用 `os.environ.get("NOTION_API_KEY")` 间接读

---

## 2. Notion 资源 ID 清单

**所有 ID 存在 `.env.local` 里**（本地），下表是**人类可读索引**——**值**见 `.env.local`。

### 2.1 页面 ID

| ENV 变量名 | 用途 | 备注 |
|---|---|---|
| `NOTION_PARENT_PAGE_ID` | 父页面「AI 工程师独立创作者」 | 5 子页面 + 3 database 都在它下面 |

### 2.2 Database + Data Source ID

**每个 database 有 2 个 ID**（Notion 2025-09-03 双轨制）：

| 用途 | ENV 变量名 | 何时用 |
|---|---|---|
| Database ID | `NOTION_DATABASE_{NAME}_ID` | 创建 page / PATCH 时 parent 用 |
| Data Source ID | `NOTION_DATA_SOURCE_{NAME}_ID` | 查询 / PATCH property 时用 |

| Database | Database ID ENV | Data Source ID ENV |
|---|---|---|
| 📥 项目调研池 | `NOTION_DATABASE_RESEARCH_POOL_ID` | `NOTION_DATA_SOURCE_RESEARCH_POOL_ID` |
| 🎬 视频选题库 | `NOTION_DATABASE_TOPICS_ID` | `NOTION_DATA_SOURCE_TOPICS_ID` |
| 📝 视频脚本库 | `NOTION_DATABASE_SCRIPTS_ID` | `NOTION_DATA_SOURCE_SCRIPTS_ID` |

---

## 3. Property Schema（必查，建完数据库必跑这个自检）

### 3.1 📥 项目调研池（8 properties + 1 title）

| # | Property Name | Type | 必填 | 备注 |
|---|---|---|---|---|
| 1 | `Name` | title | ✅ | 项目名（GitHub repo 全名） |
| 2 | `GitHub URL` | url | ✅ | 完整 URL |
| 3 | `来源` | select | ✅ | 8 个选项：GitHub Trending/朋友推荐/Twitter-X/Reddit/Hacker News/Newsletter/自己挖到/其他 |
| 4 | `分类` | multi_select | ⛔ | 9 个标签：AI Agent/Skill 框架/MCP 工具/LLM 框架/视频生成/图像生成/TTS·音频/开发工具/其他 |
| 5 | `Stars` | number | ⛔ | 整数 |
| 6 | `调研状态` | select | ✅ | 5 个：🔍待调研/📖读README中/🧪实测中/✅可整合/❌放弃 |
| 7 | `加进池子时间` | date | ⛔ | ISO 8601 日期 |
| 8 | `一句话理由` | rich_text | ✅ | 30 秒写"为什么想看" |

### 3.2 🎬 视频选题库（9 properties + 1 title = 10 个）

| # | Property Name | Type | 必填 | 备注 |
|---|---|---|---|---|
| 1 | `Name` | title | ✅ | 选题（一句话视频主题） |
| 2 | `对应项目` | rich_text | ⛔ | 文本写 GitHub repo 名 |
| 3 | `GitHub URL` | url | ⛔ | 完整 URL |
| 4 | `类型` | select | ✅ | 5 个：A单项目实测/B横向对比/C整合到工作流/D行业观察/自我介绍·起号 |
| 5 | `状态` | select | ✅ | 6 个：💡待策划/📝写脚本中/🎙️待录制/✂️剪辑中/🚀已发布/⏸️暂停·取消 |
| 6 | `目标发布日期` | date | ✅ | ISO 8601（**逼自己拍日期**） |
| 7 | `实际发布日期` | date | ⛔ | 发布后回填 |
| 8 | `B 站链接` | url | ⛔ | 发布后回填 |
| 9 | `YouTube 链接` | url | ⛔ | 发布后回填 |
| 10 | `数据笔记` | rich_text | ⛔ | 一句话体感 |

### 3.3 📝 视频脚本库（9 properties + 1 title = 10 个）

| # | Property Name | Type | 必填 | 备注 |
|---|---|---|---|---|
| 1 | `Name` | title | ✅ | 视频标题 |
| 2 | `状态` | select | ✅ | 4 个：📝草稿/✅定稿待录/🎬已发布/⏸️搁置 |
| 3 | `对应选题` | rich_text | ⛔ | 文本写选题库对应那条的标题或 ID |
| 4 | `视频类型` | select | ✅ | 5 个：A单项目实测/B横向对比/C整合到工作流/D行业观察/自我介绍·起号 |
| 5 | `目标时长(分钟)` | number | ⛔ | 整数 |
| 6 | `实际时长(分钟)` | number | ⛔ | 整数（发布后回填） |
| 7 | `B 站链接` | url | ⛔ | 发布后回填 |
| 8 | `YouTube 链接` | url | ⛔ | 发布后回填 |
| 9 | `实际发布日期` | date | ⛔ | ISO 8601 |
| 10 | `脚本正文` | rich_text | ⛔ | 整篇配音稿（一整段） |

---

## 4. 状态流转规则

```
选题（视频选题库 / 状态）：
  💡待策划 → 📝写脚本中 → 🎙️待录制 → ✂️剪辑中 → 🚀已发布
                                              ↘ ⏸️暂停/取消（任一阶段可转）

调研（调研池 / 调研状态）：
  🔍待调研 → 📖读README中 → 🧪实测中 → ✅可整合
                                       ↘ ❌放弃（任一阶段可转）

脚本（脚本库 / 状态）：
  📝草稿 → ✅定稿待录 → 🎬已发布
                          ↘ ⏸️搁置
```

**关键规则**：
- 选题库有"待策划"才进脚本库（避免脚本库变垃圾场）
- 脚本库"定稿待录"才进选题库"待录制"（避免状态错位）
- "已发布"状态在 3 个数据库里都有，对应"内容上线"那一刻

---

## 5. Notion API 2025-09-03 范式（**踩坑总结**）

### 5.1 端点选择

| 任务 | 用什么端点 | 备注 |
|---|---|---|
| **创建 database** | `POST /v1/databases` | **不能用** `/v1/data_sources`（会 400 报错） |
| **查询 / 列数据** | `POST /v1/data_sources/{id}/query` | **不能用** `/v1/databases/{id}/query` |
| **PATCH property** | `PATCH /v1/data_sources/{id}` | **不能用** `/v1/databases/{id}`（会 silent fail） |
| **创建 page** | `POST /v1/pages`（parent 用 database_id） | ✅ |
| **追加 children** | `PATCH /v1/blocks/{id}/children`（**不**带 parent） | ✅ |
| **查询 page** | `GET /v1/pages/{id}` | ✅ |
| **查询 children** | `GET /v1/blocks/{id}/children` | ✅ |

### 5.2 Silent Fail 防护 SOP（**必跑**）

**问题**：用 `POST /v1/databases` 创建时，properties 字段如果用**简写**（`"项目名": {"title": {}}`），**整个 properties 块会被静默忽略**，只留下一个 `Name` title property。

**防护步骤**（**每次建完 database 必跑**）：

```python
# 1. 建完后立刻 GET 这个 database
curl -s "https://api.notion.com/v1/databases/$DB_ID" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03"

# 2. 数 properties
# 期望：1 + 你定义的 N 个 properties
# 实际只有 1 → 静默失败，需 PATCH 补全

# 3. 用 data_sources 端点 PATCH（**不是** databases）
curl -s -X PATCH "https://api.notion.com/v1/data_sources/$DS_ID" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{"properties": {...完整 schema...}}'
```

**自检脚本**（每个 database 都跑一次）：

```bash
# 数 properties
PROP_COUNT=$(curl -s "https://api.notion.com/v1/data_sources/$DS_ID" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" | \
  python3 -c "import sys, json; print(len(json.load(sys.stdin)['properties']))")

# 数 rows
ROW_COUNT=$(curl -s -X POST "https://api.notion.com/v1/data_sources/$DS_ID/query" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d '{"page_size":100}' | \
  python3 -c "import sys, json; print(len(json.load(sys.stdin)['results']))")

echo "DB: $PROP_COUNT properties, $ROW_COUNT rows"
# 期望值（v1.0.1 校正后）：
#   调研池: 8 properties + 1 row（gardener-skill）
#   选题库: 10 properties + 2 rows（D1 自我介绍 + D1 配套素材）
#   脚本库: 10 properties + 2 rows（D1 完整脚本 + D1 配套素材）
```

### 5.3 color 选项白名单

`select` / `multi_select` 的 color 字段**只能是这 9 个值**之一：

```
default, gray, brown, orange, yellow, green, blue, purple, pink, red
```

**不支持**：`cyan` / `magenta` / `black` / `white` 等。

### 5.4 code block 的 language 白名单

`code` block 的 `language` 字段**必须是 Notion 枚举值**之一：

```
abap, abc, agda, arduino, ascii art, assembly, bash, basic, bnf, c, c#,
c++, clojure, coffeescript, coq, css, dart, dhall, diff, docker, ebnf,
elixir, elm, erlang, f#, flow, fortran, gherkin, ...
```

**最常用**：`plain text`（写提示词）、`markdown`、`bash`、`python`、`json`、`yaml`。

**绝不能用**：`text`（**这是我们踩过的坑**）。

---

## 6. ENV 字段说明（**.env.local 必备**）

### 6.1 真敏感字段（泄露=灾难）

| 字段 | 用途 |
|---|---|
| `NOTION_API_KEY` | 调 Notion API 的 Bearer Token |

### 6.2 结构性引用（v1.0.0 阶段算敏感，公开调研池后不算）

| 字段 | 用途 |
|---|---|
| `NOTION_PARENT_PAGE_ID` | 父页面 ID |
| `NOTION_DATABASE_RESEARCH_POOL_ID` | 调研池 database |
| `NOTION_DATABASE_TOPICS_ID` | 选题库 database |
| `NOTION_DATABASE_SCRIPTS_ID` | 脚本库 database |
| `NOTION_DATA_SOURCE_RESEARCH_POOL_ID` | 调研池 data source |
| `NOTION_DATA_SOURCE_TOPICS_ID` | 选题库 data source |
| `NOTION_DATA_SOURCE_SCRIPTS_ID` | 脚本库 data source |

### 6.4 dotenv 引号陷阱（v1.2 新增 · 踩坑必看）

**坑 5（2026-06-09 实测）**：`.env.local` 文件里 key **被双引号包裹**：

```bash
# ❌ 错误（双引号包裹了 value）
NOTION_API_KEY="ntn_4401623287...RF1Slv8aCF"

# ✅ 正确（无双引号）
NOTION_API_KEY=ntn_4401623287...RF1Slv8aCF
```

**症状**：Python `os.environ.get("NOTION_API_KEY")` 拿到**带引号的 52 字符字符串**（真值 50 + 2 个引号）→ 调 Notion API 返 401 → 排查半天找不到原因。

**根因**：很多 dotenv 编辑器/插件**自动加双引号**（尤其复制粘贴时）；用户从某处拷贝时自带引号；某些 IDE 模板默认带引号。

**防护 SOP**（新建/修改 .env.local 后必跑）：

```python
import os
from pathlib import Path

env_path = Path(".env.local")
with open(env_path) as f:
    for line in f:
        line = line.strip()
        if line and not line.startswith("#") and "=" in line:
            k, v = line.split("=", 1)
            # 关键：strip 双引号/单引号
            v = v.strip().strip('"').strip("'")
            if v != line.split("=", 1)[1].strip():
                print(f"⚠️ {k}: 检测到引号包裹，已自动 strip")
            env[k.strip()] = v

# 验证
api_key = env["NOTION_API_KEY"]
assert len(api_key) == 50 and not api_key.startswith('"'), f"Key 长度/格式异常: len={len(api_key)}"
print(f"✅ Key 验证通过: {api_key[:10]}...{api_key[-6:]} (len={len(api_key)})")
```

**修复方法**（如果已经踩坑）：

```bash
# 1. 打开 .env.local
vim .env.local

# 2. 把所有 NOTION_*_KEY 的 value 用引号包起来的去掉引号
# NOTION_API_KEY="ntn_..."  →  NOTION_API_KEY=ntn_...

# 3. 重跑 env-self-check.md §1 的 2 步快速自检
```

**对比**：
- 坑 4（v1.0.1 之前）：key **多打 1 个字符**（51 vs 50）→ 401
- 坑 5（v1.2 新增）：key **被引号包**（52 vs 50）→ 401
- 共同点：都是**401 unauthorized** → 走 env-self-check.md §2 的 md5 对比定位

### 6.3 加载 ENV 的标准方式

**Python 示例**（手动加载，绕开 sandbox 隔离）：

```python
import os
from pathlib import Path
from dotenv import load_dotenv

# 1. 加载 .env.local
env_path = Path(__file__).parent / ".env.local"
if env_path.exists():
    load_dotenv(env_path)

# 2. 读 ENV
api_key = os.environ.get("NOTION_API_KEY")
db_id = os.environ.get("NOTION_DATABASE_RESEARCH_POOL_ID")
```

**Shell 示例**：

```bash
# 在 shell 命令前 source
set -a
source .env.local
set +a
curl -H "Authorization: Bearer $NOTION_API_KEY" ...
```

---

## 7. 工作流路由

```
用户需求
├── "我怎么用 Notion 改数据？"           → 看本文件 §3 §4 §5
├── "我怎么读 ENV 字段？"               → 看本文件 §6
├── "我建新 database 怕踩坑"           → 看本文件 §5.2（silent fail 防护）
├── "我想往调研池/选题库/脚本库加一行"    → 走 templates/notion-row-template.md
└── "Notion API 报错了"                → 看本文件 §5（端点 + color + language 白名单）
```

---

## 8. 维护规则

- **任何新加的 page/database** → **必须更新本文件**（不然下次找不到 ID）
- **任何 schema 变更**（新增 property、修改 select options）→ **必须更新 §3**
- **任何新踩的 Notion API 坑** → **必须更新 §5**

**这是 account-ops skill 的"数据契约"**——和 `SKILL.md` 一样重要。
