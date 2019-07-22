# Step2: Netlifyデプロイ用のテンプレートをインポートする
Webhookが利用可能になりましたので、早速設定を進めます。

## 2-1: GitHubでForkする

NetlifyにGenerateしたArtifactを公開するための設定は、以下のGitHubリポジトリにまとめています。

https://github.com/getshifter/webhook-artifact-created

URLにアクセスし、右上の[Fork]ボタンで自身のアカウントでForkしましょう。

![workshop screenshot](./img/7.png)

Forkが完了したら準備OKです。
![workshop screenshot](./img/9.png)

## 2-2: ちょっとだけ仕組みを読んでみる

Netlifyのデプロイでは、`./netlify/deploy.sh`が実行されます。

これは以下のようなタスクを実行しています。

```bash
# Webhookで送信されたダウンロードURLからArtifactをDLする
$ wget -O "${ARTIFACT_ID}.tgz" "${DOWNLOAD_URL}"

# DLしたファイルを解凍する
$ tar xvzf "${ARTIFACT_ID}".tgz

# Netlifyで公開ディレクトリに指定した場所へ、静的ファイルを移動する
$ mv "${ARTIFACT_ID}" public
```

`_headers`や`_redirect`など、Netlify用に追加したいファイルがある場合は、この`./netlify/deploy.sh`に処理を追加してカスタマイズしましょう。


## Checklist
- [ ] リポジトリをforkした
- [ ] デプロイ時の処理についてざっと把握した
- [ ] カスタマイズ方法をざっと理解した