# Neovim (LazyVim) 構築トラブルシューティング備忘録


### 1. Neovim バージョン不足 (v0.9.5)
* **事象**: LazyVim を起動しても即終了、あるいはエラーを吐く。
* **原因**: LazyVim の要件が Neovim v0.10.0 以上であるため。Ubuntu 24.04 (noble) の標準 `apt` では v0.9.5 しか入らない。
* **解決**: 
    * GitHub から最新の安定版 `nvim-linux-x86_64.tar.gz` を `curl -SL` でダウンロード。
    * `/opt` などに展開し、`/usr/local/bin/nvim` にシンボリックリンクを貼る。
    * `sudo apt remove neovim` で古い方を削除し、`hash -r` でパスキャッシュをクリア。

### 2. curl ダウンロード失敗 (9バイトの罠)
* **事象**: `curl` で落としたバイナリが実行できず、中身が "Not Found" などのエラーテキストになる。
* **原因**: GitHub のリダイレクトに追従できていない。
* **解決**: `curl` に `-L` (または `-SL`) オプションを必ず付与して、転送先（リダイレクト先）まで追いかける。

### 3. lazy.nvim のコミットエラー (commit is nil)
* **事象**: `init.lua` 読み込み時に `commit is nil` という Lua エラーが出る。
* **原因**: ネットワーク不調等でインストールが中断され、`lazy-lock.json` が壊れた。
* **解決**: 
    * `rm -rf ~/.local/share/nvim/lazy` でプラグイン実体を削除。
    * `rm ~/.config/nvim/lazy-lock.json` でロックファイルを削除して再起動。
