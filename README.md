# 取扱メーカー 製品検索ツール — 公開手順書

---

## 📁 ファイル一覧

| ファイル | 説明 | GitHubにアップ |
|---|---|---|
| `index.html` | メインページ | ✅ する |
| `companies.js` | 会社リスト | ✅ する |
| `.github/workflows/deploy.yml` | 自動デプロイ設定 | ✅ する |
| `.gitignore` | config.js除外設定 | ✅ する |
| `config.js` | ⚠️ パスワード＆APIキー | ❌ しない（Secretsで管理）|

---

## 🚀 公開手順（初回のみ・約15分）

---

### STEP 1：GitHubにリポジトリを作成

1. [github.com](https://github.com) を開いてログイン
2. 右上の **「+」→「New repository」** をクリック
3. 以下のように入力：
   - **Repository name**: `maker-search`
   - **Public** を選択（無料プランはPublicのみGitHub Pages対応）
   - 「Add a README file」のチェックは **外す**
4. **「Create repository」** をクリック

---

### STEP 2：ファイルをアップロード

1. 作成したリポジトリのページで **「uploading an existing file」** をクリック
2. 以下のファイル・フォルダをすべてドラッグ＆ドロップ：
   - `index.html`
   - `companies.js`
   - `.gitignore`
   - `.github/` フォルダごと（`workflows/deploy.yml` が入っている）
3. ⚠️ **`config.js` はアップロードしない**
4. Commit message に「初回アップロード」と入力
5. **「Commit changes」** をクリック

---

### STEP 3：Secretsを設定（パスワードとAPIキーを登録）

1. リポジトリの **「Settings」タブ** をクリック
2. 左メニューの **「Secrets and variables」→「Actions」** をクリック
3. **「New repository secret」** をクリックして以下を2つ登録：

   **1つ目：社内パスワード**
   - Name: `SITE_PASSWORD`
   - Secret: `社内で決めたパスワード`（例：`makers2024`）

   **2つ目：AnthropicのAPIキー**
   - Name: `ANTHROPIC_API_KEY`
   - Secret: `sk-ant-api03-...`（Anthropicコンソールで取得）

   > APIキーは [console.anthropic.com](https://console.anthropic.com/) の
   > 「API Keys」→「Create Key」で作成できます。

---

### STEP 4：GitHub Actionsを実行

1. リポジトリの **「Actions」タブ** をクリック
2. 左メニューの **「Deploy to GitHub Pages」** をクリック
3. **「Run workflow」→「Run workflow」** をクリック
4. 緑のチェックマーク ✅ が出たら成功

   ※初回はActionsが自動実行されるので、STEPを踏まなくてもOKな場合あり

---

### STEP 5：GitHub Pages を有効化

1. **「Settings」→「Pages」** をクリック
2. **Source** を **「Deploy from a branch」** に設定
3. **Branch** を **「gh-pages」**、フォルダを **「/ (root)」** に設定
4. **「Save」** をクリック

   数分後、以下のURLでアクセスできます：
   ```
   https://あなたのGitHubユーザー名.github.io/maker-search/
   ```

---

## 🔑 利用者へのAPIキー設定（EMBEDDED_API_KEYを空にした場合）

Secretsの `ANTHROPIC_API_KEY` を空のままにした場合、利用者が自分でAPIキーを入力します：

1. ページ右上の **「⚙ 設定」** をクリック
2. APIキーを入力して **「保存して閉じる」**
3. そのブラウザに保存されるので次回から不要

---

## ✏️ 会社リストを更新する

`companies.js` を開いて会社名を追加・削除 → GitHubにアップ → 自動デプロイされます。

```javascript
const COMPANIES = [
  "(株)アイエイアイ",
  "追加したい会社名",  // ← ここに追加
  ...
];
```

---

## 🔒 パスワードを変更する

1. GitHub の **「Settings」→「Secrets」→「Actions」**
2. `SITE_PASSWORD` を **「Update」** して新しいパスワードを入力
3. 「Actions」タブから **「Run workflow」** で再デプロイ

---

## ⚠️ セキュリティについて

- URLを知っていてもパスワードなしでは使えません
- パスワードはJavaScriptコードに含まれるため、技術的には解析可能です
- 高い機密性が必要な情報の管理には使わないでください
- AnthropicコンソールでAPIキーの利用上限を設定することを推奨します

---

## ❓ トラブルシューティング

| 症状 | 対処 |
|---|---|
| Actionsが失敗する（赤×） | Secretsの名前が `SITE_PASSWORD` / `ANTHROPIC_API_KEY` と一致しているか確認 |
| ページが表示されない | Settings→Pagesで `gh-pages` ブランチが選ばれているか確認。数分待つ |
| パスワードが通らない | Secrets→`SITE_PASSWORD` の値を確認して再デプロイ |
| 「APIエラー」が出る | Anthropicコンソールでクレジット残高を確認 |
| 会社が反映されない | ブラウザのキャッシュをクリア（Ctrl+Shift+R） |
