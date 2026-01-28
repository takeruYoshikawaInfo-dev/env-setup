# UbuntuにおけるJava (OpenJDK 21) 環境構築手順書

## 概要
本手順書では、Ubuntu OSにおいて標準のパッケージマネージャー `apt` を使用し、Javaの開発環境（OpenJDK 21）をインストールし、パスの設定および動作確認を行う手順を説明します。

## 前提条件
* OS: Ubuntu 20.04 LTS 以降（22.04 / 24.04 推奨）
* 権限: `sudo` 実行権限を持つユーザーであること
* インターネット接続が確保されていること

## 手順

### 1. パッケージリストの更新
リポジトリのインデックスを最新の状態に更新し、最新のパッケージがインストールされるようにします。

```bash
# パッケージインデックスの更新
sudo apt update
```

### 2. OpenJDK 21 のインストール
JDK (Java Development Kit) をインストールします。これにはコンパイラ（javac）と実行環境（java）の両方が含まれます。

```bash
# OpenJDK 21 JDKのインストール
sudo apt install -y openjdk-21-jdk
```

### 3. インストールの確認
インストールが正常に完了したことを確認するため、バージョン情報を表示します。

```bash
# Java実行環境のバージョン確認
java -version

# Javaコンパイラのバージョン確認
javac -version
```

### 4. 環境変数の設定 (JAVA_HOME)
多くのJavaアプリケーションやビルドツール（Maven/Gradle等）で必要となる `JAVA_HOME` 環境変数を設定します。

まず、インストールパスを確認します。
```bash
# インストールパスの確認
readlink -f $(which java) | sed "s:/bin/java::"
```

出力されたパス（例: `/usr/lib/jvm/java-21-openjdk-amd64`）を `~/.bashrc` に追記します。

```bash
# 環境変数をシェル設定ファイルに追加
echo "export JAVA_HOME=$(readlink -f $(which java) | sed 's:/bin/java::')" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc

# 設定の反映
source ~/.bashrc
```

## 確認方法
環境変数が正しく設定されているか確認します。

```bash
# JAVA_HOMEの値を表示
echo $JAVA_HOME
```
`/usr/lib/jvm/java-21-openjdk-amd64` のようなパスが表示されれば成功です。

## トラブルシューティング
* **複数のJavaバージョンが混在する場合**:
  以下のコマンドを使用して、デフォルトで使用するバージョンを選択してください。
  ```bash
  # デフォルトJavaの切り替え
  sudo update-alternatives --config java
  ```
* **パッケージが見つからない場合**:
  `sudo apt update` を再度実行してください。古いUbuntuバージョンを使用している場合は、`ppa:openjdk-r/ppa` の追加が必要になる場合があります。

---
出典: [Ubuntu Packages Search](https://packages.ubuntu.com/)
