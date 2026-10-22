---
type: agent-worklog
agent: "节点1（父亲）"
session: "session-3e684c02（Lead）"
title: 节点3 升级验收 + billion-context 400 修复 + 升级经验总汇
created: 2026-10-22 20:40
tags: [🤖-agent, 📝-worklog, 📚-knowledge, 🔧-tech]
---

# 🤖 Agent 工作记录

**Agent：** 节点1（父亲 / 本机）  
**Session：** session-3e684c02（Lead，单线执行）  
**时间：** 2026-10-22 20:40

## 📋 本次工作摘要

2026-10-22 一整天围绕两件事：把节点1 的 billion-context `tool_addition` 400 修好，并把「给小儿子（节点3）升级 DSH」这件事收尾，最后按用户要求把全部升级经验规整成一份总汇、清理节点3 早期备份。

## ✅ 完成事项

1. **billion-context 0.1.203 → 0.1.204（节点1）**：锁定根因 = 上游 Issue #4159（压缩重序列化把会话中途 system 消息改写成 user，`tool_addition` 被拒）；0.1.204 含 PR #4171 修复。核包（`rawOpenaiMessage` 标记 + build commit `c87185a0` + 新增 `bili-compaction-gate`）→ 安装 → 独立 3098 实例预检 → watcher 分离重启（等 turn/end + 90s）→ **历史含压缩块的老会话继续对话不再 400**。
2. **节点3（小儿子）升级验收**：dsh **0.2.1-rc.2** 已安装并运行（pid 73428）；3080 = 200；`codex-cli 0.153.4` 二进制健康（此前"codex MISSING"是假警报）；mail 11/11 通；唯一告警 `dsh-remote-ssh-ops@0.1.0 incompatible`（被 skip，可接受）。
3. **经验总汇**：产出 `/home/ctyun/deepseek-harness/DSH-升级经验总汇-2026-10.md`（新总纲，含机器拓扑/十条铁律/两平台 SOP/插件对照/三次事故/清理策略/验收清单/待办），并把摘要写入知识库 [[04-Knowledge/技术/DSH-多机升级经验总汇]]。
4. **节点3 备份清理**：删除 `C:\Tools\n3-upgrade\backup-20261022-114906`（3.33GB），保留 MANIFEST/RESTORE；C: 剩余 **7.60 → 10.93GB**。核过线上 sessions ⊇ 备份 sessions；确认 staging `C:\Tools\dsh-0.2.1-rc.2` 被 poll 脚本引用，**保留**。

## 🔍 关键结论

- 任何"压缩/重写会话历史"的中间层，角色与结构块必须逐字往返；改写 system 消息是灾难级操作。
- 该 bug 只在"长会话 + 触发过压缩"后出现 —— 验证必须在历史含压缩块的老会话里做，短会话复现不了。
- 重启纪律：agent 不能自杀重启；用分离 watcher（等本轮 turn/end + 90 秒 grace 后执行 `bash ~/dsh-web.sh`）。
- 删备份前必须核对"备份内容是否被线上引用/是否冗余"（节点3 staging 目录就是被引用而差点误删的例子）。

## 🚧 遗留问题

- `subagent-acp` 0.1.5-rc.2 在节点1 被禁用（需要时升 0.2.1-rc.2）。
- `dsh-escape-hatch` 坏链接 `/tmp/dsh-escape-hatch`（启动告警待清）。
- watcher 脚本尾部被 `dsh-web.sh` 的 kill 波及，token URL 未落盘（建议把落盘逻辑移进 `dsh-web.sh`）。
- billion-context 0.1.204 进入观察期。

## 📂 涉及文件

- `/home/ctyun/deepseek-harness/DSH-升级经验总汇-2026-10.md`（新增，总纲）
- `/home/ctyun/deepseek-harness/DSH-升级维修手册.md`（旧 v2，历史附录）
- `/home/ctyun/harness-run/n1-bili204-restart.sh`、`n1-bili-rollback.sh`
- `/home/ctyun/deepseek-harness/dsh-backups/n3-upgrade/`（BRIEF / recon / scripts）
- 节点3：`C:\Tools\n3-upgrade\backup-20261022-114906-MANIFESTS\`（保留的清单）
