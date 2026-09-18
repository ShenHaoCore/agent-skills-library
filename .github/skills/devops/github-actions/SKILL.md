---
name: github-actions
description: >
  Guides GitHub Actions workflows: YAML triggers, jobs, matrix builds, caching,
  secrets/OIDC, permissions, and CI/CD best practices.
  USE FOR: create or fix .github/workflows/*.yml; matrix/cache/artifacts;
  secrets and OIDC cloud login; CI vs CD split; concurrency/timeouts; Actions hardening.
  关键词：GitHub Actions、CI、CD、workflow、矩阵构建、matrix、cache、secrets、OIDC、
  yaml pipeline、permissions、concurrency。
  DO NOT USE FOR: Jenkins/GitLab CI specifics; deep Dockerfile authoring (use docker);
  generic Linux sysadmin without Actions context (use linux-commands);
  app feature code unrelated to CI/CD.
license: MIT
---

# GitHub Actions

编写可靠、安全、可维护的 GitHub Actions workflow：正确触发、快速反馈、密钥不泄露。

镜像构建细节交叉加载 `devops/docker`；主机排查交叉加载 `devops/linux-commands`。

## When to Use

- 新建或修改 `.github/workflows/*.yml`
- 配置 matrix、缓存、制品上传、环境保护
- 管理 `secrets` / OIDC 云登录
- 优化 CI 时间、失败重试、并发控制
- 实现 CI（检测）与 CD（部署）分流

## When Not to Use

- Dockerfile / 镜像分层与 compose 深挖 → `devops/docker`
- 纯 Linux 进程/日志/磁盘排查、无 Actions 上下文 → `devops/linux-commands`
- Jenkins、GitLab CI、Azure Pipelines 等非 GitHub Actions
- 与流水线无关的应用业务逻辑实现

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 仓库与目标分支模型 | Recommended | PR / main / tag 触发策略 |
| 语言与包管理器 | Recommended | 影响 setup、cache、安装命令（如 `npm ci`） |
| 部署目标 | Optional | 云厂商、environment、OIDC 需求 |
| 安全要求 | Optional | 是否 pin SHA、是否禁用 `pull_request_target` 等 |

## Workflow

### Step 1: 明确流水线目标

1. PR：lint / test / build（快）。
2. main / tag：发布制品或部署（严）。
3. 用 `on:` 精确触发（含 `paths`），避免无意义空跑。

### Step 2: 最小权限与结构

1. 设置 `permissions:` 到所需最小值（如 `contents: read`）。
2. 部署用 `environment:` + 保护规则。
3. 一个 job 一个职责；用 `needs:` 表达依赖；步骤命名可读。

### Step 3: Matrix、缓存与依赖

1. OS × 版本矩阵控制组合数；用 `exclude` / `include`；`fail-fast: false` 看全失败面。
2. 用官方 `actions/cache` 或 setup-* 内置缓存；key 含 lockfile 哈希。
3. 依赖通过 lockfile 安装（`npm ci` / `dotnet restore` 等）。

### Step 4: 密钥与稳定性

1. 只用 GitHub Secrets / OIDC；永不 echo 密钥。
2. 固定 action 大版本或 commit SHA（安全敏感场景）。
3. `concurrency:` 取消过期 PR 运行；设置合理 `timeout-minutes`。

### Step 5: 验证

按 Validation 清单检查；安全与性能细节见 `references/actions-hardening.md`。容器构建步骤可对接 `devops/docker`。

## Examples

### 基础 CI

```yaml
name: ci
on:
  pull_request:
  push:
    branches: [main]

permissions:
  contents: read

concurrency:
  group: ci-${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  test:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: npm
      - run: npm ci
      - run: npm test
```

### Matrix 构建

```yaml
jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest]
        node: [18, 20]
        exclude:
          - os: windows-latest
            node: 18
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
          cache: npm
      - run: npm ci && npm run build
```

### 手动缓存示例

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.nuget/packages
    key: nuget-${{ runner.os }}-${{ hashFiles('**/*.csproj') }}
    restore-keys: |
      nuget-${{ runner.os }}-
```

### 环境与 OIDC（概念）

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    permissions:
      contents: read
      id-token: write  # OIDC
    steps:
      - uses: actions/checkout@v4
      # 使用云厂商 OIDC action，避免长期 AK/SK
```

## Validation

- [ ] `permissions` 最小化
- [ ] PR 与发布流水线分离（或明确分 job）
- [ ] 依赖通过 lockfile 安装
- [ ] 无密钥打印；secrets 仅存机密存储
- [ ] 有 timeout 与 concurrency（适用时）
- [ ] Action 版本有意固定
- [ ] 镜像相关步骤符合 `devops/docker` 安全默认（若构建镜像）

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 默认宽 `permissions` | 显式最小权限 |
| 误用 `pull_request_target` + 不可信代码 | 见 `references/actions-hardening.md` |
| 缓存 key 不含 lockfile | key 含哈希；避免污染 |
| 长期云密钥进 Secrets 且不轮换 | 优先 OIDC |
| 无 timeout，队列占满 | `timeout-minutes` + concurrency |
| 把 Dockerfile 优化写进本技能 | 交叉 `devops/docker` |

## References

- 安全与性能要点：`references/actions-hardening.md`
- 相关技能：`devops/docker`、`devops/linux-commands`
- [GitHub Actions documentation](https://docs.github.com/en/actions)
---
