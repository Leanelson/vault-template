---
type: tutorial
title: SQL Server 2008 安装教程（菜么么版）
project_id: cmm-sql-install
created: 2026-06-05
status: ✅completed
tags: [📂-tutorial, 🗄️-database, 菜么么]
---

# SQL Server 2008 安装教程（菜么么版）

> 来源：菜么么知识库（语雀）
> 原文：https://www.yuque.com/cmmzsk/chgmne/st715f

## 一、版本说明

### 菜么么支持的 SQL 版本
菜么么程序对 SQL 版本没有指定要求，从 SQL 2000 到 SQL 2019 都支持，选择习惯操作的版本安装即可。

### 何时需要安装数据库
- 门店是中餐模式**有多个客户端**；或使用 KDS 划单；同步桌台菜品数据时必须安装数据库
- 门店有**长时间查询本地数据的需求**，不安装数据库使用 SQLite 已上传数据只保留 35 天

## 二、SQL 下载地址

- **SQL 各版本百度盘**：https://pan.baidu.com/s/15tV7CzzrwkUTGRn8f9gj7w **提取码：h64d**

### SQL Server 2008 标准版
> ed2k://|file|zh-hans_sql_server_2008_enterprise_x86_x64_ia64_dvd_x14-89199.iso|3517124608|60E7AA741E6F52146FB250DCA8B94C49|/

### SQL Server 2008 R2 精简版
- 此版本无企业管理器，支持最基础的连接功能
- 下载地址：https://wwe.lanzoui.com/i5pmCpaqvzc
- **新手慎用**

### SQL 2000
用于无法安装新版本时（如 Windows XP 系统）
> ed2k://|file|sc_sql_2000a_ent.iso|476887040|1F224F6D9C757492E2388A55504E5266|/

如需通过局域网连接需安装 SQL SP4 补丁：https://wwe.lanzoui.com/iz5crpn5l7g

## 三、系统兼容性

不同版本的 Windows 系统对 SQL Server 版本的支持不同，请查阅微软官方说明：
[Windows 操作系统中的 SQL Server - SQL Server](https://learn.microsoft.com/zh-cn/troubleshoot/sql/general/use-sql-server-in-windows)

## 四、安装步骤（SQL Server 2008）

### 1. 主机安装，其余客户端免装
菜么么架构：**主机安装 SQL Server，其余客户端不需要安装。门店所有客户端连接到主机同一个数据库。**

### 2. 防火墙关闭
安装前**关闭主机防火墙**（控制面板 →系统和安全→Windows Defender 防火墙→关闭）

### 3. 运行 setup.exe
插入光盘或挂载镜像，运行 setup.exe，点击「全新安装」

### 4. 功能选择
选择「全选」安装所有功能（数据库引擎、SSMS、全文搜索等）

### 5. 实例配置
**选「默认实例」**（一般情况用默认实例即可）

### 6. 服务器配置
- 服务账户选「对每个服务使用同一账户」
- 推荐使用 `NT AUTHORITY\NetworkService`
- 排序规则保持默认 Chinese_PRC_CI_AS

### 7. 数据库引擎配置
- **身份验证选「混合模式」**
- 设置 sa 账号密码（记住这个密码！）
- 添加当前用户为管理员

## 五、菜么么客户端连接数据库设置

### 主机数据库设置
1. 启动菜么么主程序
2. 选择【**我连接到 SQL Server**】点击设置
3. 选择**「是主机」**
4. 数据库地址输入**本机 IP 地址**（如 192.168.1.250）
5. 若安装实例非默认实例，需输入实例名（如 `\SQLEXPRESS`）
6. **主机 IP 地址需固定**
7. 输入 SQL 用户名（默认 sa）和密码
8. 点击数据库名下拉选择，可看到 SQL 默认数据库即表示连接成功

### 新建数据库（首次安装需新建）
1. 点击【**新建数据库**】
2. 确认新建，输入数据库名（**自定义，建议不要使用默认数据库名**）
3. 建库成功后点击确认，保存本机设置，登陆即可

> ⚠️ 非 sa 用户新建数据库失败时请确认新增登录名是否有新增数据库权限

### 其余客户端设置
1. 选择**「不是主机」**
2. 输入**主机 IP 地址**和端口号（系统默认 8090）
3. 点击「获取」可自动获取主机数据库设置
4. 获取后保存即可

> ⚠️ 无法获取时检查：主机是否启动、IP 地址、端口号、主机与本机防火墙

## 六、连接测试失败排查

1. **主机 IP 是否改变** → 修改 IP 后需重启 SQL 服务
2. **主机防火墙是否关闭**
3. **主机 SQL 服务是否正常启动**
4. **SQL 网络协议是否启用**（TCP/IP 协议需启用）
5. 找到安装目录 `tools.exe` → 数据库连接 → 改成「我是一台独立的机器」→ 重启程序

## 七、相关链接

- 微软官方兼容说明：https://learn.microsoft.com/zh-cn/troubleshoot/sql/general/use-sql-server-in-windows
- 语雀教程原文：https://www.yuque.com/cmmzsk/chgmne/st715f
- SQL 下载地址：https://pan.baidu.com/s/15tV7CzzrwkUTGRn8f9gj7w（提取码：h64d）
- SQL 2000 SP4 补丁：https://wwe.lanzoui.com/iz5crpn5l7g