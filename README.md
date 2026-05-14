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
| **添加任务** | 「帮我加一个任务，XX，优先级XX，XX分钟」 | 自动提取标题/优先级/耗时/截止时间/标签 |
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
3. 初始化你的个人数据文件
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
帮我加一个任务，XX，优先级XX，预估 XX 分钟，XX时间前截止，标签是XX
```

Claude 会自动提取所有信息，生成任务并展示摘要，确认后即可创建。

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
用户：「帮我加一个任务，XX，优先级XX，预估 XX 分钟，XX时间前截止，标签是XX」
AI ：「好的，确认一下：
      XX | 🔴 高 | XX 分钟 | 截止 XX | 标签 XX
      ✅ 创建吗？」
用户：「确认」

用户：「帮我规划今天」
AI ：「📅 今日规划 (09:00 - 18:00)
      09:00 - 10:30  🔴 XX  预估XX'→修正XX'
      10:30 - 11:00  🟡 XX  XX'
      ...

用户：「XX 做完了，实际花了 XX 分钟」
AI ：「📊 复盘完成！预估 XX' 实际 XX'，偏差 +XX 分钟 (+XX%)
      🏆 新成就解锁：XX！」

用户：「看看我的数据」
AI ：「📊 累计完成 15 个任务 | 击败 4 个 Boss 🐉 | 连续 5 天 🔥 | 成就 3/7」

用户：「切换成游戏模式」
AI ：「⚔️ 冒险者，游戏模式已开启！今日副本攻略准备好了吗？」
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
- `tasks.json`、`energy_profile.json`、`achievements.json` 包含个人数据，已通过 `.gitignore` 排除
- 默认语言：中文

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
| **Add task** | "Add a task: XX, priority XX, XX min" | Auto-extracts title, priority, estimate, deadline, tags |
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
Add a task: XX report, priority XX, estimated XX minutes, due by XX, tags: XX
```

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
User: "Add a task: XX, priority XX, estimated XX min, due by XX, tags: XX"
AI  : "Confirming:
      XX | 🔴 High | XX min | Due XX | Tags XX
      ✅ Create?"
User: "Yes"

User: "Plan my day"
AI  : "📅 Today (09:00 - 18:00)
      09:00 - 10:30  🔴 XX  est.XX'→adj.XX'
      10:30 - 11:00  🟡 XX  XX'
      ...

User: "XX is done, actually took XX min"
AI  : "📊 Review done! Est. XX' Actual XX', deviation +XX min (+XX%)
      🏆 Achievement unlocked: XX!"

User: "Show my stats"
AI  : "📊 15 tasks completed | 4 Bosses defeated 🐉 | 5-day streak 🔥 | 3/7 achievements"

User: "Switch to game mode"
AI  : "⚔️ Adventurer, game mode activated! Ready for today's quest?"
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
- `tasks.json`, `energy_profile.json`, `achievements.json` contain personal data and are excluded via `.gitignore`
- Default language: Chinese

---

### License

MIT License
