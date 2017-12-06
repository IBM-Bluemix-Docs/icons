---

copyright:
  years: 2015, 2017

lastupdated: "2017-04-10"

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}  
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}





# アプリの管理に関するトラブルシューティング
{: #managingapps}


アプリの管理に関する一般的な問題には、アプリを更新できない、2 バイト文字が表示されないなどがあります。多くの場合、いくつかの簡単なステップを実行することで、これらの問題から復旧することが可能です。
{:shortdesc}


## 保存されていない変更があります
{: #ts_unsaved_changes}

アプリの詳細ページでナビゲートしている時に、アクションを実行できなくなり、続行する前に変更を保存するよう求めるプロンプトが出される場合があります。

アプリの詳細ページでアプリまたはサービスを確認しようとすると、次のエラー・メッセージを受信し続けます。
{: tsSymptoms}

`保存されていない変更がページ app_name にあります。変更を保存するか取り消してください。`

マウスをスクロールして、「ランタイム」ペインの**「インスタンス」**フィールドまたは**「メモリー割り当て量」**フィールドの上に移動すると、それらの値が変わります。この動作は設計によるものですが、エラー・メッセージにより、そのペインの外にナビゲートする前にメモリー設定またはインスタンス設定を保存するよう求めるプロンプトが表示されます。
{: tsCauses}

メッセージ・ウィンドウを閉じ、「ランタイム」ペイン内の**「リセット」**ボタンをクリックします。
{: tsResolve}

## Bluemix 領域間の自動フェイルオーバーを使用できない
{: #ts_failover}

{{site.data.keyword.Bluemix_notm}} 領域間の自動フェイルオーバーは使用できません。ただし、回避策として、複数の IP アドレス間のフェイルオーバーをサポートする DNS プロバイダーを使用できます。

ある {{site.data.keyword.Bluemix_notm}} 領域が使用不可になると、その領域で実行されているアプリは、たとえそれと同じアプリが別の {{site.data.keyword.Bluemix_notm}} 領域で実行されていても、使用不可になります。
{: tsSymptoms}

{{site.data.keyword.Bluemix_notm}} では、ある領域から別の領域への自動フェイルオーバーはまだ提供されていません。
{: tsCauses}

複数の ID アドレス間のインテリジェント・フェイルオーバーをサポートする DNS プロバイダーを使用し、{{site.data.keyword.Bluemix_notm}} 領域間の自動フェイルオーバーを使用可能にするように、DNS 設定を手動で構成することができます。この機能を備えた DNS プロバイダーとしては、NSONE、Akamai、Dyn があります。
{: tsResolve}

DNS 設定を構成する場合、アプリが実行されている {{site.data.keyword.Bluemix_notm}} 領域のパブリック IP アドレスを指定する必要があります。{{site.data.keyword.Bluemix_notm}} 領域のパブリック IP アドレスを取得するには、`nslookup` コマンドを使用します。例えば、次のコマンドをコマンド・ライン・ウィンドウに入力できます。
```
nslookup stage1.mybluemix.net
```

## アプリをデバッグ・モードに切り替えられない
{: #ts_debug}

Java 仮想マシン (JVM) バージョンが 8 以下の場合、デバッグ・モードを有効にできないことがあります。

**「アプリケーションのデバッグを有効にする (Enable application debug)」**を選択すると、ツールはアプリをデバッグ・モードに切り替えようとします。これにより、Eclipse ワークベンチはデバッグ・セッションを開始します。ツールがデバッグ・モードを有効にするのに成功した場合、Web アプリケーションの状況には `Updating mode`、`Developing`、および `Debugging` が表示されます。
{: tsSymptoms}

しかし、ツールがデバッグ・モードを有効にするのに失敗した場合は、Web アプリケーションの状況には、`Updating mode` と `Developing` のみが表示され、`Debugging` は表示されません。ツールは、「コンソール」ビューに以下のエラー・メッセージを表示することもあります。

```
bluemixMgmgClient - ???? [pool-1-thread-1] .... ERROR --- ClientProxyImpl: Cannot create the websocket connections for MyWebProj
com.ibm.ws.cloudoe.management.client.exception.ApplicationManagementException: javax.websocket.DeploymentException: The HTTP request to initiate the  WebSocket connection failed
at com.ibm.ws.cloudoe.management.client.impl.ClientProxyImpl.onNewClientSocket(ClientProxyImpl.java:161)
at com.ibm.ws.cloudoe.management.client.impl.ClientProxyImpl$RunServerTask.run(ClientProxyImpl.java:267)
at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:522)
at java.util.concurrent.FutureTask.run(FutureTask.java:277)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1153)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
at java.lang.Thread.run(Thread.java:785)
Caused by: javax.websocket.DeploymentException: The HTTP request to initiate the WebSocket connection failed
at  org.apache.tomcat.websocket.WsWebSocketContainer.connectToServer(WsWebSocketContainer.java:315)
at  com.ibm.ws.cloudoe.management.client.impl.ClientProxyImpl.onNewClientSocket(ClientProxyImpl.java:158)
... 6 more
Caused by: java.util.concurrent.TimeoutException
at org.apache.tomcat.websocket.AsyncChannelWrapperSecure$WrapperFuture.get(AsyncChannelWrapperSecure.java:505)
at org.apache.tomcat.websocket.WsWebSocketContainer.processResponse(WsWebSocketContainer.java:542)
at org.apache.tomcat.websocket.WsWebSocketContainer.connectToServer(WsWebSocketContainer.java:296)
... 7 more
[2016-01-15 13:33:51.075] bluemixMgmgClient - ????  [pool-1-thread-1] .... ERROR --- ClientProxyImpl: Cannot create the  websocket connections for MyWebProj
com.ibm.ws.cloudoe.management.client.exception.ApplicationManagementException: javax.websocket.DeploymentException: The HTTP request to initiate the  WebSocket connection failed
at com.ibm.ws.cloudoe.management.client.impl.ClientProxyImpl.onNewClientSocket(ClientProxyImpl.java:161)
at com.ibm.ws.cloudoe.management.client.impl.ClientProxyImpl$RunServerTask.run(ClientProxyImpl.java:267)
at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:522)
at java.util.concurrent.FutureTask.run(FutureTask.java:277)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1153)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
at java.lang.Thread.run(Thread.java:785)
Caused by: javax.websocket.DeploymentException: The HTTP request to initiate the WebSocket connection failed
at org.apache.tomcat.websocket.WsWebSocketContainer.connectToServer(WsWebSocketContainer.java:315)
at com.ibm.ws.cloudoe.management.client.impl.ClientProxyImpl.onNewClientSocket(ClientProxyImpl.java:158)
... 6 more
Caused by: java.util.concurrent.TimeoutException
at org.apache.tomcat.websocket.AsyncChannelWrapperSecure$WrapperFuture.get(AsyncChannelWrapperSecure.java:505)
at org.apache.tomcat.websocket.WsWebSocketContainer.processResponse(WsWebSocketContainer.java:542)
at org.apache.tomcat.websocket.WsWebSocketContainer.connectToServer(WsWebSocketContainer.java:296)
... 7 more
```

Java 仮想マシン (JVM) のバージョンが IBM JVM 7、IBM JVM 8、または Oracle JVM 8 より前のバージョンである場合、デバッグ・セッションを確立できません。
{: tsCauses}

ワークベンチの JVM が、これらのバージョンのいずれかである場合、デバッグ・セッションの作成時に問題が生じることがあります。ワークベンチの JVM バージョンは、通常はローカル・コンピューターのシステム JVM です。システム JVM は、実行中の {{site.data.keyword.Bluemix_notm}} Java&trade; アプリケーションの JVM と同じではありません。{{site.data.keyword.Bluemix_notm}} Java アプリケーションは、ほとんどの場合、IBM JVM 上で実行され、時には OpenJDK JVM 上で実行されることもあります。

{{site.data.keyword.eclipsetoolsfull}} が稼働している Java のバージョンを確認するには、以下の手順を実行します。
{: tsResolve}

  1. IBM Eclipse Tools for Bluemix で、**「ヘルプ」** > **「Eclipse について」** > **「インストールの詳細」** > **「構成」**を選択します。
  2. リストから `eclipse.vm` プロパティーを検索します。次の行は、`eclipse.vm` プロパティーの例を示しています。

	```
	eclipse.vm=C:\Program Files\IBM\ibm-java-sdk-80-win-x86_64\bin\..\jre\bin\j9vm\jvm.dll
	```

  3. コマンド・ラインで、Java のインストール先の `bin` ディレクトリーから `java -version` を入力します。IBM JVM バージョン情報が表示されます。

ワークベンチの JVM が、IBM JVM 7 または 8、あるいは Oracle JVM 8 より前のバージョンである場合は、以下の手順を実行して、Oracle JVM 8 に切り替えます。

  1. Oracle JVM 8 をダウンロードし、インストールします。詳しくは、[Java SE Downloads ![「外部リンク」アイコン](../icons/launch-glyph.svg "「外部リンク」アイコン")](http://www.oracle.com/technetwork/java/javase/downloads/index.html){: new_window} を参照してください。
  2. Eclipse を再始動します。
  3. `eclipse.vm` プロパティーが、Oracle JVM 8 の新しいインストール先を指しているかどうかを確認します。


## 削除したアプリの名前を再使用できない
{: #ts_reuse_appname}

アプリを削除したら、アプリ経路を削除した場合に限り、アプリ名を再使用できます。

アプリ名を再使用しようとしたときに、以下のメッセージを受け取ります。
{: tsSymptoms}

`名前は別のアプリによって既に使用されています。`

アプリが削除されるとき、アプリの URL である経路は自動的に削除されません。そのため、それは再使用できません。アプリを再使用するため経路を削除するには、アプリが作成されたスペースに移動する必要があります。
{: tsCauses}

使用されていない経路を削除するには、以下の手順を実行します。
{: tsResolve}

  1. 以下のコマンドを入力して、該当経路が現行スペースに属しているかどうかを確認します。
     ```
	 cf routes
	 ```
  2. 該当経路が現行スペースに属していない場合は、以下のコマンドを入力して、それが属しているスペースまたは組織に切り替えます。
     ```
	 cf target -o org_name -s space_name
	 ```
  3. 以下のコマンドを入力して、アプリ経路を削除します。
     ```
	 cf delete-route domain_name -n host_name
	 ```
	 例:
	    ```
	 cf delete-route mybluemix.net -n app001
	 ```

## 組織内でスペースを取得できない
{: #ts_retrieve_space}

現在の組織に、関連付けられているスペースがない場合は、アプリまたはサービスを作成できません。

Bluemix でアプリを作成しようとすると、以下のエラー・メッセージが表示されます。
{: tsSymptoms}

BXNUI0515E: 組織のスペースは取得されませんでした。 ネットワーク接続問題が発生したか、現在の組織に関連付けられているスペースがないかのいずれかです。`

このエラーは、多くの場合、カタログからアプリまたはサービスを初めて作成しようとしたときに、スペースがまだ作成されていない場合に発生します。
{: tsCauses}

現在の組織内にスペースが作成されていることを確認してください。スペースを作成するには、以下のいずれかの方法を使用します。
{: tsResolve}

  * メニュー・バーから、**「管理」>「アカウント」>「組織」**をクリックします。スペースを作成する組織を選択し、**「スペースの作成」**をクリックします。
  * cf コマンド・ライン・インターフェースに `cf create-space <space_name> -o <organization_name>` と入力します。

やり直してください。
このメッセージが再び表示される場合は、[Bluemix 状況 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://ibm.biz/bluemixstatus){: new_window} ページにアクセスし、サービスまたはコンポーネントに問題があるかどうか確認してください。

## 要求されたアクションを実行できない
{: #ts_authority}

適切なアクセス権限がないとアクションを実行できない場合があります。

サービス・インスタンスまたはアプリ・インスタンスに対してアクションを実行しようとした時、要求したアクションを実行できず、以下のいずれかのエラー・メッセージが表示されます。
{: tsSymptoms}

`BXNUI0514E: <orgName> 組織内のいずれのスペースの開発者でもありません。`

`サーバー・エラー、状況コード: 403、エラー・コード: 10003、メッセージ: 要求されたアクションの実行を許可されていません。`

アクションを実行するための適切なレベルの権限を備えていません。
{: tsCauses}

適切な権限レベルを取得するには、以下のいずれかの方法を使用します。
{: tsResolve}
 * 開発者役割を持っている、別の組織およびスペースを選択します。
 * 自分の役割を開発者に変更するように、またはスペースを作成して自分に開発者役割を割り当てるように組織マネージャーに依頼します。 詳細については、[組織およびスペースの管理](/docs/admin/orgs_spaces.html) を参照してください。

## 許可エラーのため、Bluemix サービスにアクセスできない
{: #ts_vcap}

サービス資格情報がアプリ内でハードコーディングされている場合、アプリが {{site.data.keyword.Bluemix_notm}} にアクセスした時に許可エラーが発生することがあります。

{{site.data.keyword.Bluemix_notm}} サービスと通信するようにアプリを構成した後に、そのアプリを {{site.data.keyword.Bluemix_notm}} にデプロイします。しかし、アプリを使用して {{site.data.keyword.Bluemix_notm}} サービスにアクセスできず、許可エラーを受け取ります。
{: tsSymptoms}

アプリ内にハードコーディングされている資格情報が正しくない可能性があります。サービスが再作成されるごとに、そのサービスにアクセスするための資格情報が変更されます。
{: tsCauses}

アプリ内に資格情報をハードコーディングするのではなく、VCAP_SERVICES 環境変数からの接続パラメーターを使用してください。VCAP_SERVICES 環境変数からの接続パラメーターを使用する方法は、プログラミング言語によって異なります。例えば、Node.js アプリの場合、以下のコマンドを使用できます。
{: tsResolve}

```
process.env.VCAP_SERVICES
```
他のプログラム言語で使用できるコマンドについて詳しくは、[Java ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.run.pivotal.io/buildpacks/java/java-tips.html#env-var){: new_window} および [Ruby![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.run.pivotal.io/buildpacks/ruby/ruby-tips.html#env-var){: new_window} を参照してください。


## IBM Eclipse Tools for Bluemix を使用してアプリをデプロイできない
{: #ts_bm_tools_facet}

サポートされないファセットが Eclipse プロジェクトに適用されると、IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} を使用してアプリを {{site.data.keyword.Bluemix_notm}} にデプロイできないことがあります。

Cloud Foundry CLI を使用すると、正常にアプリを {{site.data.keyword.Bluemix_notm}} にデプロイすることができます。ただし、IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} を使用すると {{site.data.keyword.Bluemix_notm}} にアプリをデプロイできず、`プロジェクト・ファセット <facet_name> はサポートされません。` というエラー・メッセージが表示されます。例:
{: tsSymptoms}
`プロジェクト・ファセット Cloud Foundry Standalone Application バージョン 1.0 はサポートされません。`

IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} は、プロジェクト・ファセットによって {{site.data.keyword.Bluemix_notm}} ランタイムにプロジェクトをマップします。ファセットは、Eclipse 内の Java EE プロジェクトの要件を定義し、異なるランタイムが異なるプロジェクトに関連付けられるようにランタイム構成の一部として使用されます。 プロジェクトに適用されるファセットが IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} によってサポートされていない場合、IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} を使用してアプリをデプロイできない可能性があります。
{: tsCauses}

IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} を使用してアプリをデプロイできるように、Eclipse プロジェクトからファセットを削除する必要があります。
{: tsResolve}

ファセットを削除するには、IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} で、プロジェクトに対して**「プロジェクト」>「プロパティー」>「プロジェクト・ファセット」**をクリックします。次に、サポートされないファセットのチェック・ボックスをクリアします。## 502 Bad Gateway エラーを受け取る
{: #ts_502_error}

{{site.data.keyword.Bluemix_notm}} でアプリと対話している時に 502 Bad Gateway エラーを受け取る場合は、{{site.data.keyword.Bluemix_notm}} の状況ページを確認し、対応するアクションを実行してください。

502 Bad Gateway で始まるエラー・メッセージを受信します。例えば、「502 Bad Gateway:
登録済みエンドポイントが要求の処理に失敗しました。(Registered endpoint failed to handle the request.)」と表示されることがあります。{: tsSymptoms}

Bad Gateway エラーは通常、サイトをホストするメイン・サーバーからのデータを保管および中継するためにプロキシー・サーバーを使用している Web サイトにアクセスした時に発生します。メイン・サーバーとプロキシー・サーバーが正しく接続されていない可能性があるため、ブラウザー・ウィンドウに HTTP 状況コード 502 が表示されます。この状況コードは、サイトのメイン・サーバーが、予期した HTTP 実装をプロキシー・サーバーから受信しなかったことを示します。
{: tsCauses}

あまり一般的ではありませんが、Bad Gateway エラーの原因として他に、インターネット・サービス・プロバイダー (ISP) のドロップアウト、ファイアウォール構成の誤り、およびブラウザー・キャッシュのエラーがあります。

{{site.data.keyword.Bluemix_notm}} サービスがダウンしていることが疑われる場合は、まず {{site.data.keyword.Bluemix_notm}} 状況 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://ibm.biz/bluemixstatus){: new_window} ページを確認してください。回避策として、別の {{site.data.keyword.Bluemix_notm}} 地域でそのサービスを使用することができます。[別の地域でのサービスの使用 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/reqnsi.html#cross_region_service){: new_window} に詳細情報があります。サービス状況が正常の場合は、問題を解決するために以下のステップを試行してください。
{: tsResolve}

  * 以下のようにしてアクションを再試行します。
    * キーボードで F5 を押すか、最新表示ボタンをクリックして、ページを再ロードします。このステップでうまくいかない場合は、ブラウザーのキャッシュと Cookie を消去してから、もう一度再ロードしてください。* 異なるブラウザーを使用します。
    * ルーター、モデム、およびコンピューターをリブートします。 これらのデバイスをリブートすると、エラー 502 につながる各種エラーが解消する可能性があります。* 時間をおいて、後で再試行します。 場合によっては、インターネット・サービス・プロバイダーまたは {{site.data.keyword.Bluemix_notm}} サービスで一時的な問題が発生していることがあります。一時的な問題が解決されるまで待ちます。* 問題が解決しない場合は、{{site.data.keyword.Bluemix_notm}} サポートに連絡してください。 詳しくは、[{{site.data.keyword.Bluemix_notm}} サポートへのお問い合わせ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/support/index.html#contacting-bluemix-support){: new_window} を参照してください。

## ディスク割り当て量を超えた
{: #ts_disk_quota}

ディスク・スペースを使い尽くした場合は、手動でディスク割り当て量を変更してディスク・スペースを追加することができます。

ディスク・スペースを使い尽くしたときに、ディスク割り当て量を超えたというメッセージが表示される場合があります。この問題を解決するために、アプリ・インスタンスをスケールアップしてディスク・スペースを追加しようとした可能性があります。例えば、アプリ詳細ページでメモリー割り当て量を変更して 256 MB から 1256 MB に拡大した、などです。しかし、ディスク割り当て量は同じままであったために、ディスク・スペースは追加されませんでした。
{: tsSymptoms}

1 つのアプリに割り当てられるデフォルトのディスク割り当て量は 1 GB です。追加のディスク・スペースが必要な場合は、ディスク割り当て量を手動で指定する必要があります。
{: tsCauses}

以下のいずれかの方法を使用してディスク割り当て量を指定します。指定できる最大ディスク割り当て量は 2 GB です。2 GB でも不十分な場合は、[オブジェクト・ストア](/docs/services/ObjectStorage/index.html) などの外部サービスを試してください。
{: tsResolve}

  * manifest.yml ファイルに以下の項目を追加します。
    ```
	disk_quota: <disk_quota>
	```
  * アプリを {{site.data.keyword.Bluemix_notm}} にプッシュする時に、以下のように `cf push` コマンドに **-k** オプションを使用します。
    ```
	cf push appname -p app_path -k <disk_quota>
	```


## Android アプリが {{site.data.keyword.mobilepushshort}} を受信できない
{: #ts_push}

Google にアクセスできない特定地域の Android アプリは、IBM {{site.data.keyword.mobilepushshort}} サービスから送信された通知を受信できません。この場合、回避策はサード・パーティーのサービスを使用することです。Bluemix アプリに {{site.data.keyword.mobilepushshort}} サービスをバインドして、登録デバイスにメッセージを送信します。ただし、Android プラットフォームで開発されたアプリは、特定の地域で通知を受信できません。
{: tsSymptoms}

IBM {{site.data.keyword.mobilepushshort}} サービスは、Google Cloud Messaging (GCM) サービスを使用して、Android プラットフォームで開発されたモバイル・アプリに通知を送ります。Android アプリが通知を受信できるようにするには、Google Cloud Messaging (GCM) サービスがモバイル・アプリからアクセス可能でなければなりません。Android アプリが GCM サービスに到達できない地域では、Android アプリは {{site.data.keyword.mobilepushshort}} を受信できません。
{: tsCauses}

回避策として、GCM サービスに依存しないサード・パーティー・サービス (例えば、[Pushy ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://pushy.me){: new_window}, [igetui ![外部リンク・アイコン ](../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.getui.com/){: new_window}、および [jpush ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.jpush.cn/){: new_window}) を使用します。
{: tsResolve}


## 組織のサービス限度を超えた
{: #ts_servicelimit}

トライアル・アカウント・ユーザーの場合、組織のサービス限度を超過すると、{{site.data.keyword.Bluemix_notm}} でアプリを作成できなくなることがあります。

{{site.data.keyword.Bluemix_notm}} でアプリを作成しようとすると、以下のエラー・メッセージが表示されます。
{: tsSymptoms}

`BXNUI2032E: <service_instances> リソースは作成されませんでした。リソースを作成するために Cloud Foundry に接続している時にエラーが発生しました。Cloud Foundry メッセージ:「組織のサービス上限を超過しました。」`

このエラーは、そのアカウントで持つことのできるサービス・インスタンス数の上限を超過した時に発生します。トライアル・アカウントでは、サービス・インスタンスの最大数は 10 です。
{: tsCauses}

不要なサービス・インスタンスをすべて削除するか、持つことができるサービス・インスタンス数の上限を撤廃します。
{: tsResolve}

  * サービス・インスタンスを削除するには、{{site.data.keyword.Bluemix_notm}} コンソールか、またはコマンド・ライン・インターフェースが使用できます。

    {{site.data.keyword.Bluemix_notm}} コンソールを使用してサービス・インスタンスを削除するには、以下のステップを実行します。
	  1. 「サービス」ダッシュボードで、削除するサービスの**「アクション」**メニューをクリックします。
	  2. **「サービスの削除」**をクリックします。その後、そのサービス・インスタンスがバインドされていたアプリを再ステージするようにプロンプトが出されます。コマンド・ライン・インターフェースを使用してサービス・インスタンスを削除するには、以下のステップを実行します。
	  1. `cf unbind-service <appname> <service_instance_name>` と入力して、サービス・インスタンスをアプリからアンバインドします。
	  2. `cf delete-service <service_instance_name>` と入力して、サービス・インスタンスを削除します。
	  3. サービス・インスタンスを削除した後、`cf restage <appname>` と入力して、サービス・インスタンスがバインドされていたアプリを再ステージすることができます。

  * 持つことのできるサービス・インスタンスの数の上限を撤廃するには、トライアル・アカウントを有料アカウントに切り替えます。 トライアル・アカウントから有料アカウントへの切り替えについて詳しくは、[プランの変更方法](/docs/pricing/index.html#changing) を参照してください。

## 実行可能ファイルを Bluemix で実行できない
{: #ts_executable}

実行可能ファイルが別の環境で開発およびビルドされた場合、{{site.data.keyword.Bluemix_notm}} でそれらを実行できないことがあります。

実行可能ファイルは、別の環境で開発とビルドを行った時は、{{site.data.keyword.Bluemix_notm}} で実行することができません。
{: tsSymptoms}

{{site.data.keyword.Bluemix_notm}} にプッシュしたいコンテンツが既に実行可能ファイルの場合、そのコンテンツは以前にビルドされているため、{{site.data.keyword.Bluemix_notm}} でビルドする必要はありません。その場合、{{site.data.keyword.Bluemix_notm}} でその実行可能ファイルを実行するためにビルドパックは必要ありません。しかし、{{site.data.keyword.Bluemix_notm}} には、ビルドパックが不要であることを明示的に示さなければなりません。
{: tsCauses}

実行可能ファイルを {{site.data.keyword.Bluemix_notm}} にプッシュする際、null-buildpack を指定する必要があります。これは、ビルドパックが必要ないことを示すものです。以下のように、`cf push` コマンドに **-b** オプションを使用して null-buildpack を指定します。
{: tsResolve}

```
cf push appname -p app_path -c <start_command> -b <null-buildpack>
```
以下に例を示します。
```
cf push appname -p app_path -c ./RunMeNow -b https://github.com/ryandotsmith/null-buildpack
```

## 組織のメモリー限度を超えた
{: #ts_outofmemory}

トライアル・アカウント・ユーザーの場合、組織のメモリー限度を超過すると、{{site.data.keyword.Bluemix_notm}} にアプリをデプロイできなくなることがあります。ユーザーにできるのは、自分のアプリが使用するメモリーを削減すること、あるいは自分のアカウントのメモリー割り当て量を増やすことです。トライアル・アカウントの最大メモリー割り当て量は 2 GB で、支払アカウントに移行することでのみ増やすことができます。{{site.data.keyword.Bluemix_notm}} にアプリをデプロイしようとすると、以下のエラー・メッセージが表示されます。
{: tsSymptoms}

`失敗 サーバー・エラー、状況コード: 400、エラー・コード: 100005、メッセージ: 組織のメモリー上限を超過しました。`

このエラーは、組織の残りメモリー量が、デプロイしたいアプリに必要なメモリー量を下回っている時に発生します。トライアル・アカウントの最大メモリー割り当て量は 2 GB です。
{: tsCauses}

アカウントのメモリー割り当て量を増加するか、アプリが使用するメモリーを削減できます。
{: tsResolve}

  * アカウントのメモリー割り当て量を増やすには、トライアル・アカウントを支払アカウントに変更してください。 トライアル・アカウントから有料アカウントへの切り替えについて詳しくは、[有料アカウント](/docs/pricing/index.html#pay-accounts) を参照してください。
  * アプリが使用するメモリーを削減するには、{{site.data.keyword.Bluemix_notm}} コンソールまたは cf コマンド・ライン・インターフェースのいずれかを使用します。

    {{site.data.keyword.Bluemix_notm}} コンソールを使用する場合は、以下のステップを実行します。

    1. 「アプリ」ダッシュボードでアプリを選択します。アプリ詳細ページが開きます。2. 「ランタイム」ペインで、そのアプリの最大メモリー上限またはアプリ・インスタンス数のいずれか、あるいはその両方を減らすことができます。

    cf コマンド・ライン・インターフェースを使用する場合は、以下のステップを実行します。

    1. 以下を実行して、アプリに使用されているメモリー量を確認します。

	  ```
	  cf apps
	  ```

	  cf apps コマンドで、自分が現行スペースにデプロイしたアプリがすべてリストされます。各アプリの状況も表示されます。
    2. アプリが使用するメモリー量を削減するには、アプリ・インスタンス数または最大メモリー上限のいずれか、あるいはその両方を減らします。

	  ```
	  cf push appname -p app_path -i instance_number -m memory_limit
      ```

    3. アプリを再始動して、変更を有効にします。


## アプリが自動的に再始動しない
{: #ts_apps_not_auto_restarted}

アプリは、アプリにバインドしているサービスが機能を停止した時、自動的に再始動されません。	  

アプリにバインドしているサービスが異常終了すると、そのアプリで停止、例外、接続障害といった問題が発生した可能性があります。{{site.data.keyword.Bluemix_notm}} では、それらの問題から復旧するためにアプリを自動的に再始動することはありません。
{: tsSymptoms}

この動作は Cloud Foundry の設計によるものです。
{: tsCauses}

コマンド・ライン・インターフェースで以下のコマンドを入力すれば、アプリを手動で再始動できます。
{: tsResolve}

```
cf push appname -p app_path
```
さらに、停止、例外、接続障害といった問題を見つけて、そのような問題から復旧するようにアプリをコーディングすることもできます。## アプリがプッシュされたときにユーザー定義変数が失われる
{: #ts_varsnotretained}

IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} からアプリを {{site.data.keyword.Bluemix_notm}} にプッシュすると、指定した変数は、マニフェスト・ファイルに保存しない限りリセットされてしまいます。

IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} からアプリを {{site.data.keyword.Bluemix_notm}} にプッシュすると、指定した変数は失われます。
{: tsSymptoms}

ユーザーが指定した変数は、それらをマニフェスト・ファイルに保存した場合のみ保存されます。
{: tsCauses}

IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} からアプリを {{site.data.keyword.Bluemix_notm}} にプッシュするときは、「アプリケーション」ウィザードの「アプリケーションの詳細 (Application details)」ページで**「マニフェスト・ファイルに保存 (Save to the manifest file)」**チェック・ボックスを選択してください。これにより、ウィザードで指定した変数が、アプリケーション用のマニフェスト・ファイルに保存されます。次にそのウィザードを開いたときに、それらの変数は自動的に表示されます。
{: tsResolve}

<!-- begin STAGING ONLY -->

## Bluemix Live Sync Debug がコマンド・ラインから開始しない
{: #ts_no_debug}

コマンド・ラインを使用して IBM Bluemix Live Sync Debug フィーチャーをアプリに対して使用可能にしたが、デバッグ・インターフェースにアクセスできません。  

**BLUEMIX_APP_MGMT_ENABLE** 環境変数を設定し、アプリにデバッグ・フィーチャーを使用可能にしました。しかし、`app_url/bluemix-debug/manage` でデバッグ・ユーザー・インターフェースにアクセスできません。
{: tsSymptoms}

以下の状況では、デバッグ・フィーチャーを使用可能にすることができません。
{: tsCauses}

  * `manifest.yml` に command 属性が含まれる場合
  * **-c** オプションを使用してアプリを {{site.data.keyword.Bluemix_notm}} にプッシュする場合

以下のいずれかのオプションを使用して問題を解決します。
{: tsResolve}

  * 推奨されるのは、IBM Node.js ビルドパックを使用してアプリを開始する方法です。詳しくは、『[{{site.data.keyword.Bluemix_notm}} への Node.js アプリケーションのデプロイ](/docs/runtimes/nodejs/index.html#nodejs_runtime)』トピックの「開始コマンド (Startup command)」セクションを参照してください。
  * `manifest.yml` の command 属性を command: null に修正するか、push コマンドを編集して `-c null` を組み込むことで、既存アプリにコマンドを使用不可にします。
  * `manifest.yml` から **command** 属性を削除します。その後、{{site.data.keyword.Bluemix_notm}} から現行アプリを削除し、アプリを再びプッシュします。

<!-- end STAGING ONLY -->  


## Bluemix 上で組織が見つからない
{: #ts_orgs}

ある {{site.data.keyword.Bluemix_notm}} 地域で作業しているときに、{{site.data.keyword.Bluemix_notm}} で自分の組織が見つからない場合があります。

{{site.data.keyword.Bluemix_notm}} コンソールに正常にログインできますが、cf コマンド・ライン・インターフェースまたは Eclipse プラグインを使用してアプリをプッシュすることができません。
{: tsSymptoms}

cf コマンド・ライン・インターフェースを使用してアプリケーションを  {{site.data.keyword.Bluemix_notm}} にプッシュしようとすると、メッセージ内に組織名が指定された、次のいずれかのメッセージが表示されます。

`組織の検索中にエラーが発生しました (Error finding org)`

`組織が見つかりません (Organization not found)`

Cloud Foundry Eclipse プラグインを使用してアプリケーションを {{site.data.keyword.Bluemix_notm}} にプッシュしようとすると、以下のエラー・メッセージが表示されます。

`cloudspace が見つかりません。(cloudspace not found.)`

この問題は、処理したい地域の API エンドポイントが指定されていないことが原因で発生します。探している組織が、別の地域にある可能性があります。
{: tsCauses}

cf コマンド・ライン・インターフェースを使用して {{site.data.keyword.Bluemix_notm}} にアプリケーションをプッシュする場合は、cf api コマンドを入力し、地域の API エンドポイントを指定します。例えば、以下のコマンドを入力して、{{site.data.keyword.Bluemix_notm}} の欧州英国地域に接続します。
{: tsResolve}

```
cf api https://api.eu-gb.bluemix.net
```
Eclipse ツールを使用してアプリケーションを {{site.data.keyword.Bluemix_notm}} にプッシュする場合は、まず {{site.data.keyword.Bluemix_notm}} サーバーを作成し、自分の組織が作成された {{site.data.keyword.Bluemix_notm}} 地域の API エンドポイントを指定します。Eclipse ツールの使用について詳しくは、『[IBM Eclipse Tools for Bluemix を使用したアプリのデプロイ (Deploying apps with IBM Eclipse Tools for Bluemix)](/docs/manageapps/eclipsetools/eclipsetools.html)』を参照してください。  

## アプリの経路を作成できない
{: #ts_hostistaken}

アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする際、指定したホスト名がすでに使用されていると、アプリの経路を作成できません。

アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする際、次のエラー・メッセージが表示されます。
{: tsSymptoms}

`経路 hostname.domainname を作成中... 失敗サーバー・エラー、状況コード: 400、エラー・コード: 210003、メッセージ: ホストは使用済みです: hostname (Creating route hostname.domainname ... FAILED Server error, status code: 400, error code: 210003, message: The host is  taken: hostname)`

この問題は、ユーザーが指定したホスト名が既に使用されている場合に発生します。
{: tsCauses}

指定するホスト名は、使用するドメイン内で固有でなければなりません。別のホスト名を指定するには、以下のいずれかの方法を使用して下さい。
{: tsResolve}

  * `manifest.yml` ファイルを使用してアプリケーションをデプロイする場合は、host オプションでホスト名を指定します。
	 
    ```
    host: host_name	
	```
  * コマンド・プロンプトからアプリケーションをデプロイする場合は、**-n** オプションを指定して `cf push` コマンドを使用します。
    ```
    cf push appname -p app_path -n host_name
    ```


## cf push コマンドを使用して WAR アプリをプッシュできない
{: #ts_cf_war}

アプリのロケーションが正しく指定されていないと、cf push コマンドを使用してアーカイブ済み Web アプリを {{site.data.keyword.Bluemix_notm}} にデプロイできない場合があります。

`cf push` コマンドを使用して WAR アプリを {{site.data.keyword.Bluemix_notm}} にアップロードする際に、次のエラー・メッセージが表示されます。
{: tsSymptoms}
`ステージング・エラー: ステージングに失敗したため、インスタンスを取得できません。(Staging error: cannot get instances since staging failed.)`

この問題は、WAR ファイルが指定されていない場合や、WAR ファイルへのパスが指定されていない場合に発生する可能性があります。
{: tsCauses}

**-p** オプションを使用して、WAR ファイルを指定するか、WAR ファイルへのパスを追加してください。以下に例を示します。
{: tsResolve}

```
cf push MyUniqueAppName01 -p app.war
```

```
cf push MyUniqueAppName02 -p "./app.war"
```
`cf push` コマンドの詳細な情報を確認するには、`cf push -h` を入力してください。





## Liberty アプリケーションが Bluemix にプッシュされる際、2 バイト文字が適切に表示されない
{: #ts_doublebytes}

サーブレットまたは JSP ファイルに対して Unicode サポートが適切に構成されていない場合、2 バイト文字が適切に表示されない可能性があります。

Liberty アプリケーションが {{site.data.keyword.Bluemix_notm}} にプッシュされたときは、アプリ内に指定されている 2 バイト文字は正しく表示されません。
{: tsSymptoms}

この問題は、サーブレットまたは JSP ファイルに対して Unicode サポートが適切に構成されていない場合に発生する可能性があります。
{: tsCauses}

サーブレットまたは JSP ファイル内で、以下のコードを使用できます。
{: tsResolve}

  * サーブレット・ソース・ファイル内
    ```
	response.setContentType("text/html; charset=UTF-8");
	```
  * JSP 内
    ```
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	```


## Node.js アプリをデプロイできない
{: #ts_nodejs_deploy}

Node.js アプリを更新する時、または Node.js アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする時に問題が発生することがあります。

Node.js アプリを更新する時、または Node.js アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする時に、以下のいずれかのエラー・メッセージが表示されることがあります。
{: tsSymptoms}

`使用可能なビルドパックによってアプリが正常に検出されませんでした。`

`インスタンス (インデックス 0) は接続の受け入れを開始できませんでした。`

`ステージングに失敗したため、インスタンスを取得できません。`

考えられる原因は以下のとおりです。
{: tsCauses}

  * 開始コマンドが指定されていません。
  *Node.js アプリのデプロイに必要なファイルが、アプリから欠落しているか、ルート・ディレクトリー以外のフォルダーにあります。

問題の原因に応じて、以下のいずれかの方法を使用します。
{: tsResolve}

  * 以下のいずれかの方法で開始コマンドを指定します。
     * cfコマンド・ライン・インターフェースを使用します。以下に例を示します。
```
		cf push MyUniqueNodejs01 -p app_path -c "node app.js"
		```
    * [package.json ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.npmjs.com/json){:new_window} ファイルを使用します。例:
	    ```
		{
      ...
  	   "scripts": {
	 		 "start": "node app.js"
 	   }
	}
	    ```
    * `manifest.yml` ファイルを使用します。例:
	    ```
		applications:
  name: MyUniqueNodejs01
  ...
  command: node app.js
  ...
        ```

  * Node.js ビルドパックがアプリを認識できるように、ご使用の Node.js アプリ内に必ず `package.json` ファイルが存在するようにしてください。このファイルは必ずアプリのルート・ディレクトリーに置いてください。以下は単純な `package.json` ファイルの例です。  
	```
	{
        "name": "MyUniqueNodejs01",
        "version": "0.0.1",
        "description": "A sample package.json file",
        "dependencies": {
                "express": "3.4.x",
                "jade": "1.1.x"
        },
        "engines": {
                "node": "0.10.x"
　　　　},
        "scripts": {
                  "start": "node app.js"
        }
 }
    ```

Node.js アプリについてさらにヒントを見るには、[Node.js アプリケーションに関するヒント (Tips for Node.js Applications) ](http://docs.cloudfoundry.org/buildpacks/node/node-tips.html ![External link icon](../icons/launch-glyph.svg "「外部リンク」アイコン"){: new_window}を参照してください。


## Eclipse に Bluemix Liberty アプリをインポートした後、`server.xml` ファイル内に構成エラーが現れる
{: #ts_eclipse}

{{site.data.keyword.Bluemix_notm}} Liberty アプリを Eclipse にインポートした後、`server.xml` ファイル内に構成エラーを認めた場合は、プロジェクトから `server.xml` ファイルを削除しなければならないことがあります。

{{site.data.keyword.Bluemix_notm}} Liberty アプリを Eclipse にインポートした後、Eclipse の「問題」ビューから `server.xml` ファイル内の構成エラーを確認できます。
{: tsSymptoms}

Liberty アプリが {{site.data.keyword.Bluemix_notm}} にプッシュされると、Liberty ビルドパックは `server.xml` ファイルを使用してアプリを構成し、`runtime-vars.xml` ファイルを生成します。アプリを Eclipse にインポートする際、`runtime-vars.xml` ファイルはご使用のローカル環境に存在しません。
{: tsCauses}

server.xml ファイルをプロジェクトから削除することで、この問題を解決できます。アプリを WAR アプリとしてプッシュすると、ビルドパックは `server.xml` ファイルを動的に作成します。詳しくは、[『Liberty for
Java』](/docs/runtimes/liberty/index.html)を参照してください。
{: tsResolve}


## カスタム・ビルドパックを使用してアプリをステージングできない
{: #ts_bp_compilation}

カスタム・ビルドパック内のスクリプトが実行可能でない場合は、そのビルドパックを使用して {{site.data.keyword.Bluemix_notm}} にアプリをデプロイできないことがあります。

カスタム・ビルドパックを使用して {{site.data.keyword.Bluemix_notm}} にアプリをデプロイすると、`「アプリケーションがステージングに失敗したため、表示するインスタンスはありません」`というエラー・メッセージが表示されます。
{: tsSymptoms}

この問題は、検出スクリプト、コンパイル・スクリプト、リリース・スクリプトなどのスクリプトが実行可能でない場合は発生する可能性があります。
{: tsCauses}

[git update![「外部リンク」アイコン](../icons/launch-glyph.svg "「外部リンク」アイコン")](http://git-scm.com/docs/git-update-index){: new_window} コマンドを使用して、各スクリプトのアクセス権を実行可能に変更できます。例えば、`git update --chmod=+x script.sh` と入力できます。
{: tsResolve}

## IBM Bluemix Continuous Delivery の Delivery Pipeline からアプリをデプロイできない
 {: #ts_devops_to_bm}

 アプリに `manifest.yml` ファイルが存在しない場合、{{site.data.keyword.contdelivery_short}} の {{site.data.keyword.deliverypipeline}} を使用してアプリをデプロイできないことがあります。

 {{site.data.keyword.contdelivery_short}} の {{site.data.keyword.deliverypipeline}} を使用してアプリをデプロイすると、エラー・メッセージ`「サポートされるアプリケーション・タイプを検出できません (Unable to detect a supported application type)」`が表示されることがあります。
{: tsSymptoms}

 この問題は、{{site.data.keyword.Bluemix_notm}} にアプリをデプロイするために、パイプラインが `manifest.yml` ファイルを必要とすることが原因で発生する可能性があります。
 {: tsCauses}

 この問題を解決するには、`manifest.yml` ファイルを作成する必要があります。`manifest.yml` ファイルの作成方法について詳しくは、『[アプリケーション・マニフェスト](/docs/manageapps/depapps.html#appmanifest)』を参照してください。
{: tsResolve}

## Meteor アプリをプッシュできない
{: #ts_meteor}

ビルドパックが正しく指定されていない場合、Meteor アプリケーションを {{site.data.keyword.Bluemix_notm}} にプッシュできない可能性があります。

{{site.data.keyword.Bluemix_notm}} に Meteor アプリをデプロイすると、`「アプリケーションがステージングに失敗したため、表示するインスタンスはありません」`というエラー・メッセージが表示されることがあります。
{: tsSymptoms}

この問題は、Meteor アプリ用に組み込みビルドパックが提供されていないことが原因で発生します。カスタム・ビルドパックを使用する必要があります。
{: tsCauses}

Meteor アプリにカスタム・ビルドパックを使用するには、以下のいずれかの方法を使用します。
{: tsResolve}

  * `manifest.yml` ファイルを使用してアプリをデプロイする場合は、buildpack オプションを使用して、カスタム・ビルドパックの URL または名前を指定します。例えば次のようにします。
  ```
buildpack: https://github.com/Sing-Li/bluemix-bp-meteor 
  ```
  * コマンド・プロンプトからアプリケーションをデプロイする場合は、`cf push` コマンドを使用し、**-b** オプションによってカスタム・ビルドパックを指定します。例えば次のようにします。
    ```
	cf push appname -p app_path -b https://github.com/Sing-Li/bluemix-bp-meteor
	```
