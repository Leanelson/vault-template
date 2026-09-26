---
type: agent-worklog
agent: "小儿子"
session: "swiftapi-net-2026-09-26"
title: 域名采购与Windows-MCP升级
created: 2026-09-26 22:59
tags: [🤖-agent, 📝-worklog, 🌐-域名, 🖥️-computer-use]
---

# 🤖 Agent 工作记录

**Agent：** 小儿子（节点3 / DESKTOP-AAC0367）  
**Session：** swiftapi-net-2026-09-26（来源：dsh-mail 委托，节点3 口述，节点1 代笔入库）  
**时间：** 2026-09-26 22:59

## 📋 本次工作摘要

小儿子今天完成两件大事：一、**多模型 API 网关生意（swiftapi.net）域名采购落定**；
二、**Windows-MCP GUI 能力升级完成**（解决跨会话桌面操控难题，获得 DOM 精确坐标能力）。

## 🔧 一、域名采购完成（多模型 API 网关「swiftapi.net」生意）

- 2026-09-26 在 **Namesilo** 用支付宝买下 `swiftapi.net`，**$15.95/年**
  （比原手册估的 ¥75 高，已按实际价修正工作区文档）。
- 状态：Active，到期 **2027-09-26**；自动续费开启、WHOIS 隐私开启、域名锁开启。
- 登记人信息：Leanelson Nelson，西安地址。
- 采购清单里「域名」一项已打勾，实际花费 **$15.95**。
- **下一步**：把 NS 改到 Cloudflare（DNS only），加 A 记录指向香港 VPS。

## 🔧 二、Windows-MCP GUI 能力升级（这台 Windows 机器的桌面操控体系）

- 装好 **Windows-MCP（CursorTouch 版，uv 安装）**，只留安全子集
  （排除 PowerShell/Registry/FileSystem 等危险工具），关闭遥测。
- **解决跨会话难题**：DSH 跑在 session 0 抓不到真实桌面截图，
  改为**服务器跑在 session 1（Admin 登录用户）**，DSH 通过
  `streamable-http + --stateless-http` 连 `127.0.0.1:8000/mcp`。
- **持久化**：计划任务 `windows-mcp-server-admin`（登录自启）
  + 包装脚本 `C:\Tools\windows-mcp-server.ps1`。
- **杀手锏：DOM 模式**（Snapshot `use_dom`）能读网页元素树，
  每个元素带精确屏幕坐标——解决此前靠视觉估坐标点击总点偏的老问题
  （这次 Namesilo 加购按钮一次命中）。

## 🔧 三、技术坑（可并入本日志，或单开技术知识）

**n3c 桥的 launch 参数解析器 bug**：`-cmdargs` 的值如果以 `-` 开头
（比如 `-NoProfile,-ExecutionPolicy,...`），会被解析成新的选项而吞掉，
导致启动的程序没拿到参数、空转在命令行。
绕法：`-cmd cmd.exe -cmdargs "/c,powershell.exe,-NoProfile,..."`，
用 `/c` 打头（不以 `-` 开头）让 cmd 自己拼命令行。

## 📌 关键产出

- **域名**：`swiftapi.net`（Namesilo，$15.95/年，Active）
- **Windows-MCP**：`C:\Tools\windows-mcp-server.ps1` + 计划任务
  `windows-mcp-server-admin`，DOM 模式读网页元素树 + 精确坐标
- **节点3 工作区文档已自改**：`deploy/procurement-checklist.md`、
  `token-resale-blueprint.md`（儿子自己改好了，无需父亲再动）

## 📎 相关笔记

- [[node3-relay]]（节点1↔节点3 传话协议，本次委托的来源通道）
- [[node3-computer-use]]（n3c 桥架构，桌面操控基础设施）
- [[小儿子-worklog-2026-09-24-mano云端打通]]（上游：人机视觉能力）

## ⚠️ 待跟进

1. **NS 迁移**：swiftapi.net 改到 Cloudflare（DNS only）+ A 记录指向香港 VPS。
2. **ICP 备案**：域名用于国内官网需 ICP 备案（与上一封回复的「国内+备案」合规线一致）；
   当前官网 www.zlh1.com 已有页面自报备案号，swiftapi.net 属新域名，待评估备案策略。
3. **Windows-MCP 安全子集**：只留安全工具，排除 PowerShell/Registry/FileSystem，
   生产使用前需再核一遍白名单边界。
4. **n3c launch bug**：`-cmdargs` 以 `-` 开头的参数会被吞，绕法已记录，待上游修复。

## 💭 反思/学习

1. **跨会话抓屏的正解是「服务器跑在用户会话」**，而不是硬啃 session 0
   的 CopyFromScreen（此前 09-21 就撞过 "The handle is invalid"）。
   这次用 Windows-MCP 跑在 session 1 一劳永逸。
2. **精确坐标来自结构而非估计**：DOM 模式给出元素坐标，比任何
   视觉估点都稳（Namesilo 加购一次命中）。
3. **采购成本要按实价修正文档**：域名 $15.95 比手册估的 ¥75 高，
   已修正，不靠估。

—— 节点1（父亲）代笔于 2026-09-26，内容源自节点3（小儿子）委托