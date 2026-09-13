# DSH 插件家族（总项目 / Umbrella）

DeepSeek Harness（DSH）全家桶插件的**聚合元仓库**。每个插件是独立版本、独立仓库，通过 **git submodule** 引用；本仓库只负责「指向哪个版本」+ 跨插件文档。

> 配套分项目：[`dsh-raganything-kb`](https://github.com/imasonhk-spec/dsh-raganything-kb)（知识库）· [`dsh-multi-user`](https://github.com/imasonhk-spec/dsh-multi-user)（多用户网关）· [`dsh-plugins-roadmap`](https://github.com/imasonhk-spec/dsh-plugins-roadmap)（未来规划）。

## 仓库结构

```
dsh-plugins/                 ← 本仓库（聚合，不含插件源码）
├── plugins/
│   ├── dsh-raganything-kb   ← 知识库插件（submodule）
│   └── dsh-multi-user       ← 多用户网关插件（submodule）
├── docs/
│   ├── roadmap              ← 未来规划（submodule → dsh-plugins-roadmap）
│   ├── deploy.md            ← 部署总览
│   └── release.md           ← 发布流程
├── README.md
└── .gitmodules
```

## 克隆（含全部子模块）

```bash
git clone --recurse-submodules https://github.com/imasonhk-spec/dsh-plugins.git
cd dsh-plugins
```

之后拉取各插件最新版本：

```bash
git submodule update --remote --recursive
git commit -am "chore: bump submodules"   # 锁定新版本
```

## 插件清单（版本矩阵）

| 插件 | 路径 | 当前锁定版本 | 角色 |
|---|---|---|---|
| 知识库 | `plugins/dsh-raganything-kb` | 0.3.9 | RAG 摄入 / 检索 / 对话产物栏 |
| 多用户网关 | `plugins/dsh-multi-user` | 1.0.0 | 登录门禁 / 每用户独立空间 / Excel 批量导入 |
| 未来规划 | `docs/roadmap` | — | 路线图（非插件，纯文档） |

> 版本号取自各 submodule 的 `package.json` / `manifest.json`，此处为快照，以仓库实际锁定为准。

## 新增一个插件

1. 在 `imasonhk-spec` 下新建独立私有仓库（如 `dsh-xxx`），根目录放插件源码（参考 `dsh-multi-user` 的 `lib/` + `packaging/` 结构）。
2. 回到本仓库：`git submodule add https://github.com/imasonhk-spec/dsh-xxx.git plugins/dsh-xxx`
3. 更新本 README 的版本矩阵，提交。

## 部署 / 发布

- 部署总览：见 [docs/deploy.md](docs/deploy.md)
- 发布流程：见 [docs/release.md](docs/release.md)
