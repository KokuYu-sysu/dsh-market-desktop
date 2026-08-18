# 下载与使用说明

> 本仓库是基于 [dsh-market](https://github.com/dsh-market/dsh-market)（MIT License）修改的版本，
> 额外适配了 **DSH Desktop**（deepseek-harness-desktop，内置 dsh `0.1.0-rc.6`）。
> 原项目版权归 `fkysly and dsh-market contributors` 所有，许可证见 [`LICENSE`](./LICENSE)。

## 一、这是什么

装在 DeepSeek Harness（含 DSH Desktop）里的可视化插件市场。打开 **设置 → 插件市场**，浏览、搜索社区插件并一键安装 / 更新 / 卸载。

## 二、相对原版做了什么改动

1. **Desktop 兼容**：把 peer / dev 依赖从 `^0.1.0-rc.7` 下调到 `^0.1.0-rc.6`，与 DSH Desktop 内置的 dsh 版本对齐，使市场能在 Desktop 的设置页正常出现。
2. **安装失败修复**：修复了安装任意插件时报
   `ERR_PNPM_LINKED_PKG_DIR_NOT_FOUND — not install from "...node_modules\<pkg>" as it does not exist`
   的问题。当 profile 里残留了失效的 `file:node_modules/<pkg>` 依赖时，市场会自动把它修复成
   `link:../node_modules/<pkg>` 并重试，不再需要手动改 `package.json`。

## 三、下载 / 安装

### 方式 1：从 npm 安装（最简单，需已发布）

```sh
dsh plugin --profile web add dshmarket
```

或安装到 desktop profile：

```sh
dsh plugin --profile desktop add dshmarket
```

重启 DSH 后打开 **设置 → 插件市场**。

### 方式 2：从 GitHub 仓库直接安装

```sh
# 把 <你的用户名>/<仓库名> 换成实际的仓库地址
dsh plugin --profile web add github:<你的用户名>/<仓库名>
```

> 注意：本仓库的 `lib/` 构建产物不在版本库里（被 `.gitignore` 忽略）。从 GitHub 直接安装时，
> 市场会触发构建脚本，pnpm ≥10 默认拦截构建脚本，需要在市场里点击「允许构建脚本并重试」。
> 若只想开箱即用，推荐使用方式 1（npm）或方式 3（本地构建）。

### 方式 3：克隆源码，本地构建后安装（最可控）

```sh
git clone https://github.com/<你的用户名>/<仓库名>.git
cd <仓库名>

# 安装依赖并构建（生成 lib/ 与 client/client.js）
pnpm install
pnpm build

# 以本地目录安装到某个 profile（用绝对路径或相对路径均可）
dsh plugin --profile web add .
```

> 在 PowerShell 里 `add .` 会按当前目录解析；其它 shell 同理。若 `dsh plugin` 提示路径问题，
> 直接传仓库的绝对路径，例如 `dsh plugin --profile web add "D:\path\to\dsh-market"`。

## 四、使用

1. 启动 DSH（命令行 `dsh web`，或直接打开 DSH Desktop）。
2. 进入 **设置 → 插件市场**。
3. 「发现」页浏览 / 搜索插件，点击「安装」；「已安装」页管理已装插件（更新、卸载、开关、分组、加载顺序、备份恢复等）。
4. 多数插件刷新页面即生效；无法热加载的变更会在顶部提示，一键重启即可。

> 首次安装需要 pnpm。市场会在页面顶部提示，并提供「一键安装 pnpm」按钮；
> 也可以手动执行 `npm i -g pnpm` 或 `corepack enable pnpm`。

## 五、常见问题

### 安装时报 `not install from ... as it does not exist`

这是 profile 里存在失效的本地依赖（`file:`/`link:` 指向不存在的目录）。本版本的市场会自动修复并重试；
若仍失败，请手动检查 `~/.dsh/profiles/<name>/package.json` 里的 `file:` / `link:` 条目是否指向有效目录。

### 设置页看不到「插件市场」

确认装到了**正在运行的那个 profile**（Desktop 对应 `desktop`，网页版对应 `web`），并且重启过 DSH。

### 提示需要构建脚本授权

pnpm ≥10 默认禁止依赖执行构建脚本，这是安全默认值。点击市场里的「允许构建脚本并重试」即可。

## 六、许可证

本仓库沿用原项目的 **MIT License**。你可以自由使用、修改、分发，但必须保留
[`LICENSE`](./LICENSE) 中的版权声明与许可条款（Copyright (c) 2026 fkysly and dsh-market contributors）。
