# GitHub Actions SSH Deploy Sample

GitHub Actions を利用した SSH デプロイのサンプル

## 概要

- 2 種類のデプロイワークフローを同梱
- 各プロジェクトの環境に合わせ、ワークフロー内のディレクトリパスは適宜変更が必要

## 共通設定 (Secrets)

`Settings > Secrets and variables > Actions` に以下を登録

- `SSH_HOST`: サーバーのホスト名 or IP
- `SSH_USER`: SSH ユーザー名
- `SSH_KEY`: SSH 秘密鍵

---

## ワークフロー 1: Git Pull (`deploy-gitpull.yml`)

サーバー上で `git pull` を実行するシンプルなデプロイ

### 手順

1.  **サーバー側**:
    - デプロイ用の SSH キーペアを作成 (`ssh-keygen`)
    - デプロイ先にリポジトリを `git clone`
2.  **GitHub リポジトリ側**:
    - `Secrets` にサーバーの秘密鍵 (`SSH_KEY`) 等を登録
    - プライベートリポジトリの場合は `Settings > Deploy keys` にサーバーの公開鍵を登録すると便利

### 注意点

- ワークフロー内のデプロイ先ディレクトリ (`cd /home/deploy/gitpull`) は要変更

---

## ワークフロー 2: Build & Deploy (`deploy-build.yml`)

`dist` ディレクトリを `zip` 化してサーバーに転送し、シンボリックリンクを切り替えるアトミックなデプロイ

### 手順

1.  **サーバー側**:
    - デプロイ用の SSH キーペアを作成 (`ssh-keygen`)
    - リリース用のディレクトリを作成 (`mkdir -p /home/deploy/build/releases`)
2.  **GitHub リポジトリ側**:
    - `Secrets` にサーバーへログイン可能な秘密鍵 (`SSH_KEY`) 等を登録

### 注意点

- ワークフロー内の各種ディレクトリパス (`RELEASE_DIR`, `APP_DIR`) は要変更

---

## ライセンス

MIT