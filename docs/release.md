# 发布流程（Release）

每个插件**独立语义化版本、独立 release**；总项目 `dsh-plugins` 只锁 submodule 指针。

## 1. 插件发版（以 `dsh-multi-user` 为例）

1. 在插件仓库改代码、自测、`verify.sh` 通过。
2. **升版本号**：改 `package.json` 的 `version` 与 `packaging/manifest.json`（务必同步）。
3. 打可移植包：
   - 把 `payload/lib/*.bak*` 移出；
   - 用 `tools/` 做超集判定（`difflib removed == 0`）确认 payload 不落后于 live；
   - 生成 `plugin/dsh-multi-user-<ver>.tgz` 与 `dsh-multi-user-<ver>-portable.tar.gz`。
4. 提交并 push 到插件仓库 `main`。
5. 在插件仓库 **GitHub Releases** 上传 `.tgz` / `portable.tar.gz`（二进制不入库）。
6. 打 tag `v<ver>`。

## 2. 总项目聚合（bump submodule）

```bash
cd dsh-plugins
git submodule update --remote plugins/dsh-multi-user   # 拉取插件最新 main
git add plugins/dsh-multi-user
# 同步更新 README 版本矩阵里的「当前锁定版本」
git commit -m "chore: bump dsh-multi-user -> v1.0.1"
git push
```

## 3. 新增插件进家族

1. 新建独立私有仓库 `dsh-xxx`，根放源码（`lib/` + `packaging/`）。
2. 本仓库：`git submodule add https://github.com/imasonhk-spec/dsh-xxx.git plugins/dsh-xxx`。
3. 更新 `README.md` 版本矩阵，提交并 push。

## 4. 约定

- 二进制（`.tgz` / `portable.tar.gz` / `node_modules` / `*.bak`）**永不入库**，只走 Release。
- 凭据默认随机生成，源码里不留服务器密码 / IP。
- 换行统一 LF（仓库根 `.gitattributes`：`* text=auto eol=lf`）。
