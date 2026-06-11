# Provenr Skills

Provenr 的个人 Codex skills 集合，主要用于规范编码行为和前端代码审查。

## Skills

### 代码行为准则

插件：`code-behavioral-guidelines`

帮助编码智能体减少常见错误，强调：

- 编码前明确假设和取舍
- 优先采用简单、必要的实现
- 只修改与目标直接相关的内容
- 定义成功标准并完成验证

目录：[plugins/code-behavioral-guidelines](plugins/code-behavioral-guidelines)

### Frontend Code Review

插件：`frontend-code-review`

适用于 React、Vue、Next.js、Nuxt 等前端项目的结构化代码审查，覆盖：

- UI 行为、组件设计和状态管理
- Design System 和组件边界
- 可访问性、键盘操作和焦点管理
- API、查询、SSR 和客户端状态边界
- 性能风险和测试覆盖

目录：[plugins/frontend-code-review](plugins/frontend-code-review)

## 安装

在 Codex 中添加 marketplace：

```text
/plugin marketplace add provenr/provenr-skills
```

按需安装插件：

```text
/plugin install code-behavioral-guidelines@provenr-skills
/plugin install frontend-code-review@provenr-skills
```

## 仓库结构

```text
plugins/
├── code-behavioral-guidelines/
│   ├── .codex-plugin/plugin.json
│   ├── assets/
│   └── skills/code-behavioral-guidelines/
└── frontend-code-review/
    ├── .codex-plugin/plugin.json
    ├── assets/
    └── skills/frontend-code-review/
```

## License

MIT
