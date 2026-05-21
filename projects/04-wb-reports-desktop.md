# 多店铺经营报告采集 · 桌面工具

<p>
  <img src="https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/PySide6-41CD52?logo=qt&logoColor=white" />
  <img src="https://img.shields.io/badge/PyInstaller-FFD43B" />
  <img src="https://img.shields.io/badge/限速自愈-845EC2" />
</p>

> 💼 公司内部工具，源码不公开。本文档描述方案、我的职责、技术难点与成果。

## 📌 项目背景

批量拉取 **22 个跨境店铺**的每周详细经营报告，按时间窗（默认滚动 30 天）汇总成一张 **81 列的 Excel**，供运营分析。

## 🙋 我的职责

独立完成桌面工具开发、接口逆向、限速调优、打包分发。

## 🏗️ 模块设计

```
┌─────────────── PySide6 GUI ───────────────┐
│  首次启动: 引导导入 token (xlsx)              │
│  主界面: 选时间窗 → 开始 → 进度 → 导出 Excel   │
│  ⏰ 定时设置: schtasks 建计划任务              │
└────────────────────┬──────────────────────┘
                     │ QThread (不卡界面)
                     ▼
┌──────────── core ────────────┐
│ wb_client   限速 + 重试 + 分页  │ ──▶ 平台财务接口(新版)
│ field_map   79 列 ↔ API 字段   │
│ exporter    openpyxl 写 Excel  │
│ job_worker  整批调度 + 补跑     │
└──────────────────────────────┘
```

## 🔑 核心实现

- **接口逆向**：对接平台新版财务接口（旧版已废弃），处理分页、限速、重试
- **限速自愈**（亮点）：单店触发限速时快速失败（不死等），整轮跑完后把失败的店**延后 30 分钟整批补跑一次**，自动恢复临时限速
- **桌面化**：PySide6 做 GUI，首次启动引导导入 token；QThread 后台跑任务不卡界面
- **定时**：用 Windows 计划任务（schtasks），到点弹出 GUI 让运营手动确认（业务要"定时弹窗"而非全自动）
- **全参数可配**：抓取间隔、重试次数、补跑、默认天数都在配置文件里
- **打包**：PyInstaller 打成免环境 .exe，发布整个文件夹拷走即用（不含 token，首次运行引导导入）

## 🐛 硬核难点（面试重点）

> **打包后程序闪退，定位到 dll 偏移级别的崩溃**
> 用 Python 3.14 + PySide6 打包后，程序在实例化复杂窗口时直接 C 层 `ACCESS_VIOLATION` 崩溃（崩在 `python314.dll` 某偏移）。逐步排查后定位到是 **3.14 + PySide6 在 PyInstaller 冻结环境下的兼容问题**，降到 **Python 3.12 打包后完全正常**。能定位到这种 native 层崩溃并找到根因，是很强的排障能力证明。

其他：`pythonw`/windowed 模式 `stdout=None`，任何 print 都崩 → 入口处重定向输出到文件。

## 📊 成果

| 指标 | 结果 |
|---|---|
| 采集规模 | 22 个店铺，干净状态 22/22 通过 |
| 性能 | 20 分钟产出 1103 行数据 |
| 输出 | 81 列 Excel，字段严格对照样本 |
| 健壮性 | 限速自愈 + 整批补跑 + 定时任务 |

## 🧩 技术栈

`Python 3.12` · `PySide6(Qt)` · `QThread` · `openpyxl` · `PyInstaller` · `接口逆向` · `schtasks`
