# カスタマイズ記録

知人のリポジトリをforkしたものを、自分用に整理・削除していく記録。

## 方針

- 使わないスキル・コマンド・エージェントは削除
- CLAUDE.md も自分の用途に合わせて整理
- docs/ に決定記録を残す

---

## スキル棚卸し

### カテゴリ分類

#### 電子工作系（KiCad / EasyEDA / JLCPCB）
おそらく不要。確認して削除候補。

- `ascii-circuit-diagram-creator`
- `easyeda2kicad`
- `jlcpcb-bom-generate-from-kicad`
- `jlcpcb-component-finder`
- `jlcpcb-component-finder-update-db`
- `kicad-sch-tweak`
- `kicad-svg-fix`
- `schemdraw-circuit-generator`

#### Copilot CLI 依存（gco / gcoc / codex）
Copilot CLI や Codex CLI を使っていなければ不要。

- `codex-2nd`
- `codex-research`
- `codex-review`
- `codex-translator`
- `codex-writer`
- `gco`
- `gco-2nd`
- `gco-research`
- `gco-review`
- `gco-stack-trace-read`
- `gco-translate`
- `gcoc`
- `gcoc-2nd`
- `gcoc-research`
- `gcoc-review`
- `copilot-instructions`

#### PR / Git 操作
汎用的に使えるものが多い。必要なものだけ残す。

- `commits` — コミットメッセージ自動生成
- `commits-auto` — 自動コミット
- `commits-forbid` — コミット禁止
- `pr` — PR作成
- `pr-complete` — PR完了処理
- `pr-make-suggestion-edit` — PRレビューコメントを編集に反映
- `pr-make-suggestion-to-pr` — サジェストをPRに反映
- `pr-recreate` — PR再作成
- `pr-revise` — PR修正
- `pr-split` — PRを分割
- `push-auto` — 自動プッシュ
- `push-forbid` — プッシュ禁止
- `b4push-wisdom` — プッシュ前チェック
- `dev-create-b4push-script` — b4pushスクリプト生成
- `git-filtered-merge` — フィルタマージ
- `git-prune-branches-both` — ブランチ削除（local+remote）
- `git-prune-branches-local` — ローカルブランチ削除
- `git-prune-branches-remote` — リモートブランチ削除

#### コードレビュー系
- `deep-review` — 詳細レビュー
- `light-review` — 軽量レビュー
- `review-loop` — レビューループ
- `x-as-pr` — diffをPR形式でレビュー

#### GitHub 操作
- `gh-actions-wisdom` — GitHub Actions ベストプラクティス参照
- `gh-fetch-issue` — GitHub Issueを画像ごとローカルに取得
- `gh-issue-with-imgs` — 画像付きIssue作成
- `dependabot-resolve` — Dependabot PR解決
- `dev-gh-actions-doc-auto-merge` — GH Actionsドキュメント自動マージ
- `dev-gha-ifttt-notify` — GitHub Actions × IFTTT通知
- `dev-ci-ifttt-notify` — CI × IFTTT通知
- `dev-actions-self-runner` — セルフホストランナー設定
- `watch-ci` — CI監視

#### Figma 関連
- `dev-figma-capture` — Figmaキャプチャ
- `dev-figma-script-install` — Figmaスクリプトインストール
- `figrefer` — Figma参照

#### フロントエンド / Web 開発
- `dev-cloudflare-pages-ci-setup` — Cloudflare Pages CI設定
- `dev-electron` — Electron開発
- `dev-npm-package` — npmパッケージ開発
- `dev-npxify` — npx化
- `dev-package-json` — package.json管理
- `dev-repo-bigbang-move` — リポジトリ大移動
- `dev-seo-ogp` — SEO/OGP設定
- `dev-tweak-serve-package-json` — serveパッケージ調整
- `netlify-cli` — Netlify CLI操作
- `lighthouse-audit` — Lighthouse監査

#### ドキュメント / テキスト系
- `format-md` — Markdownフォーマット
- `mermaid-creator` — Mermaidダイアグラム作成
- `pdf-combine` — PDF結合
- `xlsx` — Excel操作
- `youtube-text-fetch` — YouTube字幕取得
- `w-update-wording-rule` — 文言ルール更新

#### zpaper / zudocg / zudoesa 系（特定ブログ/記事ワークフロー）
元リポジトリ作者固有のワークフローと思われる。

- `zpaper-apply-voice`
- `zpaper-articlify`
- `zudocg-apply-voice`
- `zudocg-articlify`
- `zudoesa-apply-voice`
- `zudoesa-articlify`
- `zeno-tweak`

#### このリポジトリ自体の管理
- `claude-resources-share` — このリポジトリ共有
- `claudemd-refactor` — CLAUDE.mdリファクタ
- `custom-command-creator` — カスタムコマンド作成
- `globalsync` — グローバル設定同期
- `l-ccresdoc-build` — ドキュメントビルド
- `skill-creator` — スキル作成
- `skill-tweaker` — スキル調整
- `subagent-creator` — サブエージェント作成
- `subagent-tweaker` — サブエージェント調整
- `sync-force-to` — 強制同期
- `sync-to` — 同期

#### 汎用ユーティリティ
- `big-plan` — 大規模計画立案
- `lazy-dev` — 手抜き開発補助
- `log-conversation` — 会話ログ保存
- `logrefer` — ログ参照
- `purge-private-info` — プライベート情報削除
- `refer-another-project` — 別プロジェクト参照
- `retro-notes` — 振り返りメモ
- `ss` — スクリーンショット
- `x` — X(Twitter)投稿
- `x-wt-teams` — Xワークツリーチーム管理

---

## エージェント棚卸し

- `code-reviewer.md` — コードレビュー
- `frontend-developer.md` — フロントエンド開発
- `frontend-worktree-child.md` — フロントエンドworktree子エージェント
- `markdown-generator.md` — Markdownドキュメント生成
- `markdown-writer.md` — Markdownライター
- `repo-syncer.md` — リポジトリ同期
- `researcher.md` — 調査
- `text-fixer.md` — テキスト修正
- `zpaper-writer.md` — zpaper用（作者固有）
- `zudocg-writer.md` — zudocg用（作者固有）
- `zudoesa-writer.md` — zudoesa用（作者固有）

---

## コマンド棚卸し

- `bambulab-height-calculation.md` — Bambu Lab 3Dプリンタ高さ計算
- `cpwd.md` — カレントディレクトリパスコピー
- `dump-fullpath.md` — フルパスダンプ
- `next-todo.md` — 次のTODO確認
- `start-wt-dev.md` — worktree開発開始
- `vpr.md` — ビジュアルPRレビュー

---

## フック棚卸し

- `allow-worktree-teammate-edits.sh` — worktreeの仲間編集許可
- `block-sendmessage-backticks.sh` — バッククォートブロック
- `notify-ifttt.sh` — IFTTT通知（Stop時）

---

## 削除済み

### 電子工作系（2026-05-07）
`ascii-circuit-diagram-creator`, `easyeda2kicad`, `jlcpcb-bom-generate-from-kicad`, `jlcpcb-component-finder`, `jlcpcb-component-finder-update-db`, `kicad-sch-tweak`, `kicad-svg-fix`, `schemdraw-circuit-generator`

### zpaper / zudocg / zudoesa 系（2026-05-07）
`zpaper-apply-voice`, `zpaper-articlify`, `zudocg-apply-voice`, `zudocg-articlify`, `zudoesa-apply-voice`, `zudoesa-articlify`, `zeno-tweak`
agents: `zpaper-writer.md`, `zudocg-writer.md`, `zudoesa-writer.md`

### Copilot CLI / Codex 系（2026-05-07）
`codex-2nd`, `codex-research`, `codex-review`, `codex-translator`, `codex-writer`, `gco`, `gco-2nd`, `gco-research`, `gco-review`, `gco-stack-trace-read`, `gco-translate`, `gcoc`, `gcoc-2nd`, `gcoc-research`, `gcoc-review`, `copilot-instructions`

### IFTTT系（2026-05-07）
`dev-ci-ifttt-notify`, `dev-gha-ifttt-notify`, hooks: `notify-ifttt.sh`

### Figma系（2026-05-07）
`dev-figma-capture`, `dev-figma-script-install`, `figrefer`

### Bambu Lab系（2026-05-07）
commands: `bambulab-height-calculation.md`

### このリポジトリ管理・同期系（2026-05-07）
`claude-resources-share`, `globalsync`, `sync-to`, `sync-force-to`, `l-ccresdoc-build`

### X（Twitter）系（2026-05-07）
`x`, `x-as-pr`, `x-wt-teams`

### Electron / npmパッケージ開発系（2026-05-07）
`dev-electron`, `dev-npm-package`, `dev-npxify`

### 特定サービス依存系（2026-05-07）
`netlify-cli`, `dev-cloudflare-pages-ci-setup`, `lighthouse-audit`, `dev-actions-self-runner`, `dev-gh-actions-doc-auto-merge`, `dev-repo-bigbang-move`, `dev-seo-ogp`, `dev-tweak-serve-package-json`, `dev-package-json`, `dev-create-b4push-script`

### プッシュ制御系（2026-05-07）
`b4push-wisdom`, `commits-auto`, `commits-forbid`, `push-auto`, `push-forbid`

### 使い方不明確系（2026-05-07）
`logrefer`, `refer-another-project`, `w-update-wording-rule`, `watch-ci`, `ss`

### PR系（一部）（2026-05-07）
`pr-complete`, `pr-make-suggestion-edit`, `pr-make-suggestion-to-pr`, `pr-recreate`, `pr-split`（`pr` と `pr-revise` は残す）

### 特定用途系（2026-05-07）
`pdf-combine`, `xlsx`, `youtube-text-fetch`

### 使い道が薄い系（2026-05-07）
`big-plan`, `lazy-dev`, `log-conversation`, `retro-notes`, `dependabot-resolve`, `gh-actions-wisdom`, `git-filtered-merge`, `git-prune-branches-both`, `git-prune-branches-local`, `git-prune-branches-remote`

### エージェント（2026-05-07）
agents: `markdown-generator.md`, `markdown-writer.md`

---

## 残す予定のもの

（記録予定）
