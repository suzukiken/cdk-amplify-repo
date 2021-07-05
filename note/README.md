+++
title = "Amplify Appを作る話の続き"
tags = ["Amplify"]
date = "2021-03-19"
+++

自分のAmplifyの使い方については[別の記事](/aws/about-amplify)に書いた通りで、そのAmplifyで作るウェブページの方は、`react-create-app`して`npm install aws-amplify`するところを開発のスタート時点としている。

ちなみにそのコードを置くリポジトリは[このCDKプロジェクト](/aws/cdkamplify)で作っている。

Amplifyコンソールを使っているから、リポジトリにpushすると自動的にデプロイされるという仕組みとなっている。

[Githubのリポジトリ](https://github.com/suzukiken/cdkamplify-repo)

これをデプロイする際に、Systems Managerのパラメーターストアに保存してあるJavaScript（aws-exports.js）を取り込むようにしているので、デプロイ前に手作業でaws-exports.jsをパラメーターストアに保存しておく必要がある。

具体的にはこの上のサンプルのリポジトリの場合だと、`cdkamplify-exports-json`という名前でパラメーターストアに、JavaScriptのコードをtextとして保存しておくことになる。

この`cdkamplify-exports-json`という名前は[リポジトリを作成するCDKプロジェクト](/aws/cdkamplify)の方で、Amplifyコンソールに対して環境変数`SSM_PARAMETER_NAME`として設定している。

Amplifyコンソールの側は[リポジトリ](https://github.com/suzukiken/cdkamplify-repo)中のamplify.ymlに従って、getparamsというnpmスクリプトでパラメータストアの内容を取り込むようになっている。

```amplify.yml
frontend:
  phases:
    ...
    build:
      commands:
        - npm run getparams -- $SSM_PARAMETER_NAME
```

この際呼び出されるget-amplify-config.jsは、パラメータストアから得られた内容をそのまま、src/aws-exports.jsに保存する。

という仕組みで動いてます、という話でした。（文章で書いてもわけわからん）