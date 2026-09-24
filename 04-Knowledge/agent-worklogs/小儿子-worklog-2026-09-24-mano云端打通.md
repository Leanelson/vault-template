---
type: agent-worklog
agent: "小儿子"
session: "mano-cua-2026-09-24"
created: 2026-09-24 11:55
tags: [🤖-agent, 📝-worklog, 🖥️-computer-use]
---

# 🤖 Agent 工作记录

**Agent：** 小儿子（节点3 / DESKTOP-AAC0367）  
**Session：** mano-cua-2026-09-24  
**时间：** 2026-09-24 11:55

## 📋 本次工作摘要

给节点3 装上「人机视觉操作」能力：让它在真实 Windows 桌面上自己看屏幕、
自己点鼠标。方案评估了三轮，最终用 **Mano-P 云端（脑）+ n3c 桥（手）** 打通，
并**成功完成一次端到端任务**：计算器上算出 `7 × 8 = 56`，
结果由代码从 UIA 读出（不是模型自报）。

## 🔧 执行的操作

1. **评估 JEV** — 读完 `Sac-Y/Jev-cu` 源码与官方 SKILL.md，确认它只传文字不传截图、
   仅覆盖 macOS AX。两台机器都无 TypeSafe/OpenRouter 密钥，真 Jev 调不通。
2. **实测 Windows UIA** — 发现关键事实：session 0 无法 FromHandle session-1 窗口；
   UIA 语义密度按 app 差异极大（计算器 36 元素仅 1 有名、可 Invoke=0；微信 9 元素 7 有名）。
3. **实测视觉定位精度** — 用 GLM-5.3-Flash 打计算器，规整网格误差 **1~2px**；
   点落在正确 UIA 矩形内 **13/13**。
4. **核查 Mano-P** — 确认模型是 `Qwen3VLForConditionalGeneration`，基座 `Qwen/Qwen3-VL-4B`，
   8.89GB，Apache-2.0。但官方只优化 MLX（Apple 专用），节点3 无 GPU（VRAM=0）→ 本地跑不动。
5. **打通 Mano 云端** — 读 `mano-skill` v1.1.4 源码（sha256 与 Homebrew formula 一致），
   发现**云端完全不需要 API key**，唯一 header 是 User-Agent。
6. **写 `mano_bridge.py`** — Mano 出脑 → n3c 出手，含坐标校准、
   重试、窗口跟踪、代码核验。
7. **坐标校准** — 让 Mano 点已知按钮做最小二乘，误差压到 **0~1px**。

## 📌 关键产出

- **`mano_bridge.py`**（21KB）— Mano 云端 + n3c 桥，含 `--expect` 代码核验
- **坐标校准公式**（1024×768）：`real_x = 0.7959*sx + 1.9`，`real_y = 1.0847*sy - 5.7`
- **Mano 云端协议**：`POST /v1/sessions` → `/step` → `/close`，**无需 key**
- 实测：服务可用率 **90%**，延迟 9~252s/步
- 四份报告在 `~/deepseek-harness/n3-jev/`：`FINDINGS-MANO-P.md`、
  `FINDINGS-MANO-CLOUD.md`、`FINDINGS-MANO-CALIB.md`、`LESSONS.md`

## 📎 相关笔记

- [[node3-computer-use]]（n3c 桥架构，本次的基础设施）
- [[mano-cua-node3]]（节点3 上已有的 Mano 融合 skill）
- [[node3-relay]]（节点1↔节点3 传话协议）

## ⚠️ 待跟进

1. **服务端不稳定**：Mano 云端 10 次采样失败 1 次，形态是
   `HTTP 400 "Upstream API error: The parameter tools[i].type ..."`（服务端 bug）。
   已实现指数退避重试，但生产使用需评估。
2. **bash 能力未实测**：README 称 bash 模式把通过率 83%→90%，已映射到 PowerShell，待验证。
3. **隐私边界**：截图会上传 `mano.mininglamp.com`。涉及私密界面（微信聊天、密码框）
   需限定截图区域或暂停交人。
4. **只验证了计算器**：真实教学/工作辅助任务尚未跑过。

## 💭 反思/学习

**我犯了三个错，都值得记住：**

1. **真值表错了却去怪模型** — 我漏算计算器 MC/MR 内存行（整列错位 32px），
   把模型的 **1px 精度**误判为"0/3 全错"，差点否定整个方案。
   → **先验证参照物，再评判被测对象。**

2. **验证错了窗口** — Mano 执行 `open_app` 后新开了计算器窗口，
   它一直在新窗口操作，我却验证旧窗口，看到 `0` 就以为失败。
   → **多窗口应用必须跟踪模型实际操作的 hwnd。**

3. **把窗口差当成坐标错误** — 两个计算器 rect 相差 84px，
   我把它拟合成了"系统性坐标偏移"。
   → **坐标偏差先怀疑"看的不是同一个对象"。**

**最重要的一条纪律**：实测 Mano 会**自报 DONE 而屏幕并非该结果**
（它说"结果为56"时窗口显示 0）。所以成功判据只能来自代码读屏。
这条已写进 `mano_bridge.py` 的 `--expect` 机制。
