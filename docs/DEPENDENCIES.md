# ソースコード間の依存関係

## フォルダ構成（本リポジトリ）

| ディレクトリ・ファイル | 役割 |
|------------------------|------|
| `index.html` | トップ（ダッシュボード入口） |
| `pages/` | 各ビューのHTML（it_portfolio_dashboard, project_view, application_layer_view） |
| `js/` | ページごとのスクリプト |
| `config/` | アプリ設定（config.js, config.yaml）、多言語（i18n.js） |
| `data/` | 表示用JSON（it_portfolio, primary_projects, project_list など） |
| `assets/css/` | Tailwind の input.css / ビルド済み output.css |
| `docs/` | ドキュメント |

---

## 1. 全体像（レイヤー別）

```
[ビルド]  tailwind.config.js + input.css  →  output.css
[エントリ]  index.html / pages/*.html
[設定]     config.js ← config.yaml（オプション）
[多言語]   i18n.js（pages/* のみ）
[ページJS] it_portfolio_dashboard.js / project_view.js / application_layer_view.js
[データ]   data/*.json（config.yaml の data_sources でパス指定可能）
```

---

## 2. HTML が読み込むもの

| HTML | CSS | 設定 | 多言語 | 外部 | ページ用JS |
|------|-----|------|--------|------|------------|
| **index.html** | `./assets/css/output.css` | `config/config.js` | — | — | — |
| **pages/it_portfolio_dashboard.html** | `../assets/css/output.css` | `../config/config.js` | `../config/i18n.js` | js-yaml (CDN) | `../js/it_portfolio_dashboard.js` |
| **pages/project_view.html** | 同上 | 同上 | 同上 | js-yaml (CDN) | `../js/project_view.js` |
| **pages/application_layer_view.html** | 同上 | 同上 | 同上 | js-yaml (CDN) | `../js/application_layer_view.js` |

- **config.js** は全ページで読み込まれ、`window.APP_CONFIG` と `window.getDataPath` を定義する。
- **config.yaml** は `config.js` の `loadConfig()` から `fetch` される（YAML パースには js-yaml が必要）。pages/* では js-yaml を読んでいるため YAML が使える。index.html では js-yaml を読んでいない。
- **i18n.js** は pages/* のみが読み込み、`window.I18N` を定義。index は多言語用スクリプトなし。

---

## 3. 設定まわりの依存

```
config/config.js
  ├── 静的な window.APP_CONFIG（色・レイアウト・ステータスなど）
  ├── loadConfig() → fetch('../config/config.yaml')
  │     └── 要: js-yaml（YAML パース）
  └── getDataPath(key) → config.yaml の data_sources[key] または '../data/${key}.json'
```

- **config.yaml** の `data_sources` で、各データのパスを上書き可能。
- 未指定時は `../data/<key>.json` が使われる（pages/* からの相対パス）。

---

## 4. ページ用 JS が参照するもの

| JS ファイル | 利用するグローバル | 読み込むデータ（getDataPath / フォールバック） |
|-------------|--------------------|-----------------------------------------------|
| **js/it_portfolio_dashboard.js** | `config` (APP_CONFIG), `i18n` (I18N) | `it_portfolio.json`, `primary_projects.json`, `resource_allocation.json` |
| **js/project_view.js** | 同上 | `departments.json`, `project_list.json` |
| **js/application_layer_view.js** | 同上 | `primary_projects.json`, `application_list.json` |

- データパスは `getDataPath('キー')` で取得。未設定なら上記のフォールバックパス。
- 色・レイアウト・文言は `config`（および必要なら `i18n`）を参照。

---

## 5. データファイルの使用箇所

| データファイル | 使用ページ |
|----------------|------------|
| **data/it_portfolio.json** | ITポートフォリオダッシュボード |
| **data/primary_projects.json** | ITポートフォリオダッシュボード / アプリケーション層ビュー |
| **data/resource_allocation.json** | ITポートフォリオダッシュボード |
| **data/application_list.json** | アプリケーション層ビュー |
| **data/departments.json** | プロジェクトビュー |
| **data/project_list.json** | プロジェクトビュー |

---

## 6. CSS ビルドの依存

```
assets/css/input.css
  └── @tailwind base / components / utilities
        ↓ npm run build:css（Tailwind CLI）
assets/css/output.css
```

- **tailwind.config.js** の `content` に含まれるファイルがクラススキャン対象：
  - `./index.html`
  - `./pages/**/*.html`
  - `./js/**/*.js`
  - `./config/**/*.js`
- これらの HTML/JS で使われている Tailwind クラスだけが `output.css` に含まれる。

---

## 7. ページ間のリンク（ナビゲーション）

- **index.html** から以下へリンク：
  - `pages/it_portfolio_dashboard.html`
  - `pages/project_view.html`
  - `pages/application_layer_view.html`
- 各 html ページから index へのリンクは、README や実装を確認する必要あり（必要なら追加）。

---

## 8. 依存関係の簡易図（ランタイム）

```
index.html
  → assets/css/output.css
  → config/config.js

pages/it_portfolio_dashboard.html
  → assets/css/output.css
  → config/config.js
  → config/i18n.js
  → js-yaml (CDN)
  → js/it_portfolio_dashboard.js
        → config.yaml (via loadConfig)
        → data/it_portfolio.json, primary_projects.json, resource_allocation.json

pages/project_view.html
  → (同上パターン)
  → js/project_view.js
        → data/departments.json, project_list.json

pages/application_layer_view.html
  → (同上パターン)
  → js/application_layer_view.js
        → data/primary_projects.json, application_list.json
```

この内容を README に「ソースコードの依存関係は `docs/DEPENDENCIES.md` を参照」と一行だけ追記することもできます。必要ならその追記案も出します。
