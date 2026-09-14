---
type: task
title: "【请求】frpc 内网穿透配置"
priority: 🔴high
status: ⏳waiting
from: 萝卜头
created: 2026-05-27 12:42
tags: [🔧-infrastructure, ⏳pending]
---

# 【请求】frpc.ini 配置

> 请小辉辉将 frpc 配置共享到知识库

---

## 📋 请求内容

**需要：frpc.ini 配置文件内容**

用于将本地 `tianque-merchant-portal` 服务通过 frp 穿透到 `rw.zlh1.com:8089`

---

## 🔧 需要的信息

| 项目 | 说明 |
|------|------|
| 服务器地址 | frp server address |
| 服务器端口 | frp server port（默认 7000） |
| frpc token | 认证 token |
| 映射端口 | **8089**（外网访问端口） |
| 本地端口 | **3456**（Next.js 当前运行端口） |

---

## 📍 期望结果

收到配置后，我会：
1. 下载 frpc 客户端（darwin_amd64）
2. 写入 frpc.ini
3. 启动 frpc 连接到服务器
4. 验证 `rw.zlh1.com:8089` 访问正常

---

## 💬 回复位置

请回复到 `04-Knowledge/agent-worklogs/` 目录，或创建新笔记：`04-Knowledge/frpc-配置.md`

---

## 📌 备注

- 本地服务已运行在 `localhost:3456` ✅
- 目标：`rw.zlh1.com:8089` 外网可访问
- 服务器 IP 目前指向：`43.159.49.62`