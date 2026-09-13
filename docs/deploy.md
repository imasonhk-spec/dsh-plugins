# 部署总览（Deploy）

各插件以**零配置可移植包**形式交付到 DSH 主机，不依赖本仓库的 git 历史。

## 交付物

每个插件仓库根部的 `packaging/` 目录（参考 `plugins/dsh-multi-user/packaging/`）包含：

| 文件 | 作用 |
|---|---|
| `install.sh` | 把 `plugin/*.tgz` 链入目标 profile 的 `node_modules/`，并重启 `dsh-web` |
| `uninstall.sh` | 卸载 |
| `manifest.json` | 版本 / 依赖 / 入口声明 |
| `verify.*` | 安装后自检（端口可达、模块加载、端到端冒烟） |
| `tools/` | 运维辅助（nginx 补丁、用户同步校验等） |

> 二进制 `.tgz` 走 GitHub Releases，**不入库**。

## 目标主机角色（示例）

| 主机 | 角色 | 已装插件 |
|---|---|---|
| `192.168.8.6` | 多用户网关（nginx TLS → 多用户网关 → 宿主 DSH） | KB + MU |
| `100.100.6.55` | 单租户 DSH（sidecar 化） | KB |

## 安装步骤（通用）

```bash
# 1) 在目标 DSH 主机取插件可移植包（从对应插件仓库的 Release 下载 .tgz / portable.tar.gz）
# 2) 解包后执行
bash install.sh
# 3) 自检
bash verify.sh
```

## 一致性约束（重要）

在多用户网关（`8.6`）上，**KB 的三处副本必须逐字节一致**：

- 宿主 profile 的 `node_modules/dsh-raganything-kb`
- 多用户 `payload/` 内的 KB 源码
- `plugin/dsh-raganything-kb-*.tgz`（发布产物）

打包前请用 `tools/` 里的超集判定（`difflib removed == 0`）确认 payload 不落后于 live，并把 `payload/lib/*.bak*` 移出再打包。改代码后**必须升版本号**，否则 `install.sh` 不会重链。

## 隔离

多用户场景下，新插件必须支持 `{sidecarPort}` / `{home}` / `{workspace}` / `{slot}` 占位符（见 `dsh-multi-user` 的 `userProfilePatch`），确保每用户独立端口与空间，避免数据串号。
