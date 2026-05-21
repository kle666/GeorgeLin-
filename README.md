<h1 align="center">GeorgeLin · 项目作品集</h1>

<p align="center">
  <b>AI 应用开发工程师 / 全栈应用开发</b><br/>
  用 Python(FastAPI) + Vue 独立交付商业化系统 ｜ 接口逆向 ｜ 浏览器自动化 ｜ 大模型集成<br/>
  <i>重度 AI 原生开发者 —— 以一人之力交付接近小团队的产出</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Vue.js-3-4FC08D?logo=vuedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Playwright-2EAD33?logo=playwright&logoColor=white" />
  <img src="https://img.shields.io/badge/Chrome%20MV3-4285F4?logo=googlechrome&logoColor=white" />
  <img src="https://img.shields.io/badge/PySide6-41CD52?logo=qt&logoColor=white" />
  <img src="https://img.shields.io/badge/Claude%20API-D97757?logo=anthropic&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white" />
</p>

---

## 👋 关于我

AI 原生电商软件公司的应用开发工程师。过去一段时间里，我独立完成了从**商业化 SaaS 系统改造**、**第三方平台接口逆向对接**、到**浏览器扩展与桌面工具**的全栈开发。

我的工作方式：**用 AI 工具作为效率杠杆，但架构设计、技术选型、上线排障都由我自己主导。** 这让我能以一个人的投入，交付接近小团队规模的产出 —— 一个月内独立上线 6+ 个项目。

- 🛠️ **全栈**：Python(FastAPI) 后端 + Vue3 前端 + PostgreSQL/SQLite
- 🔍 **逆向 & 自动化**：抓包对接平台内部接口、Playwright/Selenium、Chrome MV3 扩展
- 🖥️ **桌面工具**：PySide6 + PyInstaller 打包免环境分发
- 🤖 **大模型集成**：Claude API 用于评分/出题/内容优化 + Prompt 工程 + token 计费
- 🔒 **商业化加固**：代码混淆、水印、接口限速、授权码、监控埋点
- 📐 **工程素养**：需求→策划→执行的流程化交付，质量门自检，Git 纪律，零返工

> 💡 我的简历项目大多为公司商业项目，**源码不便公开**；本作品集描述的是**系统架构、我的职责、技术难点与可量化成果**，欢迎面试中深入考察任何一个项目。

---

## 🧰 技术栈

| 类别 | 技术 |
|---|---|
| **后端** | Python · FastAPI · SQLAlchemy · Alembic(DB 迁移) · JWT · bcrypt |
| **前端** | Vue 3 · Vite · TypeScript · Ant Design |
| **数据库** | PostgreSQL(多租户 schema 隔离) · SQLite |
| **自动化/扩展** | Playwright · Selenium · Chrome MV3 Extension · Chrome DevTools Protocol |
| **桌面** | PySide6(Qt) · PyInstaller(onedir/windowed) |
| **大模型** | Claude API · Prompt 工程 · token 级计费 |
| **工程化** | Git(commit 链/tag/回滚) · 商业化加固 · 监控埋点 |
| **部署运维** | Linux/SSH · systemd · Docker · Nginx/ALB 排障 |

---

## 🚀 精选项目

> 按含金量排序，点标题进详情页。

### 🥇 [AI 招聘测评 SaaS · 多租户与商业化改造](./projects/01-recruit-saas.md)
将单租户内部工具重构为**可对外售卖的多租户 SaaS**：13 张表组织隔离、跨租户访问零泄露、token 级双钱包计费、前端混淆+水印+限速+监控全套商业化加固。
`FastAPI` `Vue3` `PostgreSQL` `Claude API`
> **支撑 9 个组织独立运营 · 毛利率 ~75% · 版本 v0.15→v0.23 · 需求符合率 100%**

### 🥈 [电商收款流水采集工具 · 接口逆向](./projects/02-finance-scraper.md)
抓包发现平台内部 API 无签名校验，从浏览器自动化重写为直调内部接口，性能腰斩、数据全对齐；内置 Python 实现免环境分发。
`Playwright` `Python` `逆向` `分发加固`
> **性能 95s → 39s（提速 ~59%）· 数据 100% 对齐 · 22/22 需求项零返工**

### 🥉 [跨境采购下单对接插件 · MV3 扩展 + 后端代理](./projects/03-order-plugin.md)
Chrome MV3 插件 + FastAPI 后端代理，封装第三方网关 5 步下单链路；密钥服务端隔离不下放客户端，下单幂等+重试+网络异常兜底。
`Chrome MV3` `FastAPI` `JWT` `SQLite`
> **闭环对接 7 个接口 · 真账号实测下单链路跑通 · 符合率 92%**

### [多店铺经营报告采集 · 桌面工具](./projects/04-wb-reports-desktop.md)
PySide6 桌面应用批量采集 22 个店铺的经营报告，限速自愈 + 定时任务；PyInstaller 打包免环境 .exe。
`PySide6` `PyInstaller` `逆向` `限速自愈`
> **22/22 店铺 · 20 分钟产出 1103 行数据 · 定位并解决打包 C 层崩溃**

### [跨境商品评分优化工具 · 大模型集成](./projects/05-ozon-ai-optimizer.md)
用大模型自动优化商品内容分（标题/属性/富内容），对接平台接口写回；激活码+心跳的商业化授权模型。
`Claude API` `Prompt 工程` `商业化授权`
> **单商品内容分满分 10/10 · 处理 AI 输出不可控的真实工程问题**

### [浏览器扩展 & 自动化能力合集](./projects/06-browser-automation.md)
Chrome 扩展账号切换、桌面工具计时器+并发 bug 修复、三套浏览器自动化方案选型。
`Chrome Extension` `Playwright` `PySide6`
> **3 小时完成原预估 2.7 天的工作（缩量 70%）· 10 分钟定位并发写文件竞争 bug**

---

## 💎 我的差异化：AI 原生开发

我不是"让 AI 帮我写作业"，而是**主导设计、驱动实现、自己排障验收** —— AI 是我的杠杆，不是拐杖。

- 一个月内独立交付 6+ 个项目，覆盖 SaaS / 逆向 / 扩展 / 桌面 / 大模型集成
- 每个项目走"需求→策划→执行"流程，需求符合率多数 100%，**零返工**
- bug 多为"一次定位一次修复"，从未出现反复改不好的情况
- 完整 Git 纪律（commit 链 / 大改前 tag / 可回滚），单任务最多 18 次 commit 跨 2 项目协同

> 我能现场讲清任何一个项目的架构，也能当场调试。

---

## 📫 联系方式

- 📍 福建 · 泉州（可短途通勤厦门）
- 📧 Email: `<填你的邮箱>`
- 💬 微信/电话: `<填你的联系方式>`

<p align="center"><sub>本作品集所有项目均已脱敏，不含任何密钥、服务器地址、客户信息或源码。</sub></p>
