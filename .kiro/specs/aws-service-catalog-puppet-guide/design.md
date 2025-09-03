# Design Document

## Overview

AWS Service Catalog Puppet初心者向けガイドは、日本語で書かれた包括的なドキュメントとして設計します。このガイドは、概念説明から実際の実装まで段階的に学習できる構造とし、実用的なmanifest.yamlの例を含めます。

## Architecture

### ドキュメント構造

```
README.md
├── 1. AWS Service Catalog Puppetとは
├── 2. 主要概念とアーキテクチャ
├── 3. セットアップと初期設定
├── 4. manifest.yamlの構造と設定
├── 5. マルチリージョン対応
├── 6. トラブルシューティング
└── 7. 参考資料

manifest.yaml (サンプル)
├── accounts設定
├── launches設定
├── imported-portfolios設定
└── spoke-local-portfolios設定
```

### 学習フロー設計

1. **概念理解** → **環境構築** → **設定ファイル作成** → **運用開始**
2. 各セクションは独立して読めるが、順序立てて学習することを推奨
3. 実践的な例を多用し、理論と実装のバランスを取る

## Components and Interfaces

### 1. 概念説明コンポーネント
- **目的**: AWS Service Catalog Puppetの基本概念を説明
- **内容**: 
  - Service Catalogとの関係性
  - ハブ・スポークモデルの説明
  - CI/CDパイプラインの概要
  - 利点と使用ケース

### 2. セットアップガイドコンポーネント
- **目的**: 実際の環境構築手順を提供
- **内容**:
  - 前提条件（AWS CLI、権限等）
  - CloudFormationテンプレートの使用方法
  - 初期設定手順
  - 検証方法

### 3. 設定ファイル解説コンポーネント
- **目的**: manifest.yamlの詳細な説明と例
- **内容**:
  - 各キーの詳細説明
  - 実用的なサンプル設定
  - ベストプラクティス
  - よくある設定パターン

### 4. マルチリージョン対応コンポーネント
- **目的**: 複数リージョンでの運用方法を説明
- **内容**:
  - リージョン設定方法
  - 注意点と制限事項
  - パフォーマンス考慮事項

## Data Models

### manifest.yamlの構造設計

```yaml
# アカウント定義
accounts:
  - account_id: "123456789012"  # ハブアカウント
    name: "hub-account"
    default_region: "ap-northeast-1"
    regions_enabled: ["ap-northeast-1", "us-east-1"]
    tags:
      - Key: "Environment"
        Value: "production"
  
  - account_id: "210987654321"  # スポークアカウント
    name: "spoke-account-dev"
    default_region: "ap-northeast-1"
    regions_enabled: ["ap-northeast-1"]
    tags:
      - Key: "Environment"
        Value: "development"

# 製品起動設定
launches:
  sample-product:
    portfolio: "sample-portfolio"
    product: "sample-product"
    version: "v1.0.0"
    parameters:
      InstanceType: "t3.micro"
    deploy_to:
      tags:
        - tag: "Environment"
          regions: ["ap-northeast-1"]

# インポートポートフォリオ
imported-portfolios:
  sample-portfolio:
    portfolio_id: "port-xxxxxxxxx"
    share_tag_options: true
    share_principals: true
    deploy_to:
      accounts:
        - "210987654321"

# スポークローカルポートフォリオ
spoke-local-portfolios:
  local-portfolio:
    portfolio_id: "port-yyyyyyyyy"
    deploy_to:
      accounts:
        - account_id: "210987654321"
          regions: ["ap-northeast-1"]
```

### ドキュメント構成要素

1. **セクションヘッダー**: 明確な階層構造
2. **コードブロック**: シンタックスハイライト付き
3. **注意事項ボックス**: 重要な情報の強調
4. **ステップバイステップ手順**: 番号付きリスト
5. **参考リンク**: 外部リソースへの適切な誘導

## Error Handling

### よくある問題と対処法

1. **権限エラー**
   - IAMロールの設定不備
   - クロスアカウントアクセスの問題
   - 解決方法の詳細説明

2. **設定ファイルエラー**
   - YAML構文エラー
   - 必須フィールドの欠如
   - バリデーション方法の説明

3. **デプロイメントエラー**
   - リージョン設定の問題
   - アカウントID の誤り
   - トラブルシューティング手順

## Testing Strategy

### ドキュメント品質保証

1. **内容検証**
   - 技術的正確性の確認
   - 手順の実行可能性テスト
   - リンクの有効性確認

2. **ユーザビリティテスト**
   - 初心者による手順実行
   - 理解しやすさの評価
   - フィードバック収集

3. **サンプルファイル検証**
   - manifest.yamlの構文チェック
   - 実際の環境での動作確認
   - エラーケースの検証

### 継続的改善

1. **定期的な更新**
   - AWS Service Catalog Puppetのバージョンアップ対応
   - 新機能の追加説明
   - ベストプラクティスの更新

2. **フィードバック対応**
   - ユーザーからの質問や要望
   - よくある問題の追加
   - 説明の改善