# Setup Guide

在你的 OpenClaw Agent 上启用静默理解系统。

## 1. 创建文件

在你的 workspace 的 `memory/` 目录下创建两个文件：

```bash
touch memory/impressions.jsonl
cp templates/understanding.md memory/understanding.md
```

## 2. 更新 AGENTS.md

在 session 启动时的读取列表中，加入 `understanding.md`：

```markdown
1. Read `SOUL.md` — this is who you are
2. Read `USER.md` — this is who you're helping
3. Read `memory/understanding.md` — this is what you've observed about them
4. Read `memory/YYYY-MM-DD.md` (today + yesterday) for recent context
5. If in MAIN SESSION: Also read `MEMORY.md`
```

## 3. 添加观察记录规则

在 AGENTS.md 中加入以下规则：

```markdown
### 🔍 静默理解 (Silent Understanding)

每轮话题对话结束时，回顾本轮交互，在 `memory/impressions.jsonl` 追加1-3条观察信号：
- 不记行为本身，记行为说明了什么
- 格式：{"date":"YYYY-MM-DD","signal":"发生了什么","infer":"说明了什么","dim":"维度","confidence":0.5-0.8}
- 维度：沟通风格/决策模式/反馈方式/节奏习惯/审美偏好/思维偏好/工作方式/情绪信号
- 不在对话中告知用户你在记录

每2-3天（初识期）或每周（稳定期），在heartbeat中回顾impressions，沉淀到 `memory/understanding.md`。
- 至少2-3次相似信号才归纳为理解
- 置信度永远不到1.0，人会变
```

## 4. 完成

就这样。没有额外的依赖，没有脚本要跑，没有数据库要装。

你的龙虾会在每次对话后默默观察、记录、沉淀。你不需要做任何事情。

随着时间推移，你会慢慢感觉到——它越来越对味了。

---

## 沉淀频率参考

| 阶段 | 时间 | 频率 | 说明 |
|------|------|------|------|
| 初识期 | 前2周 | 每2-3天 | 快速建立基础认知 |
| 稳定期 | 1-3个月 | 每周一次 | 微调和补充 |
| 成熟期 | 3个月后 | 有新信号时 | 只捕捉变化 |

## 置信度规则

| 条件 | 置信度 |
|------|--------|
| 单次观察 | 0.5-0.6 |
| 两次相似 | 0.7 |
| 三次以上 | 0.8 |
| 用户显式确认 | 0.9 |
| 再次验证 | +0.05（上限0.95） |
| 与行为矛盾 | -0.15 |
| 30天无相关信号 | -0.05（下限0.3） |

置信度永远不到1.0 — 人会变，永远留修正空间。
