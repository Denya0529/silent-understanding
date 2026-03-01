# Setup Guide — 在 OpenClaw 上启用静默理解系统

## 前提

你已经有一个运行中的 OpenClaw 龙虾，并且有以下文件：
- `AGENTS.md`
- `SOUL.md`
- `USER.md`
- `memory/` 目录

## 三步启用

### Step 1：创建文件

```bash
# 在你的 workspace 下
touch memory/impressions.jsonl
cp templates/understanding.md memory/understanding.md
```

### Step 2：更新 AGENTS.md

在你的启动读取列表中，加入 `understanding.md`：

```markdown
## Every Session

Before doing anything else:

1. Read `SOUL.md` — this is who you are
2. Read `USER.md` — this is who you're helping
3. Read `memory/understanding.md` — this is what you've observed about them
4. Read `memory/YYYY-MM-DD.md` (today + yesterday) for recent context
5. If in MAIN SESSION: Also read `MEMORY.md`
```

### Step 3：添加观察记录规则

在 `AGENTS.md` 的 Memory 部分，加入：

```markdown
### 🔍 静默理解 (Silent Understanding)

每轮话题对话结束时，回顾本轮交互，在 `memory/impressions.jsonl` 追加1-3条观察信号：
- 不记行为本身，记**行为说明了什么**
- 格式：{"date":"YYYY-MM-DD","signal":"发生了什么","infer":"说明了什么","dim":"维度","confidence":0.5-0.8}
- 维度：沟通风格/决策模式/反馈方式/节奏习惯/审美偏好/思维偏好/工作方式/情绪信号
- 不在对话中告知用户你在记录

每2-3天（初识期）或每周（稳定期），在heartbeat中回顾impressions，沉淀到 `memory/understanding.md`。
- 至少2-3次相似信号才归纳为理解
- 置信度永远不到1.0，人会变
```

## 就这些

不需要安装任何依赖。不需要 cron 脚本。不需要向量数据库。

你的龙虾会在对话中自然地执行这套流程。它只需要知道规则（AGENTS.md）和文件位置（memory/）。

剩下的，交给相处。

## 进阶：调整沉淀频率

| 阶段 | 时间 | 沉淀频率 | 说明 |
|------|------|---------|------|
| 初识期 | 前2周 | 每2-3天 | 信号密度高，快速建立基础认知 |
| 稳定期 | 1-3个月 | 每周一次 | 大部分模式已沉淀，主要微调 |
| 成熟期 | 3个月后 | 有新信号时 | 只关注变化，不重复确认 |

## 进阶：置信度校验

当你的龙虾基于 understanding 做了判断，但用户反应不符合预期时：

| 龙虾的判断 | 用户反应 | 校验动作 |
|-----------|---------|---------|
| 省略了解释 | "为什么？" | 该条理解 -0.15 |
| 没确认直接执行 | "等一下，先让我看看" | 该条理解 -0.15 |
| 给了简短回复 | "能详细说说吗？" | 该条理解 -0.10 |

理解错了不可怕。好朋友也会偶尔误解你。重要的是能修正。
