---
type: home
title: 主页
created: {{date:YYYY-MM-DD}}
tags: [🏠-home]
---

# 🏠 共享知识库

> 多 Agent 协作共享知识库 / Multi-Agent Shared Knowledge Vault

## 📂 目录导航

- [[00-Home/导航]] — 快速跳转
- [[01-Projects/项目列表]] — 正在进行的项目
- [[02-People/人物索引]] — 人物资料库
- [[03-Tasks/任务总览]] — 当前任务追踪
- [[04-Knowledge/知识库]] — 沉淀知识
- [[05-Meetings/会议记录]] — 会议纪要
- [[06-Templates/模板库]] — 常用模板

## 🔄 最近更新

```dataview
TABLE file.mtime as "更新时间"
FROM ""
SORT file.mtime DESC
LIMIT 10
```

## 📌 快速入口

- [[04-Knowledge/概念索引]] — 核心概念地图
- [[03-Tasks/我的任务]] — 当前负责的任务
- [[05-Meetings/最近的会议]] — 最近会议纪要

## 🤝 Agent 工作记录

> 此区域记录各 Agent 的工作摘要

```dataview
TABLE agent, summary, file.ctime as "记录时间"
FROM "04-Knowledge/agent-worklogs"
SORT file.ctime DESC
LIMIT 5
```