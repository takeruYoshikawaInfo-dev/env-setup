# Ubuntu Server 24.04 Python 開発環境構築手順書

## 1. 概要
本手順書は、Ubuntu Server 24.04において、Python 3 仮想環境（venv）を構築するための標準的な手順を記述する。

## 2. 前提条件
* OS: Ubuntu Server 24.04 LTS
* ネットワーク: インターネット接続が確立されていること
* 権限: sudo実行権限を持つユーザーで操作すること

## 3. システムパッケージの準備
Python3本体、および仮想環境構築に必要なモジュールをインストールする。

```bash
# パッケージリストの更新
sudo apt update
# 必要パッケージのインストール
sudo apt install -y python3-venv python3-pip build-essential
```

## 4. プロジェクトディレクトリの作成
ホームディレクトリ配下に、開発作業用のディレクトリを作成する。
```bash
# プロジェクトディレクトリの作成（例: my-python-app）
mkdir ~/my-python-app
# 作成したディレクトリへ移動
cd ~/my-python-app
```

## 5. 仮想環境（venv）の構築
プロジェクトディレクトリ直下に `.venv` という名前で仮想環境を作成する。
```bash
# 仮想環境の作成
python3 -m venv .venv
```

## 6. 仮想環境の有効化とパッケージ管理
作成した環境をアクティベートし、最新の `pip` を導入する。
```bash
# 仮想環境のアクティベート
source .venv/bin/activate
# pip 本体のアップグレード
pip install --upgrade pip
```

## 7. 動作確認
外部ライブラリをインストールし、正しく実行できるか確認する。
```bash
# テスト用ライブラリのインストール
pip install requests

# 確認用スクリプトの作成
cat <<EOF > main.py
import requests
import sys

print(f"Python Path: {sys.executable}")
try:
    res = requests.get('https://www.google.com', timeout=5)
    print(f"Internet Check: Status {res.status_code} (Success)")
except Exception as e:
    print(f"Error: {e}")
EOF

# スクリプトの実行
python3 main.py

#スクリプト実行後、以下のような出力が得られれば構築成功である。

(.venv) <username>@<hostname>:~/my-python-app$ python3 main.py
Python Path: /home/<username>/my-python-app/.venv/bin/python3
Internet Check: Status 200 (Success)
```



## 8. 運用上の注意
* **環境の終了**: `deactivate` コマンドを実行する。
* **環境の再開**: プロジェクトディレクトリにて `source .venv/bin/activate` を実行する。
* **IDE（VS Code等）の設定**: インタープリターのパスとして `~/my-project/.config/venv/bin/python` を指定する。

---
**出典:** [Python venv module documentation](https://docs.python.org/3/library/venv.html)
