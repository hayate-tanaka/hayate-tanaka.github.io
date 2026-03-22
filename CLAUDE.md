# CLAUDE.md — 作業ルール / Working Rules

## リポジトリ概要 / Repository Overview

Hugo + Beautiful Hugo テーマで構築された、海産生物（ウニ綱）分類学者の個人学術サイト。
GitHub Pages へ自動デプロイされる。

**スタック / Stack:**
- Static site generator: [Hugo](https://gohugo.io/) (extended edition, requires ≥ 0.124.0)
- Theme: [Beautiful Hugo](https://github.com/halogenica/beautifulhugo) (git submodule + Hugo module)
- Hosting: GitHub Pages (デプロイ: `.github/workflows/hugo.yaml`)
- Language: 主に日本語コンテンツ、英語も含む

---

## ディレクトリ構造 / Directory Structure

```
.
├── hugo.toml                   # メイン設定ファイル
├── go.mod / go.sum             # Hugo module dependencies
├── .gitmodules                 # git submodule (themes/beautifulhugo)
├── CLAUDE.md                   # このファイル
│
├── content/
│   ├── _index.md               # ホームページ（現在ほぼ空）
│   └── page/
│       ├── about.md            # プロフィール・略歴ページ
│       └── outputs.md          # CV・業績一覧ページ
│
├── layouts/
│   ├── _default/
│   │   └── baseof.html         # Hugo バージョンチェック (≥ 0.124.0)
│   ├── index.html              # カスタムホームページレイアウト
│   └── partials/
│       ├── head_custom.html    # 日本語フォント・OGP・モバイル最適化
│       └── footer_custom.html  # カスタムフッター（現在ほぼ空）
│
├── static/
│   ├── img/
│   │   ├── avatar-icon.png     # ナビゲーションのロゴ・アバター（正方形）
│   │   └── favicon.png         # ブラウザタブアイコン
│   └── images/
│       ├── header.png          # ヘッダー画像（ウニの写真）
│       └── profile.png         # プロフィール写真（ホームページ・OGP用）
│
├── data/
│   └── beautifulhugo/
│       └── social.toml         # SNSアイコン設定（30以上のプラットフォーム）
│
└── .github/
    └── workflows/
        └── hugo.yaml           # GitHub Pages 自動デプロイ
```

---

## テーマ・FWの変更を行う前に必ずやること

**Beautiful Hugo テーマに関する変更を行う前に、必ず公式リポジトリのドキュメントを読むこと。**

- 公式リポジトリ: https://github.com/halogenica/beautifulhugo
- テーマのREADME: `themes/beautifulhugo/README.md`
- exampleSite設定: `themes/beautifulhugo/exampleSite/hugo.toml`

推測や慣習で実装せず、ドキュメントに記載された正しい方法で設定すること。

---

## Beautiful Hugo 設定メモ（確認済み）

### ロゴ・アバター画像
- `static/img/avatar-icon.png` に配置する（正方形推奨）
- `hugo.toml` で `logo = "img/avatar-icon.png"` と指定
- `assets/` への配置は **誤り**。`nav.html` は `resources.Get` 失敗時に `absURL`（= `static/`）へフォールバックする

### Favicon
- `static/img/favicon.png` に配置（現在 `.png` を使用）
- `hugo.toml` で `favicon = "img/favicon.png"` と指定

### ヘッダー画像
- `static/images/` に配置し `hugo.toml` の `[[Params.bigimg]]` で指定

### 言語設定（重要）
- `languageCode = "ja"` → `<html lang="ja">` を出力。日本語サイトの宣言はこれだけで十分
- `DefaultContentLanguage = "en"` → **絶対に "ja" に変えないこと**
  - `"ja"` にするとコンテンツURLが `/ja/page/about/` になり **About・CVページが404になる**
  - URL構造の制御であり、表示言語とは無関係

---

## Hugo 設定の重要ポイント / Key hugo.toml Notes

| 設定 | 値 | 備考 |
|------|----|------|
| `baseurl` | `https://hayate-tanaka.com/` | 末尾スラッシュ必須 |
| `languageCode` | `"ja"` | `<html lang="ja">` 出力用 |
| `DefaultContentLanguage` | `"en"` | **変更禁止** — URL構造制御 |
| `theme` | `"beautifulhugo"` | git submodule として解決 |
| `[[module.imports]]` | beautifulhugo | Hugo module としても宣言 |
| `Params.mainSections` | `["post","posts"]` | ブログ記事なし。ホームにリスト表示しない |
| `Params.favicon` | `"img/favicon.png"` | |
| `[[Params.bigimg]]` | `images/header.png` | ヘッダー画像 |

**無効化されている機能:** Disqus, Google Analytics, RSS, コメント, 読了時間, 文字数, ソーシャルシェア, 関連記事

---

## カスタムレイアウト / Custom Layouts

### `layouts/index.html`
テーマのデフォルトホームをオーバーライド。プロフィール写真（丸形）＋ 名前＋説明文＋ボタン群（About / CV / X / GitHub / LinkedIn）を表示。

### `layouts/_default/baseof.html`
Hugo バージョンを確認し、0.124.0 未満の場合はエラーメッセージを出力する。

### `layouts/partials/head_custom.html`
Beautiful Hugo の `<head>` に自動挿入されるカスタム partial:
- 日本語フォントスタック（Meiryo, Hiragino Kaku Gothic ProN）
- モバイルでの横溢れ防止 CSS
- OGP `og:image` メタタグ（デフォルト: `https://hayate-tanaka.com/images/profile.png`）
- ページごとに `image` frontmatter で上書き可能
- `meta description` フォールバック（`site.Params.description` を使用）

---

## コンテンツ構造 / Content Structure

### `content/page/about.md`
- プロフィール写真、略歴、研究内容を記載
- `comments = false` を frontmatter で指定

### `content/page/outputs.md`
- 学術業績の一覧（論文・学会発表・著書・メディア出演など）
- セクション: Publications / 学会発表 / Web媒体記事 / 一般向け公演 / メディア出演 / 編著書 / 雑誌記事

### ナビゲーションメニュー
```toml
[[menu.main]]
  name = "About"
  url  = "page/about/"

[[menu.main]]
  name = "CV"
  url  = "page/outputs/"
```

---

## 開発ワークフロー / Development Workflow

### ローカル開発
```bash
# サブモジュール初期化（初回のみ）
git submodule update --init --recursive

# 開発サーバー起動（ホットリロード有効）
hugo server -D

# 本番ビルド
hugo --gc --minify
```

### デプロイ
`main` ブランチへのプッシュで `.github/workflows/hugo.yaml` が自動実行:
1. Hugo CLI (v0.140.2 extended) をインストール
2. サブモジュールを含む全履歴でチェックアウト
3. `hugo --gc --minify` でビルド
4. `public/` を GitHub Pages へデプロイ

### テーマの更新
```bash
git submodule update --remote themes/beautifulhugo
```

---

## 静的アセット / Static Assets

| ファイル | 場所 | 用途 |
|---------|------|------|
| `avatar-icon.png` | `static/img/` | ナビゲーションのアバター/ロゴ |
| `favicon.png` | `static/img/` | ブラウザタブアイコン |
| `profile.png` | `static/images/` | ホームページ・OGP画像 |
| `header.png` | `static/images/` | ページヘッダー背景（ウニ写真） |

**画像配置ルール:** `static/` 配下に置く。`assets/` は Hugo pipeline 用であり、Beautiful Hugo のテンプレートは `static/` からのフォールバックで動作する。

---

## SNS・ソーシャルリンク / Social Links

`data/beautifulhugo/social.toml` でアイコン定義。`hugo.toml` の `[Params.author]` で有効化:

```toml
[Params.author]
  name     = "田中颯 (Hayate Tanaka)"
  facebook = "tanaka.hayate"
  github   = "hayate-tanaka"
  twitter  = "HTanattyo"
  linkedin = "tanaka-hayate"
```

---

## よくある注意事項 / Common Pitfalls

1. **`DefaultContentLanguage = "ja"` にしない** — URLが `/ja/` プレフィックス付きになり既存リンクが全て壊れる
2. **画像を `assets/` に置かない** — `static/` に配置すること
3. **Hugo バージョン** — 0.124.0 以上が必須。GitHub Actions は 0.140.2 extended を使用
4. **テーマ変更前** — 必ず Beautiful Hugo の公式ドキュメントを確認すること
5. **ロゴを有効化する場合** — `hugo.toml` の `# logo = "img/avatar-icon.png"` のコメントを外す
