# Step3: Shifter Webhookで毎回変わるWordPressにURLに対応する

ShifterではWordPressのURLが起動の度に変更されます。  
そのため、Step2の方法だけでは毎回`gatsby-config.js`のURLを変更する必要があります。

そこでStep3では、常に変わるWordPressのURLに対応しつつ、WordPress側の操作だけでGatsbyのビルド・デプロイを行う方法を学びます。

## 3-1: 事前準備 - アカウントの作成
このステップでは、以下のサービスにログインします。
無料で作成できますので、まだアカウントの無い方はアカウントを作成してください。

- [Netlify](https://netlify.com/)
- [GitHub](https://github.com/)

## 3-1: GatsbyサイトにNetlifyデプロイのための設定を行う

### WordPress URLの変更: gatsby-config.js

Step2では、URLを直接入力しました。  
しかしShifterでは起動する度にURLが変わります。  
Step3では`gatsby-config.js`の`plugins[].options.baseUrlを以下のように変更します。


```javascript
module.exports = {
  siteMetadata: {
    title: 'Gatsby + WordPress Starter',
  },
  plugins: [
    'gatsby-plugin-react-helmet',
    'gatsby-plugin-sass',
    {
      resolve: 'gatsby-source-wordpress',
      options: {
        // CONTAINER_URLを利用する
        baseUrl: process.env.CONTAINER_URL.replace('https://', ''),
        // WP.com sites set to true, WP.org set to false
        hostingWPCOM: false,
        // The protocol. This can be http or https.
        protocol: 'https',
        // Use 'Advanced Custom Fields' Wordpress plugin
        useACF: false,
        auth: {},
        // Set to true to debug endpoints on 'gatsby build'
        verboseOutput: false,
      },
    },
    'gatsby-plugin-sharp',
    'gatsby-transformer-sharp',
    {
      // Removes unused css rules
      resolve:'gatsby-plugin-purgecss',
      options: {
        // Activates purging in gatsby develop
        develop: true,
        // Purge only the main css file
        purgeOnly: ['/all.sass'],
      },
    }, // must be after other CSS plugins
    'gatsby-plugin-netlify', // make sure to keep it last in the array
  ],
}

```

### ビルドコマンドの作成: build.sh

続いてNetlify上でGatsbyのビルドを実行するときのコマンドをカスタマイズします。

```bash
$ touch ./build.sh
$ chmod +x build.sh
$ vim build.sh
#!/bin/bash -x
env

if [ "${INCOMING_HOOK_BODY}" == "" ] ; then
  echo 'No webhook'
  exit 0
fi

# echo "${INCOMING_HOOK_BODY}" | jq -r .CONTAINER_URL # デバッグ用
export CONTAINER_URL=$(echo "${INCOMING_HOOK_BODY}" | jq -r .CONTAINER_URL)
npm run build
```

### デプロイ設定ファイルの作成: netlify.toml
最後にNetlifyでの公開設定を`netlify.toml`にまとめて保存します。

```toml
[build]
  publish = "public"
  command = "./build.sh"
```

## 3-2: GatsbyサイトをGitHubにアップロード

Step2で作成したGatsbyサイトをGitHubへアップロードします。

### Gitリポジトリの作成
https://github.com/new から新しくリポジトリを作成します。
今回はワークショップですので、"Public"で問題ありません。

![workshop screenshot](./img/5.png)
※ Netlifyはprivate リポジトリにも対応しています、実運用ではprivateリポジトリを利用することを推奨します。

作成後、[Clone or download]からGitリポジトリのURLをコピーしておきましょう。

![workshop screenshot](./img/6.png)

### GatsbyサイトをGit addする

Step2で作成したサイトをGit管理下に置きましょう。

```bash
$ pwd
/PATH/TO/wp-shifter

$ git init
$ git add ./
$ git commit -m "fisrt commit"
```

Gatsby側ですでに`.gitignore`が生成されていますので、実装・運用時にignoreする必要のあるファイルがでなければそのままaddすればokです。

### GitHubへアップロードする
先程作成したGitHubのリポジトリにアップロードしましょう。

```
$ git remote add origin git@github.com:YOUR_USERNAME/YOUR_REPOSITORY_NAME.git
$ git push origin master
```


#### Note: git pushがうまくいかない時

GitHubのssh接続設定がされていないか、設定に問題がある可能性があります。  
https://qiita.com/shizuma/items/2b2f873a0034839e47ce  
などを参考に設定を確認してください。

## 3-3: Netlifyでサイトを作成する
Netlifyにログインします。
https://app.netlify.com/

[New site from Git]をクリックし、サイトを作成します。
![workshop screenshot](./img/7.png)

[GitHub]を指定し、認証を許可します。
![workshop screenshot](./img/8.png)

先程作成したリポジトリを選びます。

![workshop screenshot](./img/9.png)

ビルドの設定を行います。
以下のような設定になっていることを確認してください。

|項目|値|備考|
|:--|:--|:--|
|Owner|<あなたのユーザー名>'s team|ここはどんな値でもOK|
|Branch to deploy|master||
|Build command|./build.sh||
|Public directory|public||

![workshop screenshot](./img/10.png)

最後に[Deploy site]をクリックすればOKです。

### Note:初回のビルドについて
設定が終わると、Netlifyが初回のビルドを開始します。  
しかし次のステップのビルド方法でなければ、WordPressのURLが存在しない状態のため必ず失敗します。

Failの通知や表示が出ますが、慌てずに次のステップに移りましょう。

## 3-4: Netlify WebhookをShifter WordPressから利用する

### Netlify WebhookのURLを取得する
Netlifyの管理画面を開き、先程作成したサイトを選択します。
![screenshot](./img/11.png)

[Deploys]を選択します。
![sceenshot](./img/12.png)

[Deploy Settings]を選択します。
![screenthot](./img/13.png)

スクロールして[Build hooks]を表示します。

![screenthos](./img/14.png)

[Add build hook]をクリックします。
![screenthos](./img/15.png)

[Name]にShifterのビルドで使用するということを書きます。
Branchはmasterのまま、[Save]をクリックします。

![screenthos](./img/16.png)
作成されたWebhookのURLをコピーします。

![screenthos](./img/17.png)

### ShifterでWebhookを設定する
ShifterでWordPressを起動します。  
起動後、[Dashboard]をクリックします。
![screenthos](./img/2.png)

WordPress管理画面で、[Shifter > Webhook]を選択します。
先程コピーしたNetlifyのWebhook URLを貼り付けて[Save Changes]をクリックしましょう。
![screenthos](./img/18.png)


上部管理バーの[Shifter > Send Webhook]をクリックします。

![screenthos](./img/19.png)

[Execute]をクリックします。
![screenthos](./img/20.png)

Netlifyでビルドが始まります。
![screenthos](./img/21.png)

Netlifyの[Deploys]でビルドが成功したことを確認します。
![screenthos](./img/22.png)

URLをクリックして、公開されたサイトを確認します。
![screenthos](./img/23.png)


## Checklist

- [ ] build.sh / gasby-config.js / netlify.tomlを設定した
- [ ] GatsbyサイトをGitHubにpushした
- [ ] NetlifyでGitHubのリポジトリと連携したサイトを作成した
- [ ] ShifterのWordPressからNetlifyのwebhookを実行した
- [ ] ShifterのWordPress記事をNetlify上に公開できた

## Step2: まとめ
Step3では、Gatsbyを使ったサイトを実際に運用するフローを体験しました。  
ここではWordPress管理画面からビルドボタンを都度押す必要がありますが、各サービスの機能を組み合わせることで、より実践的な運用フローを組むことも可能です。

[Advanced challenge](./advanced.md)に様々なアイディアを用意しておりますので、ぜひチャレンジしてみてください。

## Navigation
- [Step1: WordPressサイトのセットアップ](./step1.md)
- [Step2: GatsbyでWordPressのデータをインポートする](./step2.md)
- NOW -> [Step3: Shifter Webhookで毎回変わるWordPressにURLに対応する](./step3.md)
- [Advanced challenge](./advanced.md)