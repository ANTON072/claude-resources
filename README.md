# claude-resources

私の個人的な [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 設定：カスタムコマンド、スキル、エージェント。

## 内容

| ディレクトリ | 説明 |
| ----------- | ---- |
| `commands/` | カスタムスラッシュコマンド |
| `skills/` | スキル — Claudeの機能を拡張するモジュラーパッケージ |
| `agents/` | カスタムサブエージェント定義 |
| `CLAUDE.md` | すべてのプロジェクトに適用されるグローバル指示 |

### コマンド

| コマンド | 説明 |
| -------- | ---- |
| `/git-commit` | 差分を分析してコミットメッセージを自動生成し、コミットを実行する |
| `/post-pending-review` | 指定PRのコードレビューを実施し、pending reviewとして投稿する |
| `/vpr` | 現在のPRをWebブラウザで開く |

### スキル

| スキル | 説明 |
| ------ | ---- |
| `claudemd-refactor` | CLAUDE.mdをディレクトリスコープの階層構造にリファクタリング・最適化する |
| `custom-command-creator` | カスタムスラッシュコマンドを作成・管理する |
| `pr` | インテリジェントなベースブランチ検出でプルリクエストを作成する |
| `pr-revise` | 既存のPRのタイトルと説明を更新する |
| `purge-private-info` | プライベート/機密情報をスキャンして削除・置換する |
| `subagent-manage` | カスタムエージェントを作成・修正する |

### エージェント

| エージェント | モデル | 説明 |
| ------------ | ------ | ---- |
| `code-reviewer` | opus | コードレビュアー |
| `frontend-developer` | sonnet | フロントエンド開発エージェント。TDDとUI実用テストを適用し自律的に動作する |
| `researcher` | opus | リサーチャー |
| `text-fixer` | haiku | テキスト修正エージェント |

## 必要なMCP

一部のエージェントやスキルは以下のMCPサーバーに依存しています。

| MCP | 用途 | 設定方法 |
| --- | ---- | -------- |
| [Playwright](https://github.com/microsoft/playwright-mcp) | ブラウザ操作・UIテスト（`frontend-developer` エージェント） | `claude mcp add playwright npx @playwright/mcp@latest` |
| [Context7](https://github.com/upstash/context7-mcp) | ライブラリ/フレームワークのドキュメント参照（`frontend-developer`、`code-reviewer` エージェント） | `claude mcp add context7 npx @upstash/context7-mcp@latest` |
| [Serena](https://github.com/oraios/serena) | コード構造・依存関係の解析（`code-reviewer` エージェント） | `claude mcp add serena uvx serena` |

> MCPの追加はグローバル設定に反映されます。詳細は `claude mcp --help` を参照してください。

## 使い方

必要なものを参照してお使いください。スキルやコマンドを自分の環境で使うには、`$HOME/.claude/` ディレクトリにコピーします：

```
$HOME/.claude/
├── commands/    # コマンド .md ファイルをここに配置
├── skills/      # スキルディレクトリをここに配置
└── agents/      # エージェント .md ファイルをここに配置
```

コマンド、スキル、エージェントの詳細については[Claude Codeドキュメント](https://docs.anthropic.com/en/docs/claude-code)を参照してください。

## 免責事項

これは私の個人設定で、現状のまま共有しています。一部のリソースは私固有の環境やワークフローを参照している場合があります。使用は自己責任で。

## ライセンス

MIT
