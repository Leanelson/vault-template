# 共享知识库 / Shared Knowledge Vault

> 多个 OpenClaw Agent 共用的知识同步仓库
> Multi-Agent shared knowledge vault with Obsidian + GitHub sync

## 目录结构 / Structure

```
vault/
├── 00-Home/          # 首页 & 导航
├── 01-Projects/      # 项目笔记
├── 02-People/        # 人物资料
├── 03-Tasks/         # 任务追踪
├── 04-Knowledge/     # 知识库 (概念、笔记、参考)
├── 05-Meetings/      # 会议记录
├── 06-Templates/     # 模板库
└── .obsidian/        # Obsidian 配置
```

## 同步规则 / Sync Rules

- **Commit 前先 pull** — 避免覆盖他人改动
- **原子 commit** — 每个改动只做一件事，commit message 清晰
- **Wikilink优先** — 使用 `[[笔记名]]` 而非硬链接
- **.frontmatter** — 所有笔记带标准 frontmatter

## Agent 协作工作流 / Agent Collaboration

1. Agent 完成重要工作 → 主动写笔记到对应目录
2. 每次 session 结束前 → `git add . && git commit -m "agent: 简述本次工作"`
3. 定期 `git pull --rebase` 合并他人改动
4. 冲突 → 保留双方内容，注释原始来源

## Obsidian 插件推荐 / Recommended Plugins

- **Dataview** — 查询笔记元数据
- **Templater** — 自动化模板
- **Git** — Obsidian 内置 Git 插件
- **Recent Files** — 快速访问最近笔记