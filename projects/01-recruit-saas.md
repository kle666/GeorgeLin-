# 🥇 AI 招聘测评 SaaS · 多租户与商业化改造

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Vue.js-3-4FC08D?logo=vuedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Claude%20API-D97757?logo=anthropic&logoColor=white" />
  <img src="https://img.shields.io/badge/Alembic-FCA121" />
</p>

> 💼 公司商业项目，源码不公开。本文档描述系统架构、我的职责、技术难点与成果。

## 📌 项目背景

公司自研的 AI 招聘测评系统（自动出题 / 判卷 / 面试评分，底层调用大模型 API）。原本是**单租户内部工具**，业务上需要升级为**可对外售卖的多租户商业化产品**：每个客户（招聘方组织）数据互相隔离、按用量计费、且要防止外部技术客户逆向破解。

## 🙋 我的职责

**独立完成从架构设计到上线的全过程**，并跨"招聘系统 + 积分计费中台"两个项目协同改造。

## 🏗️ 系统架构

```
                     ┌─────────────────────────┐
   招聘方组织 A ──┐    │   招聘测评系统 (FastAPI)   │
   招聘方组织 B ──┼──▶ │  ┌───────────────────┐  │ ──▶ Claude API (出题/判卷/评分)
   招聘方组织 C ──┘    │  │ org_scoped_query  │  │
                     │  │ 多租户隔离中间件     │  │
                     │  └───────────────────┘  │
                     │  ┌───────────────────┐  │
                     │  │ credits_service   │  │ ──▶ 积分计费中台 (双钱包/扣费/看板)
                     │  │ 按 token 实时计费   │  │
                     │  └───────────────────┘  │
                     │  前端: Vue3 (混淆+水印)   │
                     └─────────────────────────┘
```

## 🔑 核心实现

### 1. 多租户数据隔离
- 为 **13 张业务表**统一新增 `organization_id` 外键
- 封装统一过滤工具 `org_scoped_query` / `assert_same_org`，所有查询强制带组织条件
- 候选人、出题模板、出题/判卷流水线**全链路按组织隔离**（流水线按候选人所属组织取该组织的 active 模板）
- 超级管理员（`organization_id = NULL`）可跨组织全看
- **跨组织访问统一返回 HTTP 404**（而非 403）—— 零信息泄露，攻击者连"资源是否存在"都探测不到

### 2. 商业化计费（双钱包模型）
- **月配额**（兑换码覆盖式）+ **个人余额**（充值累加），扣费先扣配额后扣余额
- 按大模型 token 用量实时扣费：`扣分 = ceil(input × 系数 + output × 系数)`
- 余额不足严格 `HTTP 402` 拒绝；扣费接口失败 `503` + 告警日志，人工补单兜底

### 3. 商业化安全加固
- **前端代码混淆**：terser mangle（toplevel + drop debugger）
- **双重水印**：SVG 平铺 + canvas 指纹 + MutationObserver 防删
- **接口限速**：HR 核心接口 30 次/分
- **接口签名**：HMAC（代码就绪，可配开关）
- **可观测性**：大模型调用 / 简历导入埋点上报，计费中台看板展示用量走势、KPI、失败 Top N

## 🐛 难点与排障（面试重点）

> **生产事故复盘 → 沉淀方法论**
> 给 13 张表加 `organization_id NOT NULL` 时，我改全了查询和部分创建逻辑，但**漏了 6 处考生侧 INSERT**（异步任务、试卷、试题、答卷等），导致上线后客户一出题就 500（异步任务建任务第一步即崩）。
> **解决**：紧急 hotfix 补全所有写入点。
> **沉淀**：总结出规范 ——「**给表加非空字段前，先 `grep` 出该模型的所有构造点，逐一传字段**」，并为异步任务设计 `_derive_org_id()` 自动推导（candidate → sheet → operator → 兜底），让 5 个调用点无需逐个改。

其他：
- 前端混淆器与构建工具不兼容 → 紧急下线后改用 terser mangle 方案
- 大模型 thinking-only 空响应但仍计费 → 双层兜底（请求侧显式禁用 + 响应侧接受 thinking 块）
- 网关 60s 超时导致评分接口 502 → 切短超时模型 + 减负方案

## 📊 成果

| 指标 | 结果 |
|---|---|
| 多租户隔离 | 13 表 · 支撑 9 个组织独立运营 · 跨租户零泄露 |
| 商业化 | token 级双钱包计费 · 毛利率 ~75% |
| 安全加固 | 混淆 + 双水印 + 限速 + 监控全部上线 |
| 版本迭代 | v0.15 → v0.23，跨 2 项目数十次 commit |
| 质量 | 需求符合率验收 100%（多租户后端核心 100%） |

## 🧩 技术栈

`Python` · `FastAPI` · `SQLAlchemy` · `Alembic` · `Vue3` · `PostgreSQL`(schema 隔离) · `Claude API` · `JWT` · `systemd` · `Docker`(PG 容器)
