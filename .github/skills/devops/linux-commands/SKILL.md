---
name: linux-commands
description: >
  Guides practical Linux/shell commands for everyday ops: files, processes, logs,
  networking basics, disk usage, and safe sudo usage.
  USE FOR: diagnose process/port/disk/log issues; file permissions and find;
  safe read-first troubleshooting command sequences on GNU/Linux hosts or containers.
  关键词：Linux、shell、bash、命令行、运维命令、日志、进程、端口、磁盘、
  journalctl、ss、权限。
  DO NOT USE FOR: full distribution hardening curricula; Windows PowerShell-only tasks;
  Docker image authoring (use docker); GitHub Actions YAML authoring (use github-actions).
license: MIT
---

# Linux Commands

用可复述、可撤销意识的命令完成日常排查与运维任务。

容器镜像构建用 `devops/docker`；CI workflow 用 `devops/github-actions`。容器内排查仍可用本技能的只读命令习惯。

## When to Use

- 查进程、端口、磁盘、日志
- 文件权限与查找
- 写安全的一串排查命令（先只读后改动）
- 主机或容器内 shell 调试

## When Not to Use

- Dockerfile / compose / 镜像优化 → `devops/docker`
- GitHub Actions workflow 编写 → `devops/github-actions`
- 仅 Windows PowerShell / CMD 任务
- 发行版级安全加固课程（防火墙策略全集、CIS benchmark 全文等）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标主机或容器 shell 访问 | Yes | SSH、本地终端或 `docker exec` |
| 症状描述 | Recommended | 如「8080 无响应」「磁盘满」「服务起不来」 |
| 发行版 / init | Optional | systemd 与否；命令以常见 GNU/Linux 为例 |
| 是否允许改动 | Optional | 默认先只读；破坏性操作需明确批准 |

## Workflow

### Step 1: 只读排查

1. 先 `ls` / `ps` / `df` / `ss` / `journalctl` / `tail`，再谈改动。
2. 记录路径、PID、端口、挂载点，避免猜。

### Step 2: 定位范围

1. 磁盘：`df` → `du` 找大目录。
2. 端口/进程：`ss` / `ps`；确认监听与归属。
3. 日志：`journalctl -u` 或应用日志 `tail`/`less`。

### Step 3: 谨慎改动

1. 破坏性命令先回显路径与 dry-run（若有）。
2. 权限：最小特权；禁止随手 `chmod -R 777`。
3. 管道保持可读；复杂逻辑写成脚本。
4. 给出命令时说明预期输出与风险。

## Examples

### 磁盘与大文件

```bash
df -h
du -h --max-depth=1 /var | sort -h | tail
```

### 端口占用

```bash
ss -lptn | grep ':8080' || true
```

### 服务日志（systemd）

```bash
journalctl -u myapp -n 100 --no-pager
```

### 进程速查

```bash
ps aux | head
pgrep -a myapp || true
```

## Validation

- [ ] 先给出只读排查序列，再给改动（除非用户只要改动命令）
- [ ] 破坏性操作标明风险与可逆性
- [ ] 未推荐 `chmod -R 777` 或无确认的 `rm -rf`
- [ ] 说明预期现象（有/无输出、权限错误等）
- [ ] 命令与发行版工具可用（如 `ss` vs 旧 `netstat`）
- [ ] 容器构建问题已分流到 `docker`；CI YAML 分流到 `github-actions`

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 一上来就删改 | 先只读定位 |
| `chmod 777` 省事 | 最小必要权限 |
| 未确认路径就 `rm -rf` | 先 `ls` / `realpath` |
| 管道不可读的一长串 | 分步或脚本化 |
| 忽略发行版差异 | 注明 GNU/Linux 假设；BusyBox 等需替换 |
| 在容器宿主上误杀进程 | 确认 PID 命名空间 |
| 把 Docker/Actions 细节塞进本技能 | 交叉 `docker` / `github-actions` |

## References

- 相关技能：`devops/docker`、`devops/github-actions`
---
