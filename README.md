 [![Build](https://github.com/kay-ws/claudecode-arch-devcontainer/actions/workflows/build.yml/badge.svg)]
# claudecode-arch-devcontainer

Arch Linux ベースの [Claude Code](https://github.com/anthropics/claude-code) 用 DevContainer。

## 概要

[sour23 氏の Qiita 記事 (Claude Code を DevContainer で実行する)](https://qiita.com/sour23/items/a86a42a675e27853299b) の Arch Linux 移植版。

- **base**: `archlinux:latest`
- **Claude Code**: 公式 [install.sh](https://claude.ai/install.sh) による native binary (npm 経由ではない)
- **配布**: GitHub Actions で毎日 JST 03:00 (UTC 18:00) に rebuild、`ghcr.io/kay-ws/claudecode-arch-devcontainer` に push

## 使い方

### VS Code

`.devcontainer/devcontainer.json` を含むこのリポジトリを clone し、Dev Containers 拡張で "Reopen in Container"。

### CLI (devcontainer + podman)

```bash
devcontainer up \
  --workspace-folder . \
  --docker-path podman \
  --remove-existing-container

devcontainer exec \
  --workspace-folder . \
  --docker-path podman \
  bash
```

### ローカル build (image が GHCR にまだ無い・上書きしたい場合)

```bash
devcontainer up \
  --workspace-folder . \
  --docker-path podman \
  --build
```

## タグ

| タグ | 内容 |
|------|------|
| `latest` | main ブランチの最新 build |
| `nightly` | scheduled build 由来 |
| `YYYYMMDD` | 日付固定 (回帰用) |
| `sha-xxxxxxx` | コミット固定 |

## 個人環境向けの追加マウント

`devcontainer.json` の `mounts` で `~/synced/shared` を container 内に bind しています。これは私 (kay) の Syncthing 同期領域で、Claude 用設定 (`claude-config/`) も含まれます。他環境で利用する場合は不要であれば `mounts` から削除してください。

`postCreateCommand` で `~/.claude/{CLAUDE.md,rules,agents}` を `~/synced/shared/claude-config/*` に symlink します。これも `~/synced/shared` mount が無い環境では dangling symlink になりますが、Claude Code は CLAUDE.md なしでも起動します。

## ライセンス

特になし (個人運用)。
