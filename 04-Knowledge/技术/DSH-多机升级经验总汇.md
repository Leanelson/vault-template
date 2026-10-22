---
type: knowledge
title: DSH 多机升级经验总汇（2026-10）
created: 2026-10-22
tags: [📚-knowledge, 🔧-tech, 🤖-agent, 🖥️-computer-use]
source: dsh-session session-3e684c02（节点1 Lead）+ 节点3 升级会话
---

# DSH 多机升级经验总汇（2026-10）

> 汇总 2026-09-18 → 2026-10-22 的「节点1（Linux）自升级」「节点3/小儿子（Windows）升级」「子代理 codex/acp 升级」「billion-context 压缩插件 400 事故」。
> **完整版（含命令与代码）**：`/home/ctyun/deepseek-harness/DSH-升级经验总汇-2026-10.md`；
> 历史附录（节点2 云机 / 6628 frp / 认证 cookie / 8 个老案例）：`/home/ctyun/deepseek-harness/DSH-升级维修手册.md`（其 §9 含明文口令，外发前删）。

## 一、机器拓扑

| 角色 | 节点1（本机，父亲） | 节点3（小儿子） | 节点2（云上旧机，历史） |
|---|---|---|---|
| 机器 | Linux `/home/ctyun` | `DESKTOP-AAC0367` Windows 10 LTSC，S4U 账号 `aiadmin` | Windows `Z11241812297328`，`cloud_user` |
| 通道 | 本机 | `ssh -i ~/.ssh/id_ed25519 -p 6681 aiadmin@192.144.234.183`；助手 `~/.local/bin/n3` | SSH 6679 / RDP 6629 |
| dsh 安装 | `/home/ctyun/.npm-global/lib/node_modules/@deepseek-ai/dsh` | `C:\Tools\node-v22.22.2-win-x64\node_modules\@deepseek-ai\dsh` | `%APPDATA%\npm\node_modules\...` |
| 启动 | `/home/ctyun/dsh-web.sh`，日志 `~/harness-run/web.log` | 计划任务 `DSH-Node3-Keep` → `C:\Tools\dsh-node3.cmd`，日志 `C:\Tools\dsh-node3.log` | 计划任务 `n2dshweb` |
| 当前版本 | **0.2.1-rc.2**（2026-10-21 升） | **0.2.1-rc.2**（2026-10-22 升） | 0.1.7-rc.2（历史） |

## 二、十条铁律（摘要）

1. 安装成功 ≠ 升级成功：必须当天重启 + 验收。
2. 动手先备份 profile 配方（`package.json` + `pnpm-lock.yaml`）。
3. 备份必须带 MANIFEST + RESTORE 才算备份。
4. 兼容门禁是静默的：`--dump-config` 退出码 0，但要 grep `skipping profile bundle` / `disabling profile plugin row`。
5. agent 不能自杀式重启：用分离 watcher（等本轮 turn/end + 90 秒再 `bash ~/dsh-web.sh`）。
6. token URL 每次启动都变；旧 URL 必 401。
7. Windows：本地 `.ps1` 传参（`n3 run` 不吃 stdin）、`Write-Output` 纯 ASCII（`Write-Host` 会变 CLIXML）、UTF-8 **无 BOM**。
8. `optionalDependencies` 会静默跳过（`@openai/codex` 平台二进制必须单独核对）。
9. 磁盘只留最近一份可还原备份；删前核对是否被引用（节点3 的 `C:\Tools\dsh-0.2.1-rc.2` 被 mail/wx poll 当 `$dsh_bin` 用着）。
10. 别在 UI 里升级手工修过的插件，spec 锁具体版本。

## 三、插件黄金基线（dsh 0.2.1-rc.2）

- `dshmarket` `^1.66.45`（npm registry，GitHub tarball 无构建产物）
- `@xgone/dsh-remote`：gh-proxy GitHub **main tarball**（npm 0.3.3 旧代码会 pending 红屏）
- `@deepseek-ai/dsh-subagent-codex` / `subagent-acp`：`0.2.1-rc.2`
- `dsh-remote-ssh-ops` / `dsh-rule-evolve`：peer `^0.1.0-rc.6`，0.2.1 下被 skip（可接受）
- `billion-context`（仅节点1）：`^0.1.204`（含 Issue #4159 修复）
- `dsh-escape-hatch`：坏链接 `/tmp/dsh-escape-hatch`，被 skip（待修）

## 四、三次关键事故

### 4.1 节点1 自升级 0.1.7-rc.2 → 0.2.1-rc.2（10-21 夜）
3 个 bundle 静默 skip + `subagent-acp` row 禁用；codex 平台二进制 optionalDependency 静默跳过须手工核对；重启由用户执行，token 变更。

### 4.2 节点3 升级 0.1.2-rc.1 → 0.2.1-rc.2（10-22）
备份（3.17GB + sha256 MANIFEST）→ staging 安装到 `C:\Tools\dsh-0.2.1-rc.2` → dump-config 预演比对 → 全局切换 + 启动器切换 → 重启 → 验收（版本/3080 200/codex-cli 0.153.4/mail 11·11）。
坑：CLIXML 污染输出；顶层 `@openai/codex` MISSING 是假警报（pnpm 嵌套解析）。

### 4.3 billion-context 0.1.203 → 0.1.204：`tool_addition` 400（本次修复）
- 症状：长会话压缩后**每轮 400**：`messages.N: tool_addition blocks can only be in system messages`（api.deepseek.com，INVALID_REQUEST）。
- 根因：上游 Issue #4159——压缩重序列化会话中途 system 消息，host `systemToUser` 把真实 system 内容改写成 user 角色，`tool_addition` 掉进 user 被拒。
- 修复：PR #4167 + #4171（`rawOpenaiMessage` verbatim sidecar，host 改写收窄到 `isRenderedSummaryMessage`），随 **v0.1.204** 发布（10-22 02:07 北京时间）。
- 验证五步：核 tarball 标记（`rawOpenaiMessage`×2 等 + build commit `c87185a0`）→ 安装 → `--dump-config` 看 `bili-compaction-gate` → 独立实例（3098）预检 → 重启后在**历史含压缩块的老会话**里继续对话。
- 回退：`bash ~/harness-run/n1-bili-rollback.sh 0.1.203`（回版本）/ `disable`（摘掉 bili 止血）。

## 五、备份与清理（2026-10-22 实测）

- 删除节点3 `C:\Tools\n3-upgrade\backup-20261022-114906`（3.33GB）：先核线上 sessions（5 个）⊇ 备份（4 个），保留 MANIFEST/RESTORE 到 `...-MANIFESTS\`；C: 剩余 **7.60 → 10.93GB**。
- 不可删：`C:\Tools\dsh-0.2.1-rc.2`（0.69GB，被 `n3-mail-poll.ps1` / `n3-wx-poll.ps1` 引用）。
- 可回收（未动）：`C:\Tools\node-v22.12.0-win-x64` 0.31GB（无引用）、pnpm store 0.74GB（`pnpm store prune`）。

## 六、验收清单（速查）

```bash
# 节点1
dsh --version && curl -s -o /dev/null -w '%{http_code}
' 127.0.0.1:3080/
grep -c 'skipping profile bundle' ~/harness-run/web.log
# 关键：打开历史里有压缩块的老会话继续发消息，不 400 才算过
```
```powershell
# 节点3
& 'C:\Tools\node-v22.22.2-win-x64\dsh.cmd' --version
(Get-PSDrive C).Free/1GB
```

## 七、待办 / 风险

- `subagent-acp` 0.1.5-rc.2 在节点1 被禁用 → 需要 ACP 子代理时升 0.2.1-rc.2。
- `dsh-escape-hatch` 坏链接待清理（消除启动告警）。
- `dsh-remote-ssh-ops` / `dsh-rule-evolve` 等上游适配。
- 节点3 staging 目录待 poll 脚本改指向后回收 0.69GB。
- watcher 脚本尾部会被 `dsh-web.sh` 的 kill 波及 → 把 URL 落盘逻辑移进 `dsh-web.sh`。
- billion-context 0.1.204 观察期（再 400 按 §4.3 回退）。

## 相关

- [[04-Knowledge/agent-worklogs/2026-10-22-节点3升级与billion-context修复|本次工作日志]]
- [[04-Knowledge/技术/跨机智能体数据迁移-分片传输与三方校验|跨机数据迁移与校验]]
