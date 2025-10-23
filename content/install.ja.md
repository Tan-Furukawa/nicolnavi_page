+++
date = '2025-10-15T13:10:14+09:00'
draft = false
title = 'NicolNavi インストールガイド'
+++


NicolNaviを安全かつ確実に導入するための手順をまとめました。ここでは「Dockerを利用する方法」と「Python環境を利用する方法」を順を追って解説します。作業前に、以下の事項を必ずご確認ください。

## 事前準備と前提条件

- 対応OS: Windows 10 以降 (64bit)、macOS 12 Monterey 以降、主な Linux ディストリビューション
- 管理者権限: 初回インストール時に要求される場合があります
- ブラウザ: Google Chrome 最新版 (推奨)
- メモリ16GB以上のPCを強く推奨します。それ以下のPCでは、正常に機能しない可能性があります。

{{< notice info >}}Google Chrome 以外のブラウザでも動作する可能性はありますが、検証済みではありません。{{< /notice >}}

## 導入方法の選び方

 - **Docker を利用**: 自動で必要な Docker 環境を構築します。環境差異を避けたい場合に最適です。
- **Python を利用**: 既存の Python 環境を活用したい、またはスクリプトを直接実行したい方に向いています。

どちらか一方の手順を選び、完了するまで順番に進めてください。

---

## Docker を利用したインストール

### Step 1. 必要ソフトのインストール

1. **Google Chrome の導入 (未インストールの場合)**
   - https://www.google.com/intl/ja_jp/chrome/ から最新のインストーラを入手し、画面の案内に従ってインストールします。
2. **Docker Desktop のインストール**
   - https://www.docker.com/ja-jp/get-started/ から、お使いのOSに合わせて `Docker Desktop Installer.exe` (Windows) または `Docker.dmg` (Mac) をダウンロードします。
   - インストーラの指示に従い、「Installation succeeded」が表示されるまで進めてください。完了後は「Close and log out」をクリックし、PCを再起動します。
   - 再起動後に Docker Desktop が自動的に起動し、利用規約が表示されたら「Accept」を押します。サインインは任意です。

{{< notice info >}}WindowsではWSL2の有効化や更新を求められる場合があります。Docker Desktopが指示する手順に従い、完了後に再度起動してください。{{< /notice >}}

### Step 2. NicolNavi の取得

1. GitHub リポジトリ (https://github.com/Tan-Furukawa/nicolnavi) を開き、`Download ZIP` を選択します。
2. ダウンロードした ZIP ファイルを任意のフォルダに展開し、内容一式を保持します。

![GitHub ダウンロード手順](/images/page/install/install_github.png)

### Step 3. Docker Desktop の起動と確認

1. Docker Desktop をダブルクリックして起動します。

{{< notice warning >}}Docker Desktop が停止していると環境の準備に失敗する可能性があります。確実に起動してください。{{< /notice >}}

### Step 4. インストーラの実行

1. 展開したフォルダ内から、OSに合わせて以下のファイルをダブルクリックします。
   - Windows: `win_nicolnavi_install.bat`
   - macOS / Linux: `mac_nicolnavi_install.sh`
2. 端末やコマンドプロンプトが開き、必要なファイルのダウンロードと初期設定が自動で実行されます。画面の表示が完了したらウィンドウを閉じて構いません。

{{< notice info >}}macOS / Linuxでダブルクリックした際に実行権限エラーが出る場合は、ターミナルで `chmod +x mac_nicolnavi_install.sh` を実行してから再度試してください。{{< /notice >}}

### Step 5. アプリの起動確認

1. 同じフォルダから、OSに合わせて以下の起動スクリプトをダブルクリックします。
   - Windows: `win_nicolnavi_exe.bat`
   - macOS / Linux: `mac_nicolnavi_exe.sh`
2. ブラウザで `http://localhost:8551/` にアクセスし、NicolNavi の画面が表示されればセットアップ完了です。

{{< notice warning >}}起動スクリプトを先に実行していないと `http://localhost:8551/` に接続できません。必ず手順通りに実行してください。{{< /notice >}}

{{< notice note >}}NicolNavi はローカルの Docker 環境で動作するため、インターネットに接続していなくても閲覧できます。{{< /notice >}}

---

## Python 環境を利用したインストール

### Step 1. 必要ソフトの確認

1. **Google Chrome**
   - 未導入の場合は前述のリンクからインストールしてください。
2. **Python および pipenv**
   - Python 3.10 以上、`pip` コマンドがインストールされていることを確認します。
   - `pip install --upgrade pip` で pip を最新化し、`pip install pipenv` を実行して pipenv を導入します。

### Step 2. NicolNavi の取得

1. GitHub リポジトリ (https://github.com/Tan-Furukawa/nicolnavi) から ZIP をダウンロードし、任意のディレクトリに展開します。
2. ターミナルを開き、展開したフォルダへ移動します。

```bash
cd /path/to/nicolnavi
```

### Step 3. 仮想環境の構築と依存関係のインストール

1. ターミナルで以下のコマンドを順に実行し、仮想環境を構築します。

```bash
pipenv shell
pipenv install
```

2. `Pipfile.lock` が作成され、依存パッケージのインストールが完了したら準備完了です。

{{< notice info >}}`pipenv shell` 実行後のプロンプトに `(nicolnavi)` などの仮想環境名が表示されることを確認してください。終了する際は `exit` と入力すると仮想環境から抜けられます。{{< /notice >}}

### Step 4. アプリの起動確認

1. 仮想環境がアクティブな状態で以下のコマンドを実行します。

```bash
python gui/src/main.py
```

2. 別途ブラウザを起動し、`http://localhost:8551/` にアクセスしてNicolNaviの画面が表示されるか確認します。

---

## トラブルシューティング

- **ブラウザで画面が表示されない**: 起動スクリプトや `python gui/src/main.py` が動作中か確認し、停止している場合は再実行してください。
- **ポート 8551 が使用中と表示される**: 既に別アプリが同ポートを使用中です。該当アプリを停止するか、設定ファイルでポート番号を変更してください。
- **Docker イメージの取得が進まない**: ネットワーク接続を確認し、プロキシ環境では適切な設定が行われているか確認してください。
- **pipenv install で失敗する**: `pipenv --rm` で仮想環境を削除し、再度 Step 3 のコマンドを実行してください。

不明点やエラーが解決しない場合は、エラーメッセージとともに開発者へお問い合わせください。
