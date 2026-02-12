# まずは現状確認
- 活用資料集のトップページは、最近アップデートされた資料がリストされているのでそれをhugoでどうやるか
	- そもそも最近アップデートされた資料をリストするべきか
- サービス（分野）ごとのページはカード形式なので既存のものを再利用できそう
- 基本的には各リーフページはspeeker deckの埋め込みのみ
	- というかサービス概要が多いのでそもそも"活用”資料集なのか？
- Cloud Nativeの欄にEOSしてるやつが入ってる
- FAQのページはspeaker deck埋め込みではなくチュートリアルのように記事になってる
	- 旧チュートリアルにあった、変なサイドメニューにはなっていない
- ページによっては帯のところに個別のリンクを埋め込んでるパターンがある
	- ![[Pasted image 20251212144426.png]]
- `_posts`の中に実際のコンテンツが入ってる
- `_pages`の中に、リーフページまでの動線ページが入ってる
- リーフページは下記のように旧メタデータが入ってる（当然）
```
---
title: "OCIの認証・認可機能の総まとめ"
excerpt: "OCIを使い始めた人が誰でも引っかかる OCI IAM と Oracle Identity Cloud Service (IDCS) の深くてフクザツな関係を紐解きます"
categories:
  - Solutions
  - IAM
tags:
  - スライドあり
  - レベル:応用(200)
header:
  teaser: https://speakerd.s3.amazonaws.com/presentations/ac3060120271495cb8da562b79414c11/slide_0.jpg
---

2019/11/20に実施した、同名セミナーの資料です。  
OCIを使い始めた人が誰でも引っかかる、OCI IAM と Oracle Identity Cloud Service (IDCS) の、深くてフクザツな関係を紐解きます。  
IAM機能のおさらいから手始めに、IDCSを使った場合にできることや、2つの認証/認可のツールの使い分け、より高度な要件(多要素認証、外部認証、IPホワイトリスティング)を行う際の方法などについて、ディープに語ります。


#### スライド

<div style="max-width:768px">

<!-- Speakerdeckから Embeded リンクを取得して貼り付け (ここから) -->
<script async class="speakerdeck-embed" data-id="ac3060120271495cb8da562b79414c11" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>
<!-- Speakerdeckから Embeded リンクを取得して貼り付け (ここまで) -->

</div>
```
- スライド自体はHTML埋め込みっぽい
- リーフページは`YYYY-MM-DD-`がファイル名のプレフィックスについてるので、それを使って最新記事一覧を表示していそう
- FAQだけなぜかディレクトリが切られてる

# ToDO
- とりあえず既存のマークダウンをフロントマターだけ編集して、hugoに持っていく
	- その際リストページはカード形式に
	- HTMLの埋め込みはどうするか考える
		- いい感じに表示する方法があれば、shortcode的な何かでどうにかできるかも（リンクを引数で）
- 基本的にはカード形式でリストする際は新しい順に表示すべき（現行もそうなってる）
- ホーム画面のレイアウトはわかりにくいので、議論が必要
- チュートリアルに比べてヘッダー部分に埋め込まれたリンクが多いのでデザインは上手くやる必要がある
- 「最近アプデされた資料」は、ページネーションの機能がある。hugoでできる？→できる
- 現行のフロントマターの`categories`の中の要素ごとに`content/`配下にディレクトリを作成して、そこにマークダウンを配置する
- タグの一覧ページは、既存のやつコピーしちゃダメ
- リンクの担保は**エイリアスで頑張る**
- 


- FAQいらなくね
	- チュートリアルに合体？
	- speakerdeckのページは目次が要らん　← 形式的にはやりやすいから作業がしやすい
	- →ただページの目的と合わない、FAQ自体消す？
- 各コンテンツの更新日時は、ファイル名から引っ張ってる
	- hugoは自動生成してるから、ファイル名から引っ張るようにする？

# ディレクトリの切り方
- contents
	- updates
	- services
		- compute
		- database
			- basedb
			- autonomous
		- developer
			- コンテナとか
			- メール
		- others
			- ocicli
		- governance-and-administration
		- iam
		- management
		- networking
		- overall
		- security
		- storage
	- solutions
		- blockchain
		- cloudnative
		- ha
		- others
	- faq
	- links
		- 裸で置いとく
		- 古いやつが多いからいずれ掃除する必要あり

# 移行作業の記録
- ディレクトリの再設計、再配置
  - content配下集約
    - services配下に各サービスの配置
      - categoriesタグで

- チュートリアルからのテンプレート変更
	- menu-filetree.htmlの変更
	- cardList.htmlの変更
	- 更新日管理方法の変更
		- これまで：ファイル名の接頭辞を取得して
		- 今後：lastmodのfrontmatterを使って管理

frontmatterについて
- lastmodを変更することで、資料の最新更新日を編集可能
- dateは投稿作成日
  - 左から優先で更新日が採用される: "lastmod", ":git", "date", ":default" 
- サムネイルはimagesを使用（チュートリアルと同じ）
  - httpもしくはhttpsで始まるものはそのままsrcとして取得する
  - ローカルの画像を指定するのもは、画像ファイル名をそのまま記述する（例:image.png）ことで、画像が使用される

残タスク
- 「ociチュートリアル」の名残を消去
  - hugo.tomlの各種変更
    - タイトル、URL関連
    - Google Analytics
  - トップページのdescription

完了タスク
- {:target="_blank"}を削除(done)
- excerptをdescriptionに変更(done)
- alisesを設定(done)
- ホーム画面で最新の記事をリストする(done)
- ホーム画面のデザイン(done)
- タグ検索のページを作る(done)
- メニューバーのCSS調整(done)
- ショートコード的なやつ使ってないか確認
  - back_to_topというやつ削除済み