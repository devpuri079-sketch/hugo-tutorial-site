---
title: テーマ:Hextra
weight: 5
---

# Hextra を使用したサイト作成手順

---

## この手順の目的

具体例として、Hugo のテーマ **[Hextra](https://github.com/imfing/hextra)** を使用してサイトを構築する方法を説明する。  
Hextra はドキュメントサイト向けに最適化されたテーマで、検索機能、サイドバー、バージョン管理などを備えている。

AI エージェント構築のドキュメントサイトでも Hextra を採用している。  

- AI エージェント構築ドキュメントサイト  
  https://devpuri079-sketch.github.io/ai-agents-site/



---

## Hextra の概要

- ドキュメントサイト向けに設計された Hugo テーマ  
- サイドバー、検索、バージョン管理などの機能を標準で提供  
- シンプルで読みやすい UI  

---

## 作成手順

### 1. 初期設定を行う

Hextra 用の Hugo プロジェクトを作成する。  
公式の手順に従い、format=yamlオプションとしている  
こうすることで設定ファイルがtomlではなく、yamlとなる
後続の手順で設定ファイルをテンプレートプロジェクトのもの(yaml)に置き換えるので  
format=yamlオプションで実行することを推奨

```bash
hugo new site my-hextra-site --format=yaml
```

作成したフォルダに移動する。

```bash
cd .\my-hextra-site\
```

Git 管理を開始する。

```bash
git init
```

Hextra を submodule として追加する。

```bash
git submodule add https://github.com/imfing/hextra.git themes/hextra
```

---

### 2. サンプルサイトをダウンロードする

Hextra にはスターターテンプレートが用意されている。  
以下のリポジトリからサンプルサイトを確認する。

- Hextra Starter Template  
  https://github.com/imfing/hextra-starter-template/tree/main

ローカルに clone して内容を確認する。

```bash
※my-hextra-site内で実行せず、同階層などに移動後に実施すること
cd ../
git clone https://github.com/imfing/hextra-starter-template.git
```

---

### 3. サンプルサイトのソースコードをコピーする

Starter Template の**content 直下のフォルダをそのままコピーする。**

```bash
cp -r hextra-starter-template/content/* my-hextra-site/content/
```

設定ファイルも Starter Template のものをコピーして上書きする。

```bash
cp hextra-starter-template/hugo.yaml my-hextra-site/hugo.yaml
```

---

### 4. 設定ファイル（YAML）を編集する

コピーした `hugo.yaml` を編集し、以下を調整する。

- サイトタイトル  
- baseURL(GitHub Pasesを使用するタイミングでも可)  
- メニュー構成  
- 不要なメニュー項目の削除  
- リンク先の修正  
- サイドバーの構成  

---

## Hextra Starter Template のフォルダ構成

Starter Template からコピーしていれば `content` 配下は以下のとおりとなる。  

```
content/
│  about.md
│  _index.md
│
└─docs
    │  first-page.md
    │  _index.md
    │
    └─folder
            leaf.md
            _index.md
```

---

### about.md  
サイト内の「About」ページの内容。  
`hugo.yaml` のメニュー設定で、このページをメニューに表示するようになっている。

---

### _index.md（ルート直下）  
サイトのトップページ。  
Hextra では、この `_index.md` がホームページとして扱われる。  
Starter Template では、トップページに **docs** と **about** へのリンクカードが表示される構成になっている。

---

## docs フォルダ（メインコンテンツ）

`docs` はドキュメントページ群をまとめるフォルダで、  
Hextra のサイト構成では「メインコンテンツ」として扱われる。

---

### docs/_index.md  
`docs` のトップページ。  
トップページのカードリンクからここに遷移する。  
Hextra では、このファイル名でないとトップページとして認識されない。

---

### docs/first-page.md  
`docs` 内の 1 ページ。  
フォルダに入れず、単体の Markdown ファイルとして配置しても問題ない。  
ファイルのヘッダー部分でtitle: Demo Pageとなっているので  
サイト内ではDemo Pageというタイトルやリンクになっている

---

## docs/folder（サブセクションの例）

`docs` 内にさらに階層を作りたい場合の例。

### folder/_index.md  
`folder` のトップページ。  
このファイルがあることで、`folder` が 1 つのセクションとして扱われ、  
`folder/_index.md` がトップページ、`leaf.md` がその配下のページになる。

### folder/leaf.md  
`folder` 内の 1 ページ。  
`folder` に `_index.md` がない場合、  
`first-page.md` と同じ階層のページとして扱われる。
- `folder` に `_index.md` がある場合
![`folder` に `_index.md` がある場合](image1.png)
- `folder` に `_index.md` がない場合
![`folder` に `_index.md` がない場合](image2.png)

---
## セクションとページの違い

Hugo では、コンテンツは大きく **セクション（Section）** と **ページ（Page）** に分かれる。

### ● セクション（Section）

- `_index.md` を持つフォルダ  
- そのフォルダが 1 つの「まとまり」として扱われる  
- セクションのトップページが生成される  
- サイドバーやメニューで階層として表示される  

例：  
- `content/docs/`  
- `content/docs/folder/`

---

### ● ページ（Page）

- 単体の Markdown ファイル  
- `_index.md` を持たないフォルダ内のファイルもページ扱い  
- セクション内の個別ページとして表示される  

例：  
- `content/docs/first-page.md`  
- `content/docs/folder/leaf.md`

---


## 画像の表示方法

Hextra では、画像の扱いが非常にシンプルで、特別な設定を行う必要はない。  
Markdown ファイルと **同じ階層に画像ファイルを配置するだけで表示できる**。

### 手順

1. 表示したい画像ファイル（例：`sample.png`）を、対象の Markdown ファイルと同じフォルダに置く  
2. Markdown 内で以下のように記述する

```
![代替テキスト](sample.png)
```

これだけで画像が表示される。

### 補足

- md ファイル名を **index.md にする必要はない**  
- Page Bundle（フォルダ＋index.md）を使わなくてもよい  
- 単純に「同じ階層に置く → パスを書く」だけで動作する  


