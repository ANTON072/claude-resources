# カスタムコマンドフロントマターリファレンス

スラッシュコマンドのフロントマターフィールドの完全なリファレンス。

## 利用可能なフィールド

| フィールド | デフォルト | 説明 |
| --------- | --------- | ---- |
| `description` | プロンプトの最初の行 | `/help` とオートコンプリートに表示される簡単な説明 |
| `argument-hint` | なし | オートコンプリートに表示される期待される引数。例：`[filename] [options]` |
| `allowed-tools` | 会話から継承 | コマンドが使用できるツール。bash実行に必要 |
| `model` | 会話から継承 | 使用する特定のモデル。[モデル概要](https://docs.claude.com/en/docs/about-claude/models/overview)を参照 |
| `disable-model-invocation` | `false` | ClaudeがSkillツールを通じて自動的に起動することを防ぐ |
| `hooks` | なし | コマンドスコープのフック。[フックドキュメント](/ja/hooks)を参照 |

## フィールドの詳細

### description

`/help` 出力とオートコンプリートメニューに表示される簡単な説明。

```yaml
---
description: 従来のメッセージでgitコミットを作成する
---
```

省略した場合、プロンプトコンテンツの最初の行が使用される。

### argument-hint

オートコンプリート中に期待される引数を表示する。何を渡すべきかをユーザーが理解するのに役立つ。

```yaml
---
argument-hint: [pr-number] [priority] [assignee]
---

優先度 $2 でPR #$1 をレビューし、$3 にアサインする。
```

一般的なパターン：

- `[filename]` — 単一の必須引数
- `[options...]` — 複数のオプション引数
- `[source] [target]` — 2つの位置引数
- `add [id] | remove [id] | list` — 複数のサブコマンド

### allowed-tools

コマンドが使用できるツールを指定する。`!`コマンド`` のbash実行に必要。

```yaml
---
allowed-tools: Bash(git:*), Bash(gh:*), Read, Grep
---
```

ツールパーミッションパターン：

| パターン | 説明 |
| ------- | ---- |
| `Read` | Readツールを許可 |
| `Bash(git:*)` | `git` で始まるbashコマンドを許可 |
| `Bash(npm run:*)` | `npm run` で始まるbashコマンドを許可 |
| `Bash(*)` | すべてのbashコマンドを許可（注意して使用） |

複数のツールはカンマで区切る。

### model

このコマンドに特定のモデルを強制する。

```yaml
---
model: claude-3-5-haiku-20241022
---
```

一般的なモデル文字列：

- `claude-sonnet-4-20250514` — Claude Sonnet 4
- `claude-3-5-haiku-20241022` — Claude 3.5 Haiku（高速、低コスト）
- `claude-opus-4-20250514` — Claude Opus 4

### disable-model-invocation

ClaudeがSkillツールを通じてこのコマンドを自動的に起動することを防ぐ。

```yaml
---
disable-model-invocation: true
---
```

使用場面：

- 副作用のあるコマンド（デプロイ、削除など）
- 手動でコントロールしたいコマンド
- センシティブな操作

### hooks

コマンド実行中に実行されるフックを定義する。コマンドが終了すると自動的にクリーンアップされる。

```yaml
---
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/validate.sh"
          once: true
  PostToolUse:
    - matcher: "Write"
      hooks:
        - type: command
          command: "./scripts/lint.sh"
---
```

フックオプション：

- `matcher`：マッチするツール名
- `type`：シェルコマンドの場合は `command`
- `command`：実行するシェルコマンド
- `once`：`true` の場合、セッションごとに一回だけ実行

## 完全な例

```yaml
---
description: バリデーション付きでアプリケーションをステージングにデプロイする
argument-hint: [environment] [version]
allowed-tools: Bash(./scripts:*), Bash(git:*), Bash(docker:*)
model: claude-sonnet-4-20250514
disable-model-invocation: true
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/pre-deploy-check.sh"
          once: true
---

バージョン $2 を $1 環境にデプロイする。

## デプロイ前コンテキスト
- 現在のブランチ: !`git branch --show-current`
- 保留中の変更: !`git status --short`

## デプロイステップ
1. テストを実行する
2. アプリケーションをビルドする
3. $1 にプッシュする
4. デプロイを確認する
```

## 文字列置換

コマンドコンテンツで利用可能なプレースホルダー：

| プレースホルダー | 説明 |
| -------------- | ---- |
| `$ARGUMENTS` | 単一の文字列としてのすべての引数 |
| `$1`、`$2`、`$N` | 位置引数（1始まり） |
| `!`コマンド`` | シェルコマンドの出力（`allowed-tools: Bash(...)` が必要） |
| `@path/to/file` | 参照ファイルの内容 |
