---
name: format-md
description: "@takazudo/mdx-formatterを使用してMarkdown（.md）とMDX（.mdx）ファイルをフォーマットする。使用タイミング：(1) ユーザーがmarkdownファイルをフォーマットしたい場合、(2) ユーザーが「mdをフォーマット」「markdownをきれいにする」「markdownフォーマットを修正」と言った場合、(3) Markdown/MDXファイルに一貫したフォーマットが必要な場合。機能：remarkベースのASTフォーマット、MDX（JSX、インポート/エクスポート）、日本語テキスト、HTML→md変換、Docusaurusのadmonitions、GFM（テーブル、取り消し線、タスクリスト）。"
---

# Markdown/MDXファイルのフォーマット

`@takazudo/mdx-formatter` を使用して指定されたMarkdownまたはMDXファイルをフォーマットします。

## 使い方

ユーザーは以下を指定できます：

- 特定のファイルパス（例：`README.md`、`docs/guide.md`）
- 複数のファイル（例：`docs/*.md`）
- ファイルが指定されていない場合は、どのファイルをフォーマットするかユーザーに質問する

## フォーマッターの詳細

- **npmパッケージ**：`@takazudo/mdx-formatter`
- **コマンド**：`npx @takazudo/mdx-formatter --write <file-path>`

## 手順

1. ユーザーがファイルパスまたはパターンを指定した場合は、そのまま使用する
2. ファイルが指定されていない場合は、どのファイルをフォーマットするかユーザーに質問する
3. Bashツールを使用してフォーマッターを実行する：

   ```bash
   npx @takazudo/mdx-formatter --write <file-path>
   ```

4. 結果をユーザーに報告する

## 例

```bash
# 単一ファイルのフォーマット
npx @takazudo/mdx-formatter --write README.md

# globパターンで複数ファイルをフォーマット
npx @takazudo/mdx-formatter --write "docs/**/*.md"
```

## 機能

フォーマッターが提供する機能：

- remarkによるASTベースのフォーマット
- MDXサポート（JSXコンポーネント、インポート/エクスポート）
- 日本語テキストの処理
- HTMLからMarkdownへの変換
- Docusaurusのadmonitionsの保持
- GFM機能（テーブル、取り消し線、タスクリスト）
