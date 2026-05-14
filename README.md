# Time Planner

个人时间规划助手 — 基于 Python CLI 的智能任务管理与每日时间规划工具。支持精力偏差修正、过载保护、Boss任务识别、拖延预警、任务拆分和游戏化成就系统。

## 功能清单

| 模块 | 功能 | 说明 |
|------|------|------|
| 一 | 添加任务 | 自动生成 ID，支持优先级、预估耗时、截止时间、标签 |
| 二 | 列出任务 | 按状态/优先级过滤，自动排序展示 |
| 三 | 修改任务 | 编辑标题、状态、优先级、截止时间等全部字段 |
| 四 | 删除任务 | 指定 ID 删除，展示确认 |
| 五 | 时间规划 | 自动生成今日时间块，拓扑排序 + 贪心选取 |
| 六 | 复盘记录 | 记录实际耗时，自动计算偏差，更新精力档案 |
| 七 | 精力档案 | 按标签查看历史偏差率，为修正提供数据基础 |
| 八 | 过载保护 | 设置每日工作时长上限，超载时自动筛选可行任务 |
| 九 | 依赖链 | 任务依赖关系管理，DFS 循环检测，Boss 任务自动识别 |
| 十 | 智能预测 | 基于历史数据预测任务耗时、置信度和拖延风险（v2） |
| 十一 | 任务拆分 | 大任务自动拆分为 4 阶段子任务，链式依赖（v2） |
| 十二 | 成长追踪 | 7 项游戏化成就，连续天数追踪，统计面板（v2） |

## 对话风格

支持 5 种可切换的对话风格：

| 风格 | 命令 | 说明 |
|------|------|------|
| `default` | `--set-style default` | 友好专业（默认） |
| `strict` | `--set-style strict` | 严厉直接，铁血教练 |
| `encourage` | `--set-style encourage` | 温暖共情，成长导师 |
| `concise` | `--set-style concise` | 极简高效，纯数据 |
| `playful` | `--set-style playful` | 游戏化冒险风格 |

## 快速开始

### 1. 初始化数据文件

```bash
cd time-planner/

# 创建空的 tasks.json
echo '{"tasks": []}' > tasks.json

# 创建空的 energy_profile.json
cat > energy_profile.json << 'EOF'
{
  "daily_limit_hours": 8.0,
  "style": "default",
  "tag_profiles": {},
  "global_avg_deviation_rate": 0.0,
  "global_avg_procrastination": 0.0,
  "global_avg_deadline_adjusted": 0.0,
  "total_reviews": 0,
  "reviews": []
}
EOF

# 创建空的 achievements.json
cat > achievements.json << 'EOF'
{
  "achievements": [],
  "stats": {
    "total_tasks_completed": 0,
    "total_bosses_defeated": 0,
    "current_streak": 0,
    "longest_streak": 0,
    "last_completed_date": null,
    "overload_trigger_count": 0
  }
}
EOF
```

> 或者参考 `examples/` 目录下的示例文件，复制后去掉 `.example` 后缀即可使用。

### 2. 添加第一个任务

```bash
python task_manager.py add --title "试用 Time Planner" --priority high --estimated-minutes 30 --tags "学习"
```

### 3. 生成今日规划

```bash
python task_manager.py plan --day-start 09:00 --day-end 18:00
```

## 命令参考

### 任务管理
```bash
python task_manager.py add    --title "..." [--priority high|medium|low] [--estimated-minutes N] [--deadline "ISO时间"] [--tags "标签1,标签2"] [--description "..."]
python task_manager.py list   [--status pending|in_progress|completed|cancelled] [--priority high|medium|low]
python task_manager.py modify <task_id> [--title "..."] [--status ...] [--priority ...] [--deadline "..."] [--clear-deadline] [--tags "..."]
python task_manager.py delete <task_id>
```

### 规划与复盘
```bash
python task_manager.py plan    [--day-start 09:00] [--day-end 18:00] [--force] [--dynamic]
python task_manager.py review  <task_id> <actual_minutes>
```

### 依赖管理
```bash
python task_manager.py deps <task_id>              # 查看依赖关系
python task_manager.py deps <task_id> --add <dep_id>    # 添加前置依赖
python task_manager.py deps <task_id> --remove <dep_id> # 移除前置依赖
```

### 精力档案与设置
```bash
python task_manager.py energy                      # 查看精力偏差档案
python task_manager.py limit                       # 查看每日工作时长上限
python task_manager.py limit --set 6               # 设置每日上限为6小时
python task_manager.py config                      # 查看当前配置
python task_manager.py config --set-style playful  # 切换对话风格
```

### 预测与预警（v2）
```bash
python task_manager.py predict <task_id>           # 预测任务耗时和拖延风险
python task_manager.py warn                        # 扫描所有任务中的高风险项
```

### 任务拆分（v2）
```bash
python task_manager.py break <task_id>             # 拆分超过120分钟的大任务
```

### 成长追踪（v2）
```bash
python task_manager.py stats                       # 查看累计统计数据
python task_manager.py achievements                # 查看全部7项成就及解锁状态
```

## 数据结构

详细的数据模型文档见 [skill.md](skill.md)。

### 7 项游戏化成就

| 成就 | 解锁条件 |
|------|----------|
| 🏆 初次胜利 | 完成第一个任务 |
| 🔥 连续三日 | 连续 3 天有完成任务 |
| ⚔️ Boss杀手 | 累计击败 5 个 Boss 任务 |
| 🎯 神射手 | 复盘偏差率低于 10% |
| ⭐ 全勤一周 | 连续 7 天有完成任务 |
| 🛡️ 拒绝过载 | 累计触发 3 次过载保护 |
| 📈 偏差改善者 | 近 7 天平均偏差率下降超过 20% |

## 项目结构

```
time-planner/
├── task_manager.py          # 核心 CLI 脚本（15条命令）
├── skill.md                 # AI Agent 技能定义文档
├── .gitignore               # 隐私保护规则
├── README.md                # 本文件
├── examples/                # 示例数据文件（可安全上传）
│   ├── tasks.example.json
│   ├── energy_profile.example.json
│   └── achievements.example.json
├── tasks.json               # 个人任务数据（不追踪）
├── energy_profile.json      # 精力偏差档案（不追踪）
└── achievements.json        # 成就统计记录（不追踪）
```

## 隐私提醒

**以下文件包含你的个人数据，已加入 `.gitignore`，永远不会被 Git 追踪：**

| 文件 | 包含的私人信息 |
|------|----------------|
| `tasks.json` | 真实任务标题、描述、截止时间、优先级、标签 |
| `energy_profile.json` | 个人精力偏差率、工作时长、复盘历史 |
| `achievements.json` | 任务完成记录、成就解锁记录、连续天数 |

如果你 fork 本项目后想要分享自己的使用体验，请确认上述文件没有被包含在你的公开仓库中。可以使用 `examples/` 目录下的示例文件代替。

## 系统要求

- Python 3.7+
- 无外部依赖（仅使用标准库）

## 许可

MIT License
