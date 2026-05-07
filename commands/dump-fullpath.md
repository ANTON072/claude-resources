---
name: dump-fullpath
description: 直前のメッセージで言及されたファイルの完全な絶対パスを出力する。
disable-model-invocation: true
---

# フルパスの出力

直前のコンテキストにあるファイルの完全な絶対パスを出力します。

## 手順

1. 直前のメッセージや最近のコンテキストを確認する
2. 言及されたファイルパス（相対パスまたは部分的なパス）を特定する
3. 完全な絶対パスのみを出力する

## 出力フォーマット

追加のテキストなしに絶対パスを1行で出力する：

```
/Users/username/path/to/file.ext
```

## 例

直前のメッセージで `src/components/Button.tsx` が言及されていた場合：

```
/Users/username/my-project/src/components/Button.tsx
```
