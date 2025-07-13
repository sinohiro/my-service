# my-service

ネットワークを学ぶためにポートフォリオページとクラウドサービスが動くサーバを構築します

## 概要

このプロジェクトは、以下の機能を提供するWebサービスです：

- **ポートフォリオページ**: 個人のプロフィールとスキルを紹介するWebページ
- **Nextcloud**: 個人用クラウドストレージサービス
- **リバースプロキシ**: Nginxを使用してルーティングを管理

## 機能

### ✨ 主要機能

- 📄 **ポートフォリオページ** (`/`)
  - レスポンシブデザイン
  - パーティクルアニメーション
  - プロフィール情報の表示

- ☁️ **Nextcloud** (`/nextcloud`)
  - ファイルアップロード・ダウンロード
  - ファイル共有
  - カレンダー・連絡先管理
  - 各種Nextcloudアプリケーション

- 🌐 **リバースプロキシ**
  - 複数サービスへの統一されたアクセス
  - 負荷分散

## ディレクトリ構造

```
my-service/
├── README.md                 # プロジェクト説明
├── docker-compose.yml        # Docker Compose設定
├── Dockerfile                # Nextcloud用カスタムDockerfile
├── default.conf              # Nginx設定ファイル
├── .env                      # 環境変数設定
├── exapmle.env               # 環境変数設定
├── webContens/               # ポートフォリオページ
│   ├── index.html            # メインHTML
│   ├── style.css             # スタイルシート
│   ├── script.js             # JavaScript
│   └── preview.png           # プレビュー画像
├── cloud-app/                # Nextcloudデータ (Docker実行時に作成)
└── cloud-db/                 # MariaDBデータ (Docker実行時に作成)

```

## セットアップ手順

### 1. 環境変数の設定

`.env`ファイルを確認・編集してデータベースの設定を行います：

```bash
# データベース設定
CLOUD_DATABASE=cloud-db
CLOUD_DB_USER=nextcloud_user
CLOUD_DB_PASSWORD=your_secure_password
CLOUD_DB_ROOT_PASSWORD=your_root_password
```

### 2. サービスの起動

```bash
# 必要なディレクトリを作成
mkdir cloud-app
mkdir cloud-db

# サービスをバックグラウンドで起動
docker compose build
docker compose up -d
```

### 3. アクセス

サービスが起動したら、以下のURLにアクセスできます：

- **ポートフォリオページ**: http://localhost/
- **Nextcloud**: http://localhost/nextcloud

## 使い方

### Nextcloudの初期設定

1. `http://localhost/nextcloud` にアクセス
2. 管理者アカウントを作成
3. データベース情報を入力：
   - データベースのユーザ名: `.env`の`CLOUD_DB_USER`
   - データベースのパスワード: `.env`の`CLOUD_DB_PASSWORD`
   - データベース名: `.env`の`CLOUD_DATABASE`
   - データベースのホスト: `cloud-db`

### サービスの管理

```bash
# サービスの状態確認
docker compose ps

# ログの確認
docker compose logs

# サービスの停止
docker compose down

# データを含めて完全削除
docker compose down -v
```

## 技術スタック

- **Frontend**: HTML5, CSS3, JavaScript, Particles.js
- **Backend**: Nextcloud (PHP)
- **Database**: MariaDB
- **Cache**: Redis
- **Web Server**: Nginx
- **Container**: Docker, Docker Compose

## ポート構成

| サービス | 内部ポート | 外部ポート |
|----------|------------|------------|
| Nginx Proxy | 80 | 80 |
| Nextcloud | 80 | - |
| MariaDB | 3306 | - |
| Redis | 6379 | 6379 |

## トラブルシューティング

### よくある問題

1. **ポート80が使用中**
   ```bash
   # 使用中のプロセスを確認
   sudo lsof -i :80
   ```

2. **データベース接続エラー**
   - `.env`ファイルの設定を確認
   - コンテナが正常に起動しているか確認

3. **ファイルアップロードサイズ制限**
   - Nextcloudの設定で最大アップロードサイズを調整

## ライセンス

このプロジェクトは個人学習用途で作成されています。
