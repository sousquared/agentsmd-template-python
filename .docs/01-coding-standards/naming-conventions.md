# 命名規則（Naming Conventions）

## インデックスと行番号の区別

**インデックス（0始まり）**: `*_index`, `*_idx` を使用
- 例: `image_index`, `array_index`

**行番号（1始まり、ファイルの実際の行番号）**: `*_line_number`, `*_line_no` を使用
- 例: `source_line_number`

**カウント（個数）**: `*_count`, `*_size` を使用
- 例: `row_count`, `header_line_count`

### 原則

- コード内では常にインデックス（0始まり）で扱う
- エラーメッセージやログ出力時のみ行番号（1始まり）に変換
- 変換は専用のヘルパーメソッドで行う

**参考**: Clean Code (Robert C. Martin) - "Use Intention-Revealing Names"

## メソッド名の明確性

**動詞 + 目的語の形式**を使用し、メソッド名だけで処理内容が明確に分かるようにする。

### 良い例

- ✅ `generate_image_prompt`: 画像プロンプトを生成
- ✅ `translate_text_to_english`: テキストを英語に翻訳
- ✅ `add_silence_to_audio`: 音声に無音を追加

### 悪い例

- ❌ `build_prompt`: promptとは何か不明確
- ❌ `translate_text`: 何から何に翻訳するのか不明確
- ❌ `process_audio`: どのような処理をするのか不明確

### 原則

- メソッド名だけで何をしているか分かる
- 動詞は処理の種類を明確に表現（`build`ではなく`create`, `extract`, `convert`など）
- 目的語は具体的に（`text`ではなく`korean_text`）

**参考**: Clean Code (Robert C. Martin) - "Use Intention-Revealing Names"

## Python命名規約

### 型アノテーション

- 型アノテーションを必須とする

### 引数指定

- 引数は明示的に指定（例: `func(x=x, y=y)`）

### 命名規約

- 役割を明確にする
- ドメインを明示する

### コメント

- コードから明らかに分かる無駄なコメントは書かない

