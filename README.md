# ⏳ Time Planner — Claude Skill

> 用自然语言管理时间。添加任务、制定每日规划、追踪习惯、预测拖延——说句话就行。

[English](#english) | [中文](#中文)

---

## 中文

---

### 这个 Skill 解决什么问题？

任务管理工具很多，但**操作太麻烦、学习成本太高**：

- 加一个任务要填七八个字段，比做任务本身还累
- 做了计划从来不执行，因为不知道哪个应该先做
- 不知道自己到底花了多少时间，预估永远不准
- 任务一多就焦虑，不知道该砍哪个、该加哪个
- 大任务拖了一个月没动，因为没拆开

这个 Skill 的核心思路是：**你只管用自然语言说出要做什么，Claude 负责在后台管理一切，并把结果用人性化的方式展示给你。**

---

### 能做什么？

| 功能 | 触发方式 | 说明 |
|------|---------|------|
| **添加任务** | 「帮我添加任务：[粘贴任务内容]」 | AI 自动分析提取标题/优先级/耗时/标签/截止时间，确认即创建 |
| **查看任务** | 「今天有什么任务」「高优先级的」 | 按状态、优先级过滤，自动排序 |
| **修改状态** | 「XX 做完了」「开始做 XX」 | 一句话改状态，自动匹配任务 |
| **删除任务** | 「删掉 XX 任务」 | 展示详情并确认后删除 |
| **每日规划** | 「帮我规划：[需求]」 | 自动识别日期时间段，不足时追问，拓扑排序 + 偏差修正 + 时间块 |
| **复盘记录** | 「XX 实际花了 XX 分钟」 | 记录偏差，更新精力档案，越用越准 |
| **精力档案** | 「看看我的偏差报告」 | 按标签展示历史偏差率 |
| **过载保护** | 规划时自动触发 | 超每日上限自动筛选最重要任务 |
| **依赖管理** | 「XX 要先完成 YY 才能开始」 | 添加前置依赖，自动检测循环，识别 Boss 阻塞 |
| **对话风格** | 「切换成游戏模式」 | 5 种风格一键切换：默认/严格/鼓励/极简/游戏 |
| **智能预测** | 「帮我预测一下会不会延期」 | 基于历史数据预测耗时和拖延风险 |
| **风险扫描** | 「扫描一下拖延风险」 | 全局扫描高风险任务 |
| **拆分大任务** | 「这个任务太大了帮我拆开」 | 自动拆成 调研→整理→输出→检查 4 阶段 |
| **成长统计** | 「看看我的数据」 | 累计完成任务、Boss击败、连续天数 |
| **成就系统** | 「我的成就解锁情况」 | 7 项游戏化成就追踪 |

---

### 安装方法

1. 打开 [Claude.ai](https://claude.ai) → 进入 Skills 设置
2. 将此仓库下载或克隆到 skills 目录
3. 从 examples 目录复制示例数据文件并初始化
4. 在任意对话中用自然语言触发即可

或者直接用 Claude Code CLI：

```bash
cd ~/.claude/skills/
git clone git@github.com:F416-Y/time-planner.git
cd time-planner

# 从示例文件创建你的数据文件
cp examples/tasks.example.json tasks.json
cp examples/energy_profile.example.json energy_profile.json
cp examples/achievements.example.json achievements.json

# 然后编辑三个文件，把示例数据替换为空初始值即可
```

---

### 快速上手

#### 第一步：初始化数据文件

把 `examples/` 下的三个示例文件复制到 skill 根目录，去掉 `.example` 后缀，将内容改为空初始值：

```
tasks.json          →  {"tasks": []}
energy_profile.json →  见示例文件中的空结构
achievements.json   →  见示例文件中的空结构
```

#### 第二步：添加你的第一个任务

对 Claude 说：

```
帮我添加任务：[粘贴你的任务内容]
```

Claude 会自动分析内容，提取标题、优先级、预估耗时、标签、截止时间等所有字段，展示分析结果，你确认后即可创建。你还可以对分析结果做修正，比如「预估改 45 分钟」。

#### 第三步：生成每日规划

```
帮我规划：[粘贴你的需求]
```

Claude 会读取你的精力偏差档案，自动修正预估耗时，按优先级和依赖关系排序，生成时间块。

#### 第四步：完成任务后复盘

```
XX 做完了，实际花了 XX 分钟
```

Claude 会记录偏差、更新你的精力档案、可能解锁新成就。复盘越多，规划越准。

---

### 对话示例

```
用户：「帮我添加任务：今天下午Ave Mujica武道馆公演前最后排练，祥子要改新曲吉他编曲，高优先，估计3小时」
AI ：「分析结果：
      标题: Ave Mujica 武道馆公演前最后排练 | 🔴 高 | 预估 180 min | 截止 今天 20:00 | 标签 乐队/练习·人际
      📝 详情: 和祥子、海铃、喵梦、初华在排练室集合，修改新曲第三段编曲
      ✅ 创建吗？」
用户：「确认」
AI ：「已创建 ✅ Ave Mujica 武道馆公演前最后排练 [20260514-mtm-04]」

用户：「帮我添加任务：晚上把今天新摘的黄瓜包好，明天带给素世」
AI ：「分析结果：
      标题: 把今天新摘的黄瓜包好，明天带给素世 | 🟢 低 | 预估 20 min | 标签 人际
      📝 详情: 挑了今天长得最好的两根黄瓜，用报纸包好系上浅绿色丝带。昨晚也想过送但没送出去
      ✅ 创建吗？」
用户：「预估改 15 分钟吧……包个黄瓜不用那么久」
AI ：「已修正，确认：把今天新摘的黄瓜包好，明天带给素世 | 🟢 低 | 预估 15 min | 标签 人际」
用户：「确认」
AI ：「已创建 ✅」

用户：「帮我规划今天」
AI ：「📅 今日规划 (09:00 - 22:00)
      09:00 - 14:00  🔴 月之森晨间课程  300'
      14:00 - 15:00  🔴 Music Nova 武道馆专访  90'→修正112'
      15:00 - 15:30  🟡 园艺部给黄瓜浇水  15'
      15:30 - 18:30  🔴 Ave Mujica 排练  180'
      ...
      ⚠️ Boss 阻断: 「与 Mortis 达成和解」阻塞了「祥子补习」，建议优先处理」

用户：「Music Nova 武道馆专访做完了，实际花了 117 分钟」
AI ：「📊 复盘完成！预估 90' 实际 117'，偏差 +27 分钟 (+30%)
      📈 标签「演出/访谈」偏差率更新：+25%
      🏆 新成就解锁：神射手！」

用户：「看看我的数据」
AI ：「📊 累计完成 12 个任务 | 击败 3 个 Boss 🐉 | 连续 2 天 🔥 | 成就 2/7」

用户：「切换成 doubao 模式」
AI ：「🥒 小睦，豆包模式已开启。今天也要默默地努力哦。」
```

---

### 适合谁用？

- 任务多而且杂、需要 AI 帮忙排优先级的人
- 经常低估耗时、想通过复盘提高预估准确性的人
- 有相互依赖的复杂任务链、需要发现「卡点」的人
- 大任务拖延症、需要自动拆分成小步骤的人
- 喜欢游戏化激励、靠成就系统保持动力的人

---

### 项目结构

```
time-planner/
├── skill.md                   # AI 行为定义
├── task_manager.py            # 后端 CLI 脚本
├── README.md
├── examples/                  # 示例数据文件
│   ├── tasks.example.json
│   ├── energy_profile.example.json
│   └── achievements.example.json
├── tasks.json
├── energy_profile.json
└── achievements.json
```

---

### 注意事项

- 精力偏差档案需要累计至少 3 次复盘才开始有明显修正效果
- `examples/` 目录下是虚构的示例数据，仅供参考数据结构
- 默认语言：中文
- 本人能力有限，很多功能可能还不够完善，欢迎大佬们提 Issue / PR 指点 🙏

---

### 许可

MIT License

---

## English

### What problem does this Skill solve?

Task management tools are everywhere, but they're **a chore to use**:

- Adding a task means filling out half a dozen fields — more work than the task itself
- Plans get made but never followed, because nothing tells you what to do first
- You have no idea how long things actually take, so estimates are always off
- Too many tasks → anxiety. Which to cut? Which to prioritize?
- Big tasks sit untouched for weeks because nobody broke them down

The core idea: **You speak in natural language. Claude handles everything behind the scenes and presents results in a human-friendly way.**

---

### What can it do?

| Feature | How to trigger | Description |
|---------|---------------|-------------|
| **Add task** | "Add a task: [paste content]" | AI auto-analyzes content, extracts all fields, confirm to create |
| **View tasks** | "What's on my plate today?" | Filter by status/priority, auto-sorted |
| **Update status** | "XX is done", "Starting XX" | One sentence to mark complete or in-progress |
| **Delete task** | "Remove XX" | Shows details, confirms before deleting |
| **Daily plan** | "Plan my day: [paste details]" | Auto-detects date/time range, topo-sort + deviation correction + time blocks |
| **Review** | "XX actually took XX minutes" | Records deviation, updates energy profile, gets smarter over time |
| **Energy profile** | "Show my deviation report" | Per-tag historical accuracy stats |
| **Overload protection** | Auto-triggered during planning | Filters to most important tasks when exceeding daily limit |
| **Dependencies** | "XX can't start until YY is done" | Add prerequisites, cycle detection, Boss task identification |
| **Style switching** | "Switch to game mode" | 5 styles: default / strict / encourage / concise / playful |
| **Smart prediction** | "Predict if this will be delayed" | Estimates real duration and procrastination risk from history |
| **Risk scan** | "Scan for procrastination risks" | Flags high-risk tasks across all incomplete items |
| **Task splitting** | "This task is too big, break it down" | Auto-splits into 4 phases: research → organize → output → review |
| **Stats** | "Show my stats" | Tasks completed, bosses defeated, streak days |
| **Achievements** | "What achievements have I unlocked?" | 7 gamified achievement badges |

---

### Installation

Via Claude Code CLI:

```bash
cd ~/.claude/skills/
git clone git@github.com:F416-Y/time-planner.git
cd time-planner

# Create your data files from examples
cp examples/tasks.example.json tasks.json
cp examples/energy_profile.example.json energy_profile.json
cp examples/achievements.example.json achievements.json

# Edit the three files to replace example data with empty defaults
```

---

### Quick Start

**Step 1 — Initialize data files**

Copy the three example files from `examples/`, remove the `.example` suffix, and replace contents with empty defaults:

```
tasks.json          →  {"tasks": []}
energy_profile.json →  see empty structure in example file
achievements.json   →  see empty structure in example file
```

**Step 2 — Add your first task**

```
Add a task: [paste your task content]
```

Claude auto-analyzes the content, extracts title, priority, estimate, tags, and deadline, then presents the analysis for your confirmation. You can also correct any field, e.g. "change estimate to 45 min".

**Step 3 — Generate a daily plan**

```
Plan my day: [paste your requirements]
```

Claude reads your energy profile, corrects estimates with historical deviation data, and generates time blocks sorted by priority and dependencies.

**Step 4 — Review after completing**

```
XX is done, actually took XX minutes
```

Claude records the deviation, updates your energy profile, and may unlock achievements. The more you review, the more accurate predictions become.

---

### Conversation example

```
User: "Add a task: Final rehearsal before Ave Mujica's Budokan concert this afternoon, Sakiko wants to rework the guitar arrangement, high priority, about 3 hours"
AI  : "Analysis:
      Title: Ave Mujica final rehearsal before Budokan | 🔴 High | Est. 180 min | Due today 20:00 | Tags band/practice·social
      📝 Details: Gather with Sakiko, Umiri, Nyamu, and Uika at the studio to rework the third section's arrangement
      ✅ Create?"
User: "Yes"
AI  : "Created ✅ Ave Mujica final rehearsal before Budokan [20260514-mtm-04]"

User: "Add a task: Wrap the cucumbers I picked today to give to Soyo tomorrow"
AI  : "Analysis:
      Title: Wrap fresh cucumbers for Soyo | 🟢 Low | Est. 20 min | Tags social
      📝 Details: Picked the two best cucumbers from the garden, wrapped in newspaper with a light green ribbon. Tried to give them yesterday but couldn't go through with it
      ✅ Create?"
User: "Change estimate to 15 min... it's just wrapping cucumbers"
AI  : "Updated. Confirm: Wrap fresh cucumbers for Soyo | 🟢 Low | Est. 15 min | Tags social"
User: "Confirm"
AI  : "Created ✅"

User: "Plan my day"
AI  : "📅 Today (09:00 - 22:00)
      09:00 - 14:00  🔴 Tsukinomori morning classes  300'
      14:00 - 15:00  🔴 Music Nova Budokan interview  90'→adj.112'
      15:00 - 15:30  🟡 Water cucumbers at gardening club  15'
      15:30 - 18:30  🔴 Ave Mujica rehearsal  180'
      ...
      ⚠️ Boss blocker: 'Reconcile with Mortis' is blocking 'Sakiko tutoring session', prioritize this"

User: "Music Nova interview is done, actually took 117 minutes"
AI  : "📊 Review done! Est. 90' Actual 117', deviation +27 min (+30%)
      📈 Tag 'interview' deviation updated: +25%
      🏆 Achievement unlocked: Sharpshooter!"

User: "Show my stats"
AI  : "📊 12 tasks completed | 3 Bosses defeated 🐉 | 2-day streak 🔥 | 2/7 achievements"

User: "Switch to doubao mode"
AI  : "🥒 Mutsumi, doubao mode activated. Let's work hard in silence today too."
```

---

### Who is this for?

- People with lots of tasks who need AI to sort priorities
- Chronic underestimators who want to improve through review
- Anyone with complex dependency chains who needs to find blockers
- Big-task procrastinators who need automatic breakdowns
- People motivated by gamification and achievement systems

---

### Project structure

```
time-planner/
├── skill.md                   # AI behavior definition
├── task_manager.py            # Backend CLI script
├── README.md
├── examples/                  # Example data files
│   ├── tasks.example.json
│   ├── energy_profile.example.json
│   └── achievements.example.json
├── tasks.json
├── energy_profile.json
└── achievements.json
```

---

### Notes

- Energy deviation profile needs at least 3 reviews before corrections become noticeable
- `examples/` contains fictional sample data for reference only
- Default language: Chinese
- I'm still learning — many features may not be perfect. Issues / PRs are more than welcome 🙏

---

### License

MIT License
