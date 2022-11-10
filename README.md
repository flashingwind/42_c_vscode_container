# norminettとc_formatter_42とclangとlldbとvscode serverを1つのDockerイメージにまとめてみた

タイトルの通り。

## 背景

42tokyoという特殊な環境では、ある特定のスタイルのコードを書くことが求められる。
Normという規則に従ったコードだ。より現実的には`norminette`コマンドがエラーを吐かないコードである。
しかし、現実的にはエラーメッセージが難解で、`c_formatter_42`などコードフォーマッターの力を借りないとやってられない。

しかし、norminettはmacOS標準のPythonで動かないというやっかいさがある(c_formatter_42は動くかもしれないが)。

これがWindowsになると、clang系コンパイラーも入れなくてはならず、なんだかよくわからない設定も必要でもっと面倒だ。

そこで、Dockerイメージ・Dockerfileを探したところ、norminettには公式のDockerイメージ・Dockerfileがあるが、c_formatter_42はないようだ。別々にDockerイメージを作るとそこそこ容量を食うことであるし、全部まとめてしまうことにした。

ただし、`c_formatter_42`は`python3 -m c_formatter_42`という形式で実行できなければならないため、Visual Studio Code Serverも中に入れないといけないことになった。

容量をケチる意味ため、肥大化しすぎなのは承知の上で、全部同じイメージに入れた。

## 必要なもの(Requirements and Preperation)

- OSにあったDocker(利用するときは常に起動していること)
  - (Windows)DockerのためにWSL2がいる(だっったらUbuntuでもいいじゃん……)
  - (Linux/Linux)`sudo usermod -aG docker $USER`で自分のアカウントでDockerを起動できるようにする
- git(ダウンロードするためだけなので、ページ上部からzipでダウンロードしてもいい)
- VSCode
  - [VSCodeの「Dev Containers」Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## Setup

1. どこかに42tokyo関係のファイルが全部入ってるディレクトリーがあるとして、そこに`cd`

   ```sh
   git clone https://github.com/flashingwind/42_vscode_server_docker.git
   dockerfile_norminette_and_c_formatter_42.git docker_42_tools
   cd docker_42_tools
   ./c_42_build.sh
   ```

1. 次の通りこのレポジトリーをダウンロード(git clone)し
1. フォルダに入り
1. `docker compose up`で起動
1. VSCodeについては後述する。

これでDockerイメージがビルドされ、vscode-serverが立ちあがる。docker-compose.ymlに指定したポートが公開される。範囲は、あなたのパソコンのファイヤーウォールが適切に設定されていれば、外からはアクセスできない。たぶん。ただ、そうでなくとも一般家庭のルーターのデフォルト設定の場合、最悪でも家の中だけになるだろう。

### Usage (macOS/Linux)

基本的には下記の通りですが、エラー時はDockerの起動と`./c_42_build.sh`を事前に実行して、イメージができているか確認してください。

Linuxでも同様に動くかもしれないです。Windows PowerShellはおそらく付属のスクリプト群の変数を変える必要があります。

#### Docker特有の注意点

Dockerの外でのカレントディレクトリー`.`をDockerコンテナ内のカレントディレクトリー`/code`に関連づけてあり、絶対パスは`/code/〜`となります。

相対パスなら同じになるので、相対パスでの指定をおすすめします。

## vscode連携

連携は初期状態で設定されているはず。

### c_formatter_42

`c_formatter_42`にはやや癖があるので、常時フォーマットさせるのではなく、必要なときだけコマンドパレットに「format」と入力し、「Format Document」などを選択するのが現実的だろう。

## CLIツール

## norminette -R CheckForbiddenSourceHeader

ターミナルで`norminette`と打つとカレントディレクトリー以下のすべての.c/.hファイルが検査される。
大きなお世話ですが、43tokyo向けなので、`norm`と打つと`norminette　-R CheckForbiddenSourceHeader`が実行できる。

```bash
# あるフォルダ以下全部の.cファイルにnorminette実行:
$ cd ~/42/ex00
$  norm
./ex00/ft_putchar.c: OK!
./ex01/ft_print_alphabet.c: OK!
./ex02/ft_print_reverse_alphabet.c: OK!
./ex03/ft_print_numbers.c: OK!
./ex04/ft_is_negative.c: OK!
./ex05/ft_print_comb.c: OK!
./ex06/ft_print_comb2.c: OK!
./ex07/ft_putnbr.c: OK!
./ex08/ft_print_combn.c: OK!

# 単一のファイルの検査:
$ norm ex00/ft_putchar.c
ex00/ft_putchar.c: OK!
```

## 注意

将来Pythonのバージョン由来のエラーとか、どうしようもないエラー(Pythonのバージョンが古いとか)が起きたら、Dockerfileの中身を自分で編修することDockerfileの中身を自分で編集してほしい。pull reqest歓迎。レポジトリーのオーナーになってくれるともっとありがたい。

## 謝辞(Thanks)

本レポジトリーは次のレポジトリーをフォークしてほんのちょっと改変したものである。

- [GitHub: alexandregv/norminette-docker)](https://github.com/alexandregv/norminette-docker)
