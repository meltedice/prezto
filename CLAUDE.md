# CLAUDE.md

このリポジトリは [Prezto](https://github.com/sorin-ionescu/prezto)（zsh 設定フレームワーク）の
個人フォークです。作業ブランチは `meltedice`、設置先は `${ZDOTDIR:-$HOME}/.zprezto`。
詳しいセットアップ手順は `README.meltedice.md` を参照。

## リポジトリ構成

- `runcoms/` … 各 zsh 設定ファイルの実体。インストール時に `~/.zshrc` などへ **シンボリックリンク**される。
  個人のカスタマイズはここを編集する。
  - `zprofile` … ログインシェルで一度だけ読まれる（PATH の土台）
  - `zshenv` … **すべての** zsh プロセスで読まれる（ログイン/非ログイン、Emacs などの GUI 起動シェル含む）
  - `zshrc` … インタラクティブシェルで読まれる
- `modules/` … Prezto のモジュール群。`prompt/external/*` や `syntax-highlighting/external` は
  **git submodule**。`modules/prompt/functions/*` の多くは submodule 内ファイルを指す **symlink**。

## ⚠️ symlink の破損に注意（重要）

`modules/prompt/functions/` 配下や submodule 内には、リンク先の実体を指す symlink が多数ある。例:

- `modules/prompt/functions/prompt_pure_setup` → `../external/pure/pure.zsh`
- `modules/prompt/functions/async` → `../external/pure/async.zsh`
- `modules/prompt/external/pure/pure.plugin.zsh` → `pure.zsh`
- `modules/syntax-highlighting/external/highlighters/*/README.md` → `../../docs/highlighters/*.md`

symlink を保持しないツール（一部の同期/バックアップ/コピー、あるいはエディタでリンク先を実体保存）を
通すと、これらが **通常ファイルに変換**されてしまう。git では `typechange`（`T`）として現れ、
submodule は `-dirty` になる。**これは意図した変更ではないのでコミットしないこと。**

### 復元方法

```sh
# メインリポジトリ側
git checkout -- modules/prompt/functions/async \
  modules/prompt/functions/prompt_agnoster_setup \
  modules/prompt/functions/prompt_meltedice_paradox_setup \
  modules/prompt/functions/prompt_powerline_setup \
  modules/prompt/functions/prompt_pure_setup

# submodule 側（各 submodule ディレクトリ内で実行）
cd modules/prompt/external/pure && git checkout -- pure.plugin.zsh && cd -
cd modules/syntax-highlighting/external && \
  git checkout -- highlighters/{brackets,cursor,line,main,pattern,root}/README.md && cd -
```

判定: `git status` で `typechange`、`ls -l` で本来 `l`（symlink）のものが `-`（通常ファイル）に
なっていれば破損。`git cat-file -p :<path>` で git に記録された本来のリンク先を確認できる。

## シェル設定の方針（runcoms）

- **Node.js は Volta + pnpm**。`nodenv` は廃止（コメントアウト）。
- **Volta / pnpm の設定は `zshenv` に置く**。Emacs など非ログインシェルからも使えるようにするため。
  `zshrc` には「`.zshenv` へ移動した」旨のコメントだけ残す。
- **npm は禁止**。`zshenv` で `npm()` 関数を定義し、実行すると pnpm を促して `return 1` する。
- **Homebrew は Apple Silicon 前提**。`zprofile` で `eval "$(/opt/homebrew/bin/brew shellenv)"`。
- 未使用ツールは削除せず **コメントアウトで無効化**（rbenv / Google Cloud SDK / mysql-client /
  Postgres.app / 1Password CLI / cargo env など）。あとで復帰しやすくするため。
- 追加済みツール: Rancher Desktop/Docker（`~/.rd/bin`）、awsume（`~/.local/bin`）、
  Claude Code の alias（`~/.claude/local/claude`）。
- **fzf** は `zshrc` で `source <(fzf --zsh)` を有効化（`^T`/`^R`/`Alt-C`/Tab 補完）。
  バイナリは Homebrew 管理（リポジトリ外）なので、別マシンでは `brew install fzf` が前提。
- **gwt**（[gko/gwt](https://github.com/gko/gwt)）は git worktree を fzf で切替/作成/削除する
  `gwt` コマンド。本体は **submodule `modules/gwt`**（prezto モジュールではなく `pmodule` 未登録）で、
  `zshrc` から `gwt.sh` と `_gwt.zsh_completion` を source している。fzf 必須。新規 clone 時は
  `git submodule update --init --recursive` で取得すること。

## 運用メモ

- **Emacs の作業ファイル**（`#zshrc#`、`.zshrc.~undo-tree~` など）が `runcoms/` に出ることがある。
  git 管理対象外なので削除する。コミットしない。
- **コミット粒度**: 関心事ごとに分ける。例として直近は次の3つに分割した。
  1. `zprofile`: Apple Silicon の Homebrew を初期化
  2. `zshrc`: 利用ツールの設定を見直し（移行分を除く）
  3. `zshenv`: Volta/pnpm を `zshenv` へ移動し非ログインシェルでも有効化（+ npm 禁止）
- submodule が壊れた状態のときは `README.meltedice.md` の "When submodules are something in
  wrong state" の手順（`rm -rf path-to-submodule` → `git checkout .`）も参照。
