# 现代 React 全栈学习指南

## bulletproof-react 的定位

本项目是**中高级参考架构**，不适合 React 初学者直接上手。原因：

- 同时引入 TanStack Query、Zustand、React Hook Form、Zod、MSW、Plop 等 6+ 个库
- feature-based 分层、单向依赖规则等模式需要一定工程经验才能理解其价值
- 使用 React 18，缺少 React 19 的 `useActionState`、`use()`、`useOptimistic` 等新特性
- 测试体系（Vitest + Testing Library + Playwright + MSW）对初学者门槛较高

---

## 推荐学习路径

### 第一阶段：React 核心（官方文档优先）

按顺序掌握以下 Hook，每个都写一个小 demo 验证理解：

1. `useState` / `useReducer` — 状态管理
2. `useEffect` — 副作用与生命周期
3. `useContext` — 跨组件共享状态
4. `useRef` / `useMemo` / `useCallback` — 性能优化
5. `useTransition` / `useDeferredValue` — 并发特性（React 18+）
6. `useActionState` — 表单与异步操作（React 19）

官方文档：https://react.dev

---

### 第二阶段：全栈骨架（create-t3-app）

用 T3 Stack 生成最小全栈项目，建立前后端心智模型：

```bash
npm create t3-app@latest
```

技术栈：Next.js App Router + tRPC + Prisma + PostgreSQL + NextAuth + Tailwind

**目标：** 跑通一个完整的数据流：`UI 组件 → tRPC router → Prisma → 数据库`

GitHub：https://github.com/t3-oss/create-t3-app

---

### 第三阶段：生产级项目（Cal.com）

在掌握 T3 Stack 后，阅读 Cal.com 源码，学习真实生产级应用的工程实践。

**GitHub：** https://github.com/calcom/cal.com（约 35k stars）

#### 核心技术栈

| 层 | 技术 |
|---|---|
| 框架 | Next.js 14+ App Router |
| API | tRPC（端到端类型安全） |
| ORM | Prisma + PostgreSQL |
| 认证 | NextAuth.js |
| 样式 | Tailwind CSS |
| 测试 | Playwright + Vitest |
| 部署 | Vercel / Docker |

#### 学习步骤

1. 用 Docker 启动本地开发环境
2. 读懂一个完整功能的数据流（如预约创建）
3. 仿写一个小功能，验证理解
4. 研究认证、邮件、支付、Webhook、多租户等可复用模式

---

## 技术选型对比

| 关注点 | bulletproof-react | Cal.com / T3 Stack |
|---|---|---|
| React 版本 | 18.3 | 19+ |
| API 层 | REST + Axios | tRPC（类型安全） |
| 服务端状态 | TanStack Query | tRPC + React Query |
| 表单 | React Hook Form + Zod | Server Actions + Zod |
| 适合阶段 | 中级，学架构规范 | 中高级，学全栈生产实践 |

---

## 关键结论

> 先用官方文档打好 React 基础 → 用 create-t3-app 跑通全栈骨架 → 用 Cal.com 学习生产级工程实践 → 回头看 bulletproof-react 理解大型项目的架构规范。
