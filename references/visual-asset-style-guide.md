# 视觉资产风格对齐 SOP

> **核心问题**：账号的视觉资产（头像 / 横幅 / 封面 / 视频片头）**必须风格统一**——**用户实战纠错原话**：「**我感觉你给的横幅背景风格和头像不匹配，请根据头像，重新设计**」。
>
> **关键洞察**：**头像 = 频道的"主调"**。所有其他视觉资产（横幅/封面/视频片头）都必须**对齐头像风格**，**否则频道看起来像 2 个不同的账号**。
>
> **关联铁律**：SKILL.md 铁律 20（v1.6.1 增）

---

## 一、3 步走：盘点 → 识别主调 → 对齐写新提示词

### Step 1：盘点账号已有视觉资产

**问自己**：
- 头像用的是什么风格？（3D / 矢量 / 插画 / 真人）
- 头像主色调是什么？（黑白蓝 / 暖色 / 冷色 / 多彩）
- 头像的几何感如何？（圆润 / 棱角 / 抽象 / 具体）

**正例**（B 站「二大爷和铁柱」频道）：
- 头像 = C 版极简 Logo
- 风格 = 极简矢量
- 色调 = 黑白蓝
- 几何感 = 圆润（狗脸圆润）
- 元素 = 电路板 + LCD 眼睛 + X_X 翻车眼

### Step 2：识别主调（写下来当 reference）

| 维度 | 频道主调值 |
|---|---|
| **风格** | 极简矢量 |
| **色调** | 黑白蓝（**不用暖色**） |
| **笔触** | 干净几何（**不用 3D 不用写实**） |
| **几何感** | 圆润 + 卡通 |
| **核心元素** | 电路板 + LCD + 翻车表情 |
| **氛围** | 反差萌（萌宠 + 翻车暗示） |

### Step 3：写新视觉提示词时**显式加风格约束**

**在提示词末尾加一段**（**这是关键**）：

```
XXX，匹配账号 logo 美学（极简矢量 / 黑白蓝 / 干净几何 / 圆润卡通），
不要 3D、不要渐变、不要写实风格
```

**中文版**（中文平台用）：
```
XXX，纯矢量平面极简风，匹配账号 logo 极简美学，
不要渐变、不要 3D、不要写实风格
```

---

## 二、实战案例：横幅与头像风格对齐

### ❌ **错误示范**（v1.0 / v1.6 之前的我）

头像：C 版极简矢量 · 黑白蓝 · 圆润卡通
横幅：3D 渲染 + 暖色渐变 + 主人剪影 + 机器人宠物 + Pixar 风

**问题**：
- 风格不匹配（**矢量 vs 3D**）
- 色调不匹配（**黑白蓝 vs 暖色渐变**）
- 笔触不匹配（**干净几何 vs 写实渲染**）
- 元素不延续（**头像的电路板 / LCD 眼睛 / 翻车表情 → 横幅里完全没有**）

**用户原话**：「**我感觉你给的横幅背景风格和头像不匹配**」

### ✅ **正确示范**（v3 已落地）

头像：C 版极简矢量 · 黑白蓝 · 圆润卡通
横幅：
- **同款配色**：纯黑底（#000000）+ 白文字 + 蓝色 LED 圆点
- **同款元素**：电路板走线 + LCD 眼睛 + X_X 翻车表情
- **同款风格**：纯矢量平面极简风
- **同款笔触**：干净几何

**效果**：
- 频道视觉风格**完全统一**
- 用户进主页看到头像 → 看到横幅 → "**这是同一个频道**"
- 视觉资产**协同强化品牌**

---

## 三、风格对齐的 4 个核心检查项

写新视觉提示词时**逐项自检**：

### ✅ 1. 配色检查
- ❌ 暖色（橙 / 黄 / 红）vs 频道冷色调
- ✅ 复用频道已有配色（**精确指定 HEX 值**：`#000000` / `#FFFFFF` / `#0066CC`）

### ✅ 2. 笔触检查
- ❌ 3D 渲染 / 写实风 / 摄影 / 油画
- ✅ 矢量平面 / 极简 / 几何 / 卡通
- 在提示词里**显式写**："no 3D, no gradient, no realistic, no photographic"

### ✅ 3. 元素检查
- ❌ 头像里有电路板 + LCD 眼睛 → 横幅里完全没有
- ✅ 复用核心元素（**电路板走线 / LCD 眼睛 / 翻车表情**）作为装饰图案
- **至少 2-3 个核心元素**在新视觉里出现

### ✅ 4. 几何检查
- ❌ 头像圆润卡通 → 横幅用了棱角分明的几何
- ✅ 同一几何感（**全圆润 / 全棱角 / 风格统一**）

---

## 四、提示词模板（**4 步必含**）

写任何视觉资产提示词时（**头像 / 横幅 / 封面 / 视频片头**），**必须包含这 4 步**：

```
1. 【主体描述】：画什么（人物 / 场景 / 元素）
2. 【风格约束】：匹配频道主调
3. 【配色约束】：精确指定 HEX 值
4. 【反词约束】：no gradient / no 3D / no realistic
```

**完整模板**：

```text
【主体】+ 【场景】
+ "matching the channel's logo aesthetic"（匹配频道 logo 美学）
+ "in [主调风格] style"（极简矢量 / 平面卡通 / 写实摄影 / 等）
+ "color scheme: #000000, #FFFFFF, #0066CC"（精确配色）
+ "no gradient, no 3D, no realistic"（反词）
+ "1024x1024 / 1440x224 / 2560x1440"（精确尺寸）
```

**正例**（v3 已落地）：
```text
Minimalist wide banner for YouTube channel, brand showcase for "二大爷和铁柱" account, left side features bold white Chinese text "二大爷和铁柱" with subtitle "实测 · 翻车 · 干活" in smaller sans-serif below, right side has abstract circuit board pattern (thin white lines on black background with small blue LED dots scattered as decorative nodes, like a schematic diagram), two small LCD screen elements with "X_X" expression icons scattered as visual rhythm, pure black background (#000000), no gradients, no 3D, no realistic rendering, pure vector flat style matching minimalist logo aesthetic, clean geometric composition, 1440x224
```

**反例**（v1.0 已废弃）：
```text
Wide banner image, "Erdaye & His AI Pet" brand showcase, left side shows Erdaye's silhouette (a middle-aged man with hands behind back), right side shows TieZhu the AI pet (a small cute robot), middle space has the text "Erdaye & His AI Pet" with subtitle "AI Workflow · 4D Control · Real Fails", warm gradient background (deep blue to soft orange, like dawn), soft glow effects, 2560x1440, professional banner design
```

**反例的问题**：
1. **warm gradient** → 暖色渐变（**和头像冷色调冲突**）
2. **soft glow effects** → 柔光（**和头像干净几何冲突**）
3. **AI Workflow · 4D Control · Real Fails** → 英文术语（**和铁律 21 冲突**）
4. **没有风格匹配**约束（**和铁律 20 冲突**）

---

## 五、自检 4 问（**写完视觉提示词必过**）

1. **配色对齐吗？**（**精确写 HEX 值**）
2. **笔触对齐吗？**（**显式写 "no 3D / no gradient"**）
3. **元素延续吗？**（**至少 2-3 个频道核心元素**）
4. **观众 3 秒认得出"这是同一频道"吗？**（**整体感**）

**任何 1 个 NO** → 重写。

---

## 六、什么时候触发这个 SOP

**触发场景**（**一踩即中**）：
- ✅ 写新横幅提示词
- ✅ 写封面提示词
- ✅ 写视频片头 / 片尾提示词
- ✅ 写其他视觉元素提示词（表情包 / 直播封面 / 视频水印 等）
- ✅ 设计 4 平台不同尺寸的视觉资产

**每次必跑**：
- 写提示词前 → **盘点已有视觉资产**
- 写提示词中 → **加风格约束 + 配色 HEX + 反词**
- 写完提示词后 → **过自检 4 问**

---

## 七、关联资源

- **铁律 20**（v1.6.1 增）→ `SKILL.md §关键原则` 第 20 条
- **铁律 21**（v1.6.1 增）→ 关于生图提示词全中文
- **频道主调**（v3 资料包）→ `Notion 父页面/🏠 账号资料 v3/四、头像 + 五、背景图`
- **C 版头像提示词**（v3 已落地）→ `templates/visual-asset-prompt-template.md`（待创建）
