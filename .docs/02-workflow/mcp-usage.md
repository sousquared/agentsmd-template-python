# MCP使用ガイド（MCP Usage Guide）

## 作業方針

作業の際に、Codex MCPやContext7 MCP、Serena MCP、kiri MCPを使って、調査させて、まとめて報告する。

エージェントはMCPの調査結果をレビューして、あらゆる観点から評価して点数と理由をつけてMCPに返答する。点数が80点以上になったら、PRやIssueを作成or修正する。

Codex MCPが動かないか1分以上応答がない場合はCodexコマンドを使用する。それも1分以上応答がない場合は他のMCPで暫定的に採点して、後からリファクタする。

## Context7 MCP Server

実装前にライブラリの最新ドキュメントを取得:

1. `resolve-library-id`: ライブラリIDを取得
2. `get-library-docs`: 最新ドキュメントを取得

**例**: `/google-cloud/google-cloud-texttospeech`, `/googleapis/google-generativeai`

## Serena MCP Server

コードベース操作に使用:

**検索**:
- `find_symbol`: シンボル検索（クラス、関数等）
- `find_referencing_symbols`: 参照箇所を検索
- `search_for_pattern`: パターン検索

**編集**:
- `replace_symbol_body`: シンボルの本体を置換
- `insert_after_symbol`: シンボルの後に挿入
- `insert_before_symbol`: シンボルの前に挿入

**活用例**:
- リファクタリング時のシンボル検索
- 大規模な変更時のパターン検索
- 型安全な編集操作

## Codex MCP Server

AI支援によるコーディングタスクの実行に使用:

**主要機能**:
- `codex`: プロンプトを受け取り、コーディングタスク、質問、分析を実行
  - 基本形式: `codex [PROMPT]` - インタラクティブまたは非インタラクティブモード
  - モデル選択: `-m`オプション（例: `gpt-5-codex`, `gpt-4`, `gpt-3.5-turbo`）
  - サンドボックスモード: `-s`オプション（`read-only`, `workspace-write`, `danger-full-access`）
  - 承認ポリシー: `-a`オプション（`untrusted`, `on-failure`, `on-request`, `never`）
  - 作業ディレクトリ指定: `-C <DIR>`オプション
  - 画像添付: `-i <FILE>`オプション
  - フルオートモード: `--full-auto`フラグ（低摩擦サンドボックス自動実行）

**サブコマンド**:
- `exec`: 非インタラクティブモードで実行（エイリアス: `e`）
- `resume`: 以前のインタラクティブセッションを再開
- `apply`: Codex Agentが生成した最新のdiffを適用（エイリアス: `a`）
- `sandbox`: Codex提供のサンドボックス内でコマンドを実行（エイリアス: `debug`）

**補助機能**:
- `listSessions`: アクティブな会話セッション一覧の取得
- `help`: Codex CLIのヘルプ情報取得
- `ping`: MCPサーバー接続テスト

**活用例**:
- 複雑なコーディングタスクのAI支援
- コードレビューと分析
- セッションベースの段階的な実装
- 画像を含む技術的な質問（スクリーンショット、図表等）

## kiri MCP Server

コードベースの理解と分析に特化したツール

**主要機能**:
- `context_bundle`: コードコンテキストの抽出（🎯 PRIMARY TOOL）
  - タスクや質問に関連するコードを自動的に発見・ランク付け
  - フレーズ認識（kebab-case、snake_caseを単一フレーズとして認識）
  - パス基準のスコアリング（キーワードがパスに含まれるファイルを優先）
  - ファイルタイプの優先順位付け（実装ファイル優先）
  - 依存関係分析（import関係を考慮）
  - セマンティック類似度によるランキング
  - `compact: true`でトークン消費を95%削減可能

- `files_search`: 特定のキーワードや識別子でファイルを検索
  - 関数名、クラス名、エラーメッセージなどの正確な検索
  - 言語（`lang`）、拡張子（`ext`）、パスプレフィックス（`path_prefix`）でフィルタリング可能

- `snippets_get`: 特定ファイルパスからコードスニペットを取得
  - シンボル境界（関数、クラス、メソッド）を使用したインテリジェントな抽出
  - ファイル全体を読み込まずに効率的にコード取得

- `semantic_rerank`: セマンティック類似度による再ランク付け
  - ファイル候補リストを意味的関連性で並び替え
  - `files_search`の結果を洗練させる際に有効

- `deps_closure`: 依存関係グラフのトラバース
  - 影響分析（ファイル変更時の影響範囲把握）
  - インポートチェーンの追跡
  - 循環依存の発見
  - リファクタリング計画に必須

**使い分け**:
- **広範な探索**: `context_bundle`を使用（例: "画像生成、Vertex AI Gemini"）
- **正確な検索**: `files_search`を使用（例: `generate_image`関数を探す）
- **影響分析**: `deps_closure`を使用（例: `image_generation.py`を変更したら何が影響を受けるか）
- **効率的な読み込み**: `snippets_get`を使用（例: 特定ファイルの特定関数のみ取得）

**活用例**:
- コードベースの探索と理解（新規参加時、不慣れなモジュール）
- リファクタリング前の影響範囲調査
- 特定機能の実装箇所の特定
- 依存関係の可視化とモジュール境界の理解
- バグ修正時の関連コード発見

**ベストプラクティス**:
1. **context_bundle**を最初のステップとして使用（具体的なキーワードを含む明確な目標を指定）
2. `compact: true`で候補リストを取得後、必要なファイルのみ`snippets_get`で取得（トークン効率化）
3. 抽象的な動詞（"understand", "explore"）ではなく、具体的なキーワードを使用
4. ファイル編集時は`artifacts.editing_path`を指定してコンテキストを強化

