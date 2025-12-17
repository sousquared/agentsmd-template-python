# PR作成ワークフロー（PR Workflow）

## Epic Issue + PRによる管理

大きな機能はEpic Issueで管理し、個々のPRを紐付ける:

```bash
# 1. Epic Issue作成（機能全体の管理）
gh issue create \
  --title "Epic: 機能全体の名前" \
  --body "
## 実装Plan
- [ ] PR: サブタスク1
- [ ] PR: サブタスク2
- [ ] PR: サブタスク3

## 完了条件
- [ ] すべてのテストが通る
- [ ] README.md作成
"

# 2. ブランチ作成
git checkout -b feature/xxx

# 3. Draft PR作成（Epic Issueを参照、充実した本文）
gh pr create --draft \
  --title "feat: サブタスク1"
# PR本文はテンプレート（.github/pull_request_template.md）に従って記載
# - Part of #<Epic Issue番号>
# - 概要、背景・目的、変更内容、影響範囲、テスト等を記載

# 4. 実装

# 5. PR description更新（実装に応じて更新）

# 6. Ready for review
gh pr ready

# 7. マージ
```

## Epic Issueの役割

- 機能全体の進捗管理
- 依存関係の明確化
- 完了条件の定義

## PRの役割

- 個別タスクの実装
- TDDサイクル（[TDD・DDDワークフロー](./tdd-ddd-workflow.md)参照）
- レビュー単位

## サブタスク管理の方針

**基本方針**: 基本はPRのみで管理、必要に応じてSub Issueも作成可

Epic配下のサブタスクは、基本的にはPRのみで管理します。ただし、必要に応じてSub Issueを作成することも可能です。PR本文テンプレート（`.github/pull_request_template.md`）を使用して、充実した情報を記載します。

## PR bodyテンプレート（ブランチ別）

**ケース別のIssue参照とクローズ方針**:

| ケース | ベース ← ヘッド                         | 想定スコープ   | PR body冒頭      | Issueクローズ方針 |
| ------ | --------------------------------------- | -------------- | ---------------- | ----------------- |
| A      | `epic/feature-name ← feature/subtask-*` | サブタスク     | `Part of #<Epic>` | - （Issueなし）   |
| B      | `main ← epic/feature-name`              | Epic統合      | `Closes #<Epic>` | EpicをClose       |
| C      | `main ← feature/*`                      | 小規模／Hotfix | `Closes #<Issue>` | 単体でClose       |

> **重要**:
>
> - **基本的にSub Issueは作成しない**: Epic配下のサブタスクはPRのみで管理します。ただし、必要に応じてSub Issueを作成することも可能です。
> - **PR本文テンプレート**: `.github/pull_request_template.md`を使用して、充実した情報を記載します。
> - **GitHubのクローズ動作**: `Closes #123`は、PRがマージされた時点でIssueをクローズします。

**コマンド例**:

```bash
# ケースA: epicブランチへのサブタスクPR
gh pr create --base epic/feature-name \
  --head feature/subtask-1 \
  --title "feat: サブタスク1"
# PR本文はテンプレートに従って記載
# - Part of #100
# - 概要、背景・目的、変更内容、影響範囲、テスト等

# ケースB: mainへのEpic PR
gh pr create --base main \
  --head epic/feature-name \
  --title "Epic: feature-name" \
  --body "$(cat <<'EOF'
Closes #100

## Summary
Epic完了：すべてのサブタスクが統合され、テストが通過しました。

## Testing
- [x] 全テスト通過
- [x] 動作確認完了
EOF
)"

# ケースC: mainへの小規模PR
gh pr create --base main \
  --head feature/hotfix-123 \
  --title "fix: Hotfix for issue 123" \
  --body "$(cat <<'EOF'
Closes #123

## Summary
緊急修正：xxxの不具合を修正

## Testing
- [x] 再現確認
- [x] 修正確認
EOF
)"
```

**PR本文テンプレート**:

充実したPR本文テンプレートを使用することで、Sub Issue相当の情報をPRに集約します。テンプレートは`.github/pull_request_template.md`に配置されています。

