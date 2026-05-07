---
name: gh-issue-with-imgs
description: "CLIを使って画像付きのGitHub Issueを作成する。画像をGitHubリリースアセットとしてアップロードし、Issue本文に埋め込む。使用タイミング：(1) スクリーンショットが必要なIssueを作成する場合、(2) ブラウザUIなしでプログラム的に画像を添付する場合、(3) ユーザーが「画像付きIssue」「gh issue with imgs」「スクリーンショット付きIssueを作成」と言った場合。"
user-invocable: true
argument-hint: <owner/repo> <title> --body <body> --img <path> [--img <path>...]
allowed-tools:
  - Bash
  - Read
  - Glob
---

# 画像付きGitHub Issue

リリースアセットを画像ホスティングとして使用して、画像付きのGitHub Issueを作成します。

GitHub CLIはIssueへの画像添付をネイティブにサポートしていません。このスキルは、専用のリリースに画像をアップロードし、そのアセットURLをIssue本文のMarkdownに埋め込むことで回避します。

## 使い方

引数：`<owner/repo> <title> --body <body> --img <path> [--img <path>...]`

複数の `--img` フラグをサポート。`--body` がない場合は空の文字列を使用。

## プロセス

### ステップ1：引数を解析する

スキルの引数から以下を抽出する：

- `owner/repo`（必須）— ターゲットリポジトリ
- タイトル（必須）— Issueのタイトル
- `--body`（オプション）— Issue本文のテキスト
- `--img`（1つ以上）— 画像ファイルへのパス

### ステップ2：Attachmentsリリースの存在を確認する

リポジトリに `_attachments` タグ付きの非ドラフトリリースがあるか確認する。なければ作成する。

古い**ドラフト**の `_attachments` リリースが存在する場合（このfix以前のもの）、先に削除する — ドラフトリリースにはタグがないため、アップロードされたアセットは認証なしのアクセスで404を返す。

```bash
# タグ付き（非ドラフト）リリースが存在するか確認
gh release view _attachments --repo <owner/repo> 2>/dev/null

# 存在しない場合、"_attachments"という名前の古いドラフトリリースを確認してクリーンアップ
# （ドラフトリリースにはタグがないため、APIでタイトルで検索）
OLD_DRAFT_ID=$(gh api repos/<owner/repo>/releases --jq '.[] | select(.draft == true and .name == "_attachments") | .id' 2>/dev/null)
if [ -n "$OLD_DRAFT_ID" ]; then
  gh api -X DELETE repos/<owner/repo>/releases/$OLD_DRAFT_ID
fi

# 非ドラフトリリースを作成（--draft は使わない — ドラフトリリースのアセットは
# 認証なしのリクエストで404を返す。ブラウザでログイン済みの場合は動作するように見えるが）
gh release create _attachments --title "_attachments" --notes "Issueの画像添付ファイル。削除しないでください。" --repo <owner/repo>
```

### ステップ3：画像をアップロードする

各 `--img` パスについて、コリジョンを避けるためのユニークなファイル名を生成し、アップロードする。

```bash
# ユニーク名を生成：タイムスタンプ + 元のファイル名
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
UNIQUE_NAME="${TIMESTAMP}-$(basename <path>)"

# アップロード
gh release upload _attachments "<path>#${UNIQUE_NAME}" --repo <owner/repo> --clobber
```

**重要**：`#name` 構文はアップロード時にアセットをリネームする。名前のコリジョンが発生した場合は `--clobber` で上書き。

`gh release upload` が `#name` リネーム構文をサポートしていない場合は、代わりにファイルをユニーク名で一時的な場所にコピーする：

```bash
TMPFILE="/tmp/${UNIQUE_NAME}"
cp "<path>" "$TMPFILE"
gh release upload _attachments "$TMPFILE" --repo <owner/repo> --clobber
rm "$TMPFILE"
```

### ステップ4：アセットURLを取得する

アップロードされたアセットのダウンロードURLを取得する。

```bash
gh api repos/<owner/repo>/releases/tags/_attachments \
  --jq '.assets[] | select(.name == "<UNIQUE_NAME>") | .browser_download_url'
```

### ステップ5：Issue本文を構築する

本文に画像のMarkdownを追加する：

```markdown
<元の本文テキスト>

![<filename>](<asset_url>)
```

複数の画像の場合は、それぞれ別の行に追加する。

### ステップ6：Issueを作成する

```bash
gh issue create \
  --repo <owner/repo> \
  --title "<title>" \
  --body "$(cat <<'EOF'
<画像を埋め込んで構築した本文>
EOF
)"
```

作成されたIssueのURLを表示する。

## メモ

- `_attachments` リリースは実際の（非ドラフト）リリースなので、アセットURLは認証なしでアクセス可能
- `--draft` は絶対に使わない — ドラフトリリースのアセットは認証なしのリクエストで404を返す（ユーザーがログイン済みのブラウザでは動作するように見えても）
- アセットURLはリリースが存在する限り永続的
- パブリックとプライベートリポジトリの両方で動作する（プライベートリポジトリのアセットはアクセスに認証が必要）
- 画像サイズ制限：GitHubリリースアセットと同じ（ファイルあたり2GB）
- サポートフォーマット：あらゆる画像フォーマット（png、jpg、gif、svg、webp など）
