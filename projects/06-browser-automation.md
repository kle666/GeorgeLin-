# 浏览器扩展 & 自动化能力合集

<p>
  <img src="https://img.shields.io/badge/Chrome%20Extension-4285F4?logo=googlechrome&logoColor=white" />
  <img src="https://img.shields.io/badge/Playwright-2EAD33?logo=playwright&logoColor=white" />
  <img src="https://img.shields.io/badge/Selenium-43B02A?logo=selenium&logoColor=white" />
  <img src="https://img.shields.io/badge/PySide6-41CD52?logo=qt&logoColor=white" />
</p>

> 💼 均为公司项目，源码不公开。本页汇总浏览器扩展与自动化方面的多个小项目，证明能力广度。

## 🧩 子项目一览

### 1. 钱包插件账号切换（Chrome 扩展）
- **做了什么**：给某钱包插件加多账号一键切换功能
- **亮点**：**3 小时完成原预估 2.7 天的工作（缩量 70%）**
- **难点排障**：遇到"中断丢配置 / 主程序覆写 config"双重根因的隐蔽 bug，通过**日志时序还原 + 配置读写对照，10 分钟定位到"并发写文件竞争"**
- **工程习惯**：大改前先 commit + 打 tag，回滚成本可控

### 2. 桌面上架工具计时器 + BUG 修复（PySide6）
- **做了什么**：给桌面上架工具加耗时计时器，并修复一批 GUI bug
- **难点**：涉及 GUI 的耗时 IO 操作（下载/上传/网络请求）打包后会卡死界面 → 改为 daemon 线程化处理
- **沉淀**：总结出"打包前 checklist"（AST 检查 → 源码模式跑最早期路径 → 关键 GUI 事件无阻塞）

### 3. 浏览器自动化方案选型
能按场景选用三套方案：
| 方案 | 适用场景 |
|---|---|
| **Playwright** | 现代 SPA、需要稳定等待/拦截网络 |
| **Chrome DevTools Protocol** | 需要贴近浏览器底层、性能/网络分析 |
| **本地脚本 + Selenium** | 简单稳定的固定流程 |

## 🛠️ 这一类工作沉淀的通用经验

- Windows + Python 后台服务的坑：`pythonw` 下 `stdout=None` 导致 print 崩溃、计划任务调度、SPA 渲染时序、UI 框架元素遮挡等
- Chrome MV3 扩展在 Windows 下：直接 `build` 加载 `dist` 比 dev 模式稳定
- 杀浏览器进程前必须确认（自动化用的 Chrome 与日常 Chrome 进程名相同，无脑杀会毁掉现场）

## 🧩 技术栈

`Chrome MV3 Extension` · `Playwright` · `Selenium` · `Chrome DevTools Protocol` · `PySide6` · `QThread` · `Windows 计划任务`
