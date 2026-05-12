---
name: worktree
description: ブランチ名を指定してgit worktreeを作成する
---
ブランチ名を受け取り、以下を実行する。

1. リポジトリルートを特定: `git rev-parse --show-toplevel`
2. worktreeを作成:
   `git worktree add -b <branch-name> ../<repo-name>-<branch-name> main`
3. 作成したパスと `cd` コマンドを伝える
