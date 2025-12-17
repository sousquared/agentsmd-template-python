# Gitワークフロー（Git Workflow）

## ブランチ戦略

**原則**: mainは常に安定（テスト通過 + 動作確認済み）

**⚠️ 重要**: mainブランチへの直接コミットは**禁止**されています。すべての変更は作業ブランチを作成し、PRを通じてマージしてください。

**階層構造**:

```
main (安定版)
  ↑
epic/feature-name (統合・テスト用)
  ↑
  ├─ feature/subtask-1 (個別タスク)
  ├─ feature/subtask-2 (個別タスク)
  └─ feature/subtask-3 (個別タスク)
```

**運用フロー**:

```bash
# 1. Epicブランチ作成
git checkout -b epic/feature-name

# 2. 個別タスクブランチ作成（epicから分岐）
git checkout -b feature/subtask-1

# 3. 実装 → epicにマージ
gh pr create --base epic/feature-name \
  --title "feat: subtask-1"
# PR本文はテンプレートに従って充実した内容を記載
# - Part of #<Epic Issue番号>
# - 概要、背景・目的、変更内容、影響範囲、テスト等

# → レビュー → マージ（epicへ）

# 4. 次のタスク（epicから分岐）
git checkout epic/feature-name
git checkout -b feature/subtask-2
# → ブランチ作成 → PR作成 → マージを繰り返す

# 5. Epic完了後、統合テスト
git checkout epic/feature-name
pytest  # 全テスト通過を確認
# 動作確認も実施

# 6. Mainにマージ
gh pr create --base main \
  --title "Epic: feature-name" \
  --body "$(cat <<'EOF'
Closes #<Epic Issue番号>

## Summary
Epic完了：すべてのサブタスクが統合され、テストが通過しました。

## Testing
- [ ] 全テスト通過
- [ ] 動作確認完了
EOF
)"
# → レビュー → マージ（mainへ）
```

**メリット**:

- mainが常に安定
- epicブランチで統合テスト
- 個別タスクごとにレビュー

**注意点**:

- テストが通らないコードはepicにもマージしない
- epicが長生きしすぎないよう、適度な粒度に分割

## 並行作業戦略（git worktree）

**複数のEpicやタスクを並行開発する場合、git worktreeを使用して独立した作業ディレクトリを作成します。**

**課題**: 別ウィンドウで異なるブランチを作業すると、ブランチ切り替えで混在する

**解決策**: git worktreeでブランチごとに独立したディレクトリを作成

**セットアップ手順**:

```bash
# 1. メインディレクトリでブランチA作成
cd /path/to/project
git checkout -b branch-a

# 2. ブランチB用のworktreeを別ディレクトリに作成
git worktree add ../project-branch-b branch-b

# 3. 各ディレクトリで独立して作業
# - ウィンドウ1: /path/to/project (branch-a)
# - ウィンドウ2: /path/to/project-branch-b (branch-b)

# 4. 各ディレクトリで実装
cd ../project-branch-b
# → 実装 → コミット → Push
```

**命名規則**:

- メインディレクトリ: `{project-name}/`（最初のブランチ用）
- worktree: `{project-name}-{branch-suffix}/`（追加ブランチ用）

**例（Epic並行開発）**:

```
project-name/          # Epic #1
project-name-feature/   # Epic #2
```

**メリット**:

- ブランチの混在を完全に防止
- 複数のブランチを同時に作業可能
- gitの状態が完全に独立
- ウィンドウごとに異なる作業に集中できる

**管理コマンド**:

```bash
git worktree list              # worktree一覧表示
git worktree remove path       # worktree削除
git worktree prune             # 不要なworktree情報をクリーンアップ
```

**注意点**:

- 各worktreeは独立した作業ツリーだが、gitリポジトリは共有
- 同じブランチを複数のworktreeでチェックアウトできない
- worktree削除時は必ず`git worktree remove`を使用

