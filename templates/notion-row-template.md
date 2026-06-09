# Notion 3 个 Database 添加行模板

> **复制模板 → 改字段 → 通过 Notion API 添加**。用之前先看 `references/notion-schema.md`。

## 前置：加载 ENV

**每次操作前**先 source `.env.local`（本地）：

```bash
set -a
source .env.local
set +a
```

**验证 ENV 加载成功**：

```bash
echo "API key prefix: ${NOTION_API_KEY:0:8}"
echo "调研池 DB: $NOTION_DATABASE_RESEARCH_POOL_ID"
```

---

## 模板 1：往「📥 项目调研池」加一行

### 必填字段（4 个）

| 字段 | 值范例 | 模板 |
|---|---|---|
| `Name`（title） | `leonluo2008-ops/gardener-skill` | `<owner>/<repo>` 全名 |
| `GitHub URL` | `https://github.com/leonluo2008-ops/gardener-skill` | 完整 URL |
| `来源`（select） | `自己挖到` | 8 选 1 |
| `调研状态`（select） | `🔍待调研` | 5 选 1（默认 🔍待调研） |

### 推荐填（3 个）

| 字段 | 值范例 | 备注 |
|---|---|---|
| `分类`（multi_select） | `["Skill 框架", "AI Agent"]` | 9 选 N |
| `Stars`（number） | `12` | 整数 |
| `一句话理由`（rich_text） | "号称能从 AI 对话里挖出 skill 机会" | **30 秒写清楚** |

### 选填（看完再填）

| 字段 | 何时填 |
|---|---|
| `加进池子时间`（date） | 不填（**Notion 自动记录**） |

### 完整 JSON Payload

```json
{
  "parent": {"type": "data_source_id", "data_source_id": "REPLACE_WITH_NOTION_DATA_SOURCE_RESEARCH_POOL_ID"},
  "properties": {
    "Name": {"title": [{"text": {"content": "<项目名>"}}]},
    "GitHub URL": {"url": "<完整 URL>"},
    "来源": {"select": {"name": "<8 选 1>"}},
    "分类": {"multi_select": [{"name": "<标签 1>"}, {"name": "<标签 2>"}]},
    "Stars": {"number": <整数>},
    "调研状态": {"select": {"name": "🔍待调研"}},
    "加进池子时间": {"date": {"start": "<YYYY-MM-DD>"}},
    "一句话理由": {"rich_text": [{"text": {"content": "<一句话>"}}]}
  }
}
```

### 调 API 命令

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d @/tmp/row_research.json | python3 -c "import sys, json; d=json.load(sys.stdin); print('✅', d.get('id', d))"
```

### 来源字段的 8 个有效值

```
GitHub Trending
朋友推荐
Twitter/X
Reddit
Hacker News
Newsletter
自己挖到
其他
```

### 分类字段的 9 个有效值

```
AI Agent
Skill 框架
MCP 工具
LLM 框架
视频生成
图像生成
TTS/音频
开发工具
其他
```

### 调研状态的 5 个有效值

```
🔍待调研
📖读README中
🧪实测中
✅测完，可整合
❌不适合/放弃
```

---

## 模板 2：往「🎬 视频选题库」加一行

### 必填字段（5 个）

| 字段 | 值范例 | 模板 |
|---|---|---|
| `Name`（title） | "我试了 gardener-skill：号称能自动生成 skill，实际用下来..." | 选题 = 视频主题（含钩子） |
| `对应项目`（rich_text） | `leonluo2008-ops/gardener-skill` | 文本写 GitHub repo 名 |
| `GitHub URL` | `https://github.com/leonluo2008-ops/gardener-skill` | 完整 URL |
| `类型`（select） | `A 单项目实测` | 5 选 1 |
| `目标发布日期`（date） | `2026-06-12` | **逼自己拍日期** |

### 选填（5 个）

| 字段 | 何时填 |
|---|---|
| `状态`（select） | 默认 `💡待策划`（**不填**也默认这个） |
| `实际发布日期` | 发布后回填 |
| `B 站链接` | 发布后回填 |
| `YouTube 链接` | 发布后回填 |
| `数据笔记` | 发布后回填 |

### 完整 JSON Payload

```json
{
  "parent": {"type": "data_source_id", "data_source_id": "REPLACE_WITH_NOTION_DATA_SOURCE_TOPICS_ID"},
  "properties": {
    "Name": {"title": [{"text": {"content": "<选题>"}}]},
    "对应项目": {"rich_text": [{"text": {"content": "<repo 名>"}}]},
    "GitHub URL": {"url": "<完整 URL>"},
    "类型": {"select": {"name": "<5 选 1>"}},
    "状态": {"select": {"name": "💡待策划"}},
    "目标发布日期": {"date": {"start": "<YYYY-MM-DD>"}}
  }
}
```

### 调 API 命令

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d @/tmp/row_topic.json | python3 -c "import sys, json; d=json.load(sys.stdin); print('✅', d.get('id', d))"
```

### 类型字段的 5 个有效值

```
A 单项目实测
B 横向对比
C 整合到工作流
D 行业观察
自我介绍/起号
```

### 状态的 6 个有效值

```
💡待策划
📝写脚本中
🎙️待录制
✂️剪辑中
🚀已发布
⏸️暂停/取消
```

---

## 模板 3：往「📝 视频脚本库」加一行

### 必填字段（3 个）

| 字段 | 值范例 | 模板 |
|---|---|---|
| `Name`（title） | "我试了 gardener-skill：号称能自动生成 skill" | 视频标题 |
| `状态`（select） | `📝草稿` | 4 选 1（默认 📝草稿） |
| `视频类型`（select） | `A 单项目实测` | 5 选 1 |

### 推荐填（3 个）

| 字段 | 值范例 |
|---|---|
| `对应选题`（rich_text） | "选题库 D3 gardener-skill 那条" |
| `目标时长(分钟)`（number） | `12` |
| `实际时长(分钟)`（number） | `11`（发布后回填） |

### 选填（4 个）

| 字段 | 何时填 |
|---|---|
| `B 站链接` | 发布后回填 |
| `YouTube 链接` | 发布后回填 |
| `实际发布日期` | 发布后回填 |
| `脚本正文` | 整篇配音稿（一整段） |

### 完整 JSON Payload

```json
{
  "parent": {"type": "data_source_id", "data_source_id": "REPLACE_WITH_NOTION_DATA_SOURCE_SCRIPTS_ID"},
  "properties": {
    "Name": {"title": [{"text": {"content": "<视频标题>"}}]},
    "状态": {"select": {"name": "📝草稿"}},
    "视频类型": {"select": {"name": "<5 选 1>"}},
    "对应选题": {"rich_text": [{"text": {"content": "<选题库对应那条>"}}]},
    "目标时长(分钟)": {"number": <整数>},
    "脚本正文": {"rich_text": [{"text": {"content": "<整篇配音稿>"}}]}
  }
}
```

### 调 API 命令

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03" \
  -H "Content-Type: application/json" \
  -d @/tmp/row_script.json | python3 -c "import sys, json; d=json.load(sys.stdin); print('✅', d.get('id', d))"
```

### 状态的 4 个有效值

```
📝草稿
✅定稿待录
🎬已发布
⏸️搁置
```

### 视频类型字段的 5 个有效值

```
A 单项目实测
B 横向对比
C 整合到工作流
D 行业观察
自我介绍/起号
```

---

## 通用 SOP：3 步加一行

1. **复制对应模板的 JSON 骨架**到 `/tmp/row_*.json`
2. **改占位符**（`<...>` 全部替换成实际值）
3. **跑上面的 curl 命令** → 看返回的 `id`

**10 秒内能加一行**——这就是你日常往 Notion 填数据的方式。

---

## 反模式

- ❌ **每次手写完整 JSON**（**复制模板改占位符**）
- ❌ **忘了 source .env.local**（API key 是空 = 401）
- ❌ **不验证 select 选项名**（拼错就 400，**用本文件列的有效值**）
- ❌ **不查 ENV 是否加载成功**（跑完发现 API key 是空 = 白干）
- ❌ **直接 push .env.local 到 git**（**违反核心安全原则**）
