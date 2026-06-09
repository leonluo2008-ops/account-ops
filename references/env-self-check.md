# 本地 ENV 自检 SOP

> **每次新建/修改 `.env.local` 后必跑**。今天（2026-06-09）踩过的坑：手抄 key 多了 1 个字符（51 vs 50），导致整个 Notion API 自检全部 401。

## 1. 2 步快速自检

```bash
# === 步骤 1：source + 验证 key 长度 ===
cd <skill-dir>
set -a && source .env.local && set +a
echo "len: ${#NOTION_API_KEY}, prefix: ${NOTION_API_KEY:0:8}"
# 期望：prefix = "ntn_..."，len 在合理范围（Notion 是 50）

# === 步骤 2：实际调一次 API 验证 ===
curl -s -w "\nHTTP: %{http_code}\n" "https://api.notion.com/v1/users/me" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2025-09-03"
```

**判定**：

| HTTP | 含义 | 下一步 |
|---|---|---|
| 200 + bot 信息 | ✅ 一切正常 | 开始干活 |
| 401 unauthorized | ❌ key 错了 | 走 §2 md5 对比 |
| 404 not_found | ⚠️ 资源 ID 错了 | 检查 `NOTION_*_ID` 系列变量 |

## 2. 当出现 401：用 md5 定位字符级差异

**根因**：手抄 key 容易**多打/少打字符**（典型：复制时多带了换行/空格/尾部 F）。

**双源 md5 对比**（找到哪个文件里的 key 是真值）：

```bash
KEY_LOCAL=$(grep "^NOTION_API_KEY" .env.local | cut -d'"' -f2)
KEY_OTHER=$(grep "^NOTION_API_KEY" ~/.hermes/profiles/huiben/.env | cut -d'=' -f2- | tr -d '"' | tr -d "'")

HASH_A=$(echo -n "$KEY_LOCAL" | md5sum | cut -c1-12)
HASH_B=$(echo -n "$KEY_OTHER" | md5sum | cut -c1-12)

echo ".env.local  key: ${KEY_LOCAL:0:8}...${KEY_LOCAL: -4} (len=${#KEY_LOCAL}) md5=$HASH_A"
echo "system .env key: ${KEY_OTHER:0:8}...${KEY_OTHER: -4} (len=${#KEY_OTHER}) md5=$HASH_B"

if [ "$HASH_A" = "$HASH_B" ]; then
  echo "✅ md5 一致 = 物理一致"
else
  echo "❌ md5 不一致 = 其中一个是错的"
  echo "   → 用可信源（system .env）覆盖 .env.local："
  echo "     cp ~/.hermes/profiles/huiben/.env .env.local"
  echo "   → 或者重新申请一个 Notion integration key"
fi
```

## 3. ENV 字段完整清单（v1.0.1 校正后）

**9 个字段**——必须全在 `.env.local` 里：

| 字段 | 用途 | 数量 |
|---|---|---|
| `NOTION_API_KEY` | Bearer Token | 1 |
| `NOTION_PARENT_PAGE_ID` | 父页面 | 1 |
| `NOTION_DATABASE_*_ID` | 3 个 database | 3 |
| `NOTION_DATA_SOURCE_*_ID` | 3 个 data source | 3 |
| **合计** | | **9** |

**自检命令**（数 ENV 字段数）：

```bash
set -a && source .env.local && set +a
# 数 NOTION_ 开头的变量
env | grep -c "^NOTION_"  # 期望：9
```

## 4. Notion 数据库自检 SOP

**每次建完/修改 database 后**必跑——验证 property 数 + row 数对得上：

```python
import os, subprocess, json
api_key = os.environ["NOTION_API_KEY"]

databases = {
    "📥 调研池": (os.environ["NOTION_DATA_SOURCE_RESEARCH_POOL_ID"], 8, 1),
    "🎬 选题库": (os.environ["NOTION_DATA_SOURCE_TOPICS_ID"], 10, 2),
    "📝 脚本库": (os.environ["NOTION_DATA_SOURCE_SCRIPTS_ID"], 10, 2),
}

for name, (dsid, exp_p, exp_r) in databases.items():
    p = subprocess.run(["curl", "-s", f"https://api.notion.com/v1/data_sources/{dsid}",
         "-H", f"Authorization: Bearer {api_key}",
         "-H", "Notion-Version: 2025-09-03"],
        capture_output=True, text=True, timeout=10)
    pcount = len(json.loads(p.stdout).get("properties", {}))
    
    r = subprocess.run(["curl", "-s", "-X", "POST", f"https://api.notion.com/v1/data_sources/{dsid}/query",
         "-H", f"Authorization: Bearer {api_key}",
         "-H", "Notion-Version: 2025-09-03",
         "-H", "Content-Type: application/json",
         "-d", '{"page_size":100}'],
        capture_output=True, text=True, timeout=10)
    rcount = len(json.loads(r.stdout).get("results", []))
    
    p_ok = "✅" if pcount == exp_p else "❌"
    r_ok = "✅" if rcount == exp_r else "❌"
    print(f"{p_ok} {r_ok} {name}: {pcount} props (期望 {exp_p}), {rcount} rows (期望 {exp_r})")
```

**判定**：
- 全 ✅ → 工作系统完整
- ❌ properties → 看 `references/notion-schema.md §5.2` 走 silent fail 防护 SOP
- ❌ rows → 检查是不是 API key 失效（401）/ schema 不匹配

## 5. ENV 安全 checklist

每次操作完**必检**：

- [ ] `.env.local` 已 `chmod 600`（**不是**默认 644）
- [ ] `.env.local` 已在 `.gitignore`（`*.local` 或 `.env.local` 规则）
- [ ] `.env.local.example` 是**模板**（值是 `your_xxx_here` 占位符，**不是真 key**）
- [ ] 远端仓的 `.env.local.example` 用 `gh api .../contents/.env.local.example` 验过（确认是模板）
- [ ] 远端仓**没有** `.env.local`（用 `gh api .../contents/.env.local` 验过，期望 404）
- [ ] `.env.local` 里**绝不**出现 `ntn_xxx` 真实值 出现在 `SKILL.md` / `README.md` / `references/` / `templates/` 任何 md 文件里

## 6. 复盘：今天踩的坑（v1.0.1 之前）

**坑 1**：`POST /v1/databases` 建库时 properties 全部 silent fail，只存了 1 个 Name。
- 防护：建完 GET property 数对不上 → 立刻 PATCH `/v1/data_sources/{id}` 补全（**不是** `/v1/databases/{id}`，那个也 silent fail）。
- 详见 `references/notion-schema.md §5.2`

**坑 2**：`color: "cyan"` → 400 validation_error。
- 防护：color 只用 9 个白名单值（`default/gray/brown/orange/yellow/green/blue/purple/pink/red`）。
- 详见 `references/notion-schema.md §5.3`

**坑 3**：`code block language: "text"` → 400 validation_error。
- 防护：language 用枚举值（`"plain text"` 带空格、`markdown`、`bash`、`python` 等）。
- 详见 `references/notion-schema.md §5.4`

**坑 4**（**今天最阴险**）：`.env.local` 里的 key 多打 1 个字符（51 vs 50），导致所有 API 调用 401。
- 防护：**本文件** + `SKILL.md` 的 ENV 自检 SOP 速查
- 根因：手抄 key 时尾部多了 1 个 F
- 修复：md5 对比定位 → 从可信源覆盖

**坑 5**（**v1.2 新增 · 2026-06-09**）：`.env.local` 里的 key **被双引号包裹**（`NOTION_API_KEY="ntn_..."`），导致 Python `os.environ.get()` 拿到 52 字符字符串（50 + 2 个引号），调 Notion API 返 401。
- 防护：**Python 解析时 strip 双引号/单引号**（参考 `notion-schema.md §6.4`）
- 根因：dotenv 编辑器/插件自动加引号；复制粘贴自带引号；某些 IDE 模板默认带引号
- 修复：手动去引号 或 Python 自动 strip
- **判定技巧**：拿到 key 后**先 print 长度**——**50 字符 + 不是 52/53** = 正常；如果 52/53 几乎一定是引号问题

**坑 5 速查脚本**（3 秒定位）：

```python
from pathlib import Path

env_path = Path(".env.local")
with open(env_path) as f:
    for line in f:
        if line.startswith("NOTION_API_KEY"):
            v = line.split("=", 1)[1].strip()
            v_clean = v.strip('"').strip("'")
            print(f"原值长度: {len(v)}, 去引号后: {len(v_clean)}")
            if len(v) != len(v_clean):
                print("⚠️ 检测到引号包裹，需 strip")
            else:
                print("✅ 无引号问题")
            break
```
