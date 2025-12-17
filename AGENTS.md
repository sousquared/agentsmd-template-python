Don't hold back. Give it your all!

# リポジトリガイドライン

## プロジェクト概要（Overview）

Pythonプロジェクトのテンプレート。AIエージェントと人間の両方が参照する知識ベースを含む。

**技術スタック**: 詳細は [pyproject.toml](pyproject.toml) を参照（プロジェクトに応じて設定）

**環境変数**: 詳細は [ビルド＆テスト](.docs/04-reference/build-test.md) を参照（プロジェクトに応じて設定）

**主要機能**: プロジェクトに応じて設定

**設計書**: コードの docstring（実装詳細）、PR description（設計判断）

## 作業方針

**⚠️ 重要**: main ブランチへの直接コミットは**禁止**されています。すべての変更は作業ブランチを作成し、PR を通じてマージしてください。

作業をなるべく小さな単位に分けてレビューしやすること、必要ならば全体の epic issue による管理や、小さい単位でブランチ&PR を管理するようにして（sub issue は必ずしも作る必要はない）。

作業の際に、Codex MCP や Context7 MCP, Serena MCP, kiri MCP を使って、調査させて、まとめて報告して。あなたは MCP の調査結果をレビューして、あらゆる観点から評価して点数と理由をつけて MCP に返答して。点数が 80 点以上になったら、PR や Issue を作成 or 修正して。

Codex MCP が動かないか 1 分以上応答がない場合は Codex コマンドを使用する。それも 1 分以上応答がない場合は他の MCP で暫定的に採点して、後からリファクタする。

詳細は [MCP 使用ガイド](.docs/02-workflow/mcp-usage.md) を参照してください。

## 知識ベース（Knowledge Base）

このプロジェクトの詳細な知識は、`.docs/` ディレクトリにモジュール化して格納されています。エージェントと人間の両方が必要に応じて該当するファイルを参照してください。

### クイックナビゲーション

| カテゴリ                                                | 用途           | 主要ファイル                                                                                                                                                                                                                                                          |
| ------------------------------------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[エンジニアリング原則](.docs/00-core-principles.md)** | 常に参照       | 思考と成熟、シンプルな実装、安定した基盤、プロセスとコミュニケーション                                                                                                                                                                                                |
| **[コーディング規約](.docs/01-coding-standards/)**      | コーディング時 | [命名規則](.docs/01-coding-standards/naming-conventions.md)、[コード構造](.docs/01-coding-standards/code-structure.md)、[Docstring ガイド](.docs/01-coding-standards/docstring-guide.md)                                                                              |
| **[ワークフロー・プロセス](.docs/02-workflow/)**        | 開発プロセス時 | [MCP 使用](.docs/02-workflow/mcp-usage.md)、[PR 作成](.docs/02-workflow/pr-workflow.md)、[Git ワークフロー](.docs/02-workflow/git-workflow.md)、[TDD・DDD ワークフロー](.docs/02-workflow/tdd-ddd-workflow.md)、[設計記録](.docs/02-workflow/documentation-policy.md) |
| **[ドメイン固有](.docs/03-domain-specific/)**           | ドメイン作業時 | プロジェクト固有のドメイン知識を追加                                                                                                                                                                                                                                   |
| **[リファレンス](.docs/04-reference/)**                 | 必要時参照     | [セキュリティ](.docs/04-reference/security.md)、[ビルド＆テスト](.docs/04-reference/build-test.md)、[ライブラリガイド](.docs/04-reference/library-guides.md)、[チェックリスト](.docs/04-reference/checklists.md)                                                      |

### 想定ユースケース

1. **新規コード実装時**: `00-core-principles.md` → `02-workflow/tdd-ddd-workflow.md` → `01-coding-standards/` → `03-domain-specific/`（該当する場合）
2. **PR 作成時**: `02-workflow/pr-workflow.md` → `02-workflow/git-workflow.md` → `04-reference/checklists.md`（チェックリスト）
3. **MCP 使用時**: `02-workflow/mcp-usage.md` → `04-reference/checklists.md`（MCP 使用チェックリスト）
4. **セットアップ時**: `04-reference/build-test.md` → `04-reference/security.md`
5. **コードレビュー時**: `04-reference/checklists.md`（コードレビューチェックリスト）を参照

詳細は [知識ベース README](.docs/README.md) を参照してください。

## メンテナンスポリシー（Maintenance policy）

- 繰り返し指示された内容は`.docs/`ディレクトリの該当ファイルに反映を検討
- 冗長性を排除し、簡潔で密度の濃い文書を維持
- 設計詳細はコードと PR に記録、AGENTS.md はエントリーポイントとして最小限に保つ
- 知識ベースの構造は`.docs/README.md`を参照

