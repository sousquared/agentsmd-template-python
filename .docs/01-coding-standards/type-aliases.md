# 型エイリアス（Type Aliases）

## 型エイリアスの活用

**意味のある型エイリアスを定義する**:

複雑な型は型エイリアスとして定義し、型エイリアス名は意味を明確に表現する。型エイリアスはドキュメントとしても機能する。

### 例

```python
type ImagePath = str
type AudioPath = str
type TextContent = str
type PromptText = str
```

### 原則

- 複雑な型（3階層以上）は型エイリアス化
- 型エイリアス名は意味を明確に表現
- ドメインの概念を反映した名前を使用

**参考**: Python Type Hints - "Type Aliases"

