# チェックリスト・コマンド例（Checklists & Commands）

エージェント実行時に使用するチェックリストとよく使用するコマンド例です。

## PR作成チェックリスト

PR作成前に確認すべき項目：

- [ ] テストがすべて通過している（該当する場合）
- [ ] 型チェックが通過している（該当する場合）
- [ ] フォーマットが適用されている（該当する場合）
- [ ] 関連するIssueまたはEpic Issueを参照している
  - Issueをcloseする場合は、PR本文に`Closes #123`または`Fixes #123`を記入する
  - Epic Issueの一部の場合は、`Part of #123`を記入する（closeしない）
- [ ] PR本文に変更内容、背景、影響範囲を記載している
- [ ] レビューが必要な箇所にコメントを追加している

## コードレビューチェックリスト

コードレビュー時に確認すべき項目：

- [ ] 命名規則に従っている（[命名規則](../01-coding-standards/naming-conventions.md)参照）
- [ ] マジックナンバーが定数化されている（[コード構造](../01-coding-standards/code-structure.md)参照）
- [ ] 責務が適切に分離されている（[コード構造](../01-coding-standards/code-structure.md)参照）
- [ ] ネストが2段階までに抑えられている（[コード構造](../01-coding-standards/code-structure.md)参照）
- [ ] Docstringが適切に記載されている（[Docstringガイド](../01-coding-standards/docstring-guide.md)参照）
- [ ] 型エイリアスが適切に使用されている（[型エイリアス](../01-coding-standards/type-aliases.md)参照）

## MCP使用時のチェックリスト

MCPを使用する際の確認項目：

- [ ] 適切なMCPツールを選択している（[MCP使用ガイド](../02-workflow/mcp-usage.md)参照）
- [ ] 調査結果をレビューし、80点以上を確認している
- [ ] 調査結果に基づいて適切なアクションを取っている
- [ ] Codex MCPが応答しない場合のフォールバック手順を理解している

## コマンド例

詳細な実行手順は [ビルド＆テスト](./build-test.md) を参照してください。

### よく使用するコマンド

プロジェクトに応じて設定してください。

```bash
# 依存関係インストール（プロジェクトに応じて変更）
# uv sync
# pip install -r requirements.txt
# poetry install

# テスト実行（プロジェクトに応じて変更）
# pytest
# python -m pytest
# uv run pytest

# PR作成（Epic配下のサブタスク）
gh pr create --base epic/feature-name \
  --head feature/subtask-1 \
  --title "feat: サブタスク1"
```

詳細は各ワークフローファイルを参照してください。

