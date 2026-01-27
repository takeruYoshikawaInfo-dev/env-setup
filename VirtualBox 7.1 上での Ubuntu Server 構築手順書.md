# VirtualBox への Ubuntu Server 構築手順書


## 1. 概要
本資料は、ネットワーク構成に「ブリッジアダプター」を採用し、Ubuntu Server 24.04 LTS を構築する手順をまとめたものである。これにより、ホストOSから仮想マシンのIPアドレスへ直接 SSH 接続を行う。


## 2. 前提条件
* Host OS: Windows 11
* Software: Oracle VM VirtualBox, Tera Term インストール済み
* Network: ルーター等の DHCP サーバーが存在し、同一LAN内で通信可能な環境であること
* ISO Image: ubuntu-24.04-live-server-amd64.iso


## 3. 構築手順

### ステップ 1: 仮想マシンの作成とリソース割当
1. VirtualBox で「新規(N)」をクリック。
2. 以下の項目を設定する。
   * 名前: Ubuntu-Bridge
   * ISO イメージ: ダウンロードしたファイルを選択
   * タイプ: Linux / バージョン: Ubuntu (64-bit)
   * 自動インストールをスキップ(U): チェックを入れる
   <img width="815" height="559" alt="image" src="https://github.com/user-attachments/assets/7b3c6c75-a7c0-40f6-a36f-b64b188685d9" />

  
3. ハードウェアの割り当て（推奨値）:
   * メモリ: 4096 MB
   * プロセッサー: 2 CPU
   * ストレージ: 25.00 GB (VDI, 動的割当)
   <img width="815" height="559" alt="image" src="https://github.com/user-attachments/assets/a5a5c943-ad65-4d27-8e08-1b8146a30e7e" />


### ステップ 2: ネットワーク設定（ブリッジへの変更）
1. 作成した仮想マシンを選択し「設定」>「ネットワーク」>「アダプター 1」を開く。
2. 以下の通り設定を変更する。
   * 割り当て: ブリッジアダプター
   * 名前: Windows側で実際に通信に使用しているアダプター（Wi-Fi または Ethernet）を選択。
   <img width="826" height="484" alt="image" src="https://github.com/user-attachments/assets/9e1c5a59-8acd-49a3-a61a-899bd3e891c0" />




### ステップ 3: Ubuntu Server のインストール
1. 仮想マシンを起動し、インストーラーを開始。
2. Network connections 画面で、eth0 等に 192.168.xx.xx などの IP アドレスが自動で割り当てられていることを確認。
3. Profile setup 画面で、ログイン用のユーザー名とパスワードを設定。
4. SSH Setup 画面で **[X] Install OpenSSH server** にチェックを入れる（スペースキーで選択）。
   <img width="1282" height="875" alt="image" src="https://github.com/user-attachments/assets/d212247b-1c54-449d-9849-1faf52c2dc67" />

6. インストール完了後、Reboot Now を選択。


## 4. SSH 接続手順 (Tera Term)

### ステップ 4: Ubuntu の IP アドレス確認
仮想マシンのコンソール上でログインし、以下のコマンドで現在の IP アドレスを確認する。

# IP アドレス確認コマンド
```bash
ip addr show enp0s3
# (inet の後ろにある 192.168.x.x 等の数値をメモする)
```

## ステップ 5: Tera Term での接続
1. Tera Term を起動。
2. 「新しい接続」ウィンドウで以下を入力。
   * ホスト: ステップ4で確認した IP アドレス
   * TCPポート#: 22
   * サービス: SSH
3. 「OK」をクリックし、ユーザー名とパスワードを入力してログイン。

## 6. トラブルシューティング
* IP アドレスが取得できない:
  * ブリッジアダプターの設定で、正しい親アダプター（ホストOSが現在使っているもの）が選ばれているか確認してください。
* SSH 接続がタイムアウトする:
  * Windows 側のファイアウォール設定、または公共 Wi-Fi などの「プライバシーセパレーター」機能により同一ネットワーク内の通信が制限されていないか確認してください。

## 7. 情報出典元
* VirtualBox Networking Modes: https://www.virtualbox.org/manual/ch06.html
* Ubuntu Server Guide: https://ubuntu.com/server/docs
* Ubuntu Server Download : https://ubuntu.com/download/server
