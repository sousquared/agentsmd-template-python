# 型アノテーション（Type Annotations）

## 基本方針

**すべての関数・メソッドに型アノテーションを付与する**:

- 関数・メソッドの引数と戻り値には必ず型アノテーションを付与
- 変数にも可能な限り型アノテーションを付与（特に複雑な型や推論が困難な場合）
- 型チェックツール（pyright）を活用して型の整合性を保つ

### 原則

- 型アノテーションは可読性と保守性を向上させる
- 型アノテーションはドキュメントとしても機能する
- 型チェックエラーは修正する（`# type: ignore`は最小限に）

**参考**: PEP 484 - Type Hints, PEP 526 - Syntax for Variable Annotations

## 関数・メソッドの型アノテーション

### 必須項目

すべての関数・メソッドで以下を明示する：

```python
def calculate_total(items: list[str], discount: float = 0.0) -> float:
    """商品リストの合計金額を計算する"""
    total = sum(item.price for item in items)
    return total * (1 - discount)
```

### 戻り値がない場合

`None`を返す場合は`-> None`を明示：

```python
def log_message(message: str) -> None:
    """メッセージをログに出力する"""
    print(f"[LOG] {message}")
```

### 戻り値がない関数

Pythonの関数は常に値を返すため、明示的に`None`を返すか`-> None`を指定：

```python
def process_data(data: dict[str, Any]) -> None:
    """データを処理する"""
    # 処理ロジック
    return  # または return None
```

## 変数の型アノテーション

### 推奨ケース

以下の場合は変数にも型アノテーションを付与：

- 複雑な型（ジェネリクス、Union、Optionalなど）
- 型推論が困難な場合
- コードの可読性が向上する場合

```python
# 複雑な型の場合
user_data: dict[str, list[dict[str, Any]]] = fetch_user_data()

# Optional型の場合
result: Optional[str] = find_value(key)

# 型推論が困難な場合
config: Config = load_config()
```

### 省略可能なケース

以下の場合は型アノテーションを省略可能：

- 型が自明な場合（`x: int = 0`）
- 型推論が容易な場合（`name: str = "test"`）

```python
# 型アノテーション省略可能
count = 0
name = "test"
items = []
```

## 特殊な型の使用

### Optional型

値が`None`になる可能性がある場合は`Optional`を使用：

```python
from typing import Optional

def find_user(user_id: int) -> Optional[User]:
    """ユーザーを検索する（見つからない場合はNone）"""
    # 実装
```

Python 3.10以降では`X | None`も使用可能：

```python
def find_user(user_id: int) -> User | None:
    """ユーザーを検索する（見つからない場合はNone）"""
    # 実装
```

### Union型

複数の型を受け入れる場合は`Union`を使用：

```python
from typing import Union

def process_value(value: Union[int, str]) -> str:
    """整数または文字列を処理する"""
    return str(value)
```

Python 3.10以降では`X | Y`も使用可能：

```python
def process_value(value: int | str) -> str:
    """整数または文字列を処理する"""
    return str(value)
```

### Literal型

特定の値のみを受け入れる場合は`Literal`を使用：

```python
from typing import Literal

def set_status(status: Literal["active", "inactive", "pending"]) -> None:
    """ステータスを設定する"""
    # 実装
```

### Any型

`Any`は型チェックを無効化するため、最小限に使用：

```python
from typing import Any

# 避けるべき例
def process_data(data: Any) -> Any:  # ❌ 型チェックの意味がない
    pass

# やむを得ない場合のみ
def handle_unknown_type(value: Any) -> str:  # ✅ 外部APIなどで型が不明な場合
    return str(value)
```

## ジェネリクス

### 基本的なジェネリクス

`list`、`dict`などの組み込み型は直接型パラメータを指定：

```python
def get_items() -> list[str]:
    """文字列のリストを取得する"""
    return ["item1", "item2"]

def get_user_map() -> dict[str, User]:
    """ユーザーIDをキーとするユーザーマップを取得する"""
    return {"user1": User(...)}
```

### カスタムジェネリクス

型パラメータを持つクラスや関数を定義する場合：

```python
from typing import TypeVar, Generic

T = TypeVar("T")

class Container(Generic[T]):
    def __init__(self, value: T) -> None:
        self.value = value
    
    def get(self) -> T:
        return self.value
```

## 型エイリアスとの連携

複雑な型は型エイリアスとして定義（詳細は[型エイリアス](./type-aliases.md)を参照）：

```python
from typing import TypeAlias

# 複雑な型を型エイリアス化
type UserData = dict[str, dict[str, list[str]]]

def process_user_data(data: UserData) -> None:
    """ユーザーデータを処理する"""
    # 実装
```

## 型チェックツールとの連携

### pyrightの設定

プロジェクトでは`pyright`を使用して型チェックを行う：

- `typeCheckingMode = "basic"`（pyproject.tomlで設定）
- 型チェックエラーは修正する
- `# type: ignore`コメントは最小限に使用

### 型チェックの実行

```bash
# 型チェックを実行
pyright .

# または開発依存関係をインストール後
pytest --type-check-mode=basic
```

## 型アノテーションが不要なケース

以下の場合は型アノテーションを省略可能：

- プライベートメソッド（`_`で始まる）で型が自明な場合
- テストコードで型が自明な場合
- プロトタイプや実験的なコード

ただし、可能な限り型アノテーションを付与することを推奨。

## ベストプラクティス

### 1. 一貫性を保つ

同じパターンの型アノテーションは一貫して使用：

```python
# ✅ 一貫した書き方
def process_items(items: list[str]) -> list[str]:
    pass

def process_users(users: list[User]) -> list[User]:
    pass
```

### 2. 型アノテーションとdocstringの連携

型アノテーションとdocstringは補完関係：

```python
def calculate_total(items: list[Item], discount: float = 0.0) -> float:
    """商品リストの合計金額を計算する
    
    Args:
        items: 商品のリスト
        discount: 割引率（0.0-1.0）
    
    Returns:
        合計金額（割引適用後）
    """
    # 実装
```

### 3. 型アノテーションの更新

型アノテーションは実装と同期させる：

- 関数のシグネチャを変更したら型アノテーションも更新
- 型チェックエラーが出たら修正する
- リファクタリング時も型アノテーションを維持

**参考**: 
- PEP 484 - Type Hints
- PEP 526 - Syntax for Variable Annotations
- PEP 585 - Type Hinting Generics In Standard Collections
- PEP 604 - Allow writing union types as X | Y
- PEP 695 - Type Parameter Syntax
