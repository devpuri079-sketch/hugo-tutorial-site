---
weight: 3
---

# Hugo サイトを GitHub に push して GitHub Pages で公開する手順

---

## この手順の目的

このページでは、ローカルで作成した Hugo サイトを GitHub にアップロードし、  
GitHub Pages を使ってインターネット上に公開するまでの手順を説明する。

---

## 1. GitHub リポジトリを作成する

まず GitHub にアクセスし、新しいリポジトリを作成する。

- GitHub にログイン  
- 右上の **「＋」 →「New repository」** を選択  
- リポジトリ名を入力（Hugo のプロジェクトフォルダ名と揃えると管理しやすい）  
- **Public** を選択  
  - ※GitHub Pages は Free / Pro プランでは Private リポジトリでは利用できません  
  - この手順では Pages を使用するため、Public で作成してください  
- README などは作成せず、空の状態で作成する  

リポジトリが作成されたら、表示されるリポジトリ URL を控えておく。

---

## 2. `baseURL` を設定する（GitHub Pages では必須）

GitHub Pages では、公開 URL に合わせて `baseURL` を設定する必要があります。  
設定しない場合、CSS や画像が正しく読み込まれず、テーマが反映されません。

`hugo.toml`（または `config.toml`）に以下を記載します。

```toml
baseURL = "https://ユーザー名.github.io/リポジトリ名/"
```

※ 最後の `/` を忘れないこと。

---

## 3. ドラフト設定を確認する（draft = true のままだと公開されません）

Hugo の記事は、デフォルトで `draft = true` の場合があります。

例：

```toml
draft = true
```

このままだと **GitHub Pages に公開されません**。

公開したい記事は必ず：

```toml
draft = false
```

に変更してください。

---

## 4. ローカルの Hugo プロジェクトと GitHub を紐づける

作成した GitHub リポジトリを push の送信先として登録する。

```bash
git remote add origin https://github.com/ユーザー名/リポジトリ名.git
```

---

## 5. `.gitignore` を作成する

Hugo では、ビルド生成物やキャッシュ類を Git に含める必要がないため、  
プロジェクト直下に `.gitignore` を作成し、以下を記載する。

```
/public/
/resources/_gen/
/.hugo_build.lock
/.hugo/
.DS_Store
Thumbs.db
node_modules/
*.log
```

---

## 6. ローカルのファイルを GitHub に push する

初回コミットと push を行う。

```bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

push が成功すると、GitHub 上に Hugo プロジェクトがアップロードされる。

---

## 7. GitHub Pages の Build and Deployment を設定する

GitHub のリポジトリページで以下を設定する。

1. **Settings** を開く  
2. 左メニューから **Pages** を選択  
3. **Build and deployment** の項目で  
   **Source: GitHub Actions** を選択する

---

## 8. GitHub Actions 用の `hugo.yaml` を作成する

GitHub Actions に Hugo のビルド方法を指示するため、  
以下のファイルを作成する。

```
.github/workflows/hugo.yaml
```

内容は次の通り。

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

---

## 9. 設定ファイルを push して Actions の実行を確認する

作成した workflow ファイルを GitHub に push する。

```bash
git add .
git commit -m "Add GitHub Actions workflow"
git push
```

push すると GitHub Actions が自動で動き始める。  
GitHub の **Actions** タブでビルドが成功していることを確認する。

---

## 10. 公開されたサイトを確認する

Actions が成功すると、GitHub Pages の URL が有効になる。

URL は以下の形式。

```
https://ユーザー名.github.io/リポジトリ名/
```

ブラウザでアクセスし、Hugo サイトが表示されれば公開完了。

---

# まとめ

このページでは、Hugo サイトを GitHub に push し、  
GitHub Pages で公開するまでの手順を説明した。

- GitHub リポジトリを Public で作成  
- `baseURL` を設定  
- draft を false にする  
- ローカルと GitHub を紐づけ  
- `.gitignore` を設定  
- 初回 push  
- GitHub Pages を GitHub Actions モードに設定  
- `hugo.yaml` を作成して自動ビルド  
- Actions の成功を確認  
- 公開 URL をチェック  

これで Hugo サイトをインターネット上に公開できるようになった。


