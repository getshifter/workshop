# Advanced

より実践的な運用についてチャレンジされたい方向けのコンテンツです。
ぜひチャレンジして、ご自身のブログやQiitaなどで紹介してください。


## Challenge1: 問い合わせフォームの設置

Gatsbyで作成したSPAに問い合わせフォームを設置する必要があります。
Netlifyが提供するForm機能や、Zendesk / Intercom / Google Formsなどを利用して、サイトに問い合わせフォームを設置してみましょう。

https://www.netlify.com/docs/form-handling/

## Challenge2: 画像の表示
ワークショップの内容だけでは、画像の同期がされていません。

[S3-Uploads](https://github.com/humanmade/S3-Uploads)などのS3 / CloudFrontや外部サービスへ画像をsyncさせるプラグイン・SDKを活用して、Netlify側で画像を表示させてみましょう。

## Challenge3: 記事公開時のビルド
NetlifyのWebhookは、Shifterのプリインストールプラグインが用意しているWP APIのエンドポイントをPOSTすることで実行しています。

POST: shifter/v1/webhook
https://github.com/getshifter/shifter-wp-webhook/blob/master/libs/scripts.js#L7-L17

save_postなど、記事公開時に実行されるhookとこのAPIを組み合わせることで、  
「WordPressで記事を公開すると、Gatsbyのビルドが実行される」というワークフローも作ることができます。

通常のWordPressを運用するような形でGatsbyのビルドを実行する方法について、ぜひ考えてみてください。

## Challenge4: git pushでのビルド
ShifterはAPIを公開しています。
https://support.getshifter.io/en/articles/762113-shifter-api

このAPIを利用することで、git pushでNetlifyのビルドが実行された時にもビルドを成功するようにできます。

- 環境変数に記録したログイン情報でログイン
- ShifterのWordPressを起動する
- ShifterのWordPress URLを取得する
- 環境変数に設定する
- ビルドを実行する

## Challenge5: 通常のWordPressサイトからのマイグレーション
Gatsbyのビルドは、`CONTAINER_URL`にWordPressのURLが入っていれば、通常のWordPressサイトでも利用できます。
この環境変数を利用して、通常のWordPressサイトをGatsbyでビルドし、その後Shifterにマイグレーションするという段階的なマイグレーションが可能です。

通常のWordPressサイトをServerlessにするマイグレーションについて、ぜひチャレンジしてください。
