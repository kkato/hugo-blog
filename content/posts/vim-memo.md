---
title: "Vimキーバインド 備忘録"
date: 2024-09-21T13:04:51+09:00
draft: false
categories: ["vim"]
---

同僚のfohteさんの [なれる! Vimmer](https://gist.github.com/fohte/f99aab472119acd736dfc688bfdf383c) が面白かったので、自分の備忘録としてまとめてみました。

### 移動系
- `gg`, `G`: ファイル先頭に移動 / ファイル最終行に移動
- `^`: カーソル行の先頭に移動
- `$`: カーソル行の末尾に移動
- `w`: 単語ごと後ろに移動
- `b`: 単語ごと前に移動

### より便利な移動系
- `{`, `}`: 空行へ移動
- `%`: 対応する括弧に移動

### 編集系
- `dd`: カーソル行を削除
- `yy`: コピー
- `p`: ペースト
- `>>`, `<<`: インデント

### 検索系
- `/`: 検索する
-  `n`: 次の検索結果に移動
- `N`: 前の検索結果に移動
- `%`: カーソル下の単語で検索
- `f` + 一文字: カーソル行の指定した文字まで移動
- `F` + 一文字: カーソル行の指定した文字まで逆方向移動