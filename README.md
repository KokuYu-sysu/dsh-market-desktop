# dsh-market-desktop

The plugin market for [DSH Desktop](https://github.com/anywhere-labs/deepseek-harness-desktop) (the Electron desktop app of DeepSeek Harness). Forked from [dsh-market](https://github.com/dsh-market/dsh-market) (MIT License).

## Changes vs upstream

- **Desktop compatibility**: peer/dev dependencies pinned from `^0.1.0-rc.7` down to `^0.1.0-rc.6`, matching the dsh version bundled with DSH Desktop, so the market shows up in Desktop's settings page.
- **Install fix**: fixes `ERR_PNPM_LINKED_PKG_DIR_NOT_FOUND` (`not install from ... as it does not exist`) when installing any plugin — if the profile has a stale `file:node_modules/<pkg>` dependency, the market now rewrites it to `link:../node_modules/<pkg>` and retries automatically.

## Install

### Option 1: install straight from this repo (recommended)

```sh
dsh plugin --profile desktop add github:KokuYu-sysu/dsh-market-desktop
```

### Option 2: clone, build locally, install (for developers / further changes)

```sh
git clone https://github.com/KokuYu-sysu/dsh-market-desktop.git
cd dsh-market-desktop
pnpm install
pnpm build
dsh plugin --profile desktop add .
```

### About npm

`dsh plugin add dshmarket` installs the **upstream** `dshmarket` package from npm (rc.7 deps), **not this fork**.
To offer an npm install, publish this fork under a new package name (e.g. `dsh-market-desktop`).

## Usage

1. Start DSH Desktop.
2. Open **Settings → Plugin Market**.
3. Browse / search and click **Install**.

> The first install needs pnpm; the market prompts at the top and offers one-click setup.

## FAQ

### `not install from ... as it does not exist`

The profile has a stale local dependency. This version auto-repairs it; if it still fails, check the `file:` / `link:` entries in `~/.dsh/profiles/<name>/package.json`.

### "build scripts" authorization prompt

pnpm ≥10 blocks dependency build scripts by default (usually when installing other plugins that need a build step). Click "Allow build scripts and retry" in the market.

## License

[MIT](./LICENSE), copyright by the original authors `fkysly and dsh-market contributors`.
