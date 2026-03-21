# CLAUDE.md — 作業ルール

## テーマ・FWの変更を行う前に必ずやること

**Beautiful Hugo テーマに関する変更を行う前に、必ず公式リポジトリのドキュメントを読むこと。**

- 公式リポジトリ: https://github.com/halogenica/beautifulhugo
- テーマのREADME: `themes/beautifulhugo/README.md`
- exampleSite設定: `themes/beautifulhugo/exampleSite/hugo.toml`

推測や慣習で実装せず、ドキュメントに記載された正しい方法で設定すること。

## Beautiful Hugo 設定メモ（確認済み）

### ロゴ・アバター画像
- `static/img/avatar-icon.png` に配置する（正方形推奨）
- `hugo.toml` で `logo = "img/avatar-icon.png"` と指定
- `assets/` への配置は **誤り**。`nav.html` は `resources.Get` 失敗時に `absURL`（= `static/`）へフォールバックする

### Favicon
- `static/img/favicon.ico` に配置
- `hugo.toml` で `favicon = "img/favicon.ico"` と指定

### ヘッダー画像
- `static/images/` に配置し `hugo.toml` の `[[Params.bigimg]]` で指定
