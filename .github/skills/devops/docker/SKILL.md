---
name: docker
description: >
  Guides Docker image and container workflows: Dockerfile best practices,
  multi-stage builds, compose, layer caching, and safe runtime defaults.
  USE FOR: containerize an app; write/optimize Dockerfile; multi-stage builds;
  docker compose local orchestration; non-root, healthcheck, .dockerignore.
  关键词：Docker、Dockerfile、镜像、容器、compose、多阶段构建、分层缓存、
  非 root、健康检查、.dockerignore。
  DO NOT USE FOR: Kubernetes-only manifests; GitHub Actions YAML without containers
  (use github-actions); generic Linux ops without Docker context (use linux-commands);
  bare-metal app code with no container intent.
license: MIT
---

# Docker

构建小而安全的镜像，并用可重复方式运行容器。

CI 中构建/推送交叉加载 `devops/github-actions`；容器内或主机排查交叉加载 `devops/linux-commands`。

## When to Use

- 编写 / 优化 Dockerfile
- docker compose 本地编排
- 非 root、健康检查、分层缓存问题
- 缩小镜像、分离 build 与 runtime

## When Not to Use

- 纯 GitHub Actions workflow（无容器） → `devops/github-actions`
- 主机进程/日志/磁盘等通用 Linux 排查 → `devops/linux-commands`
- 仅 Kubernetes YAML / Helm，无 Docker 镜像问题
- 不涉及容器的应用功能开发

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 应用启动命令与端口 | Yes | ENTRYPOINT/CMD 与对外端口 |
| 运行时与构建工具链 | Recommended | 如 Node 20、.NET 8、Python 版本 |
| 目标环境 | Optional | 本地 compose / CI 镜像 / 生产 registry |
| 基础镜像约束 | Optional | distroless、alpine、公司私有基础镜像 |

## Workflow

### Step 1: 镜像设计

1. 选精简基础镜像；用多阶段构建分离 build 与 runtime。
2. 依赖安装层与源码层分开，最大化缓存。
3. 用 `.dockerignore` 排除密钥、`.git`、`node_modules` 等。

### Step 2: 运行时安全默认

1. 以非 root 用户运行；只暴露必要端口。
2. 生产机密勿写进镜像或 compose 明文（用密钥注入）。
3. 需要时加 `HEALTHCHECK` 或编排层探针。

### Step 3: 本地与 CI

1. 本地编排优先 `compose.yml`。
2. CI 构建/推送步骤对接 `devops/github-actions`（权限、OIDC、cache）。
3. 验证：本地 `docker build` / `run`；按 Validation 检查。

## Examples

### 多阶段 Node 镜像

```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
USER node
COPY --from=build /app/dist ./dist
CMD ["node", "dist/server.js"]
```

### 构建与运行

```bash
docker build -t myapp:local .
docker run --rm -p 8080:8080 myapp:local
```

### Compose 骨架（概念）

```yaml
services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      - NODE_ENV=production
    # secrets via env file / orchestrator — never bake into image
```

## Validation

- [ ] 多阶段或等价方式使 runtime 不含构建工具链（适用时）
- [ ] `.dockerignore` 排除密钥与无关大目录
- [ ] 默认非 root；仅暴露必要端口
- [ ] 依赖层与源码层分离，利于缓存
- [ ] 无生产密钥写入镜像层或 compose 明文
- [ ] CI 集成时权限与密钥符合 `devops/github-actions` 约定
- [ ] （可选）镜像可构建且容器监听预期端口

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 单阶段巨型镜像 | 多阶段；runtime 只拷产物 |
| 先 COPY 全源码再 npm install | 先 lockfile + 安装，再 COPY 源码 |
| root 运行 | `USER` 非特权用户 |
| 密钥进镜像 / ARG 进历史 | BuildKit secret；运行时注入 |
| latest 标签漂移 | 钉次版本或摘要 |
| 把 CI YAML 细节堆进本技能 | 交叉 `github-actions` |
| 容器内排查命令不当 | 交叉 `linux-commands` |

## References

- 相关技能：`devops/github-actions`、`devops/linux-commands`
- [Dockerfile best practices](https://docs.docker.com/build/building/best-practices/)
---
