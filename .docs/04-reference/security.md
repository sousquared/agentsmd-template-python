# セキュリティ（Security）

## 認証情報の管理

- **環境変数**: `.env`で管理
- **.gitignore**: `.env`を除外、`.env.example`のみバージョン管理
- **機密情報**: APIキーや認証情報は環境変数で管理し、コードに直接記載しない

## 認証設定

プロジェクトに応じて設定してください。

### 例: Google Cloud認証

```bash
# Application Default Credentials (ADC)
gcloud auth application-default login

# クォータプロジェクト
gcloud auth application-default set-quota-project <PROJECT_ID>
```

### 例: APIキー認証

```bash
# 環境変数で管理
export API_KEY=your_api_key_here
```

## セキュリティベストプラクティス

- **最小権限の原則**: 必要最小限の権限のみを付与
- **認証情報のローテーション**: 定期的に認証情報を更新
- **依存関係の更新**: セキュリティパッチを適用
- **コードレビュー**: 認証情報の漏洩がないか確認

