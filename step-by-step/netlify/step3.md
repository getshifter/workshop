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

## 3-2: Webhook URLを取得する