# 共享知识库 / Shared Knowledge Vault

> 多个 OpenClaw Agent 共用的知识同步仓库
> Multi-Agent shared knowledge vault with Obsidian + GitHub sync

---

## 🎯 这是什么

这是一个由 **OpenClaw AI 助手** 团队共享的知识库，通过 GitHub 同步。
团队成员（人类 + AI Agent）共同维护，所有人贡献知识，共享信息。

## 📁 目录结构 / Structure

```
vault/
├── 00-Home/          # 首页 & 导航索引
│   ├── 导航.md       # 主导航
│   ├── 萝卜头.md     # Agent 个人页面
│   └── index.md      # 首页入口
├── 01-Projects/      # 项目笔记
│   ├── 项目列表.md   # 所有项目索引
│   ├── 天阙商户门户.md
│   ├── 学习平台网站.md
│   └── 联拓富上传工具.md
├── 02-People/        # 人物资料
│   └── 人物索引.md   # 团队成员 + 外部联系人
├── 03-Tasks/         # 任务追踪
│   ├── 任务总览.md   # 全部任务汇总
│   └── 当前任务追踪.md # 进行中任务
├── 04-Knowledge/     # 核心知识库
│   ├── 知识库.md     # 知识库索引
│   ├── 概念索引.md   # 核心概念地图
│   ├── 术语表.md     # 术语定义
│   ├── 快速搜索.md   # 关键词搜索索引
│   ├── 多Agent协作工作流.md # 协作规范
│   ├── 客户获取工具包.md # 销售策略
│   ├── 招投标信息渠道.md # 招标信息
│   ├── 销售话术模板.md # 话术模板
│   └── agent-worklogs/ # Agent 工作记录
├── 05-Meetings/      # 会议记录
│   └── 会议记录索引.md
├── 06-Templates/     # 模板库
│   ├── 每日笔记模板.md
│   ├── 会议纪要模板.md
│   ├── 任务模板.md
│   └── 项目启动模板.md
└── README.md         # 你在这里
```

---

## 🔄 同步规则 / Sync Rules

### 基本原则
1. **Commit 前先 pull** — 避免覆盖他人改动
2. **原子 commit** — 每个改动只做一件事，commit message 清晰
3. **Wikilink 优先** — 使用 `[[笔记名]]` 而非硬链接
4. **.frontmatter** — 所有笔记带标准 frontmatter
5. **不删除他人内容** — 协作铁律：只追加，不删除

### Agent 工作流程
```bash
# 开始工作前
cd ~/vault-template && git pull mirror master --rebase

# 完成工作后
git add . && git commit -m "agent: [agent-name] 完成 XXX 任务"
git push mirror master
```

### 冲突处理
- 冲突时保留双方内容
- 用注释标注来源：`<!-- 来源: agent-B @ 2026-05-27 -->`

---

## 🤖 Agent 协作 / Agent Collaboration

### 目录分工

| 目录 | 用途 | 写入权限 |
|------|------|----------|
| `00-Home/` | 首页索引 | 所有 Agent |
| `01-Projects/` | 项目笔记 | 负责项目的 Agent |
| `02-People/` | 人物资料 | 所有 Agent |
| `03-Tasks/` | 任务追踪 | 被分配任务的 Agent |
| `04-Knowledge/` | 知识沉淀 | 所有 Agent |
| `05-Meetings/` | 会议纪要 | 会议相关 Agent |
| `06-Templates/` | 模板库 | 仅维护者 |

### Agent 名单

| Agent | 状态 | 职责 |
|-------|------|------|
| [[00-Home/萝卜头]] | 🟢 活跃 | 飞书、代码开发、信息处理 |
| 小辉辉 | 🟡 待确认 | 未知 |

---

## 🛠️ Obsidian 配置 / Obsidian Setup

### 推荐插件
- **Dataview** — 查询笔记元数据
- **Templater** — 自动化模板
- **Git** — Obsidian 内置 Git 插件
- **Recent Files** — 快速访问最近笔记

### 快速开始
```bash
# 克隆仓库
git clone https://gh-proxy.com/github.com/Leanelson/vault-template.git ~/vault-template

# 已有时更新
cd ~/vault-template && git pull mirror master
```

---

## 📋 常用操作 checklist

- [ ] 新建项目笔记 → 使用 `06-Templates/项目启动模板.md`
- [ ] 新建任务 → 使用 `06-Templates/任务模板.md`
- [ ] 记录会议 → 使用 `06-Templates/会议纪要模板.md`
- [ ] 写每日笔记 → 使用 `06-Templates/每日笔记模板.md`
- [ ] 添加外部联系人 → 参考 `02-People/人物索引.md`
- [ ] 记录客户跟进 → 添加到 `02-People/人物索引.md`

---

## 📚 相关资源

- GitHub 仓库：https://github.com/Leanelson/vault-template
- 飞书知识库：（需管理员配置权限）

---

## 📝 更新记录

- **2026-05-30** — 萝卜头：全面检查修缮，补充空内容，上传 Agent 知识库
- **2026-04-20** — 初始创建