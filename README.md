# 彩月つくし Official Website
## Cloudflare Pages デプロイ＆カスタマイズ手順書

---

## フォルダ構成

```
saizuki/
├── index.html          # HOME（カルーセル）
├── profile.html        # PROFILE（プロフィール）
├── news.html           # NEWS（ニュース）
├── biography.html      # BIOGRAPHY（バイオグラフィー）
├── works.html          # WORKS（出演作品）
├── contact.html        # CONTACT（お問い合わせ）
├── css/
│   └── style.css       # 全ページ共通スタイル
├── js/
│   ├── main.js         # 共通JS（ナビ・アニメーション）
│   └── news-data.js    # ★ ニュースデータ（ここだけ編集）
└── images/             # ★ 写真はここに保存
    ├── slide01.jpg     # トップカルーセル 1枚目
    ├── slide02.jpg     # トップカルーセル 2枚目
    ├── slide03.jpg     # トップカルーセル 3枚目
    ├── slide04.jpg     # トップカルーセル 4枚目
    ├── slide05.jpg     # トップカルーセル 5枚目
    ├── profile.jpg     # プロフィール写真
    ├── og-image.jpg    # SNSシェア用サムネイル（1200×630px）
    └── works/          # 出演作品の画像
        └── work01.jpg  など
```

---

## STEP 1 — 写真を準備する

| ファイル名 | 推奨サイズ | 用途 |
|-----------|-----------|------|
| slide01〜05.jpg | 1920×1280px 以上 | トップカルーセル |
| profile.jpg | 760×1014px 以上（3:4比率） | プロフィール写真 |
| og-image.jpg | 1200×630px | SNSシェア時のサムネイル |
| works/work*.jpg | 600×800px（3:4比率） | 出演作品 |

**画像形式について：**
- `.jpg` か `.webp` を使ってください（.webpの方がファイルが軽い）
- 拡張子を `.webp` にした場合は、HTML内の `slide01.jpg` も `slide01.webp` に変更してください

---

## STEP 2 — コンテンツを編集する

### ニュースを更新する
`js/news-data.js` を開いて、配列に追加するだけです：

```javascript
{
  date: "2025.06.01",
  category: "STAGE",        // INFORMATION / STAGE / MEDIA のいずれか
  title: "〇〇公演への出演が決定しました。"
},
```

### Instagram / X（Twitter）のURLを設定する
全HTMLファイルの以下の2箇所を実際のURLに差し替えてください：

```html
<a href="https://www.instagram.com/【あなたのID】/">Instagram</a>
<a href="https://x.com/【あなたのID】">X / Twitter</a>
```

### プロフィール文を編集する
`profile.html` の `profile-bio` 内のテキストを書き換えてください。

---

## STEP 3 — Googleフォームを作成する

1. Google Workspace のアカウントで [forms.google.com](https://forms.google.com) を開く
2. 「新しいフォーム」を作成し、以下の質問を追加：
   - お名前（短文テキスト・必須）
   - メールアドレス（短文テキスト・必須）
   - お問い合わせ種別（プルダウン：出演依頼／メディア取材／MC・講演／ファンメール／その他）
   - 件名（短文テキスト）
   - お問い合わせ内容（長文テキスト・必須）
   - 個人情報の取り扱いに同意する（チェックボックス）

3. 右上「送信」→「<>（埋め込み）」タブ → URLをコピー
4. `contact.html` を開き、以下のiframeのコメントアウトを外してURLを貼り付ける：

```html
<iframe
  src="https://docs.google.com/forms/d/e/【ここにURL】/viewform?embedded=true"
  title="お問い合わせフォーム"
  loading="lazy">
</iframe>
```

5. 同じく `contact.html` の `<div id="form-placeholder">` の行を削除する

---

## STEP 4 — Cloudflare Pages にデプロイする

### 方法A：ドラッグ＆ドロップ（最も簡単・Gitなし）

1. [Cloudflare Dashboard](https://dash.cloudflare.com/) にログイン
2. 左メニュー「Workers & Pages」→「Pages」→「Create a project」
3. 「Direct Upload」を選択
4. `saizuki` フォルダごとドラッグ＆ドロップ
5. プロジェクト名（例：saizuki）を設定して「Deploy site」
6. `https://saizuki.pages.dev` でサイトが公開されます 🎉

### カスタムドメイン（saizuki.com）を接続する

1. Cloudflare Pagesのプロジェクト画面 →「Custom domains」→「Set up a custom domain」
2. `saizuki.com` を入力
3. お名前.com の DNS設定で以下を追加：
   - タイプ: `CNAME`
   - ホスト: `@`
   - 値: `saizuki.pages.dev`

> ⚠️ お名前.comのCNAMEは反映に最大24時間かかる場合があります

---

## STEP 5 — 更新時の手順

コンテンツを変更したい場合：
1. PCでHTMLまたはJSファイルを編集
2. Cloudflare Pages の「Deployments」→「Direct Upload」で再アップロード

---

## よくある質問

**Q: 写真が表示されない**
A: `images/` フォルダの中のファイル名と、HTML内のファイル名（拡張子含む）が完全一致しているか確認してください。大文字小文字も区別されます。

**Q: スマートフォンで文字が小さい**
A: `css/style.css` の `:root` 内の `--font-size` を調整してください。

**Q: カルーセルの切り替え速度を変えたい**
A: `index.html` 内の `setInterval(next, 5000)` の `5000` をミリ秒単位で変更してください（5000 = 5秒）。

**Q: ニュースのカテゴリを増やしたい**
A: `news-data.js` の `category` に新しい文字列を追加し、`news.html` の `.news-filters` 部分にボタンを追加してください。
