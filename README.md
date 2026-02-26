# rust-rebuilder

`rust-rebuilder` 是一个用于 **将现有项目分阶段重写到 Rust** 的 Codex Skill，重点是：

- 行为等价优先（先对齐语义，再优化）
- 小步可验证迁移（每批都可编译、可测试、可回归）
- 上游仓库持续同步（commit / PR / release 跟踪）
- 后端 Rust 编码规范内建（SQLx、Tokio、错误处理、可见性等）

## 核心能力

- 迁移工作流：范围定义 → 架构映射 → 批次切片 → 重写实施 → 回归验证 → 上游同步
- 依赖预检：自动检测 `grok-search`（skill 或 MCP）与 `github-helper` skill
- 风险控制：提供重写误区清单，避免“能跑但不可维护”的迁移结果
- 后端守卫：对 API/DB/异步任务类代码强制执行 Rust backend guardrails

## 仓库结构

```text
.
├── SKILL.md
├── agents/openai.yaml
├── references/
│   ├── rust-language-update-playbook.md
│   ├── rust-backend-guidelines.md
│   ├── rewrite-pitfalls-and-antipatterns.md
│   └── github-upstream-sync.md
└── scripts/
    ├── check_dependencies.py
    └── upstream_sync_report.py
```

## 依赖要求

至少满足以下条件：

1. 已安装 `grok-search` skill **或** 已配置 `grok-search` MCP
2. 已安装 `github-helper` skill

推荐仓库：

- grok-search skill: <https://github.com/Frankieli123/grok-skill>
- grok-search MCP: <https://github.com/GuDaStudio/GrokSearch>
- github-helper: <https://github.com/dandandujie/github-helper>

## 快速开始

1. 运行依赖预检：

```bash
python3 scripts/check_dependencies.py
```

2. 使用技能发起重写任务（示例）：

```text
使用 $rust-rebuilder，把 <源项目/模块> 分三批重写到 Rust；
每批给出等价验证方案、风险点和与 upstream 同步策略。
```

3. 若为后端代码，先阅读：

- `references/rust-backend-guidelines.md`

4. 若源项目在 GitHub，生成上游差异报告：

```bash
python3 scripts/upstream_sync_report.py --repo /path/to/rewrite-repo --branch main
```

## 输出约定（每批次）

- Rewrite Scope
- Equivalence Strategy
- Rust Design Decisions
- Risk Register
- Upstream Sync Notes

## 适用场景

- 单体/微服务项目迁移到 Rust
- 需要长期跟随上游仓库演进的重写项目
- 对性能、稳定性、可维护性有明确要求的后端系统重构
