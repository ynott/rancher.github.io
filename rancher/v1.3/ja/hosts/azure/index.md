* * *

title: Azure へのホスト追加 layout: rancher-default-v1.3 version: v1.3 lang: ja

* * *

## Azure へのホスト追加

Rancher は、`docker machine` を利用して [Microsoft Azure](https://azure.microsoft.com) へのホストのプロビジョニングをサポートします。

### Azure でホストを起動する

  1. スライダーで、立ち上げたいホスト数を選択します。
  2. **名前**を入力します。また、必要に応じてホストの**説明**を入力します。
  3. Azure アカウントから **アカウントのアクセス** の情報を入力します。 これには、**ユーザー名**、**パスワード**、**サブスクリプション ID**、**サブスクリプションの証明書**が含まれます。
    
    > **注:**サブスクリプション証明書を貼り付ける場合は、base64 形式の文字列でなければなりません。

  4. 起動したい**イメージ**を選択します。`docker machine` が Azure に対してサポートしているものであれば、Rancher でもサポートされます。

  5. イメージの **サイズ** を選択します。
  6. デフォルトから変更したい場合は、**SSHポート**、**Dockerポート**、**Docker Swarm マスターポート** を指定します。
  7. **Publish Settings ファイル** を入力します。
  8. Azure のリソースを配置したい **リージョン** を選択します。
  9. (任意) ホストの整理のため **[ラベル]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/#labels)** を設定したり、[サービスやロードバランサーをスケジュールしたり]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/scheduling/)、[ホストIP以外のIPを使う際に外部DNSレコードを設定することができます。]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)
 10. (Optional) In **Advanced Options**, customize your `docker-machine create` command with [Docker engine options](https://docs.docker.com/machine/reference/create/#specifying-configuration-options-for-the-created-docker-engine).
 11. When complete, click **Create**.

Once you click on create, Rancher will create the Azure virtual machine and launch the *rancher-agent* container in the droplet. In a couple of minutes, the host will be active and available to start [adding services]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/adding-services/).