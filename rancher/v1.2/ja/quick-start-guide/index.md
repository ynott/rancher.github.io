---

title: Quick Start Guide
layout: rancher-default-v1.2
version: v1.2
lang: ja
redirect_from:
  - /rancher/quick-start-guide/
  - /rancher/latest/en/quick-start-guide/
---

## クイック スタート ガイド ##

このガイドでは、1台のLinuxサーバーに全てインストールして動く、最も簡単なRancherをインストールしてみます。

### Linuxホストを準備する

3.10 + のカーネルが最低限入っている64 ビット Ubuntu 16.04 と Linux ホストを準備します。 ノートパソコンでも、仮想環境でも、物理サーバーも利用可能です。 Linux ホストには少なくとも **1GB** のメモリがあることを確認してください。 [Docker](https://www.docker.com/) をホストにインストールします。

サーバーにDockerをインストールするには、 [Docker](https://docs.docker.com/engine/installation/linux/ubuntulinux/) 社の手引きを参照してください。

> **注:** 現在、Windows と Mac のDockerはサポートされていません。

### Rancher サーバータグ

`rancher/server:latest` タグはRancherの安定版ビルドにつけられます。Rancher社が推奨している本番環境展開用です。 各マイナーリリースタグに対して、それぞれのバージョンでのドキュメントを提供します。

もし、CI自動化フレームワークで検証済の最新開発版のビルドに興味があれば、最新の開発版のリリースタグが付いた[リリースページ](https://github.com/rancher/rancher/releases) を確認してください。 このリリースは本番環境デプロイ用ではありません。 開発ビルドには、全て開発リリースであることを示す`*-pre{n}` という接尾辞が付加されます。 `rc{n}` サフィックスの付いているリリースはいずれも使用しないでください。 `rc` ビルドは、Rancherチームが開発版ビルドをテストするためのものです。

### Rancherサーバーを動かす

Rancherサーバーを起動するコマンドは1つだけです。コンテナを起動したら、コンテナのログを調べて、サーバが起動して動いているかを確認します。

```bash
$ sudo docker run -d --restart=unless-stopped -p 8080:8080 rancher/server
# Rancherのログを表示
$ sudo docker logs -f <CONTAINER_ID>
```

Rancherサーバーの起動に数分おまちください。 `.... Startup Succeeded, Listening on port....` が表示されたら、RancherのUIは起動して稼働中です。 ログのこの行が表示されたら、ほとんど設定は終了です。 この出力の後に追加のログが出力される可能性があるので、これが初期起動時のログの最後の行であるとは限りません。

UIは `8080` ポートで動いているので、表示するには `http://<SERVER_IP>:8080` をブラウザーで開いてください。 Rancherサーバーとブラウザーが同じホストで動いている場合は、`http://192.168.1.100:8080` のように実IPを使ってください。`http://localhost:8080` や `http://127.0.0.1:8080` としないようにしてください。

> **注:** Rancherサーバーにはアクセス制限が設定されておらず、このIPアドレスにアクセスできる誰でもこのUIとAPIを利用できます。 [アクセス制限]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/configuration/access-control/)を設定することをお勧めします。

### ホストを追加

わかりやすくするため、Rancherサーバーを実行しているサーバーをRancherのホストとして追加します。実際の運用では、専用のRancherを実行するホストを持つことをお勧めします。

ホストを追加するには、UIから**インフラストラクチャー**をクリックして、**ホスト**ページを表示してください。 **ホストの追加**をクリックします。 Rancherで利用するURLを選択するように求められます。 このURLは、Rancher サーバー が動いているURLで、これから追加するRancherホストから接続可能なものでなくてはなりません。 この設定は、RancherサーバーがファイヤウォールでNATされたり、ロードバランサーを介してインターネットに公開される場合に便利です。 ホストに`192.168.*.*`のようなプライベートやローカルIPなアドレスがついていた場合、Rancherサーバーにホストが本当にURLにアクセスできるかどうかを尋ねる警告が表示されます。

今のところ、これらの警告を無視することができます。これはRancherサーバーホスト自体を追加するだけです。 **保存**をクリックします。 デフォルトでは、Rackherエージェントコンテナを起動するDockerコマンドを提供する**カスタム**オプションが選択されます。 RancherがDocker Machineを使用してホストを起動するクラウドプロバイダのオプションもあります。

UIでは、ホスト上で開く必要があるポートの指示とオプションの設定項目が表示されます。 Rancherサーバーも稼動しているホストを追加するので、このホストで使用するパブリックIPを追加する必要があります。 このオプションで入力されたIPにより、カスタムコマンドでの環境変数が自動的に変更されます。

Rancherサーバーを実行しているホストでこのコマンドを実行します。

Rancher UIで**閉じる**をクリックすると、**インフラストラクチャ** -> **ホスト**ビューに戻ります。 数分後に、ホストが自動的に表示されます。

### UIを使用してコンテナを作成する

画面 ** Stacks ** に移動します。まだサービスがない場合は、ウェルカム画面の** Define a service **ボタンをクリックします。 Rancherに既にサービスが設定されている場合は、既存のスタックの**サービスの追加**をクリックするか、新しいスタックを作成してサービスを追加できます。 新しいスタックを作成する必要がある場合は、**スタックを追加**をクリックし、名前と説明を入力して**作成**をクリックします。 次に、**サービスの追加**をクリックします。

"first-container"のような名前でサービスを追加します。 デフォルトの設定を使用して**作成**をクリックするだけです。 Rancherは、ホスト上で2つのコンテナの起動を開始します。 1つ目のコンテナは、私たちが要求した***first-container***です。 もう1つのコンテナは***ネットワークエージェント***です。これは、ホスト間ネットワーク、ヘルスチェックなどのタスクを処理するためにRancherによって作成されたシステムコンテナです。 ***ネットワークエージェント***コンテナは**スタック**ページに表示されませんが、コンテナはホスト上で実行されています。 このコンテナーは** Infrastructure ** -> ** Hosts **ページで** Show System **チェックボックスを入れると表示でき、** Infrastructure ** -> **コンテナー**画面でも確認できます。

ホストの持っているIPアドレスに関係なく、コンテナが異なるホスト間で相互に通信できるように、***first-container***と***ネットワークエージェント***の両方に`10.42.*.* `の範囲のIPアドレスで、Rancherが管理するオーバーレイネットワークを作成しました。

***first-container***のドロップダウンをクリックすると、コンテナの停止、ログの表示、コンテナコンソールへのアクセスなどの管理アクションを実行できます。

### DockerのCLIを直接使ってコンテナを作成する

Rancherは、コンテナがUI以外から作成されていても、ホスト上のコンテナを表示します。ホストのシェル端末からコンテナを作成します。

```bash
$ docker run -d -it --name=second-container ubuntu:14.04.2
```

UIでは、 ***セカンドコンテナ*** がホスト上にポップアップ表示されます。

RancherはDockerデーモンで発生するイベントに反応し、実際動作環境とRancherからの状況を調和させるために調整します。 [native docker CLI]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/native-docker/)でRancherを使用する方法について詳しく読むことができます。

***第2コンテナ*** のIPアドレスを見ると、` 10.42.*.* `の範囲にはないことがわかります。 これは、Dockerデーモンによって割り当てられた通常のIPアドレスです。 CLIを使用してDockerコンテナを作成するとこのようになります。

RancherのオバーレイネットワークからIPアドレスを渡して、CLIを使用してDockerコンテナを作成するにはどうすればよいでしょうか？することは、コマンドにラベルを追加するだけです。

```bash
$ docker run -d -it --label io.rancher.container.network=true ubuntu:14.04.2
```

<br /> ` io.rancher.container.network `というラベルを Dockerのコマンドラインでヒントとして渡すことで、Rancher はオーバーレイネットワークに接続するコンテナを作成します。

### マルチコンテナアプリケーションの作成

個別のコンテナを作成する方法と、クロスホストネットワークでそれらがどのように接続されるかを説明しました。 しかし、実際のアプリケーションのほとんどは複数のサービスで構成されており、各サービスは複数のコンテナで構成されています。 たとえば、WordPressアプリケーションは、次のようなサービスで構成されます。

  1. ロードバランサー。ロードバランサーはインターネットからのリクエストをWordPressアプリケーションにリダイレクトします。
  2. WordPressのサービスは、2つのWordPressコンテナで構成されます。
  3. データベースサービスが、1つのMySQLコンテナで構成されます。

ロードバランサーはWordPressサービスに接続し、WordPressサービスはMySQLサービスにリンクします。

このセクションでは、RancherにWordPressアプリケーションを作成して展開する方法を説明します。

**スタック**ページに移動します。まだサービスがない場合は、ウェルカム画面の**サービスの追加**ボタンをクリックします。 すでにサービスがある場合は、既存のスタックの**サービスの追加**をクリックするか、新しいスタックを作成してサービスを追加することができます。 新しいスタックを作成する必要がある場合は、**スタックの追加**をクリックし、名前と説明を入力して**作成**をクリックします。 次に、**サービスの追加**をクリックします。

まず、*データベース*というデータベースサービスを作成し、mysqlイメージを使用します。 **コマンド**タブで、環境変数` MYSQL_ROOT_PASSWORD = pass1 ` を追加します。 **作成**をクリックします。 すべてのサービスが含まれるスタックページがすぐに表示されます。

次に、もう一度 **サービスの追加** をクリックして、別のサービスを追加します。 WordPressサービスとmysqlサービスへのリンクを追加します。 * mywordpress * という名前を使用して、wordpressイメージを使用しましょう。 スライダを動かして、サービスのスケールをコンテナ2つにします。 **サービスリンク**に*データベース*サービスを追加し、* mysql *という名前を指定します。 Dockerの場合と同様に、Rancherは、* mysql *の名前を選択すると、リンクされたデータベースとしてWordPressイメージに必要な環境変数をリンクします。 **作成**をクリックします。

最後にロードバランサーを作成します。 **サービスの追加** ボタンの横にあるドロップダウンメニューアイコンをクリックします。 ** ロードバランサーの追加 **を選択します。 * wordpresslb *のような名前を入力し、wordpress アプリケーションにアクセスするために使用するホスト上のソースポートとターゲットポートを選択します。 この場合、両方で` 80 `番ポートを使用します。 ターゲットとするサービスは* mywordpress *になります。 **作成**をクリックします。

マルチサービスアプリケーションが完成しました！ ** スタック **ページで、ロードバランサが公開しているポートをリンクとして見つけることができます。 そのリンクをクリックすると、新しいブラウザが開き、wordpressアプリケーションが表示されます。

### Rancher Composeを使用して複数コンテナアプリケーションを作成する

このセクションでは、Rancher Composeというコマンドラインツールを使用して前のセクションで作成した同じWordPressアプリケーションを作成して配備する方法を説明します。

Rancher Composeツールは、一般的なDocker Composeツールと同じように機能します。 これは同じ` docker-compose.yml `ファイルを取り込み、Rancherにアプリケーションをデプロイします。 ` docker-compose.yml `ファイルを拡張して上書きする` rancher-compose.yml `ファイルに、追加の属性を指定することができます。

前のセクションでは、ロードバランサを備えたWordpressアプリケーションを作成しました。 Rancherで作成した場合は、スタックのドロップダウンメニューから** Export Config **を選択して、UIから直接ファイルをダウンロードできます。 ` docker-compose.yml `と` rancher-compose.yml `ファイルは次のようになります。

#### Example docker-compose.yml

```yaml
mywordpress:
  tty: true
  image: wordpress
  links:
  - database:mysql
  stdin_open: true
wordpresslb:
  ports:
  - 80:80
  tty: true
  image: rancher/load-balancer-service
  links:
  - mywordpress:mywordpress
  stdin_open: true
database:
  tty: true
  image: mysql
  stdin_open: true
  environment:
    MYSQL_ROOT_PASSWORD: pass1
```

#### Example rancher-compose.yml

```yaml
mywordpress:
  scale: 2
wordpresslb:
  scale: 1
  load_balancer_config:
    haproxy_config: {}
  health_check:
    port: 42
    interval: 2000
    unhealthy_threshold: 3
    healthy_threshold: 2
    response_timeout: 2000
database:
  scale: 1
```

フッターの右側にある` CLIのダウンロード`をクリックして、RancherのUIからRancher Composeバイナリをダウンロードします。 Windows、Mac、Linux用のバイナリを提供しています。

Rancher Composeを使用してRancherでサービスを開始するには、Rancher Composeでいくつかの変数を設定する必要があります。 Rancher UIで[環境APIキー]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/api-keys/)を作成する必要があります。 ** API **をクリックし、** APIキーを追加**をクリックします。 ユーザー名 (アクセスキー) とパスワード (秘密キー) を保存します。 Rancher Composeに必要な環境変数を設定する: `RANCHER_URL`, `RANCHER_ACCESS_KEY`, and `RANCHER_SECRET_KEY`.

```bash
# Set the url that Rancher is on
$ export RANCHER_URL=http://server_ip:8080/
# Set the access key, i.e. username
$ export RANCHER_ACCESS_KEY=<username_of_key>
# Set the secret key, i.e. password
$ export RANCHER_SECRET_KEY=<password_of_key>
```

` docker-compose.yml `と` rancher-compose.yml `を保存したディレクトリに移動し、コマンドを実行します。

```bash
$ rancher-compose -p NewWordpress up
```

Rancherでは、** NewWordPress **という新しいスタックが作成され、すべてのサービスが起動されます。
