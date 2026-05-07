# claude-resources

私の個人的な [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 設定：カスタムコマンド、スキル、エージェント、フック、スクリプト。

## 内容

| ディレクトリ | 説明 |
| ----------- | ---- |
| `commands/` | カスタムスラッシュコマンド（`/commits`、`/pr`、`/purge-private-info` など） |
| `skills/` | スキル — Claudeの機能を拡張するモジュラーパッケージ |
| `agents/` | カスタムサブエージェント定義（code-reviewer、tdd-developer など） |
| `hooks/` | フックスクリプト（例：停止時のIFTTT通知） |
| `doc/` | リソースを閲覧するためのDocusaurusドキュメントサイト |
| `CLAUDE.md` | すべてのプロジェクトに適用されるグローバル指示 |

## 使い方

### オプションA：Claude Codeプラグインとしてインストール（推奨）

このリポジトリにはClaude Codeマーケットプレイスマニフェストが含まれています。一度にすべてをインストールできます：

```
/plugin marketplace add takazudo/claude-resources
/plugin install claude-resources@takazudo-claude-resources
```

インストール後、Claude Codeはバンドルされたすべてのコマンド、スキル、エージェント、フックを認識します。不要なものは `/plugin disable` で無効化できます。

### オプションB：手動コピー

必要なものを参照してお使いください。スキルやコマンドを自分の環境で使うには、`$HOME/.claude/` ディレクトリにコピーします：

```
$HOME/.claude/
├── commands/    # コマンド .md ファイルをここに配置
├── skills/      # スキルディレクトリをここに配置
├── agents/      # エージェント .md ファイルをここに配置
├── hooks/       # フックスクリプトをここに配置
└── scripts/     # ユーティリティスクリプトをここに配置
```

コマンド、スキル、エージェントの詳細については[Claude Codeドキュメント](https://docs.anthropic.com/en/docs/claude-code)を参照してください。

## 免責事項

これは私の個人設定で、現状のまま共有しています。一部のリソースは私固有の環境やワークフローを参照している場合があります。使用は自己責任で — 使用前に確認し、自分の環境に合わせて調整してください。

## ライセンス

MIT
