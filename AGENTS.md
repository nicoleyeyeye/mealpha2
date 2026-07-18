# 项目上下文

### 版本技术栈

- **Framework**: Next.js 16 (App Router)
- **Core**: React 19
- **Language**: TypeScript 5
- **UI 组件**: shadcn/ui (基于 Radix UI)
- **Styling**: Tailwind CSS 4
- **Charts**: recharts
- **Database**: Supabase (PostgreSQL)
- **Auth**: Supabase Auth (邮箱登录)
- **LLM**: coze-coding-dev-sdk (截图 OCR)

## 项目概述

个人投资跟踪工具（投资管家），支持 A 股和基金的自选池管理、行情追踪、规则提醒、交易日志。

## 目录结构

```
├── public/                     # 静态资源
├── scripts/                    # 构建与启动脚本
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   ├── supabase-config/   # Supabase 配置 API
│   │   │   ├── market/
│   │   │   │   ├── quotes/        # 行情实时数据代理
│   │   │   │   └── history/       # 行情历史数据代理
│   │   │   ├── ocr/recognize/     # 截图 OCR 识别
│   │   │   └── alerts/check/      # 提醒规则检查
│   │   ├── login/                 # 登录/注册页
│   │   ├── detail/[id]/           # 标的详情页（走势图+提醒）
│   │   ├── layout.tsx             # 根布局
│   │   ├── page.tsx               # 首页（路由到 Dashboard）
│   │   └── globals.css            # 全局样式 + 设计 Tokens
│   ├── components/
│   │   ├── ui/                    # shadcn/ui 组件
│   │   ├── theme-provider.tsx     # 深色/浅色主题
│   │   └── dashboard.tsx          # 主面板（行情/自选/交易/我的）
│   ├── lib/
│   │   ├── utils.ts               # cn() 工具
│   │   ├── supabase-config-inject.tsx  # Supabase 配置注入
│   │   ├── supabase-browser.ts    # 浏览器端 Supabase 客户端
│   │   ├── auth-context.tsx       # Auth 上下文
│   │   └── verify-session.ts      # 服务端身份验证
│   ├── storage/database/
│   │   ├── supabase-client.ts     # Supabase 服务端客户端
│   │   └── shared/schema.ts       # Drizzle schema 定义
│   └── server.ts
├── DESIGN.md                      # 设计规范
├── next.config.ts
├── package.json
└── tsconfig.json
```

## 核心功能

1. **自选池**：手动添加 / 截图 OCR 识别批量导入
2. **行情看板**：实时行情（红涨绿跌），走势图（30/60/90天）
3. **规则提醒**：阈值规则 + 邮件通知（同一天同规则只发一次）
4. **交易日志**：买入/卖出记录，盈亏统计
5. **用户体系**：邮箱登录/注册（Supabase Auth）
6. **主题**：深色/浅色切换

## 数据源

- 基金实时估值：天天基金 `fundgz.1234567.com.cn`
- 基金历史净值：天天基金 `api.fund.eastmoney.com`
- A股实时行情：新浪财经 `hq.sinajs.cn`
- A股历史K线：东方财富 `push2his.eastmoney.com`

## 包管理规范

**仅允许使用 pnpm** 作为包管理器。

## 数据库

- 表：watchlist, alert_rules, trade_logs
- RLS：场景 D（用户私有数据），所有表通过 user_id + auth.uid() 隔离
- 字段命名：snake_case

## 设计规范

参见 DESIGN.md。关键：红涨绿跌、移动端优先、Noto Sans SC + JetBrains Mono。
