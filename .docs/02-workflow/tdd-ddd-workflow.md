# TDD・DDDワークフロー（TDD・DDD Workflow）

このプロジェクトでは、TDD（Test-Driven Development）とDDD（Domain-Driven Design）を組み合わせて開発します。

## TDDの基本サイクル

TDDは以下の3つのステップを繰り返します：

### 1. レッド（Red）

仕様に対して失敗する（エラーになる）テストコードを書く

- テストケース = 実行可能な仕様
- テストを書くことで、実装すべき振る舞いを明確にする
- テストが失敗することを確認する

### 2. グリーン（Green）

レッドのテストをもとに成功する（パスする）コードを書く

- テストが通る最小限の実装を行う
- 設計の良し悪しは一旦置いておき、まずは動作するコードを書く
- テストが成功することを確認する

### 3. リファクタリング（Refactoring）

できたものに対して余分なものをそぎ落とし、きれいに整える

- コードの重複を削除する
- 命名を改善する
- 構造を整理する
- テストは常にグリーンのまま維持する

## DDDとの組み合わせ

TDDとDDDを組み合わせる際のポイント：

1. **ドメインモデルから始める**: テストはドメインの概念を反映したものにする
2. **責務の分離**: テストを通じて、各クラスの責務を明確にする
3. **ユビキタス言語**: テストコードでもドメインの用語を使用する

## 実践例

```python
# 1. レッド: 失敗するテストを書く
def test_generate_image_prompt():
    text = "example"
    result = generate_image_prompt(text)
    assert result.startswith("An image of")

# 2. グリーン: テストが通る最小限の実装
def generate_image_prompt(text: str) -> str:
    return "An image of Example."

# 3. リファクタリング: コードを整理
def generate_image_prompt(text: str) -> str:
    translated_text = translate_to_english(text)
    return f"An image of {translated_text}."
```

## 注意点

- **テストを先に書く**: 実装コードより先にテストコードを書く
- **小さなステップ**: 一度に大きな変更をせず、小さなステップで進める
- **テストの維持**: リファクタリング時もテストが常にグリーンのまま維持する
- **設計の記録**: 設計判断はdocstringやPR descriptionに記録する（[設計記録の方針](./documentation-policy.md)参照）

## 関連ドキュメント

- [PR作成ワークフロー](./pr-workflow.md): PRの役割としてTDDサイクルを実施
- [設計記録の方針](./documentation-policy.md): テストが仕様書として機能する
- [コード構造](../01-coding-standards/code-structure.md): リファクタリング時のガイドライン

