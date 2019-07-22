# Step3: Netlifyにサイトを作成する

## 3-1: サイトを作成する
Netlifyにログインします。
https://app.netlify.com/

[New site from Git]をクリックし、サイトを作成します。
![workshop screenshot](./img/10.png)

[GitHub]を指定し、認証を許可します。
![workshop screenshot](./img/11.png)

先程作成したリポジトリを選びます。

![workshop screenshot](./img/12.png)

ビルドの設定は以下のような設定にします。

|項目|値|備考|
|:--|:--|:--|
|Owner|<あなたのユーザー名>'s team|ここはどんな値でもOK|
|Branch to deploy|master||
|Build command|bash -x ./netlify/deploy.sh||
|Public directory|public||

![workshop screenshot](./img/13.png)

最後に[Deploy site]をクリックすればOKです。

### Note:初回のビルドについて
設定が終わると、Netlifyが初回のビルドを開始します。  
しかし次のステップのビルド方法でなければ、WordPressのURLが存在しない状態のため必ず失敗します。

Failの通知や表示が出ますが、慌てずに次のステップに移りましょう。

## 3-2: Webhook URLを取得する

### Netlify WebhookのURLを取得する
Netlifyの管理画面を開き、先程作成したサイトを選択します。
![screenshot](../gatsby/img/11.png)

[Deploys]を選択します。
![sceenshot](../gatsby/img/12.png)

[Deploy Settings]を選択します。
![screenthot](../gatsby/img/13.png)

スクロールして[Build hooks]を表示します。

![screenthos](../gatsby/img/14.png)

[Add build hook]をクリックします。
![screenthos](../gatsby/img/15.png)

[Name]にShifterのビルドで使用するということを書きます。
Branchはmasterのまま、[Save]をクリックします。

![screenthos](../gatsby/img/16.png)
作成されたWebhookのURLをコピーします。

![screenthos](../gatsby/img/17.png)


## Checklist
- [ ] Netlifyでサイトを作成した
- [ ] Netlifyのデプロイ設定を行った
- [ ] 初回のビルドが実行され、failした
- [ ] NetlifyのWebhook URLを取得した

## Navigation
- [Step1: Webhook対応プランに変更する](./step1.md)
- [Step2: Netlifyデプロイ用のテンプレートをインポートする](./step2.md)
- Now -> [Step3: Netlifyにサイトを作成する](./step3.md)
- [Step4: ShifterサイトからWebhookでデプロイする](./step4.md)
- [Tier Down: プランをダウングレードする](./tierdown.md)
- [Advanced challenge](./advanced.md)