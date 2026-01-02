# bds-manager

Minecraft Bedrock Dedicated Server（BDS）を  
**Docker / docker-compose** で管理するための mono-repo 構成プロジェクト。

BDS 本体には `itzg/minecraft-bedrock-server` イメージを使用し、  
サーバー制御・管理用の backend / frontend を同一リポジトリ内で管理する。

小規模・個人開発を前提に、  
**「迷わず動かせる」「考える時間を減らす」** を最優先にした構成。

---

## 構成概要

- BDS（Minecraft Bedrock Server）
- 管理用 Backend（BDS 制御・メタ情報管理）
- 管理用 Frontend（操作 UI）
- Database（メタ情報・ログ管理）

すべてを **1 リポジトリ・1 docker-compose** で扱う。

```
bds-manager
├─ docker-compose.yml
├─ .env
├─ init-sql/
│  └─ 01-init.sql
├─ backend/
│  └─ (Spring Boot)
├─ frontend/
│  └─ (Next.js / React)
├─ data/
│  └─ （worlds etc）
└─ README.md
```

---

## 使用技術

- Docker / docker-compose
- Minecraft Bedrock Dedicated Server  
  - `itzg/minecraft-bedrock-server`
- Backend  
  - Java / Spring Boot（想定）
- Frontend  
  - Next.js / React（想定）
- Database  
  - PostgreSQL

---

## 前提条件

- Docker
- docker-compose
- Git

---

## セットアップ手順

### リポジトリのクローン

```bash
git clone git@github.com:n1sp/bds-manager.git
cd bds-manager
```

---

### 環境変数の設定

`.env` を作成し、DB 接続情報を設定する。

```env
DB_USER=bds
DB_PASSWORD=your_password
DB_NAME=bds_meta
```

※ `.env` は Git 管理しないこと。

---

## 起動方法

```bash
docker-compose up -d --build
```

起動後の想定：

- BDS（Bedrock）：UDP `19133`
- Backend API：`http://localhost:8080`
- Frontend：`http://localhost:3000`

---

## 停止方法

```bash
docker-compose down
```

データも含めて削除する場合：

```bash
docker-compose down -v
```

---

## データ永続化について

- BDS ワールドデータ  
  - Docker volume: `bds-world`
- DB データ  
  - Docker volume: `db-data`

通常の `docker-compose down` では削除されない。

---

## 設計方針

- BDS 本体の管理は itzg イメージに委譲する
- backend / frontend は BDS の **制御・可視化・自動化** に集中させる
- 属人的な手作業を減らし、再現性のある運用を目指す
- 将来的な Linux サーバー移行を前提とした構成

---

## 今後の拡張予定

- BDS 起動 / 停止 API
- プレイヤー参加・退出ログの収集
- プレイ時間の算出
- ワールドバックアップの自動化
- WebSocket によるリアルタイム状態表示
- 外部サーバー（Linux）への移行

---

## 注意事項

- Bedrock は UDP 通信を使用するため、FW / ルーター設定に注意
- itzg イメージ固有の設定は公式 README を参照
- 公開運用時はセキュリティ対策を行うこと

---

## ライセンス

未定。  
依存する Docker イメージ・ライブラリのライセンスは各公式に従う。
