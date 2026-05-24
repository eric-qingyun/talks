# 天选事业分享会演讲网页 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 实现一份单文件 HTML（`/Users/apple/talks/destined-career/index.html`），用于明天（2026-05-25）20-30 分钟现场宣讲 + 会后阅读。

**Architecture:** 单文件、零依赖、零构建。所有 HTML/CSS/JS 内联在 `index.html` 里。KaTeX 0.16.x 的 CSS/JS 也内嵌进文件，确保断网可用。两种模式（阅读 / 演讲者）通过 body 上的 `.presenter` class 切换，CSS 用 scroll-snap-y 实现"翻页"，JS 处理键盘导航和点击交互。

**Tech Stack:** HTML5 + 原生 CSS（CSS variables + scroll-snap + IntersectionObserver）+ 原生 JS（无 jQuery / 无框架）+ KaTeX 0.16.x（内嵌）

**Spec:** `/Users/apple/talks/destined-career/docs/superpowers/specs/2026-05-24-destined-career-talk-design.md`

**Spec §9 默认值（已确认）：**
- 盖洛普才干数：(c) 保持现状（开头文案 34，列出 33）
- 增强回路图：(a) 横向箭头链路图
- 关键词碰撞：连线图

---

## File Structure

```
/Users/apple/talks/destined-career/
├── index.html              # 唯一交付物，所有代码内联
├── outline.md              # 大纲源文件（Task 0 创建，便于实现时引用复制粘贴）
├── docs/superpowers/
│   ├── specs/2026-05-24-destined-career-talk-design.md   (已存在)
│   └── plans/2026-05-24-destined-career-talk.md          (本文件)
└── .gitignore              (已存在，含 .superpowers/)
```

**只有 `index.html` 一份代码文件**。所有 CSS 在 `<style>` 块里，所有 JS 在 `<script>` 块里。

---

## Task 0: 保存大纲源文件 + 确认空 git 仓库就绪

**Files:**
- Create: `/Users/apple/talks/destined-career/outline.md`

- [ ] **Step 1:** 把用户提供的大纲完整保存到 `outline.md`，便于后续任务引用粘贴

写入以下内容（这是用户给的原始大纲，保留原文，仅做轻微的 markdown 标题/列表格式化以便查阅）：

```markdown
# 命题：如何快速寻找到自己的天选事业（探索天选事业方向的通用技巧分享）

## 一、定义与推导：拆解"天选事业"的底层代码

**叠个甲：** 本套路不是完美公式，有用就行。

### 1. 传统的思维模型：刺猬理论

**出处：** 吉姆·柯林斯（Jim Collins）管理学名著《从优秀到卓越》。

**核心：** "狐狸知道很多事情，但刺猬知道一件大事。"

**传统公式：**

$$天选事业 = 热爱 \cap 擅长 \cap 商业价值$$

❌ **常见卡点：** 这个模型太静态、太孤立了。就像相亲时硬性要求对方必须满足"生理性喜欢（热爱） + 灵魂伴侣（擅长） + 还要贼有钱（商业）"，结果就是直接卡死，寸步难行。

### 2. 用心理学降维重构：解构"找不到的热爱"

**引入理论：** 自我决定论（Self-Determination Theory）

**核心公式：**

$$热爱 = 自主感 + 胜任感 + 归属感$$

- **自主感：** 我自己想去做，而不是被谁指使（等同于兴趣）。
- **胜任感：** 事情不断做成，让我很有成就感（正向反哺擅长）。
- **归属感：** 在群体中被需要、被认可（正向催化商业价值）。

### 3. 用认知科学重构：解构"什么是擅长"

**引入理论：** 盖洛普优势方程

**核心公式：**

$$天赋 \times 投入 = 优势（擅长）$$

- **天赋：** 大脑天生输入和输出信息的"硬件习惯"（不费力）。
- **投入：** 兴趣（自主感）会疯狂源源不断地为"投入时间"进行电量输送。

### 📌 本章核心结论：动态递进模型

热爱、擅长与商业不是孤立的，而是一个顺理成章的增强回路：

💡 **诞生路径：** 因为好玩试了一事（兴趣） → 发现由于底层硬件契合，没费太大力气（天赋） → 做得还不错并拿到成果（胜任） → 得到别人的赞赏与认可（归属） → 更想做了，投入更多时间变得极度精通（擅长） → 帮更多人解决问题并拿到付费（商业变现）。

**最终得到我们的演化核心模型：**

$$天选事业 = 天赋 \times 兴趣 \times 变现$$

## 二、相关技巧：寻找你的三大拼图

### 🗺️ 拼图一：天赋（认领你的底层硬件与战术 App）

#### 1. 底层系统：荣格八维（OS 操作系统）

**官方测试链接：** Sakinorva 荣格八维测试

**本质：** 大脑功耗最低、发热最小的底层信息处理算法。

##### 🧭 输入端 - 感知功能（你的大脑如何抓取数据）：

- **Se（外倾感觉）：**
  - 一句话讲透本质：脑子里没有多余杂念，100%沉浸在此时此地的物理世界。
  - 【一秒钟识别】窗外掉下一片树叶，他是班里第一个注意到的人。
- **Si（内倾感觉）：**
  - 一句话讲透本质：极其尊重过去成功的经验和规矩，新事物必须跟大脑硬盘里的"历史模版"对齐。
  - 【一秒钟识别】错题本战神，写步骤排版错了一点心里就极度难受。
- **Ne（外倾直觉）：**
  - 一句话讲透本质：脑子像断了线的风筝，看到一个点瞬间跨界联想到一万个可能性。
  - 【一秒钟识别】听到"牛顿"能一秒联想到苹果、乔布斯、手机游戏，然后在台下傻笑。
- **Ni（内倾直觉）：**
  - 一句话讲透本质：大脑后台自动给海量乱象"脱水"，过滤噪音，直接吐出指向未来的终极答案。
  - 【一秒钟识别】盯物理大题三秒直接报出答案，说不清怎么推导的，但答案就是对的。

##### ⚙️ 输出端 - 判断功能（数据采集完，你以什么标准输出行动）：

- **Te（外倾思考）：**
  - 一句话讲透本质：冷酷、理智、只看客观指标和 KPI。怎么效率高、怎么拿第一就怎么来。
  - 【一秒钟识别】大扫除一秒把全班当工具人分配："你擦窗、你扫地，5分钟搞定！"
- **Ti（内倾思考）：**
  - 一句话讲透本质：不在乎奖金和第一名，只要逻辑有一丝丝不通透、不自洽，就死磕到底去 Debug。
  - 【一秒钟识别】拿到万能公式不满足，课后花两小时自己手推证明一遍底层原理，否则睡不着。
- **Fe（外倾情感）：**
  - 一句话讲透本质：拥有恐怖的情感触角，做决定唯一标准是"大家高不高兴，集体的面子和不和谐"。
  - 【一秒钟识别】班级外交官，两同学吵架，他进去开个玩笑说几句贴心话瞬间破冰。
- **Fi（内倾情感）：**
  - 一句话讲透本质：极度注重内心的真实与初心，不符合内心道德法庭的事，给多少钱老子也不干。
  - 【一秒钟识别】拒绝随大流，"我的心不答应，天王老子来我也不去"。

##### 🎭 全景互动名场面：女朋友微信发来"我感冒了，好难受"

- **Se 抓取：** 看到字面事实，感冒=呼吸道感染，时间是晚上10:15。
- **Si 抓取：** 调取病史档案，她上个月也是这时感冒，连烧三天，吃蓝色药管用。
- **Ne 抓取：** 脑洞连续剧，下周一她还要汇报，请假怎么办？项目会不会黄？现在要不要抢号？
- **Ni 抓取：** 核心动机脱水，她不是在汇报病情，她是在表达脆弱、要偏爱和陪伴。
- **Te 决策：** "多喝热水，吃一粒对乙酰氨基酚，明天还烧去挂呼吸科。"（送去体温计和药，高效但冷酷）
- **Ti 决策：** "流鼻涕还是干咳？测体温没？先回忆下潜伏期接触了谁，我分析下是甲流还是风寒，对症下药。"
- **Fe 决策：** "心疼坏了宝贝！抱抱！帮你点了热粥半小时到，我跟手头开会的说一声马上过去陪你！"
- **Fi 决策：** 陷入深沉的感同身受："看到你难受，我心里特别堵。我想实实在在地陪着你，我现在推了社交过去照顾你。"（拒绝任何渣男套路情话，绝对真诚）

#### 2. 顶层应用：盖洛普克利夫顿优势（上层高级 Apps）

**官方测试链接：** 盖洛普优势识别器 2.0 官方中文页面

**本质：** 你的硬件系统在真实世界里死磕后，磨炼出来的 34 个高频、好用的战术武器。

**一、执行力** · 解决"如何把事做成"
- 成就（能量电池）
- 统筹（拼图大师）
- 信仰（价值观死士）
- 公平（SOP 制定者）
- 审慎（排雷专家）
- 纪律（自律时钟）
- 专注（制导导弹）
- 责任（言出必行）
- 排难（起死回生 Debug）

**二、影响力** · 解决"如何让别人听你的"
- 行动（破局催化剂）
- 统率（带头冲锋）
- 沟通（金句捕手）
- 竞争（胜负欲极强）
- 追求（无情雕琢极致）
- 自信（孤勇前行）
- 取悦（社交破冰船）
- 体谅（情绪安抚接收器）

**三、关系建立** · 解决"如何让大家一起走"
- 适应（随遇而安）
- 关联（宏大格局）
- 发展（灵魂伯乐）
- 和谐（最大公约数）
- 包容（不让人落单）
- 个别（定制化识人）
- 积极（快乐源泉）
- 交往（深度知己）

**四、战略思维** · 解决"如何看清前方方向"
- 分析（数据实证）
- 前瞻（画未来蓝图）
- 理念（概念连连看）
- 搜集（移动智库囤积狂）
- 思维（大脑心流漫游）
- 学习（迷恋从不懂到懂）
- 历史（查清前因后果）
- 战略（导航过滤捷径）

### 🧩 拼图二：兴趣（稀缺物盘点 + 5Why 追问）

**判定标准：** 找兴趣就跟谈恋爱一样，看你有没有心甘情愿给 Ta 你认为最稀缺的资源（时间/金钱/情绪/灵感）。

#### 🛠️ 实操技术 1：稀缺物追踪

- **时间流向：** 刷得停不下来的抖音/小红书标签、深夜还在看的书籍类别。
- **金钱流向：** 账单里除了维持生存外，你愿意溢价付费的类目。
- **情绪心流：** 让你忘记时间、极度亢奋或治愈的时刻。
- **灵感捕捉：** 你的 flomo/备忘录里记录最多的奇思妙想。

#### 🛠️ 实操技术 2：5Why 深度剥离（以购买"一次性袜子"为例）

- **1Why：** 为什么要买？→ 随用随丢不用洗，减少行李占用。
- **2Why：** 为什么不想洗和占用行李？→ 讨厌洗晾晒的管理和时间消耗，想让出行更轻松。
- **3Why：** 为什么讨厌晾晒管理？→ 想减少生活中琐事的时间投入。
- **4Why：** 为什么不愿投入琐事？→ 认为把精力和时间花在机械重复劳动上是对生命的浪费。
- **5Why：** 为什么觉得是浪费？→ 核心本质：我需要极高的"掌控感"和"自由度"，我更喜欢"创造"和"高效率"，而不是"维持现状"。

#### 🛠️ 实操技术 3：关键词提取与交叉碰撞

**提取赛道关键词：**
- 浅层：极简生活、空间收纳。
- 底层：流程自动化与效率工具（消灭一切机械重复）、个人精力管理。

**进行异质碰撞（连连看）：**

💡 比如：将你的兴趣清单（效率工具、精力管理、神经科学、空间美学、行为心理学……）进行交叉：

【神经科学】 × 【教育学】 × 【效率工具】 = 开发一套能让普通人根据大脑科学高效学习的个人 Skill Tree 自动化系统。

### 💰 拼图三：销售与变现（个人精益创业 MVP 三问）

**核心逻辑：** 天赋如果不封装成产品交付给别人，就只是一堆写在纸上的测试报告。

- **Who（谁有痛点）：** 谁会为了你擅长的这件事感到痛苦？（例：那些每天做数据报表、被低效机械重复折磨到深夜 12 点的打工人）
- **What（你的产品）：** 你能提供一个什么具体的"最小好东西"帮他解决？（例：别去想开公司。一份消灭重复劳动的【自动化数据看板 Excel 模版】、一次 1 对 1 的效率陪跑咨询）
- **How（怎么让他知道）：** 你通过什么渠道把这个好东西推到他眼前？（例：朋友圈现身说法、小红书垂直内容输出、熟人社群介绍背书）
```

- [ ] **Step 2:** 确认仓库已 git init 且 spec 已提交

```bash
cd /Users/apple/talks/destined-career && git log --oneline
```

Expected: 至少有一条 commit（spec 那条 `spec: 天选事业分享会演讲网页设计`）

- [ ] **Step 3:** Commit outline

```bash
cd /Users/apple/talks/destined-career && git add outline.md && git commit -m "docs: 保存分享会大纲源文件"
```

---

## Task 1: 骨架 + 设计系统（CSS tokens / 基础排版 / 8 个空 section）

**Files:**
- Create: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 创建 index.html 骨架，包含所有 CSS tokens、基础排版规则、8 个空 `<section>` 占位

写入以下内容（这是最终文件的"地基"，后续任务都往这上面长肉）：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>如何快速寻找到自己的天选事业</title>
<style>
  /* === Design Tokens === */
  :root {
    --c-blue: #1F3D7A;
    --c-blue-text: #1A2238;
    --c-bg: #FBF9F4;
    --c-red: #C1352F;
    --c-red-bg: #F0D8D6;
    --c-gray: #8A8A8A;
    --c-gray-line: #C8C8C8;
    --c-gray-border: #D4D4D4;
    --c-callout-bg: #EFEBE2;
    --c-white: #FFFFFF;

    --font-cn: -apple-system, BlinkMacSystemFont, "PingFang SC", "Microsoft YaHei", sans-serif;
    --font-en: -apple-system, "SF Pro Text", "Inter", sans-serif;
    --font-mono: "SF Mono", "JetBrains Mono", Consolas, monospace;

    --content-max: 920px;
    --section-gap: 96px;
  }

  /* === Reset & Base === */
  * { box-sizing: border-box; }
  html, body { margin: 0; padding: 0; }
  body {
    font-family: var(--font-cn);
    background: var(--c-bg);
    color: var(--c-blue-text);
    font-size: 16px;
    line-height: 1.75;
    -webkit-font-smoothing: antialiased;
    text-rendering: optimizeLegibility;
  }

  h1, h2, h3, h4 { color: var(--c-blue); font-weight: 700; line-height: 1.3; margin: 0; }
  h1 { font-size: 52px; }
  h2 { font-size: 32px; }
  h3 { font-size: 24px; }
  h4 { font-size: 20px; line-height: 1.5; }
  p { margin: 0; }
  a { color: var(--c-blue); }
  ul, ol { padding-left: 1.5em; margin: 0; }

  /* === Section 容器 === */
  section {
    max-width: var(--content-max);
    margin: 0 auto;
    padding: var(--section-gap) 32px;
    scroll-snap-align: start;
  }
  .section-label {
    display: inline-block;
    font-family: var(--font-en);
    font-size: 13px;
    letter-spacing: 0.1em;
    color: var(--c-blue);
    border: 1px solid var(--c-blue);
    border-radius: 3px;
    padding: 2px 10px;
    margin-bottom: 16px;
    font-weight: 600;
  }
  .section-title { margin-bottom: 8px; }
  .section-subtitle { color: var(--c-gray); font-size: 18px; line-height: 1.6; margin-bottom: 32px; }

  /* === 入场动画基础（具体激活在 Task 8） === */
  section { opacity: 1; transform: none; }
  /* placeholder; 在 Task 8 改成 opacity:0 + .in-view 触发 */
</style>
</head>
<body>

<!-- ===== Section 00 · 封面 ===== -->
<section id="s00" data-num="0">
  <h2 class="section-title">[封面]</h2>
</section>

<!-- ===== Section 01 · 命题 & 叠甲 ===== -->
<section id="s01" data-num="1">
  <span class="section-label">01 · 命题</span>
  <h2 class="section-title">[命题 & 叠甲]</h2>
</section>

<!-- ===== Section 02 · 心理学重构 ===== -->
<section id="s02" data-num="2">
  <span class="section-label">02 · 心理学重构</span>
  <h2 class="section-title">[解构"找不到的热爱"]</h2>
</section>

<!-- ===== Section 03 · 认知科学重构 ===== -->
<section id="s03" data-num="3">
  <span class="section-label">03 · 认知科学重构</span>
  <h2 class="section-title">[解构"什么是擅长"+ 演化核心模型]</h2>
</section>

<!-- ===== Section 04 · 拼图一 · 天赋 ===== -->
<section id="s04" data-num="4">
  <span class="section-label">04 · 拼图一</span>
  <h2 class="section-title">[天赋：荣格八维 + 盖洛普 34]</h2>
</section>

<!-- ===== Section 05 · 拼图二 · 兴趣 ===== -->
<section id="s05" data-num="5">
  <span class="section-label">05 · 拼图二</span>
  <h2 class="section-title">[兴趣：稀缺物盘点 + 5Why]</h2>
</section>

<!-- ===== Section 06 · 拼图三 · 变现 ===== -->
<section id="s06" data-num="6">
  <span class="section-label">06 · 拼图三</span>
  <h2 class="section-title">[变现：MVP 三问]</h2>
</section>

<!-- ===== Section 07 · 收尾 ===== -->
<section id="s07" data-num="7">
  <span class="section-label">07 · 收尾</span>
  <h2 class="section-title">[总公式回扣]</h2>
</section>

</body>
</html>
```

- [ ] **Step 2:** 浏览器验证

Run: `open /Users/apple/talks/destined-career/index.html`

Expected:
- 暖白底色 `#FBF9F4`
- 8 个 section 顺序排列，每节间距明显（96px padding）
- 标题靛蓝 `#1F3D7A`
- 节标签（"01 · 命题"等）是带边框的小标签

- [ ] **Step 3:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 骨架 + 设计 token + 8 个 section 占位"
```

---

## Task 2: 填充内容 — Section 00（封面）+ 01（命题）+ 02（心理学）

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 替换 Section 00（封面）

把 `<section id="s00" data-num="0">...</section>` 整个替换为：

```html
<section id="s00" data-num="0" class="cover">
  <div class="cover-inner">
    <div class="cover-label">分享 · 2026.05.25</div>
    <h1 class="cover-title">如何快速寻找到<br>自己的天选事业</h1>
    <p class="cover-sub">探索天选事业方向的通用技巧分享</p>
    <p class="cover-hint">↓ 向下滚动开始</p>
  </div>
</section>
```

紧跟着在 `</style>` 之前追加 CSS：

```css
/* === Cover === */
.cover { min-height: 100vh; display: flex; align-items: center; justify-content: center; text-align: center; padding-top: 0; padding-bottom: 0; }
.cover-inner { max-width: 720px; }
.cover-label { font-family: var(--font-en); font-size: 13px; letter-spacing: 0.15em; color: var(--c-red); margin-bottom: 24px; font-weight: 600; }
.cover-title { font-size: 64px; line-height: 1.2; margin-bottom: 20px; color: var(--c-blue); }
.cover-sub { font-size: 20px; color: var(--c-gray); margin-bottom: 80px; }
.cover-hint { font-size: 14px; color: var(--c-gray); letter-spacing: 0.1em; }
```

- [ ] **Step 2:** 替换 Section 01（命题 & 叠甲）

替换 `<section id="s01">` 整块为：

```html
<section id="s01" data-num="1">
  <span class="section-label">01 · 命题</span>
  <h2 class="section-title">为什么传统的"天选事业"公式总让你卡死？</h2>
  <p class="section-subtitle">叠个甲：本套路不是完美公式，有用就行。</p>

  <h4 style="margin-top:32px">1. 传统模型：刺猬理论</h4>
  <p class="quote">"狐狸知道很多事情，但刺猬知道一件大事。"<br><span class="quote-attr">— 吉姆·柯林斯《从优秀到卓越》</span></p>

  <p style="margin-top:24px">传统公式：</p>
  <div class="formula" data-katex>$$天选事业 = 热爱 \cap 擅长 \cap 商业价值$$</div>

  <div class="x-block">
    <span class="x-mark">❌</span>
    <div>
      <strong>常见卡点：</strong>这个模型太静态、太孤立了。就像相亲时硬性要求对方必须满足"生理性喜欢（热爱） + 灵魂伴侣（擅长） + 还要贼有钱（商业）"，结果就是直接卡死，寸步难行。
    </div>
  </div>
</section>
```

追加 CSS（紧跟在 Cover CSS 后面）：

```css
/* === Quote === */
.quote { border-left: 2px solid var(--c-gray-line); padding: 8px 0 8px 16px; font-style: italic; color: var(--c-gray); margin: 16px 0; font-size: 16px; line-height: 1.8; }
.quote-attr { font-style: normal; font-size: 13px; }

/* === Formula 容器 === */
.formula { background: var(--c-callout-bg); border-left: 3px solid var(--c-blue); padding: 20px 24px; margin: 24px 0; border-radius: 4px; font-size: 18px; text-align: center; }
.formula .katex { font-size: 1.4em; }

/* === X-Block (常见卡点提示框) === */
.x-block { display: flex; gap: 12px; padding: 16px 20px; background: var(--c-red-bg); border-radius: 4px; margin: 24px 0; line-height: 1.7; }
.x-mark { font-size: 20px; flex-shrink: 0; }
```

- [ ] **Step 3:** 替换 Section 02（心理学重构）

替换 `<section id="s02">` 整块为：

```html
<section id="s02" data-num="2">
  <span class="section-label">02 · 心理学重构</span>
  <h2 class="section-title">用心理学降维：解构"找不到的热爱"</h2>
  <p class="section-subtitle">引入理论：自我决定论（Self-Determination Theory）</p>

  <div class="formula" data-katex>$$热爱 = 自主感 + 胜任感 + 归属感$$</div>

  <div class="three-pillars">
    <div class="pillar">
      <h4>自主感</h4>
      <p>我自己想去做，而不是被谁指使（等同于<span class="hl-blue">兴趣</span>）。</p>
    </div>
    <div class="pillar">
      <h4>胜任感</h4>
      <p>事情不断做成，让我很有成就感（正向反哺<span class="hl-blue">擅长</span>）。</p>
    </div>
    <div class="pillar">
      <h4>归属感</h4>
      <p>在群体中被需要、被认可（正向催化<span class="hl-blue">商业价值</span>）。</p>
    </div>
  </div>
</section>
```

追加 CSS：

```css
/* === Three Pillars (三柱) === */
.three-pillars { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 24px; }
.pillar { background: var(--c-white); border-left: 3px solid var(--c-blue); padding: 20px 22px; border-radius: 4px; }
.pillar h4 { color: var(--c-red); font-size: 18px; margin-bottom: 8px; }
.pillar p { font-size: 15px; line-height: 1.7; }
.hl-blue { color: var(--c-blue); font-weight: 600; }
```

- [ ] **Step 4:** 浏览器验证

Run: `open /Users/apple/talks/destined-career/index.html`

Expected:
- Cover 满屏（min-height 100vh），大标题居中，"↓ 向下滚动开始"在下方
- Section 01：引用块（左竖线 + 斜体灰字）、formula 占位（公式以 `$$...$$` 文本形式显示，Task 7 会接入 KaTeX 渲染）、❌ 浅红提示框
- Section 02：三柱并排，每柱白底 + 靛蓝左竖线 + 朱砂标题

- [ ] **Step 5:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 填充封面 + 命题 + 心理学重构"
```

---

## Task 3: 填充内容 — Section 03（认知科学 + 演化核心模型 + 增强回路）

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 替换 Section 03

替换 `<section id="s03">` 整块为：

```html
<section id="s03" data-num="3">
  <span class="section-label">03 · 认知科学重构</span>
  <h2 class="section-title">用认知科学重构：解构"什么是擅长"</h2>
  <p class="section-subtitle">引入理论：盖洛普优势方程</p>

  <div class="formula" data-katex>$$天赋 \times 投入 = 优势（擅长）$$</div>

  <div class="three-pillars" style="grid-template-columns: 1fr 1fr;">
    <div class="pillar">
      <h4>天赋</h4>
      <p>大脑天生输入和输出信息的"<span class="hl-blue">硬件习惯</span>"（不费力）。</p>
    </div>
    <div class="pillar">
      <h4>投入</h4>
      <p>兴趣（自主感）会疯狂源源不断地为"<span class="hl-blue">投入时间</span>"进行电量输送。</p>
    </div>
  </div>

  <div class="callout-key">
    <div class="callout-label">📌 本章核心结论 · 动态递进模型</div>
    <p>热爱、擅长与商业不是孤立的，而是一个顺理成章的<strong>增强回路</strong>：</p>
  </div>

  <!-- 增强回路图（横向箭头链路） -->
  <div class="flow-chain">
    <div class="flow-node"><span class="flow-tag">兴趣</span>因为好玩试了一事</div>
    <div class="flow-arrow">→</div>
    <div class="flow-node"><span class="flow-tag">天赋</span>底层硬件契合，没费太大力气</div>
    <div class="flow-arrow">→</div>
    <div class="flow-node"><span class="flow-tag">胜任</span>做得不错并拿到成果</div>
    <div class="flow-arrow">→</div>
    <div class="flow-node"><span class="flow-tag">归属</span>得到别人赞赏与认可</div>
    <div class="flow-arrow">→</div>
    <div class="flow-node"><span class="flow-tag">擅长</span>更想做了，投入更多时间，极度精通</div>
    <div class="flow-arrow">→</div>
    <div class="flow-node flow-final"><span class="flow-tag">变现</span>帮更多人解决问题并拿到付费</div>
  </div>

  <p style="margin-top:48px; text-align:center; font-size: 18px;">最终得到我们的演化核心模型：</p>

  <div class="formula formula-hero" data-katex>$$天选事业 = 天赋 \times 兴趣 \times 变现$$</div>
</section>
```

追加 CSS：

```css
/* === Callout Key (📌 关键结论) === */
.callout-key { background: var(--c-callout-bg); border-left: 3px solid var(--c-blue); padding: 20px 24px; border-radius: 4px; margin: 32px 0 24px; }
.callout-label { font-weight: 700; color: var(--c-blue); margin-bottom: 8px; font-size: 16px; }
.callout-key p { line-height: 1.8; }

/* === Flow Chain (增强回路横向链路) === */
.flow-chain { display: flex; flex-wrap: wrap; align-items: stretch; gap: 6px; margin: 16px 0 8px; }
.flow-node { flex: 1 1 140px; background: var(--c-white); border: 1px solid var(--c-gray-border); border-left: 3px solid var(--c-blue); border-radius: 4px; padding: 12px 14px; font-size: 13px; line-height: 1.5; display: flex; flex-direction: column; gap: 6px; }
.flow-tag { display: inline-block; align-self: flex-start; padding: 1px 8px; background: var(--c-blue); color: var(--c-white); border-radius: 2px; font-size: 11px; font-weight: 600; }
.flow-final { border-left-color: var(--c-red); }
.flow-final .flow-tag { background: var(--c-red); }
.flow-arrow { color: var(--c-blue); font-weight: 700; font-size: 18px; align-self: center; flex-shrink: 0; padding: 0 2px; }

/* === Hero Formula === */
.formula-hero { font-size: 22px; padding: 32px 24px; margin-top: 16px; }
.formula-hero .katex { font-size: 1.8em; }
```

- [ ] **Step 2:** 浏览器验证

Expected:
- 03 节有：盖洛普公式 callout、天赋/投入 两柱、📌 关键结论 callout（暖米色背景 + 靛蓝左竖线）、6 节点横向 flow（最后一节"变现"是朱砂红色）、底部"演化核心模型"hero formula
- 横向 flow 在窄屏会折行（flex-wrap）

- [ ] **Step 3:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 填充认知科学 + 演化核心模型 + 增强回路图"
```

---

## Task 4: 填充内容 — Section 04（荣格八维 + 盖洛普 33）静态结构

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

> 本任务先把所有结构和数据塞进去，**默认全部展开/全部 chip 静态显示**。Task 10/11 再加点击折叠/展开交互。

- [ ] **Step 1:** 替换 Section 04

替换 `<section id="s04">` 整块为：

```html
<section id="s04" data-num="4">
  <span class="section-label">04 · 拼图一 · 天赋</span>
  <h2 class="section-title">认领你的底层硬件与战术 App</h2>
  <p class="section-subtitle">荣格八维（OS 操作系统）+ 盖洛普 34（上层高级 Apps）</p>

  <!-- ===== 4.1 荣格八维 ===== -->
  <h3 style="margin-top:48px; color: var(--c-blue);">🧭 4.1 荣格八维 · 底层 OS</h3>
  <p style="color: var(--c-gray); margin: 8px 0 24px;">大脑功耗最低、发热最小的底层信息处理算法。</p>

  <div class="jung-section-label">输入端 · 感知功能（你的大脑如何抓取数据）</div>
  <div class="jung-grid">
    <div class="jung-cell" data-fn="Se">
      <div class="jung-head"><span class="jung-code">Se</span><span class="jung-name">外倾感觉</span></div>
      <div class="jung-essence">沉浸物理世界 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>脑子里没有多余杂念，100% 沉浸在此时此地的物理世界。</p>
        <p><strong>🎯 一秒钟识别：</strong>窗外掉下一片树叶，他是班里第一个注意到的人。</p>
      </div>
    </div>
    <div class="jung-cell" data-fn="Si">
      <div class="jung-head"><span class="jung-code">Si</span><span class="jung-name">内倾感觉</span></div>
      <div class="jung-essence">尊重过去经验 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>极其尊重过去成功的经验和规矩，新事物必须跟大脑硬盘里的"历史模版"对齐。</p>
        <p><strong>🎯 一秒钟识别：</strong>错题本战神，写步骤排版错了一点心里就极度难受。</p>
      </div>
    </div>
    <div class="jung-cell" data-fn="Ne">
      <div class="jung-head"><span class="jung-code">Ne</span><span class="jung-name">外倾直觉</span></div>
      <div class="jung-essence">跨界联想万种可能 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>脑子像断了线的风筝，看到一个点瞬间跨界联想到一万个可能性。</p>
        <p><strong>🎯 一秒钟识别：</strong>听到"牛顿"能一秒联想到苹果、乔布斯、手机游戏，然后在台下傻笑。</p>
      </div>
    </div>
    <div class="jung-cell" data-fn="Ni">
      <div class="jung-head"><span class="jung-code">Ni</span><span class="jung-name">内倾直觉</span></div>
      <div class="jung-essence">脱水指向未来 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>大脑后台自动给海量乱象"脱水"，过滤噪音，直接吐出指向未来的终极答案。</p>
        <p><strong>🎯 一秒钟识别：</strong>盯物理大题三秒直接报出答案，说不清怎么推导的，但答案就是对的。</p>
      </div>
    </div>
  </div>

  <div class="jung-section-label" style="margin-top:32px">输出端 · 判断功能（数据采集完，你以什么标准输出行动）</div>
  <div class="jung-grid">
    <div class="jung-cell" data-fn="Te">
      <div class="jung-head"><span class="jung-code">Te</span><span class="jung-name">外倾思考</span></div>
      <div class="jung-essence">KPI 优先 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>冷酷、理智、只看客观指标和 KPI。怎么效率高、怎么拿第一就怎么来。</p>
        <p><strong>🎯 一秒钟识别：</strong>大扫除一秒把全班当工具人分配："你擦窗、你扫地，5分钟搞定！"</p>
      </div>
    </div>
    <div class="jung-cell" data-fn="Ti">
      <div class="jung-head"><span class="jung-code">Ti</span><span class="jung-name">内倾思考</span></div>
      <div class="jung-essence">逻辑 Debug 死磕 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>不在乎奖金和第一名，只要逻辑有一丝丝不通透、不自洽，就死磕到底去 Debug。</p>
        <p><strong>🎯 一秒钟识别：</strong>拿到万能公式不满足，课后花两小时自己手推证明一遍底层原理，否则睡不着。</p>
      </div>
    </div>
    <div class="jung-cell" data-fn="Fe">
      <div class="jung-head"><span class="jung-code">Fe</span><span class="jung-name">外倾情感</span></div>
      <div class="jung-essence">集体情感触角 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>拥有恐怖的情感触角，做决定唯一标准是"大家高不高兴，集体的面子和不和谐"。</p>
        <p><strong>🎯 一秒钟识别：</strong>班级外交官，两同学吵架，他进去开个玩笑说几句贴心话瞬间破冰。</p>
      </div>
    </div>
    <div class="jung-cell" data-fn="Fi">
      <div class="jung-head"><span class="jung-code">Fi</span><span class="jung-name">内倾情感</span></div>
      <div class="jung-essence">内心道德法庭 ▽</div>
      <div class="jung-detail">
        <p><strong>一句话本质：</strong>极度注重内心的真实与初心，不符合内心道德法庭的事，给多少钱老子也不干。</p>
        <p><strong>🎯 一秒钟识别：</strong>拒绝随大流，"我的心不答应，天王老子来我也不去"。</p>
      </div>
    </div>
  </div>

  <!-- ===== 4.1.5 全景互动名场面 ===== -->
  <div class="scene-block">
    <h4 class="scene-title">🎭 全景互动名场面</h4>
    <p class="scene-sub">女朋友微信发来："我感冒了，好难受 😷"<br><span style="color:var(--c-gray); font-size:14px;">同样一句话，八维大脑会做出什么反应？</span></p>

    <div class="scene-tabs" role="tablist">
      <button class="scene-tab is-active" data-tab="Se">Se 抓取</button>
      <button class="scene-tab" data-tab="Si">Si 抓取</button>
      <button class="scene-tab" data-tab="Ne">Ne 抓取</button>
      <button class="scene-tab" data-tab="Ni">Ni 抓取</button>
      <button class="scene-tab" data-tab="Te">Te 决策</button>
      <button class="scene-tab" data-tab="Ti">Ti 决策</button>
      <button class="scene-tab" data-tab="Fe">Fe 决策</button>
      <button class="scene-tab" data-tab="Fi">Fi 决策</button>
    </div>

    <div class="scene-panels">
      <div class="scene-panel is-active" data-panel="Se">看到字面事实，感冒 = 呼吸道感染，时间是晚上 10:15。</div>
      <div class="scene-panel" data-panel="Si">调取病史档案，她上个月也是这时感冒，连烧三天，吃蓝色药管用。</div>
      <div class="scene-panel" data-panel="Ne">脑洞连续剧，下周一她还要汇报，请假怎么办？项目会不会黄？现在要不要抢号？</div>
      <div class="scene-panel" data-panel="Ni">核心动机脱水，她不是在汇报病情，她是在表达脆弱、要偏爱和陪伴。</div>
      <div class="scene-panel" data-panel="Te">"多喝热水，吃一粒对乙酰氨基酚，明天还烧去挂呼吸科。"（送去体温计和药，高效但冷酷）</div>
      <div class="scene-panel" data-panel="Ti">"流鼻涕还是干咳？测体温没？先回忆下潜伏期接触了谁，我分析下是甲流还是风寒，对症下药。"</div>
      <div class="scene-panel" data-panel="Fe">"心疼坏了宝贝！抱抱！帮你点了热粥半小时到，我跟手头开会的说一声马上过去陪你！"</div>
      <div class="scene-panel" data-panel="Fi">陷入深沉的感同身受："看到你难受，我心里特别堵。我想实实在在地陪着你，我现在推了社交过去照顾你。"（拒绝任何渣男套路情话，绝对真诚）</div>
    </div>
  </div>

  <!-- ===== 4.2 盖洛普 34 ===== -->
  <h3 style="margin-top:64px; color: var(--c-blue);">⚙️ 4.2 盖洛普克利夫顿 · 上层 34 个高级 Apps</h3>
  <p style="color: var(--c-gray); margin: 8px 0 24px;">你的硬件系统在真实世界里死磕后，磨炼出来的高频战术武器。点击 chip 看每项的核心天赋表现。</p>

  <div class="gallup-domain" data-domain="exec">
    <div class="gallup-domain-head"><span class="gallup-domain-num">一</span><span class="gallup-domain-name">执行力</span><span class="gallup-domain-q">解决"如何把事做成"</span></div>
    <div class="gallup-chips">
      <button class="chip" data-name="成就" data-desc="能量电池">成就</button>
      <button class="chip" data-name="统筹" data-desc="拼图大师">统筹</button>
      <button class="chip" data-name="信仰" data-desc="价值观死士">信仰</button>
      <button class="chip" data-name="公平" data-desc="SOP 制定者">公平</button>
      <button class="chip" data-name="审慎" data-desc="排雷专家">审慎</button>
      <button class="chip" data-name="纪律" data-desc="自律时钟">纪律</button>
      <button class="chip" data-name="专注" data-desc="制导导弹">专注</button>
      <button class="chip" data-name="责任" data-desc="言出必行">责任</button>
      <button class="chip" data-name="排难" data-desc="起死回生 Debug">排难</button>
    </div>
    <div class="gallup-detail" hidden></div>
  </div>

  <div class="gallup-domain" data-domain="infl">
    <div class="gallup-domain-head"><span class="gallup-domain-num">二</span><span class="gallup-domain-name">影响力</span><span class="gallup-domain-q">解决"如何让别人听你的"</span></div>
    <div class="gallup-chips">
      <button class="chip" data-name="行动" data-desc="破局催化剂">行动</button>
      <button class="chip" data-name="统率" data-desc="带头冲锋">统率</button>
      <button class="chip" data-name="沟通" data-desc="金句捕手">沟通</button>
      <button class="chip" data-name="竞争" data-desc="胜负欲极强">竞争</button>
      <button class="chip" data-name="追求" data-desc="无情雕琢极致">追求</button>
      <button class="chip" data-name="自信" data-desc="孤勇前行">自信</button>
      <button class="chip" data-name="取悦" data-desc="社交破冰船">取悦</button>
      <button class="chip" data-name="体谅" data-desc="情绪安抚接收器">体谅</button>
    </div>
    <div class="gallup-detail" hidden></div>
  </div>

  <div class="gallup-domain" data-domain="rel">
    <div class="gallup-domain-head"><span class="gallup-domain-num">三</span><span class="gallup-domain-name">关系建立</span><span class="gallup-domain-q">解决"如何让大家一起走"</span></div>
    <div class="gallup-chips">
      <button class="chip" data-name="适应" data-desc="随遇而安">适应</button>
      <button class="chip" data-name="关联" data-desc="宏大格局">关联</button>
      <button class="chip" data-name="发展" data-desc="灵魂伯乐">发展</button>
      <button class="chip" data-name="和谐" data-desc="最大公约数">和谐</button>
      <button class="chip" data-name="包容" data-desc="不让人落单">包容</button>
      <button class="chip" data-name="个别" data-desc="定制化识人">个别</button>
      <button class="chip" data-name="积极" data-desc="快乐源泉">积极</button>
      <button class="chip" data-name="交往" data-desc="深度知己">交往</button>
    </div>
    <div class="gallup-detail" hidden></div>
  </div>

  <div class="gallup-domain" data-domain="strat">
    <div class="gallup-domain-head"><span class="gallup-domain-num">四</span><span class="gallup-domain-name">战略思维</span><span class="gallup-domain-q">解决"如何看清前方方向"</span></div>
    <div class="gallup-chips">
      <button class="chip" data-name="分析" data-desc="数据实证">分析</button>
      <button class="chip" data-name="前瞻" data-desc="画未来蓝图">前瞻</button>
      <button class="chip" data-name="理念" data-desc="概念连连看">理念</button>
      <button class="chip" data-name="搜集" data-desc="移动智库囤积狂">搜集</button>
      <button class="chip" data-name="思维" data-desc="大脑心流漫游">思维</button>
      <button class="chip" data-name="学习" data-desc="迷恋从不懂到懂">学习</button>
      <button class="chip" data-name="历史" data-desc="查清前因后果">历史</button>
      <button class="chip" data-name="战略" data-desc="导航过滤捷径">战略</button>
    </div>
    <div class="gallup-detail" hidden></div>
  </div>
</section>
```

- [ ] **Step 2:** 追加对应 CSS

```css
/* === Jung Grid === */
.jung-section-label { font-size: 13px; font-weight: 600; letter-spacing: 0.05em; color: var(--c-red); margin-bottom: 12px; padding-bottom: 6px; border-bottom: 1px solid var(--c-gray-border); }
.jung-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; }
.jung-cell { background: var(--c-white); border: 1px solid var(--c-gray-border); border-radius: 4px; padding: 14px 16px; cursor: pointer; transition: border-color 200ms, box-shadow 200ms; }
.jung-cell:hover { border-color: var(--c-blue); box-shadow: 0 1px 6px rgba(31,61,122,0.1); }
.jung-cell.is-open { border-color: var(--c-blue); border-left: 3px solid var(--c-blue); padding-left: 14px; }
.jung-head { display: flex; align-items: baseline; gap: 8px; margin-bottom: 4px; }
.jung-code { font-family: var(--font-en); font-weight: 700; font-size: 18px; color: var(--c-blue); }
.jung-name { font-size: 14px; color: var(--c-blue-text); }
.jung-essence { font-size: 13px; color: var(--c-gray); }
.jung-detail { max-height: 0; overflow: hidden; transition: max-height 250ms ease-out; background: var(--c-bg); margin-top: 0; border-radius: 3px; }
.jung-cell.is-open .jung-detail { max-height: 400px; margin-top: 10px; padding: 12px 14px; }
.jung-detail p { font-size: 13px; line-height: 1.7; margin-bottom: 6px; }
.jung-detail p:last-child { margin-bottom: 0; }

/* === Scene (名场面) === */
.scene-block { margin: 40px 0; padding: 24px 24px 28px; background: var(--c-callout-bg); border-radius: 6px; }
.scene-title { color: var(--c-blue); font-size: 22px; margin-bottom: 8px; }
.scene-sub { color: var(--c-blue-text); margin-bottom: 20px; line-height: 1.7; }
.scene-tabs { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 16px; }
.scene-tab { background: var(--c-white); border: 1px solid var(--c-gray-border); border-radius: 16px; padding: 6px 14px; font-size: 13px; font-family: inherit; color: var(--c-blue-text); cursor: pointer; transition: all 150ms; }
.scene-tab:hover { border-color: var(--c-blue); }
.scene-tab.is-active { background: var(--c-blue); color: var(--c-white); border-color: var(--c-blue); }
.scene-panels { background: var(--c-white); border-radius: 4px; padding: 18px 22px; min-height: 80px; line-height: 1.8; }
.scene-panel { display: none; }
.scene-panel.is-active { display: block; }

/* === Gallup 34 === */
.gallup-domain { margin: 20px 0 0; padding: 20px 22px; background: var(--c-white); border-left: 3px solid var(--c-blue); border-radius: 4px; }
.gallup-domain-head { display: flex; align-items: baseline; gap: 10px; margin-bottom: 12px; }
.gallup-domain-num { color: var(--c-red); font-weight: 700; font-size: 15px; }
.gallup-domain-name { font-size: 18px; font-weight: 700; color: var(--c-blue); }
.gallup-domain-q { color: var(--c-gray); font-size: 14px; }
.gallup-chips { display: flex; flex-wrap: wrap; gap: 8px; }
.chip { background: var(--c-bg); border: 1px solid var(--c-gray-border); border-radius: 16px; padding: 5px 14px; font-size: 13px; font-family: inherit; color: var(--c-blue-text); cursor: pointer; transition: all 150ms; }
.chip:hover { border-color: var(--c-blue); }
.chip.is-active { background: var(--c-blue); color: var(--c-white); border-color: var(--c-blue); }
.gallup-detail { margin-top: 14px; padding: 12px 16px; background: var(--c-callout-bg); border-radius: 4px; font-size: 14px; line-height: 1.7; }
.gallup-detail strong { color: var(--c-blue); }
```

- [ ] **Step 3:** 浏览器验证

Expected:
- 4.1 荣格：8 个白色卡片（2 行×4 列），输入端 4 个 + 输出端 4 个；分隔标签朱砂红
- 4.1.5 名场面：暖米底块，8 个 tab chip，下方白色面板显示当前 tab 内容
- 4.2 盖洛普：4 个白色块（4 大领域），每块内顶部"一/二/三/四 + 领域名 + 问题"，下方 chip 行，最后一个空 `.gallup-detail`
- 此时所有 jung-cell 都折叠（max-height 0），所有 chip 都没选中，Task 10/11 加交互后才动起来

⚠️ 现在点击不会有反应（交互在 Task 10/11）。本 Task 只验证视觉结构。

- [ ] **Step 4:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 填充荣格八维 + 名场面 + 盖洛普 34 静态结构"
```

---

## Task 5: 填充内容 — Section 05（兴趣）+ 06（变现）+ 07（收尾）

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 替换 Section 05（兴趣）

替换 `<section id="s05">` 整块为：

```html
<section id="s05" data-num="5">
  <span class="section-label">05 · 拼图二 · 兴趣</span>
  <h2 class="section-title">稀缺物盘点 + 5Why 追问</h2>
  <p class="section-subtitle">找兴趣就跟谈恋爱一样，看你有没有心甘情愿给 Ta 你认为最稀缺的资源。</p>

  <!-- 技术 1：稀缺物追踪 -->
  <h4 style="margin-top:32px">🛠️ 实操技术 1 · 稀缺物追踪</h4>
  <div class="resource-grid">
    <div class="resource-cell">
      <div class="resource-icon">⏱️</div>
      <h5>时间流向</h5>
      <p>刷得停不下来的抖音/小红书标签、深夜还在看的书籍类别。</p>
    </div>
    <div class="resource-cell">
      <div class="resource-icon">💰</div>
      <h5>金钱流向</h5>
      <p>账单里除了维持生存外，你愿意溢价付费的类目。</p>
    </div>
    <div class="resource-cell">
      <div class="resource-icon">💗</div>
      <h5>情绪心流</h5>
      <p>让你忘记时间、极度亢奋或治愈的时刻。</p>
    </div>
    <div class="resource-cell">
      <div class="resource-icon">💡</div>
      <h5>灵感捕捉</h5>
      <p>你的 flomo/备忘录里记录最多的奇思妙想。</p>
    </div>
  </div>

  <!-- 技术 2：5Why -->
  <h4 style="margin-top:48px">🛠️ 实操技术 2 · 5Why 深度剥离</h4>
  <p style="color:var(--c-gray); margin: 4px 0 16px;">以购买"一次性袜子"为例</p>
  <div class="why-ladder">
    <div class="why-step"><span class="why-label">1Why</span><span class="why-q">为什么要买？</span><span class="why-a">随用随丢不用洗，减少行李占用。</span></div>
    <div class="why-step"><span class="why-label">2Why</span><span class="why-q">为什么不想洗和占用行李？</span><span class="why-a">讨厌洗晾晒的管理和时间消耗，想让出行更轻松。</span></div>
    <div class="why-step"><span class="why-label">3Why</span><span class="why-q">为什么讨厌晾晒管理？</span><span class="why-a">想减少生活中琐事的时间投入。</span></div>
    <div class="why-step"><span class="why-label">4Why</span><span class="why-q">为什么不愿投入琐事？</span><span class="why-a">认为把精力和时间花在机械重复劳动上是对生命的浪费。</span></div>
    <div class="why-step why-final"><span class="why-label">5Why</span><span class="why-q">为什么觉得是浪费？</span><span class="why-a"><strong>核心本质：</strong>我需要极高的"<strong>掌控感</strong>"和"<strong>自由度</strong>"，我更喜欢"创造"和"高效率"，而不是"维持现状"。</span></div>
  </div>

  <!-- 技术 3：关键词碰撞 -->
  <h4 style="margin-top:48px">🛠️ 实操技术 3 · 关键词提取与交叉碰撞</h4>

  <p style="margin-top:16px"><strong>提取赛道关键词：</strong></p>
  <ul style="margin: 8px 0 16px;">
    <li><strong>浅层：</strong>极简生活、空间收纳。</li>
    <li><strong>底层：</strong>流程自动化与效率工具（消灭一切机械重复）、个人精力管理。</li>
  </ul>

  <p><strong>进行异质碰撞（连连看）：</strong></p>

  <div class="cross-diagram">
    <div class="cross-kw">神经科学</div>
    <div class="cross-x">×</div>
    <div class="cross-kw">教育学</div>
    <div class="cross-x">×</div>
    <div class="cross-kw">效率工具</div>
    <div class="cross-eq">=</div>
    <div class="cross-result">开发一套能让普通人根据大脑科学高效学习的个人 Skill Tree 自动化系统</div>
  </div>

  <p style="margin-top:16px; color:var(--c-gray); font-size: 14px;">💡 把你的兴趣清单（效率工具、精力管理、神经科学、空间美学、行为心理学……）两两/三三交叉，常常蹦出意想不到的方向。</p>
</section>
```

追加 CSS：

```css
/* === Resource Grid (稀缺物 4 格) === */
.resource-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-top: 16px; }
.resource-cell { background: var(--c-white); border: 1px solid var(--c-gray-border); border-radius: 4px; padding: 16px 14px; text-align: center; }
.resource-icon { font-size: 26px; margin-bottom: 6px; }
.resource-cell h5 { color: var(--c-blue); font-size: 15px; margin: 0 0 6px; font-weight: 700; }
.resource-cell p { font-size: 13px; line-height: 1.6; color: var(--c-blue-text); }

/* === Why Ladder (5Why 阶梯) === */
.why-ladder { border-left: 1px solid var(--c-gray-line); padding-left: 0; margin-top: 16px; }
.why-step { padding: 10px 0 10px 20px; border-bottom: 1px dashed var(--c-gray-border); display: grid; grid-template-columns: 60px 1fr; column-gap: 12px; row-gap: 4px; align-items: baseline; margin-left: -1px; border-left: 1px solid var(--c-gray-line); }
.why-step:last-child { border-bottom: none; }
.why-label { color: var(--c-blue); font-weight: 700; font-family: var(--font-en); }
.why-q { color: var(--c-blue-text); font-weight: 600; grid-column: 2; }
.why-a { grid-column: 2; color: var(--c-blue-text); line-height: 1.7; font-size: 14px; }
.why-final { background: var(--c-red-bg); border-left: 3px solid var(--c-red); padding-left: 17px; border-radius: 0 4px 4px 0; }
.why-final .why-label { color: var(--c-red); }
.why-final .why-q { color: var(--c-red); }

/* === Cross Diagram (关键词碰撞连线图) === */
.cross-diagram { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; margin: 16px 0; padding: 18px 20px; background: var(--c-callout-bg); border-radius: 6px; }
.cross-kw { padding: 6px 14px; background: var(--c-white); border: 1px solid var(--c-blue); border-radius: 4px; color: var(--c-blue); font-weight: 600; font-size: 14px; }
.cross-x { color: var(--c-gray); font-weight: 700; font-size: 18px; }
.cross-eq { color: var(--c-red); font-weight: 700; font-size: 22px; margin: 0 4px; }
.cross-result { flex: 1 1 280px; color: var(--c-blue-text); font-size: 14px; line-height: 1.7; padding: 10px 14px; background: var(--c-white); border-left: 3px solid var(--c-red); border-radius: 0 4px 4px 0; }
```

- [ ] **Step 2:** 替换 Section 06（变现）

替换 `<section id="s06">` 整块为：

```html
<section id="s06" data-num="6">
  <span class="section-label">06 · 拼图三 · 变现</span>
  <h2 class="section-title">个人精益创业 MVP 三问</h2>
  <p class="section-subtitle">天赋如果不封装成产品交付给别人，就只是一堆写在纸上的测试报告。</p>

  <div class="mvp-grid">
    <div class="mvp-card">
      <div class="mvp-q">Who</div>
      <h4>谁有痛点？</h4>
      <p>谁会为了你擅长的这件事感到痛苦？</p>
      <div class="mvp-eg"><strong>例：</strong>那些每天做数据报表、被低效机械重复折磨到深夜 12 点的打工人</div>
    </div>
    <div class="mvp-card">
      <div class="mvp-q">What</div>
      <h4>你的产品？</h4>
      <p>你能提供一个什么具体的"最小好东西"帮他解决？</p>
      <div class="mvp-eg"><strong>例：</strong>别去想开公司。一份消灭重复劳动的【自动化数据看板 Excel 模版】、一次 1 对 1 的效率陪跑咨询</div>
    </div>
    <div class="mvp-card">
      <div class="mvp-q">How</div>
      <h4>怎么让他知道？</h4>
      <p>你通过什么渠道把这个好东西推到他眼前？</p>
      <div class="mvp-eg"><strong>例：</strong>朋友圈现身说法、小红书垂直内容输出、熟人社群介绍背书</div>
    </div>
  </div>
</section>
```

追加 CSS：

```css
/* === MVP Grid === */
.mvp-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 24px; }
.mvp-card { background: var(--c-white); border-radius: 6px; padding: 22px 22px 18px; border: 1px solid var(--c-gray-border); display: flex; flex-direction: column; }
.mvp-q { font-family: var(--font-en); font-weight: 800; font-size: 13px; letter-spacing: 0.15em; color: var(--c-red); margin-bottom: 12px; }
.mvp-card h4 { color: var(--c-blue); font-size: 20px; margin-bottom: 8px; }
.mvp-card > p { color: var(--c-blue-text); font-size: 14px; line-height: 1.7; margin-bottom: 12px; }
.mvp-eg { background: var(--c-callout-bg); padding: 10px 12px; border-radius: 4px; font-size: 13px; line-height: 1.7; margin-top: auto; }
.mvp-eg strong { color: var(--c-blue); }
```

- [ ] **Step 3:** 替换 Section 07（收尾）

替换 `<section id="s07">` 整块为：

```html
<section id="s07" data-num="7" class="closing">
  <span class="section-label">07 · 收尾</span>
  <h2 class="section-title">回到那个核心公式</h2>

  <div class="formula formula-hero" data-katex>$$天选事业 = 天赋 \times 兴趣 \times 变现$$</div>

  <div class="closing-recap">
    <div class="recap-line"><span class="recap-tag">天赋</span>认领你的底层 OS + 战术 Apps</div>
    <div class="recap-line"><span class="recap-tag">兴趣</span>追踪稀缺物，5Why 挖到核心，关键词交叉碰撞</div>
    <div class="recap-line"><span class="recap-tag">变现</span>Who · What · How，最小好东西先做出来</div>
  </div>

  <p class="closing-cta">现在轮到你了。</p>
  <p class="closing-q">⛳️ Q & A</p>
</section>
```

追加 CSS：

```css
/* === Closing === */
.closing { text-align: center; }
.closing .formula-hero { text-align: center; }
.closing-recap { display: flex; flex-direction: column; gap: 12px; max-width: 560px; margin: 32px auto 48px; text-align: left; }
.recap-line { background: var(--c-white); padding: 12px 18px; border-left: 3px solid var(--c-blue); border-radius: 0 4px 4px 0; font-size: 15px; line-height: 1.7; }
.recap-tag { display: inline-block; padding: 1px 10px; background: var(--c-blue); color: var(--c-white); border-radius: 2px; font-size: 12px; font-weight: 700; margin-right: 10px; vertical-align: 1px; }
.closing-cta { font-size: 22px; color: var(--c-red); font-weight: 700; margin-top: 32px; }
.closing-q { font-size: 32px; color: var(--c-blue); margin-top: 24px; letter-spacing: 0.1em; }
```

- [ ] **Step 4:** 浏览器验证

Expected:
- 05：稀缺物 4 格、5Why 阶梯（最后一行朱砂高亮）、关键词碰撞图（暖米底 + 3 个白色 keyword + × × = + 红色边线结果框）
- 06：MVP 三卡（Who/What/How），每卡顶部红色标签
- 07：核心公式 hero + 三行 recap（每行靛蓝小标签）+ "现在轮到你了" + "⛳️ Q & A"

- [ ] **Step 5:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 填充兴趣 + 变现 + 收尾"
```

---

## Task 6: KaTeX 内嵌 + 公式渲染

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 下载 KaTeX 的 CSS 和 JS 内容到本地（先临时存到 /tmp）

```bash
cd /tmp && curl -sSL https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.css -o katex.css && curl -sSL https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.js -o katex.js && curl -sSL https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/contrib/auto-render.min.js -o katex-auto.js && wc -l katex.*
```

Expected：三个文件下载成功，每个有内容。如果断网/CDN 慢，先尝试 `unpkg.com` 或 `npmmirror.com` 作为替代源。

- [ ] **Step 2:** 把 KaTeX CSS 内嵌到 `<style>` 块之前（独立 `<style>`），KaTeX JS 内嵌到 `</body>` 之前

注意：KaTeX CSS 引用了字体文件（KaTeX_*.woff2）。**为了保证彻底离线可用且单文件**，CSS 里所有 `url(./fonts/...)` 引用需要被替换。**简化做法**：CSS 里的 `@font-face` 段全部删除，转而依赖系统字体 fallback（公式数字/符号用 `font-family: KaTeX_Main, Times New Roman, serif` 这种栈，浏览器找不到 KaTeX 字体会用 Times，依旧可读，只是观感不完美）。

具体步骤：

1. 打开 `/tmp/katex.css`，用编辑器或 sed 删掉所有 `@font-face { ... }` 块（一共有十几个，分散在文件顶部）。简单做法：

```bash
cd /tmp && python3 -c "
import re
with open('katex.css') as f: css = f.read()
css = re.sub(r'@font-face\s*\{[^}]*\}', '', css)
with open('katex-nofont.css', 'w') as f: f.write(css)
print('done; size:', len(css))
"
```

2. 把 `katex-nofont.css` 的内容粘贴进 `index.html` 的 `<head>` 里，紧跟在 `<title>` 后面，放在自定义 `<style>` **之前**（这样后面我们的样式可以覆盖 KaTeX 默认）：

```html
<title>如何快速寻找到自己的天选事业</title>
<style id="katex-css">
/* === KaTeX 内嵌 CSS（去掉了 @font-face）=== */
... 粘贴 katex-nofont.css 的全部内容 ...
</style>
<style>
  /* === Design Tokens === (你已有的样式不变) */
```

3. 把 `katex.js` 和 `katex-auto.js` 的内容粘贴进 `</body>` 前的 `<script>` 块：

```html
<script id="katex-js">
/* === KaTeX 内嵌 JS === */
... 粘贴 katex.js 的全部内容 ...
</script>
<script id="katex-autorender-js">
/* === KaTeX Auto-Render 内嵌 JS === */
... 粘贴 katex-auto.js 的全部内容 ...
</script>
<script>
  // === 我们自己的初始化代码 ===
  document.addEventListener('DOMContentLoaded', function() {
    renderMathInElement(document.body, {
      delimiters: [
        {left: '$$', right: '$$', display: true},
        {left: '$', right: '$', display: false}
      ],
      throwOnError: false
    });
  });
</script>
</body>
```

- [ ] **Step 3:** 浏览器验证

Run: `open /Users/apple/talks/destined-career/index.html`

Expected:
- 所有公式（Section 01 的 `天选事业 = 热爱 ∩ 擅长 ∩ 商业价值`、02 的 `热爱 = 自主感 + 胜任感 + 归属感`、03 的两个公式、07 的 hero）都被渲染成数学排版样式（不再显示 `$$`）
- 字体可能不是 KaTeX 默认而是系统 serif（因为我们删了 @font-face），但符号、上下标都正确
- 中文（天选事业、自主感等）正常显示

如果有公式没渲染：检查 `<div class="formula" data-katex>$$...$$</div>` 的 `$$` 是否完整无误。

- [ ] **Step 4:** 测试断网

在 macOS 上断开 WiFi，刷新页面（cmd+R）。Expected：公式仍正常显示（因为 KaTeX 已经内嵌）。然后恢复 WiFi。

- [ ] **Step 5:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 内嵌 KaTeX，公式离线渲染"
```

---

## Task 7: 入场动画（IntersectionObserver）

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 修改 section 的初始 CSS，加上动画过渡

在 `<style>` 里找到这两行：

```css
section { opacity: 1; transform: none; }
/* placeholder; 在 Task 8 改成 opacity:0 + .in-view 触发 */
```

替换为：

```css
/* === Entrance Animation === */
section { opacity: 0; transform: translateY(24px); transition: opacity 500ms ease-out, transform 500ms ease-out; }
section.in-view { opacity: 1; transform: translateY(0); }
@media (prefers-reduced-motion: reduce) {
  section { opacity: 1 !important; transform: none !important; transition: none !important; }
}
```

封面 section 应当立刻可见（避免页面打开是空白）。在 `<body>` 里给封面加 `is-cover-loaded` 标记，或直接让 `#s00` 始终 `.in-view`。最简方法：

在 `</style>` 之前追加：

```css
#s00 { opacity: 1; transform: none; }
```

- [ ] **Step 2:** 在 KaTeX 初始化的 `<script>` 块里追加 IntersectionObserver 逻辑

在 `renderMathInElement(...)` 调用之后，追加：

```javascript
    // === 入场动画 ===
    var observer = new IntersectionObserver(function(entries) {
      entries.forEach(function(entry) {
        if (entry.isIntersecting) {
          entry.target.classList.add('in-view');
        }
      });
    }, { threshold: 0.15 });
    document.querySelectorAll('section').forEach(function(s) {
      if (s.id !== 's00') observer.observe(s);
    });
```

- [ ] **Step 3:** 浏览器验证

Run: `open /Users/apple/talks/destined-career/index.html`

Expected:
- 打开页面：封面立刻显示
- 向下滚动：每个新进入视口的 section 淡入 + 轻微上移 500ms
- 滚回上方：已显示的 section 不会重置（in-view 不删除）
- 系统偏好"减少动效"开启的话动画跳过（macOS：系统设置 → 辅助功能 → 显示 → 减少动态效果）

- [ ] **Step 4:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: section 入场动画"
```

---

## Task 8: 演讲者模式（class toggle + 键盘导航 + 指示器）

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 在 `</body>` 之前（任何 `<script>` 之前），加入演讲者模式 UI 元素

```html
<!-- 演讲者模式 UI -->
<button id="presenter-toggle" class="presenter-toggle" type="button" aria-label="切换演讲者模式">▶ 演讲者</button>
<div id="presenter-indicator" class="presenter-indicator" hidden>1 / 8</div>
```

- [ ] **Step 2:** 追加 CSS

```css
/* === Presenter Mode UI === */
.presenter-toggle { position: fixed; right: 20px; bottom: 20px; z-index: 1000; padding: 8px 14px; background: var(--c-white); border: 1px solid var(--c-blue); color: var(--c-blue); border-radius: 20px; font-family: inherit; font-size: 13px; font-weight: 600; cursor: pointer; box-shadow: 0 2px 8px rgba(31,61,122,0.15); transition: all 150ms; }
.presenter-toggle:hover { background: var(--c-blue); color: var(--c-white); }
.presenter .presenter-toggle::before { content: "✕ 退出 "; }
.presenter .presenter-toggle { font-size: 12px; }
/* hide the original "▶ 演讲者" text when in presenter mode — simplest: */
.presenter .presenter-toggle { color: var(--c-white); background: var(--c-blue); }

.presenter-indicator { position: fixed; right: 20px; bottom: 64px; z-index: 1000; padding: 4px 12px; background: rgba(255,255,255,0.92); border: 1px solid var(--c-gray-border); color: var(--c-blue); border-radius: 12px; font-family: var(--font-en); font-size: 13px; font-weight: 600; }

/* === Presenter Mode 生效样式 === */
.presenter html, .presenter body { height: 100%; overflow: hidden; }
.presenter body { scroll-snap-type: y mandatory; overflow-y: scroll; height: 100vh; }
.presenter section { min-height: 100vh; display: flex; flex-direction: column; justify-content: center; padding-top: 56px; padding-bottom: 56px; max-width: 1100px; }
.presenter .cover { padding: 0; }
.presenter h1 { font-size: 84px; }
.presenter h2 { font-size: 44px; }
.presenter h3 { font-size: 30px; }
.presenter h4 { font-size: 26px; }
.presenter p, .presenter li { font-size: 20px; line-height: 1.7; }
.presenter .section-subtitle { font-size: 22px; }
.presenter .formula { font-size: 24px; }
.presenter .formula-hero { font-size: 30px; }
.presenter .formula-hero .katex { font-size: 2.2em; }
.presenter .jung-cell { padding: 18px 22px; }
.presenter .chip { font-size: 16px; padding: 7px 18px; }
.presenter .scene-tab { font-size: 16px; padding: 8px 18px; }
.presenter .why-step { font-size: 16px; }
```

- [ ] **Step 3:** 追加 JS（紧跟在 IntersectionObserver 代码之后）

```javascript
    // === 演讲者模式 ===
    var body = document.body;
    var btn = document.getElementById('presenter-toggle');
    var indicator = document.getElementById('presenter-indicator');
    var sections = Array.from(document.querySelectorAll('section'));

    function isPresenter() { return body.classList.contains('presenter'); }

    function updateIndicator() {
      if (!isPresenter()) return;
      var current = 1;
      var midY = window.innerHeight / 2;
      for (var i = 0; i < sections.length; i++) {
        var r = sections[i].getBoundingClientRect();
        if (r.top <= midY && r.bottom > midY) { current = i + 1; break; }
      }
      indicator.textContent = current + ' / ' + sections.length;
    }

    function enterPresenter() {
      body.classList.add('presenter');
      btn.textContent = ' 退出';
      indicator.hidden = false;
      updateIndicator();
    }
    function exitPresenter() {
      body.classList.remove('presenter');
      btn.textContent = '▶ 演讲者';
      indicator.hidden = true;
    }
    function togglePresenter() { isPresenter() ? exitPresenter() : enterPresenter(); }

    btn.addEventListener('click', togglePresenter);

    function gotoIndex(idx) {
      if (idx < 0) idx = 0;
      if (idx >= sections.length) idx = sections.length - 1;
      sections[idx].scrollIntoView({ behavior: 'smooth', block: 'start' });
      // 翻页时重置目标 section 的 .in-view 触发动画
      var target = sections[idx];
      target.classList.remove('in-view');
      requestAnimationFrame(function() {
        requestAnimationFrame(function() { target.classList.add('in-view'); });
      });
    }
    function currentIndex() {
      var midY = window.innerHeight / 2;
      for (var i = 0; i < sections.length; i++) {
        var r = sections[i].getBoundingClientRect();
        if (r.top <= midY && r.bottom > midY) return i;
      }
      return 0;
    }

    document.addEventListener('keydown', function(e) {
      // 'P' 切换模式（任何模式下都能用）
      if (e.key === 'p' || e.key === 'P') { togglePresenter(); e.preventDefault(); return; }
      if (!isPresenter()) return;
      // 演讲者模式下的导航
      switch (e.key) {
        case 'ArrowRight': case 'ArrowDown': case 'PageDown': case ' ':
          gotoIndex(currentIndex() + 1); e.preventDefault(); break;
        case 'ArrowLeft': case 'ArrowUp': case 'PageUp':
          gotoIndex(currentIndex() - 1); e.preventDefault(); break;
        case 'Home':
          gotoIndex(0); e.preventDefault(); break;
        case 'End':
          gotoIndex(sections.length - 1); e.preventDefault(); break;
        case 'Escape':
          exitPresenter(); e.preventDefault(); break;
      }
    });

    // 演讲者模式滚动时更新指示器
    document.body.addEventListener('scroll', function() {
      if (isPresenter()) updateIndicator();
    }, { passive: true });
    window.addEventListener('scroll', function() {
      if (isPresenter()) updateIndicator();
    }, { passive: true });
```

- [ ] **Step 4:** 浏览器验证

Expected:
- 右下角看到"▶ 演讲者"按钮
- 点击或按 P：body 加 `.presenter` class，字号放大，每个 section 强制满屏 + scroll-snap，指示器显示 "1 / 8"
- 按 → / ↓ / PgDn / Space：滚到下一个 section，指示器更新
- 按 ← / ↑ / PgUp：滚到上一个
- Home / End：跳首末
- Esc：退出回到阅读模式
- 每次翻页目标 section 的入场动画重新触发（fade-in + slide-up）

⚠️ 已知小问题：演讲者模式下封面的 padding 是 0，要确保封面内容居中。如果偏上，给 `.presenter .cover-inner` 加 `margin: auto` 即可。

- [ ] **Step 5:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 演讲者模式（class toggle + 键盘 + 指示器）"
```

---

## Task 9: 荣格 8 维点击展开 + 名场面 tab 切换

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 在主 `<script>` 块（KaTeX 初始化所在的那个）末尾追加：

```javascript
    // === 荣格 8 维点击展开（accordion）===
    document.querySelectorAll('.jung-cell').forEach(function(cell) {
      cell.addEventListener('click', function() {
        var wasOpen = cell.classList.contains('is-open');
        document.querySelectorAll('.jung-cell.is-open').forEach(function(c) { c.classList.remove('is-open'); });
        if (!wasOpen) cell.classList.add('is-open');
      });
    });

    // === 名场面 tab 切换 ===
    document.querySelectorAll('.scene-tab').forEach(function(tab) {
      tab.addEventListener('click', function() {
        var name = tab.getAttribute('data-tab');
        var block = tab.closest('.scene-block');
        block.querySelectorAll('.scene-tab').forEach(function(t) { t.classList.remove('is-active'); });
        block.querySelectorAll('.scene-panel').forEach(function(p) { p.classList.remove('is-active'); });
        tab.classList.add('is-active');
        block.querySelector('.scene-panel[data-panel="' + name + '"]').classList.add('is-active');
      });
    });
```

- [ ] **Step 2:** 浏览器验证

Expected:
- 点 Se 卡片 → 该卡展开（详细一句话 + 一秒识别），其他卡保持折叠
- 点 Si 卡片 → Se 折叠，Si 展开（accordion 行为）
- 再点 Si 同一张 → Si 折叠（不再有展开的）
- 名场面：默认显示"Se 抓取"内容；点 Te 决策 chip → tab 高亮切换到 Te，下方面板显示 Te 决策的内容

- [ ] **Step 3:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 荣格 8 维点击展开 + 名场面 tab 切换"
```

---

## Task 10: 盖洛普 chip 点击展开（每栏独立）

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 继续在主 `<script>` 块末尾追加：

```javascript
    // === 盖洛普 chip 点击展开（每栏独立）===
    document.querySelectorAll('.gallup-domain').forEach(function(domain) {
      var detail = domain.querySelector('.gallup-detail');
      domain.querySelectorAll('.chip').forEach(function(chip) {
        chip.addEventListener('click', function() {
          var wasActive = chip.classList.contains('is-active');
          domain.querySelectorAll('.chip').forEach(function(c) { c.classList.remove('is-active'); });
          if (wasActive) {
            detail.hidden = true;
            detail.innerHTML = '';
          } else {
            chip.classList.add('is-active');
            var name = chip.getAttribute('data-name');
            var desc = chip.getAttribute('data-desc');
            detail.hidden = false;
            detail.innerHTML = '<strong>' + name + '</strong>（' + desc + '）';
          }
        });
      });
    });
```

- [ ] **Step 2:** 浏览器验证

Expected:
- 点"执行力"栏的"成就" chip → 该 chip 高亮，下方 `.gallup-detail` 显示 "**成就**（能量电池）"
- 点同一栏的"统筹" → "成就"取消高亮，"统筹"高亮，detail 更新为 "**统筹**（拼图大师）"
- 再点"统筹"一次 → 取消高亮，detail 收起
- "影响力"栏的 chip 不影响"执行力"栏（每栏独立工作）

- [ ] **Step 3:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 盖洛普 chip 点击展开（每栏独立）"
```

---

## Task 11: 响应式（手机布局）+ 打印样式

**Files:**
- Modify: `/Users/apple/talks/destined-career/index.html`

- [ ] **Step 1:** 在 `</style>` 之前追加响应式 + 打印样式

```css
/* === Responsive === */
@media (max-width: 1279px) {
  section { padding: 80px 28px; }
  .jung-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 767px) {
  :root { --section-gap: 60px; }
  h1 { font-size: 36px; }
  h2 { font-size: 26px; }
  h3 { font-size: 20px; }
  h4 { font-size: 18px; }
  .cover-title { font-size: 40px; }
  section { padding: 60px 20px; }
  .jung-grid { grid-template-columns: 1fr; }
  .three-pillars { grid-template-columns: 1fr !important; }
  .resource-grid { grid-template-columns: repeat(2, 1fr); }
  .mvp-grid { grid-template-columns: 1fr; }
  .flow-chain { flex-direction: column; }
  .flow-arrow { transform: rotate(90deg); }
  .cross-diagram { flex-direction: column; align-items: stretch; gap: 8px; }
  .cross-x, .cross-eq { text-align: center; }
  .presenter-toggle, .presenter-indicator { display: none; }
  /* 手机不支持演讲者模式 */
}

/* === Print === */
@media print {
  body { background: white; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
  section { opacity: 1 !important; transform: none !important; max-width: 100%; padding: 28px 16px; page-break-inside: avoid; break-inside: avoid; }
  /* 全部 jung 展开 */
  .jung-cell .jung-detail { max-height: none !important; margin-top: 10px !important; padding: 12px 14px !important; }
  /* 名场面 8 panel 全部列出 */
  .scene-tabs { display: none; }
  .scene-panel { display: block !important; padding: 8px 0; border-bottom: 1px dashed #ccc; }
  .scene-panel::before { content: attr(data-panel) ' · '; font-weight: 700; color: #1F3D7A; }
  .scene-panel:last-child { border-bottom: none; }
  /* 盖洛普 chip 全部展开为列表 */
  .gallup-chips { display: block; }
  .chip { display: block; background: none !important; color: inherit !important; border: none !important; padding: 4px 0 !important; text-align: left; font-size: 13px; }
  .chip::before { content: '· '; color: #1F3D7A; font-weight: 700; }
  .chip::after { content: ' — ' attr(data-desc); color: #666; font-size: 12px; }
  .gallup-detail { display: none; }
  /* 演讲者 UI 隐藏 */
  .presenter-toggle, .presenter-indicator { display: none; }
}
```

- [ ] **Step 2:** 浏览器验证（响应式）

Run: `open /Users/apple/talks/destined-career/index.html`

打开浏览器开发者工具 → 设备模拟 → iPhone 14（390px）：
- 8 节都能纵向滚到
- 荣格变成 1×8 单列
- 资源 4 格变成 2×2
- MVP 三卡变成 1×3 纵叠
- flow-chain 变成纵向（箭头旋转 90°）
- 演讲者按钮被隐藏

- [ ] **Step 3:** 浏览器验证（打印）

按 cmd+P 调出打印预览：
- 演讲者按钮、指示器消失
- 所有荣格卡都展开显示详情
- 名场面 8 panel 全部纵向列出（不是 tab）
- 盖洛普 chips 变成纵向列表，带 "· 成就 — 能量电池" 格式
- 颜色保留
- 排版整洁，无显著溢出

按 Esc 取消打印。

- [ ] **Step 4:** Commit

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "feat: 响应式手机布局 + 打印样式"
```

---

## Task 12: 完整验收 + 兼容性测试 + 最终 commit

**Files:** 无修改（除非发现 bug）

- [ ] **Step 1:** 按 spec §8 完整验收清单逐项跑

打开 `open /Users/apple/talks/destined-career/index.html`，逐项验证：

**功能正确性**：
- [ ] 双击 `index.html` 在 Chrome 打开，无 console 报错
- [ ] 在 Safari 打开（`open -a Safari index.html`），无报错
- [ ] 断网状态下页面所有内容（含公式）正常显示
- [ ] 所有 8 个 section 内容完整、顺序正确
- [ ] KaTeX 渲染的公式都正确显示，无 `$$...$$` 原始文本残留
- [ ] 阅读模式：从上滚到下，所有 section 入场动画各触发一次
- [ ] 演讲者模式按钮可点击进入；键盘 → ← ↑ ↓ PgUp PgDn Space 翻页；Esc 退出；Home/End 跳首末；P 切换
- [ ] 荣格 8 格：点任一格展开详情，其他自动收起；再点同格收起
- [ ] 荣格名场面：默认 Se，点其他 chip 切换
- [ ] 盖洛普 4 栏 chip：每栏独立工作，点 chip 显示对应详情
- [ ] 演讲者模式翻页时，目标 section 入场动画重新触发

**视觉**：
- [ ] 主蓝 #1F3D7A / 暖白 #FBF9F4 / 朱砂 #C1352F 三色严格按规范
- [ ] 1920×1080 投影分辨率（开发者工具响应式）显示无溢出
- [ ] iPhone Safari（开发者工具响应式）排版正确

**降级与兼容**：
- [ ] macOS 系统设置 → 辅助功能 → 显示 → 减少动态效果 开启 → 动画全部关闭
- [ ] cmd+P 打印预览：所有折叠展开、按钮隐藏、排版整洁
- [ ] 文件大小检查：

```bash
ls -lh /Users/apple/talks/destined-career/index.html
```

Expected: < 300KB

- [ ] **Step 2:** 修复发现的 bug（如果有）

如果有任何验收项失败，回到对应任务定位问题。常见小问题：
- KaTeX 渲染失败 → 检查 `$` 配对、`\` 是否被双重转义
- 演讲者模式键盘冲突 → 检查 `keydown` listener 没被其他 listener 拦截
- 入场动画在演讲者模式不重新触发 → 检查 `requestAnimationFrame` 双重包裹是否存在

修复后 commit：

```bash
cd /Users/apple/talks/destined-career && git add index.html && git commit -m "fix: 验收阶段发现的小问题"
```

如果无 bug，跳过此步。

- [ ] **Step 3:** 最终演练

完整走一遍明天的上台流程：

1. 关上所有 IDE / 终端
2. `open /Users/apple/talks/destined-career/index.html`
3. 按 F11 全屏
4. 按 P 进入演讲者模式
5. 按方向键从封面翻到收尾，每节大致看一眼，演练讲稿
6. 在 04 节展开 2-3 个荣格卡，切 2-3 个名场面 chip
7. 在 04 节展开 2-3 个盖洛普 chip
8. 翻到收尾，看核心公式 + Q&A 收尾页
9. 按 Esc 退出演讲者，cmd+R 刷新，确认回到阅读模式
10. 模拟会后分享：把 index.html 拖到 iMessage / 邮件 / AirDrop，发给自己另一台设备打开看

**通过标准**：整个流程 0 卡顿、0 报错、0 视觉异常。

- [ ] **Step 4:** 打 tag 标记交付版本

```bash
cd /Users/apple/talks/destined-career && git tag -a v1.0 -m "天选事业分享会 v1.0 交付" && git log --oneline
```

---

## 备用方案：如果时间紧

如果实施过程中遇到时间紧迫（比如已经凌晨 2 点还没做完 Task 7），按优先级砍：

1. **可砍 Task 11 的打印样式**：明天不需要打印。手机响应式如果排版破了也能凑合。
2. **可砍 Task 10 的盖洛普 chip 交互**：让 chip 始终显示 `name (desc)` 即可，台上不需要点开。
3. **可砍 Task 9 的入场动画**：删掉 IntersectionObserver，section 始终 opacity 1。
4. **不可砍**：Task 0-6（内容 + KaTeX）+ Task 8 演讲者模式（核心场景需要）。

---

## Self-Review Notes（实施者无需关注，写计划者的自检记录）

- ✅ Spec coverage：§1–§8 全部映射到 Task 0–12
- ✅ §9 (a)/(c)/连线图 默认值已写入计划顶部和对应 Task
- ✅ §3 8 个 section → Task 1 占位 + Task 2/3/4/5 内容填充
- ✅ §4 两种模式 → Task 8
- ✅ §5.1/5.2 点击交互 → Task 9/10
- ✅ §5.3 入场动画 → Task 7
- ✅ §5.4 KaTeX → Task 6
- ✅ §6 视觉系统 → Task 1 tokens + 每个内容 Task 追加 CSS
- ✅ §6.5/6.6 响应式 + 打印 → Task 11
- ✅ §8 验收清单 → Task 12
- ✅ 无 TBD / TODO 占位
- ✅ 类型 / class 名一致（.jung-cell / .scene-tab / .chip / .presenter 全程统一）
