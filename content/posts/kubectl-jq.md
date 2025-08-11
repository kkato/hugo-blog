---
title: "kubectlコマンドとjqコマンドの便利な組み合わせ"
date: 2025-08-10T19:52:30+09:00
draft: false
tags: ["kubernetes", "command"]
---

jqコマンドとは、JSON形式のデータを処理・変換するためのコマンドです。
普段はkubectlコマンドやAWSコマンドと組み合わせて使うことが多いのですが、使い方やオプションをきちんと調べたことがなかったため、本記事の執筆を機に改めておさらいしようと思います。
この記事では、知っておくと便利な関数やjqコマンドの実践的な組み合わせ例を紹介します。

## インストール方法

macOSの場合は以下のコマンドでインストールできます。
```bash
brew install jq
```

## jqコマンドの関数

### keys関数

これまでは`jq ','`のように全階層をまとめて表示してキーを把握していましたが、keys関数を使うと一階層ずつキーを表示できます。

```bash
# labelのキーを取得する
kubectl get pod my-pod -o json | jq '.metadata.labels | keys'
```

### select関数

select関数は条件にマッチした項目のみを抽出します。

```bash
# Running状態ではないPodの一覧を取得する
kubectl get pods -A -o json | jq '.items[] | select(.status.phase != "Running") | .metadata.name'

# 上記の応用として、namespace、Pod名、Podの状態も合わせて表示する
kubectl get pods -A -o json | jq -r '.items[] | select(.status.phase != "Running") | "\(.metadata.namespace) \(.metadata.name) \(.status.phase)"' | column -t
```

2つ目のコマンドではいくつかのオプションや機能を使っているため、それらについて説明します。
- `-r`: ダブルクォートを削除するオプション(デフォルトでは`"`が付いてくる)
- `"\(filter)"`: 文字列の中で `\(filter)` を使うことで、抽出した値を文字列に埋め込める。これにより、複数の値を空白区切りで1行に表示できる
- `columnt -t`: columnコマンドの`-t`オプションを使うことで、区切り文字を基準に列を揃えて表形式で表示できます

### contains関数

contains関数は指定した文字列が含まれていればtrueを返します。

```bash
# コンテナ名に'ghr.io'を含むPodを検索する
kubectl get pods -o json | jq '.items[] | select(.spec.containers[].name | contains("gcr.io")) | .metadata.name'
```

## まとめ

kubectlとjqの組み合わせは、Kubernetesの運用において非常に強力です。特に、`keys`、`select`、`contains`などの関数を活用することで、複雑なJSONデータから必要な情報を効率的に抽出できます。今回は、普段自分が使っている基礎的な関数やオプションを紹介しましたが、他にも便利な機能がまだまだあるので、今後さらに活用していきたいと思います。
