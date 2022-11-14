# norminetteとc_formatter_42とclangとlldbを1つのDockerイメージにまとめてみた(VSCode devcontainerから使う想定)

タイトルの通り。C言語問題のほとんどに対応する。

ただし、Windows版DockerもWSL2を使うらしく、どうせWSL2を使うのであれば、Ubuntuなどを入れる方がかんたんかもしれない。
VSCodeとの連携は、それぞれに対応する拡張を入れればどちらもできるようです(Ubuntuを使う場合はpipコマンドで2ツールをインストールするなどしてください。code-serverは要るのだろうか……？)。

## 現状でできること・できないこと

コンテナ起動後、

1. インストール済みになっている拡張機能を自分で有効にしないといけない
1. `norminette`と`c_formatter_42`はVSCodeと上手く連携しているっぽい
1. Windows環境で
1. コマンドパレットから実行・デバッグ実行すると起動できない(ビルドはされる): .vscode/launch.json、.vscode/tasks.jsonの記述が正しくないためか？

## 必要なもの(Requirements)

- Docker(利用するときは常に起動していること)
  - (Windows)DockerのためにWSL2もいる
  - (Linux/Linux)`sudo usermod -aG docker $USER`で自分のアカウントでDockerを起動できるようにする
- git(このレポジトリーを一度ダウンロードするためなので、gitを使わず、このページ上部からzipでダウンロードしてもいい)
- VSCode
  - [VSCodeの「Dev Containers」Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)も必要

## Setup

1. 適当なディレクトリーで、次の通りこのレポジトリーをダウンロード(git clone)

   ```sh
   git clone https://github.com/flashingwind/42_c_vscode_container.git
   cd 42_c_vscode_container
   ```
1. 作成されたディレクトリーに`cd 42_c_vscode_container`
1. このディレクトリーに`c00`のようなディレクトリーを置く
1. `code .`でVSCode起動
1. 左下、緑のところをクリック、「Reopen in Container」
1. しばらくコンテナイメージのビルドに時間がかかる(左下の緑の領域に進行状況が表示されている)
1. 起動に成功すると、左下の緑の領域に「Dev Container: 42_c_tools-code-container」と表示され、「Getting Started」画面になる
1. 拡張機能が無効になっている場合があるので、確認し、有効化

### Usage (macOS/Linux)

1. パレットからターミナルを開いて、通常通りコンパイル、実行する
1. norminetteと`c_formatter_42`も動く

#### Docker特有の注意点

Dockerの外でのカレントディレクトリー`.`をDockerコンテナ内のカレントディレクトリー`/code`に関連づけてあり、絶対パスは`/code/〜`となります。

## 注意

将来Pythonのバージョン由来のエラーとか、どうしようもないエラー(Pythonのバージョンが古いとか)が起きたら、Dockerfileの中身を自分で編修することDockerfileの中身を自分で編集してほしい。pull reqest歓迎。レポジトリーのオーナーになってくれるともっとありがたい。

## 謝辞(Thanks)

本レポジトリーは次のレポジトリーをフォークしてほんのちょっと改変したものである。

- [GitHub: alexandregv/norminette-docker)](https://github.com/alexandregv/norminette-docker)
