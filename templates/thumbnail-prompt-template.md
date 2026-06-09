# 封面提示词模板

> **3:4 竖版（1080×1440）**，B 站/抖音/视频号通用。**5 秒测试**：手机小图能不能看清标题，看不清就改。

## 通用规则

| 维度 | 要求 |
|---|---|
| 画幅 | 3:4 竖版，1080×1440 |
| 风格 | 扁平 + 极简 + 工程师气质（**不卡通、不花哨**） |
| 重点 | 标题文字 > 视觉元素 |
| 字体 | 粗体无衬线（**思源黑体 / Inter / 阿里巴巴普惠体**） |
| 颜色 | 1 主色 + 1 强调色 + 1 背景色（**不要 3 个以上**） |
| 反模式 | ❌ 标题党、❌ 黄色感叹号、❌ 红色 "震惊"、❌ emoji 大杂烩 |

## 3 版风格（按视频类型选）

### A 版（推荐）—— 扁平 + 极简

**适用**：A 类（单项目实测）、B 类（横向对比）

```
3:4 vertical, 1080x1440, minimalist flat design
Background: deep dark navy (#0F172A) with subtle gradient
Main element: a single GITHUB OCTOCAT LOGO rendered in flat white outline, top-center
Title text (bold sans-serif, white, 2 lines, left-aligned):
  Line 1: "[副标题]" (32pt)
  Line 2: "[主标题]" (40pt, accent color #22D3EE cyan)
Subtitle (small, gray):
  "[类型标签] · 第 N 期"
Bottom: a small terminal/CLI mockup showing command output
Style: tech clean, professional, NO emoji, NO cartoon
Mood: serious, expert, no-nonsense
```

**占位符**：
- `[副标题]`：一行简短副标题（如"我试了 XXX"）
- `[主标题]`：一行钩子标题（如"GitHub 上最火的 AI 神器"）
- `[类型标签]`：A/B/C/D
- `[第 N 期]`：期数

### B 版 —— 大字 + 深色背景

**适用**：D 类（行业观察）、观点类视频

```
3:4 vertical thumbnail
Background: solid #0F172A
Foreground: a single block of text in bold sans-serif
  "[主标题第一行]"
  "[主标题第二行]"
  "[主标题第三行]"
Style: BIG text dominant, minimal decoration, white text on dark
Accent: small GitHub Octocat icon in corner
Vibe: punchy, direct, engineer talking to engineer
```

**占位符**：
- `[主标题第一/二/三行]`：3 行短句，**每行不超过 8 个字**

### C 版 —— 人脸 + 文字

**适用**：D1 自我介绍、Q&A、阶段性总结（**仅重要视频用**）

```
3:4 vertical
Background: deep gradient dark navy → black
Subject: half-body shot of a Chinese male engineer, late 20s/early 30s,
  wearing plain black T-shirt, slight smile, looking directly at camera
Lighting: clean key light from front-left, no harsh shadows
Title text overlay (right side, white bold sans-serif):
  Line 1: "[主标题]" (48pt)
  Line 2: "[副标题]" (36pt cyan)
Bottom-right: small text "[类型标签] · 第 N 期"
Style: documentary portrait, NOT posed, natural expression
```

**占位符**：
- `[主标题]`：1 行主标题
- `[副标题]`：1 行副标题
- `[类型标签]`：A/B/C/D

## 用前 5 秒测试

1. **导出 1080×1440 PNG**
2. **导入手机相册**
3. **小图模式**（iPhone 桌面大小）能不能看清标题？
4. **看不清 = 字体太小** → 调大
5. **太挤 = 信息太多** → 删一行

## 工具推荐

| 工具 | 用途 | 推荐度 |
|---|---|---|
| **即梦** | A/B 版提示词生成 | ⭐⭐⭐⭐⭐ |
| **Midjourney** | C 版人像 | ⭐⭐⭐⭐ |
| **ComfyUI** | 极致定制 | ⭐⭐⭐（学习成本高） |
| **Canva** | 提示词 + 模板混合 | ⭐⭐⭐⭐ |
| **Figma** | 后期排版调整 | ⭐⭐⭐⭐⭐ |

## 反模式

- ❌ **3 行以上文字**（**小图看不清**）
- ❌ **多色搭配**（**视觉杂乱**）
- ❌ **emoji 替代文字**（**不专业**）
- ❌ **标题用"！"**（**跟你人设冲突**）
- ❌ **真人人脸每次都用**（**C 版偶尔用**）
- ❌ **2K/4K 导出**（**B 站/抖音用不到 4K**）
