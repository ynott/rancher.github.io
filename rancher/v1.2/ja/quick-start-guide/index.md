* * *

title: Quick Start Guide layout: rancher-default-v1.2 version: v1.2 lang: ja redirect_from: - /rancher/quick-start-guide/ - /rancher/latest/en/quick-start-guide/

* * *

## ## クイック スタート ガイド

このガイドでは、1台のLinuxサーバーに全てインストールして動く、最も簡単なRancherをインストールしてみます。

### Linuxホストを準備する

3.10 + のカーネルが最低限入っている64 ビット Ubuntu 16.04 と Linux ホストを準備します。 ノートパソコンでも、仮想環境でも、物理サーバーも利用可能です。 Linux ホストは少なくとも**1 GB** のメモリを持っていることを確認してください。 [Docker](https://www.docker.com/) をホストにインストールします。

サーバーにDockerをインストールするには、 [Docker](https://docs.docker.com/engine/installation/linux/ubuntulinux/) 社の手引きを参照してください。

> **注:**現在、Windows と Mac のDockerはサポートされていません。

### Rancher サーバータグ

`rancher/server:latest` タグはRancherの安定版ビルドにつけられます。本番環境でのデプロイで、Rancher社が推奨しているものです。 それぞれのマイナーリリースのタグでは、それらの特定のバージョンでのドキュメントを提供します。

もし、CIの自動化フレームワークで検証済の最新の開発版のビルドを試してみるのに興味があるのであれば、最新の開発版のリリースタグが付いた[リリースページ](https://github.com/rancher/rancher/releases) を確認してください。 これらのリリースは本番環境でのデプロイ用ではありません。 すべての開発ビルドには、それが開発リリースであることを示す`*-pre{n}` という接尾辞が付加されます。 `rc{n}` サフィックスの付いているリリースはどれも使用しないでください。 `rc` ビルドは、Rancherチームが開発版ビルドをテストするためのものです。

### Rancherサーバーを動かす

Rancherサーバーを起動するコマンドは1つだけです。コンテナを起動したら、コンテナのログを調べて、サーバが起動して動いているかを確認します。

```bash
$ sudo docker run -d --restart=unless-stopped -p 8080:8080 rancher/server
# Rancherのログを表示
$ sudo docker logs -f <CONTAINER_ID>
```

Rancherサーバーの起動に数分おまちください。 `が表示されたら.... Startup Succeeded, Listening on port...`, RancherのUIは起動して稼働中です。 ログのこの行が表示されたら、ほとんど設定は終了です。 この出力の後に追加のログが存在する可能性があるので、初期起動時のログの最後の行であるとは考えないでください。

UIは`8080`ポートで公開されています。したがって、UIを表示するには`http://<SERVER_IP>:8080`を開いてください。 Rancherサーバーとブラウザーが同じホストで動いている場合は、次のように実際のIPを使ってください。`http://192.168.1.100:8080` 。`http://localhost:8080` や `http://127.0.0.1:8080`としないようにしてください。

> **注:** Rancherサーバーにはアクセス制御が設定されておらず、このIPアドレスにアクセスできる誰でもこのUIとAPIを利用できます。 [アクセス制御]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/configuration/access-control/)を設定することをお勧めします。

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

### Create a Container through Native Docker CLI

Rancher will display any containers on the host even if the container is created outside of the UI. Create a container in the host's shell terminal.

```bash
$ docker run -d -it --name=second-container ubuntu:14.04.2
```

In the UI, you will see ***second-container*** pop up on your host!

Rancher reacts to events that happen on the Docker daemon and does the right thing to reconcile its view of the world with reality. You can read more about using Rancher with the [native docker CLI]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/native-docker/).

If you look at the IP address of the ***second-container***, you will notice that it is not in `10.42.*.*` range. It instead has the usual IP address assigned by the Docker daemon. This is the expected behavior of creating a Docker container through the CLI.

What if we want to create a Docker container through CLI and still give it an IP address from Rancher’s overlay network? All we need to do is add a label in the command.

```bash
$ docker run -d -it --label io.rancher.container.network=true ubuntu:14.04.2
```

<br /> The label `io.rancher.container.network` enables us to pass a hint through the Docker command line so Rancher will set up the container to connect to the overlay network.

### Create a Multi-Container Application

We have shown you how to create individual containers and explained how they would be connected in our cross-host network. Most real-world applications, however, are made out of multiple services, with each service made up of multiple containers. A WordPress application, for example, could consist of the following services:

  1. A load balancer. The load balancer redirects Internet traffic to the WordPress application.
  2. A WordPress service consisting of two WordPress containers.
  3. A database service consisting of one MySQL container.

The load balancer targets the WordPress service, and the WordPress service links to the MySQL service.

In this section, we will walk through how to create and deploy the WordPress application in Rancher.

Navigate to the **Stacks** page, if there are still no services, you can click on the **Add Service** button in the welcome screen. If there are already services, you can click on **Add Service** in any existing stack or create a new stack to add services in. If you need to create a new stack, click on **Add Stack**, provide a name and description and click **Create**. Then, click on **Add Service**.

First, we'll create a database service called *database* and use the mysql image. In the **Command** tab, add the environment variable `MYSQL_ROOT_PASSWORD=pass1`. Click **Create**. You will be immediately brought to a stack page, which will contain all the services.

Next, click on **Add Service** again to add another service. We'll add a WordPress service and link to the mysql service. Let's use the name, *mywordpress*, and use the wordpress image. We'll move the slider to have the scale of the service be 2 containers. In the **Service Links**, add the *database* service and provide the name *mysql*. Just like in Docker, Rancher will link the necessary environment variables in the WordPress image from the linked database when you select the name as *mysql*. Click **Create**.

Finally, we'll create our load balancer. Click on the dropdown menu icon next to the **Add Service** button. Select **Add Load Balancer**. Provide a name like *wordpresslb* and select a source port and target port on the host that you'll use to access the wordpress application. In this case, we'll use `80` for both ports. The target service will be *mywordpress* service. Click **Create**.

Our multi-service application is now complete! On the **Stacks** page, you'll be able to find the exposed port of the load balancer as a link. Click on that link and a new browser will open, which will display the wordpress application.

### Create a Multi-Container Application using Rancher Compose

In this section, we will show you how to create and deploy the same WordPress application we created in the previous section using a command-line tool called Rancher Compose.

The Rancher Compose tool works just like the popular Docker Compose tool. It takes in the same `docker-compose.yml` file and deploys the application on Rancher. You can specify additional attributes in a `rancher-compose.yml` file which extends and overwrites the `docker-compose.yml` file.

In the previous section, we created a Wordpress application with a load balancer. If you had created it in Rancher, you can download the files directly from our UI by selecting **Export Config** from the stack's dropdown menu. The `docker-compose.yml` and `rancher-compose.yml` files would look like this:

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

Download the Rancher Compose binary from the Rancher UI by clicking on `Download CLI`, which is located on the right side of the footer. We provide the ability to download binaries for Windows, Mac, and Linux.

In order for services to be launched in Rancher using Rancher Compose, you will need to set some variables in Rancher Compose. You will need to create an [environment API Key]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/api-keys/) in the Rancher UI. Click on **API** and click on **Add API Key**. Save the username (access key) and password (secret key). Set up the environment variables needed for Rancher Compose: `RANCHER_URL`, `RANCHER_ACCESS_KEY`, and `RANCHER_SECRET_KEY`.

```bash
# Set the url that Rancher is on
$ export RANCHER_URL=http://server_ip:8080/
# Set the access key, i.e. username
$ export RANCHER_ACCESS_KEY=<username_of_key>
# Set the secret key, i.e. password
$ export RANCHER_SECRET_KEY=<password_of_key>
```

Now, navigate to the directory where you saved `docker-compose.yml` and `rancher-compose.yml` and run the command.

```bash
$ rancher-compose -p NewWordpress up
```

In Rancher, a new stack will be created called **NewWordPress** with all of the services launched.