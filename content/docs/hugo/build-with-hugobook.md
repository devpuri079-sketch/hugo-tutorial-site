---
title: テーマ:Hugo Book
weight: 6
---

# Book テーマを使用したサイト作成手順

---

## この手順の目的

この手順では、Hugo のテーマ **Book** を使用してサイトを構築する方法を説明する。  
Book テーマはドキュメントサイトやチュートリアルサイトに適しており、サイドバー構成や階層的なページ管理がしやすい。

このサイトも **Book テーマで作成している**。

Book テーマの公式ページはこちら。

- Book テーマ  
  https://github.com/alex-shpak/hugo-book

---

## Book テーマの概要

- ドキュメントサイト向けに設計された Hugo テーマ  
- サイドバー、検索、階層構造の管理がしやすい  
- exampleSite が充実しており、構成を模倣しやすい  

---

## 作成手順

### 1. 初期設定を行う

```bash
hugo new site my-hugobook-site
```

```bash
cd .\my-hugobook-site\
```

```bash
git init
```

Book テーマを submodule として追加する。

```bash
git submodule add https://github.com/alex-shpak/hugo-book themes/hugo-book
```

---

### 2. サンプルサイトのソースコードをコピーする

Book テーマの exampleSite の中で、最も内容が充実しているのは **content.en**。  
これをそのままコピーする。

```bash
cp -r .\themes\hugo-book\exampleSite\content.en\* .\content\
```

設定ファイル（hugo.toml）もコピーして上書きする。

```bash
cp .\themes\hugo-book\exampleSite\hugo.toml .\
```

---

### 3. 設定ファイル（TOML）を編集する

exampleSite は多言語構成になっているため、  
`hugo.toml` の **Multi-lingual mode config（[languages] の記述）** を一旦コメントアウトする。

理由：

- 今回コピーしたのは **en のみ**  
- 多言語設定を残すと、存在しない言語フォルダを参照してエラーになる可能性がある

#### その他編集ポイント

- サイトタイトルの修正  
- baseURL の設定(GitHub Pasesを使用するタイミングでも可)  
- メニュー構成の調整  
- 不要な項目の削除  

---

### 4. サイトを実行する

```bash
hugo server
```

ブラウザで以下にアクセスする。

```
http://localhost:1313/
```

Book テーマの構成がそのまま反映されたサイトが表示される。

---

## exampleSite の構成について

Book テーマの exampleSite は、実際のサイト構成を理解するための参考になる。  
以下のように活用すると効率がよい。

### ● 1. サイトを眺めながら、どのファイルがどのページか確認する

- ブラウザでサイトを開く  
- `content` の中身を見ながら  
  「このページはどのファイルが対応しているのか」  
  を照らし合わせて理解する  

### ● 2. exampleSite を模倣しながらページを追加する

- まずは exampleSite の構成をそのまま真似する  
- `_index.md` の書き方や front matter を参考にする  
- 新しい章（chapter）を作りたい場合はフォルダ＋`_index.md` を追加する

### ● 3. 不要なページを少しずつ削除しながら、自分のページを追加していく

- 最初から全部消すと構造がわからなくなる  
- 必要なページを追加しつつ、不要なページを徐々に削除していく  
- exampleSite の構造を残しながら編集すると、Book テーマの動作を理解しやすい

---

## 画像の表示方法

Book テーマでは、画像の扱いは比較的シンプル。  
以下の方法で画像を表示できる。

### ● 手順

1. **Markdown ファイルと同じ名前のフォルダ**を、同じ階層に作成する  
   例：  
   ```
   page1.md
   page1/
       image1.png
   ```

2. 画像ファイルをそのフォルダに格納する  
3. Markdown 内で以下のように記述する

```
![代替テキスト](image1.png)
```

### ● 補足

- Book テーマは Page Bundle 方式（`index.md`＋フォルダ）を使わなくても動作する  
- 「md と同名フォルダに画像を置く」方式が最もシンプルで扱いやすい  
- 画像パスはフォルダ名を書かず、**ファイル名だけ**でよい  
  （Book テーマが自動的に同名フォルダを参照するため）


