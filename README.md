# ETOFILE – デプロイ手順

## ファイル構成

```
etofile/
├── server.js      ← サーバー本体
├── index.html     ← フロントエンド（全部入り）
├── package.json
└── .gitignore
```

---

## 1. GitHubにリポジトリを作る

1. https://github.com/new を開く
2. Repository name: `etofile`
3. **Private** を選ぶ（無料）
4. "Create repository" をクリック

```bash
# プロジェクトフォルダで実行
cd etofileのフォルダ
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/あなたのユーザー名/etofile.git
git push -u origin main
```

---

## 2. Railwayにデプロイ

1. https://railway.app にアクセス（GitHubアカウントでログイン）
2. "New Project" → "Deploy from GitHub repo"
3. `etofile` リポジトリを選択
4. 自動でデプロイが始まる（2〜3分）

### 環境変数の設定（任意）

Railway の Variables タブで設定：

| Key | Value | 説明 |
|-----|-------|------|
| `PORT` | （自動設定） | Railwayが自動で入れてくれる |
| `UPLOAD_DIR` | `/tmp/etofile` | アップロード保存先（デフォルト） |

---

## 3. 独自ドメインの設定

### Railway側
1. プロジェクトの "Settings" → "Domains"
2. "Custom Domain" をクリック
3. `etofile.com`（または使いたいドメイン）を入力
4. 表示されるCNAMEレコードをコピー

### ドメインレジストラ側（お名前.com / Cloudflare など）
1. DNSレコードを追加：
   - **タイプ**: CNAME
   - **名前**: `@`（または `www`）
   - **値**: Railwayで表示されたCNAME（例: `xxx.up.railway.app`）
2. 反映まで最大48時間（だいたい数分〜数時間）

### HTTPSは自動
Railwayが自動でSSL証明書を発行してくれます。

---

## 4. 注意事項

### ファイルは再起動で消える
Railwayはエフェメラルなのでデプロイやクラッシュ時にアップロードファイルが消えます。
本番運用するなら以下を検討：

- **Cloudflare R2**（無料枠あり）
- **AWS S3**
- **Backblaze B2**（安い）

### メモリとストレージ
Railwayの無料プランはメモリ512MB・ストレージ1GB。
大量利用するなら有料プラン（$5/月〜）を検討。

---

## ローカル開発

```bash
npm install
npm run dev
# → http://localhost:3000
```
