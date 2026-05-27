---
type: workflow
title: 多 Agent 知识同步工作流
created: {{date:YYYY-MM-DD}}
tags: [🤖-agent, 🔄-sync, 📋-workflow]
---

# 🔄 多 Agent 知识同步工作流

> 多个 OpenClaw Agent 如何通过共享 Vault + GitHub 同步知识

## 核心机制

```
┌─────────────┐     GitHub      ┌─────────────┐
│  Agent A    │◄── push/pull ──►│  vault-     │
│  (本地)     │                 │  template   │
├─────────────┤                 │  repo       │
│  Agent B    │◄── push/pull ──►│  (GitHub)   │
│  (远程/其他)│                 │             │
└─────────────┘                 └─────────────┘
```

## 同步规则

### 1. 每次工作完成后写笔记
```bash
cd ~/vault-template
# Agent 主动记录工作内容到对应目录
git add . && git commit -m "agent: [agent-name] 完成 XXX 任务"
git push mirror master  # 通过 gh-proxy 推送到 GitHub
```

### 2. 开始工作前拉取最新
```bash
git pull mirror master --rebase  # 保持历史线性
```

### 3. 冲突处理原则
- **不删除**他人内容，只追加
- 冲突时保留双方内容，用注释标注来源
- 例：`<!-- 来源: agent-B @ 2026-05-27 -->`

## 目录分工约定

| 目录 | 用途 | 写入权限 |
|------|------|----------|
| `00-Home/` | 首页索引 | 所有 Agent |
| `01-Projects/` | 项目笔记 | 负责项目的 Agent |
| `02-People/` | 人物资料 | 所有 Agent |
| `03-Tasks/` | 任务追踪 | 被分配任务的 Agent |
| `04-Knowledge/` | 知识沉淀 | 所有 Agent |
| `05-Meetings/` | 会议纪要 | 会议相关 Agent |
| `06-Templates/` | 模板库 | 仅维护者 |

## Agent 工作记录格式

所有 Agent 在 `04-Knowledge/agent-worklogs/` 下记录工作：

```markdown
---
type: agent-worklog
agent: "萝卜头"
session: "main-2026-05-27"
created: 2026-05-27 09:30
---

# 🤖 Agent 工作记录

## 本次工作摘要
- 完成了 XXX 任务
- 关键决策：...

## 关键产出
- [[相关笔记链接]]

## 待跟进
- [ ] ...
```

## 自动同步 Cron（可选）

在 OpenClaw 中设置定时同步：

```javascript
// 每小时自动拉取最新 + 自动推送本地更改
cron.add({
  name: "vault-sync",
  schedule: { kind: "cron", expr: "0 * * * *", tz: "Asia/Shanghai" },
  payload: { kind: "agentTurn", message: "执行 git pull && git push" },
  sessionTarget: "isolated"
})
```

## Obsidian 端配置

1. 下载 Obsidian（https://obsidian.md）
2. 安装后打开库 → 选择 `~/vault-template`
3. 推荐插件：
   - **Templater** — 自动化模板插入
   - **Dataview** — 元数据查询
   - **Git** — 内置 Git 插件（需额外安装社区插件）
4. 启用 Obsidian Git 插件配置自动提交

## 快速检查清单

- [ ] `~/vault-template` 已克隆/存在
- [ ] 已配置 Git 全局身份
- [ ] 已添加 `mirror` remote（gh-proxy 方式）
- [ ] 新 Agent 加入时 clone 仓库即可开始协作

## GitHub 仓库

> 替换 `USERNAME` 为你的 GitHub 用户名

```bash
# 克隆（任何新设备/新 Agent）
git clone https://gh-proxy.com/github.com/Leanelson/vault-template.git ~/vault-template

# 已有时更新
cd ~/vault-template && git pull mirror master
```