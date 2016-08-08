---
タイトル：インテグレータへの展開を自動化
---

＃インテグレータへの展開を自動化します

{:toc}

「（/ガイド/ /-展開を配信）」ガイドでは、簡単に生産にGitHubのからあなたのコードを取得するために使用するサーバーを構築する方法について説明します。しかし、あなたは、コードをデプロイするための個別のサービスをホストしたくない場合はどうなりますか？あなただけのコードをマージし、それが別のアプリケーションを維持することを考えずにデプロイしたい場合はどう？ [Delivering deployments] [Deployments API] [deploy API]

あなたのリポジトリに加えられた変更を受け取り、インテグレータへの展開を提供するためにそれを設定するには、GitHubの自動デプロイメントサービスを利用することができます。自動デプロイメントサービスは、二つの事象に基づいてペイロードを提供することができます：プッシュが行われ、いつでも（/ガイド/ビル・-CI-サーバー/）されたとき。 [the CI status is passing]

ここでは、プロセスがどのように見えるかを実証する図は次のとおりです。

```
+--------------------+        +--------+                    +-----------+
| GitHubの自動展開| | GitHubの| | Herokuの|
|サービス| + -------- + + ----------- +
+--------------------+         |                                  |
|                         |                                  |
|配置を作成| |
|------------------------>|                                  |
|                         |                                  |
|                         |                                  |
| |展開のイベント|
|                         |--------------------------------->|
|                         |                                  |
| |展開ステータス（保留中）|
|                         |<---------------------------------|
|                         |                                  |
|                         |                                  |
| |展開ステータス（成功）|
|                         |<---------------------------------|
|                         |                                  |
```

{{#tip}}

自動デプロイメントサービスのみ通常 `master`であるデフォルトのブランチからの変更を拾うことに注意してください。

{{/tip}}

##リポジトリにプッシュするたびに展開を送信

自動導入サービスは、プッシュがデフォルトのブランチに行われたときに展開を作成するための責任を負うことになります。次に、我々はそれらの展開イベントを受信し、プロジェクトの展開を処理するためにサービスを設定します。

1.あなたのウェブフックを設定しているリポジトリに移動します。
リポジトリの右サイドバー4. ** ** をクリックします。 <span aria-label="The edit icon" title="The edit icon" class="octicon octicon-tools"> </span>
左3.、**ウェブフック＆サービス**をクリックします。
：https://github-images.s3.amazonaws.com/help/settings/webhooks_and_services_menu.png [The webhooks and services menu]
4. [** **、次に入力サービスを追加し、「GitHubの自動デプロイメントを。」 ！ （/assets/images/add_github_autodeploy_service.png） [Adding the GitHub Auto-Deployment service]
** GitHubのトークン5. [**、あなたが作成したアクセストークンを貼り付けます。それは、少なくとも `repo`スコープを持っている必要があります。詳細については、「（https://help.github.com/articles/creating-an-access-token-for-command-line-use）。」 [Creating an access token for command-line use]
**環境下で6.は、**、オプションで、あなたの展開を送信したい環境のリストを提供します。これは、ご使用の環境を記述するために（https://developer.github.com/v3/repos/deployments/#parameters）することができます。デフォルトでは、「生産」です。 [any string you define]
7. *のみ*したい場合は、正常には、**状態に連続テストスイート、選択**展開に合格したことを構築します。
8.あなたはGitHubのエンタープライズでこのサービスを実行している場合は、アプライアンスの（https://developer.github.com/v3/enterprise/#endpoint-urls）に渡す必要があります。 [endpoint URL]
9. [** **サービスを追加します。

##展開に積分器をフック

私たちの展開を実現するために、我々は、例えば、サービスとしてHerokuのを使用します。

1.あなたのウェブフックを設定しているリポジトリに移動します。
リポジトリの右サイドバー4. ** ** をクリックします。 <span aria-label="The edit icon" title="The edit icon" class="octicon octicon-tools"> </span>
左3.、**ウェブフック＆サービス**をクリックします。
：https://github-images.s3.amazonaws.com/help/settings/webhooks_and_services_menu.png [The webhooks and services menu]
4. **、次に入力し、サービスを追加」Herokuのを。」 ！ （/assets/images/add_heroku_autodeploy_service.png） [Adding the GitHub Auto-Deployment service]
5. GitHubのリポジトリがに展開する必要がありますHerokuのアプリケーションの名前を入力します。
6.であなた（https://devcenter.heroku.com/articles/oauth#direct-authorization）を入力します。あなた自身がHerokuののマニュアルの指示に従って、これを生成する必要があります。 [Heroku OAuth token]
7. [** GitHubのトークン**、あなたが以前のものと同じトークンを貼り付けます。
8. ** **サービスを追加します。

今から、あなたの `master`ブランチに行われたコミット - プル要求をマージから生成されたものを含む - は、自動的にあなたのHerokuのアプリケーションへの展開をトリガします。

[deploy API] ：/ V3 /レポ/展開/
_and_services_menu.png [The webhooks and services menu]
4. **、次に入力し、サービスを追加」Herokuのを。」 ！ （/assets/images/add_heroku_autodeploy_service.png） [Adding the GitHub Auto-Deployment service]
5. GitHubのリポジトリがに展開する必要がありますHerokuのアプリケーションの名前を入力します。
6.であなた（https://devcenter.heroku.com/articles/oauth#direct-authorization）を入力します。あなた自身がHerokuののマニュアルの指示に従って、これを生成する必要があります。 [Heroku OAuth token]
7. [** GitHubのトークン**、あなたが以前のものと同じトークンを貼り付けます。
8. ** **サービスを追加します。

今から、あなたの `master`ブランチに行われたコミット - プル要求をマージから生成された