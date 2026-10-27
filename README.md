# 转账反诈守门工具 · Web 版

「转账反诈守门工具」线上运行版本的静态镜像仓库。原应用部署在秒哒（Miaoda）平台：
**https://app-fdq96u1bspoh.miaoda.online**

本仓库把该应用的运行文件完整抓取、改写为相对路径，使其脱离秒哒域名也能独立托管（GitHub Pages / 任意静态服务器）。

## 在线访问

- 秒哒原版：https://app-fdq96u1bspoh.miaoda.online
- GitHub Pages：https://ouyang-187569.github.io/transfer-fraud-guard-web/

## 功能

- **转账检测**：填写收款人、账户、金额、时间、附言，点击「安全检测并转账」实时给出 0-100 风险评分与可疑点。
- **习惯画像**：常用收款人、日常单笔均值、习惯样本，历史记录与基准数据维护。
- **二次确认弹窗**：中高风险转账前拦截提示。
- **悬浮 AI 反诈助手**：右下角常驻，可带入当前转账上下文对话。

> 本工具仅做安全辅助检测，**不涉及真实资金操作**；遇疑似诈骗请拨打 **96110**。

## 技术栈

| 项 | 说明 |
| --- | --- |
| 前端 | React + TypeScript 构建的 SPA（Vite / Rolldown 打包） |
| 样式 | Tailwind CSS，`assets/index-*.css` |
| 数据 | Supabase（秒哒托管实例 `backend.miaoda.online`），前端持 anon key |
| 托管 | 纯静态，无服务端代码 |

## 目录结构

```
.
├── index.html              # 入口（资源路径已改写为相对路径）
├── assets/                 # 构建产物：主 JS/CSS、按需加载的 chunk、封面图
│   ├── index-*.js          # 应用主包（约 1.4 MB）
│   ├── index-*.css
│   ├── cover.png
│   └── chunk-*.js / *.css  # 路由与依赖按需加载的分块
├── .nojekyll               # 关闭 GitHub Pages 的 Jekyll 处理
└── README.md
```

## 本地运行

任意静态服务器即可（不要直接双击 `index.html`，ES Module 需要 http 协议）：

```bash
# 方式一：Node
npx serve .

# 方式二：Python
python -m http.server 8080
```

然后访问 `http://localhost:8080`。

## 说明与限制

- 本仓库为**构建产物**（压缩后的 JS/CSS），不含原始工程源码与 source map；源工程在秒哒平台上维护。
- 重新抓取时做了两处改写：
  1. `index.html` 中指向秒哒 CDN 的绝对地址 → 相对路径 `assets/`；
  2. 主包内 `BASE_URL` 常量 → `./`，使按需加载的 chunk 从本站点取。
- 应用的数据读写仍指向秒哒托管的 Supabase 实例（`https://backend.miaoda.online/projects/supabase…`），因此换域名后**后端与数据保持不变**；若该实例下线，登录与数据功能会失效。
- 外部依赖：代码字体来自 `miaoda-font.cdn.bcebos.com`，字体文件来自 `resource-static.bj.bcebos.com`，均为公开 CDN。
- 秒哒平台注入的 `miaoda-external.js`（分享卡片 / 平台运行时）保留在页面中，在非秒哒域名下大概率静默失败，不影响应用本体。

## 相关仓库

- `transfer-fraud-guard`：同一需求下的零依赖原生实现（HTML + CSS + ESM，含 33 个单元测试），与本仓库代码不同源。

## 免责声明

本工具仅提供安全辅助检测与反诈知识参考，不构成任何投资、法律或资金操作建议。任何转账决策请以官方渠道核实为准，疑似被骗请立即拨打 **96110**。
