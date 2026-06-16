# 鍛錬 / Kintore — Training Log

iPhone のホーム画面から起動できる、シンプルなフリーウェイト・トレーニング記録アプリ。
バックエンド不要、依存パッケージなし、`index.html` 1ファイルで完結します。

![tab](https://img.shields.io/badge/tabs-記録%20|%20履歴%20|%20推移%20|%20連携-d4ff00?style=flat-square&labelColor=0a0a0a)
![storage](https://img.shields.io/badge/storage-localStorage-888?style=flat-square&labelColor=0a0a0a)
![license](https://img.shields.io/badge/license-MIT-888?style=flat-square&labelColor=0a0a0a)

---

## 機能

- **よく使う** — 過去の記録から使用頻度の高い種目を自動集計。記録があれば種目選択シートを開いたとき最初に表示
- **記録** — 8カテゴリ（胸 / 背 / 肩 / 腕 / 脚 / 体幹 / 自重 / **ケーブル** / カスタム）から種目を選択。**横断検索**機能付き
- **自重・ケーブル対応** — それぞれ専用タブ＋他カテゴリにも分散配置。検索でどこからでも見つかる
- **時間ベース種目** — プランクなど秒数記録対応
- **履歴** — セッション別に展開可能。総ボリューム・部位コード・PR バッジ表示
- **推移** — 種目別の折れ線グラフ。フリーウェイトは最大重量 / 総ボリューム / 推定1RM、自重は最大レップ / 合計レップ
- **実績** — 21種類のバッジ
- **PR演出** — 自己ベスト更新時に紙吹雪付き祝福オーバーレイ
- **ストリークコメント** — 連続記録日数に応じた動的なコメント表示
- **連携** — Notion 用 Markdown / Google カレンダー .ics / JSON バックアップ・復元

依存ゼロ。React も Babel も外部CDNも使っていません。`index.html` 1ファイルだけ。

明るいライトテーマ（白背景＋オレンジアクセント）でスポーティーなデザイン。
アプリ内のアイコンは絵文字ではなくテーマカラーのオリジナル SVG（端末によらず同じ見た目）。

---

## iPhone へのインストール (PWA 化)

1. このリポジトリで GitHub Pages を有効化（下記参照）
2. 発行された URL を iPhone の **Safari** で開く
3. 共有ボタン → **「ホーム画面に追加」**
4. ホーム画面のアイコンをタップするとフルスクリーンで起動

データは Safari の `localStorage` に保存されます。Safari のサイトデータを消すと記録も消えるため、定期的に「連携」タブから JSON バックアップを取ってください。

---

## GitHub にアップする手順

### A. ブラウザだけで完結する方法（推奨・最速）

1. GitHub にログインし、右上の **+ → New repository** をクリック
2. Repository name に `kintore`（任意）を入力、**Public** を選択、**Add a README file** にチェック → **Create repository**
3. 作成されたリポジトリのページで **Add file → Upload files**
4. 手元の `index.html` と `README.md` をドラッグ＆ドロップ → **Commit changes**

### B. コマンドラインから

```bash
# 新しいフォルダで初期化
mkdir kintore && cd kintore

# index.html と README.md を配置（このリポジトリの内容をコピー）

git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<YOUR_USERNAME>/kintore.git
git push -u origin main
```

---

## GitHub Pages で公開する

1. リポジトリの **Settings → Pages**
2. **Source** で `Deploy from a branch` を選択
3. **Branch** を `main` / `(root)` に設定 → **Save**
4. 数十秒後にページ上部に `https://<YOUR_USERNAME>.github.io/kintore/` のような URL が表示される
5. その URL を iPhone の Safari で開く

---

## ファイル構成

```
kintore/
├── index.html      ← アプリ本体（React + Recharts を CDN から読み込み）
├── README.md
└── .gitignore
```

ビルドステップなし。`index.html` を直接ブラウザで開けば動きます。

---

## 技術メモ

- 依存ライブラリなし（Vanilla JavaScript + SVG）
- データ永続化は `localStorage`（端末ローカル）
- 推定1RMは Epley 式 `weight × (1 + reps / 30)`
- ストリーク日数はトップ右上に表示（連続記録日カウンタ）
- ファイル1個・約42KB。オフラインでも動作

---

## カスタマイズしたい場合

`index.html` 内の `LIB` を編集すると種目リストを変更できます。
配色は CSS の `:root { --accent: ... }` でまとめて管理しています。
アプリ内アイコンは `ICONS` オブジェクト（インラインSVG）を編集すると差し替えられます。
「よく使う」の表示件数は `renderSheet` 内の `computeFrequentExercises(12)` の数値で調整できます。

---

## 更新履歴

### v1.3 (2026-06-16)

- ホーム画面アイコンを PNG 化（オレンジ背景＋白バーベル、180x180 を base64 埋め込み）。iOS は apple-touch-icon に SVG を使えないため、確実に表示されるよう修正

### v1.2 (2026-06-14)

- PR祝福画像を起動時に先読みデコードし、初回PR時の一瞬のひっかかりを解消
- 紙吹雪に `pointer-events:none` を付与し、タップが確実に「続ける」ボタンへ届くよう改善
- 紙吹雪の量（24→14）と演出時間（2.5s→2s）を調整

### v1.1 (2026-06-12)

- 種目選択シートに「★ よく使う」カテゴリを追加（使用頻度の自動集計・記録があれば最初に表示）
- アプリ内の絵文字（🏋️ / ∅ / 📈 / 🔍 / 📅 など）をテーマカラーのオリジナル SVG アイコンに置換
- 修正: ケーブル種目のフラグが保存時に失われ、履歴で「ケーブル」タグが表示されない問題

---

## ライセンス

MIT
