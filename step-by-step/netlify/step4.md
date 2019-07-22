# Step4: ShifterサイトからWebhookでデプロイする
最後にいよいよShifterで生成した静的ファイルをNetlifyへデプロイしていきます。

## 4-1: Webhookを設定する
Webhookデプロイを設定したいサイトに移動し、[Webhooks]へ移動します。

[Add new webhook]で以下のように入力します。

- Event Type: Webhook:Artifact:Created
- Method: POST
- URL: <Step3で取得したNetlifyのWebhook URL>


設定を確認し、[Submit]をクリックしましょう。

![workshop screenshot](./img/14.png)

[Your Webhooks]に追加されれば設定完了です。

![workshop screenshot](./img/15.png)

なお、Webhookは`Disable`にすることで一時的に停止することも可能です。


## 4-2: GenerateしてWebhookを実行する

[Site Overview]に移動し、　[Generate Artifact]から[Generate]を実行します。
![workshop screenshot](./img/16.png)

Generateが成功すると、Webhook経由でNetlifyのビルドが開始されます。

![workshop screenshot](./img/17.png)

Shifter Dashboardの[Webhooks]からNetlifyのWebhookを実行した結果が確認できます。
こちらはあくまでAPI呼び出し結果のみで、ビルドの結果についてはNetlify側で確認する必要があります。

![workshop screenshot](./img/18.png)


## 4-3: Netlifyで公開されていることを確認する

Netlify側でデプロイが成功していることを確認しましょう。

![workshop screenshot](./img/19.png)

[Published]になっていれば、画面に表示されている[XXXX.netlify.com]のURLからNetlifyにデプロイされたWordPressサイトを確認できます。

![workshop screenshot](./img/20.png)


## Checklist
- [ ] ShifterでWebhookを設定した
- [ ] Generateを実行し、WebhookでNetlifyと連携した
- [ ] Netlifyでビルドが成功した
- [ ] Netlifyでサイトが公開された