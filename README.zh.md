# dsh-market-desktop

适用于 [DSH Desktop](https://github.com/anywhere-labs/deepseek-harness-desktop)（DeepSeek Harness 的 Electron 桌面端）的插件市场。基于 [dsh-market](https://github.com/dsh-market/dsh-market)（MIT License）修改。

## 相对原版的改动

- **Desktop 兼容**：把 peer / dev 依赖从 `^0.1.0-rc.7` 下调到 `^0.1.0-rc.6`，与 DSH Desktop 内置的 dsh 版本对齐，使市场能在 Desktop 的设置页正常出现。
- **安装失败修复**：修复安装任意插件时报 `ERR_PNPM_LINKED_PKG_DIR_NOT_FOUND`（`not install from ... as it does not exist`）的问题——当 profile 里残留失效的 `file:node_modules/<pkg>` 依赖时，市场会自动把它改成 `link:../node_modules/<pkg>` 并重试。

## 安装

### 方式 1：从本仓库直接安装（推荐，最简单）

```sh
dsh plugin --profile desktop add github:KokuYu-sysu/dsh-market-desktop
```

### 方式 2：克隆源码，本地构建后安装（适合开发者 / 二次修改）

```sh
git clone https://github.com/KokuYu-sysu/dsh-market-desktop.git
cd dsh-market-desktop
pnpm install
pnpm build
dsh plugin --profile desktop add .
```

### 关于 npm

`dsh plugin add dshmarket` 安装的是 npm 上的**原版** `dshmarket`（依赖 `0.1.0-rc.7`），**不是本分支**。
若希望用户用 npm 安装，请把本分支以一个新包名（例如 `dsh-market-desktop`）发布到 npm。

## 使用

1. 启动 DSH Desktop。
2. 打开 **设置 → 插件市场**。
3. 浏览 / 搜索插件，点击「安装」。

> 首次安装需要 pnpm；市场页顶部会提示，并支持一键安装。

## 常见问题

### 安装报 `not install from ... as it does not exist`

profile 里有失效的本地依赖。本版本会自动修复；若仍失败，检查 `~/.dsh/profiles/<name>/package.json` 里的 `file:` / `link:` 条目。

### 提示需要构建脚本授权

pnpm ≥10 默认禁止依赖执行构建脚本（通常发生在安装其它需要构建的插件时）。点击市场里的「允许构建脚本并重试」即可。

## 许可证

[MIT](./LICENSE)，版权归原作者 `fkysly and dsh-market contributors` 所有。
